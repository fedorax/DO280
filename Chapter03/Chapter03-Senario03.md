1.1. ~/DO280/labs/auth-review/tmp_users HTPasswd 認証ファイルから analyst ユーザーを削除します。

```
htpasswd -D ~/DO280/labs/auth-review/tmp_users analyst
```

1.2. tester、leader、admin、developerのパスワードを更新する

```
htpasswd -b ~/DO280/labs/auth-review/tmp_users tester 'L@bR3v!ew' &&
htpasswd -b ~/DO280/labs/auth-review/tmp_users leader 'L@bR3v!ew' &&
htpasswd -b ~/DO280/labs/auth-review/tmp_users admin 'L@bR3v!ew' &&
htpasswd -b ~/DO280/labs/auth-review/tmp_users developer 'L@bR3v!ew' 
```

1.3. HTPasswdの確認

```
cat ~/DO280/labs/auth-review/tmp_users
```

2.1. 設定ファイル読み込み

```
source /usr/local/etc/ocp4.config
```

2.2 OpenShiftにログイン

```
oc login -u kubeadmin -p ${RHT_OCP4_KUBEADM_PASSWD} https://api.ocp4.example.com:6443
```

2.3 HTPasswdファイルをシークレットに追加

```
oc create secret generic htpasswd -n openshift-config --from-file htpasswd=~/DO280/labs/auth-review/tmp_users
```

2.4 oauthの設定にHTPasswdの設定を追加

```
# Test/review patch
oc patch oauth cluster -p '{"spec":{"identityProviders":[{"htpasswd":{"fileData":{"name":"htpasswd"}},"name":"htpasswd","type":"HTPasswd"}]}}' --type=merge --dry-run=server -o yaml

# Apply patch
oc patch oauth cluster -p '{"spec":{"identityProviders":[{"htpasswd":{"fileData":{"name":"htpasswd"}},"name":"htpasswd","type":"HTPasswd"}]}}' --type=merge
```

3.1. adminユーザにcluseter-adminの権限を追加

```
oc adm policy add-cluster-role-to-user cluster-admin admin
```

3.2. adminユーザにクラスター全体にプロジェクトを作成する機能を削除します。

```
oc adm policy remove-cluster-role-from-group  self-provisioner system:authenticated:oauth
```

4.1. managers という名前のグループを作成し、そのグループに leader ユーザーを追加します。

```
oc adm groups new  managers

oc adm groups add-users managers leader
```

4.2. プロジェクト作成権限を managers グループに付与します。

```
oc adm policy add-cluster-role-to-group  self-provisioner managers
```

5.1 developersグループを作成し、グループにdeveloperを追加して、edit権限を追加する

```
oc adm groups new developers 
oc adm groups add-users developers developer
oc policy add-role-to-group edit developers
```

5.2 qaグループを作成し、グループにtesterを追加して、edit権限を追加する

```
oc adm groups new qa 
oc adm groups add-users qa tester
oc policy add-role-to-group view qa
```
