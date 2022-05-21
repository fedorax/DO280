1. 設定ファイル読み込み

```
source /usr/local/etc/ocp4.config
```

2. HTPasswd 作成

```
htpasswd -c -B -b ~/DO280/labs/auth-provider/htpasswd admin redhat
```

3. HTPasswd ユーザ追加

```
htpasswd -b ~/DO280/labs/auth-provider/htpasswd developer developer
```

4. HTPasswd 確認

```
cat ~/DO280/labs/auth-provider/htpasswd
```

5. OpenShiftにログイン

```
oc login -u kubeadmin -p ${RHT_OCP4_KUBEADM_PASSWD} https://api.ocp4.example.com:6443
```

6. HTPasswdのシークレットを作成

```
oc create secret generic localusers -n openshift-config --from-file htpasswd=/home/student/DO280/labs/auth-provider/htpasswd 
```

7. cluster-adminの権限をadminユーザに追加

```
oc adm policy add-cluster-role-to-user cluster-admin admin
```

8. oauthの設定

```
oc get oauth cluster -o yaml > ~/DO280/labs/auth-provider/oauth.yaml
```

9. oauthにHTPasswdを追加

```yaml
spec:
  identityProviders:
  - htpasswd:
      fileData:
        name: localusers
    mappingMethod: claim
    name: myusers
    type: HTPasswd
```

10. oauthの設定ファイルを置き換え

```
oc replace -f ~/DO280/labs/auth-provider/oauth.yaml
```

11. ユーザ一覧を確認

```
oc get users
```

12. ID一覧を確認

```
oc get identity
```

13. localuserのシークレットをダウンロード

```
oc extract secret/localusers -n openshift-config  --to ~/DO280/labs/auth-provider/ --confirm
```

14. localuserのシークレットを更新

```
oc set data secret/localusers -n openshift-config --from-file htpasswd=/home/student/DO280/labs/auth-provider/htpasswd
```

16. ランダムなパスワードを生成して、managerユーザの追加し、htpasswdのシークレットを更新

```
MANAGER_PASSWD="$(openssl rand -hex 15)"
htpasswd -b ~/DO280/labs/auth-provider/htpasswd manager ${MANAGER_PASSWD}
oc set data secret/localusers -n openshift-config --from-file htpasswd=/home/student/DO280/labs/auth-provider/htpasswd
```

17. HTPasswdでmanagerユーザの削除し、シークレットを更新

```
htpasswd -D ~/DO280/labs/auth-provider/htpasswd manager
oc set data secret/localusers -n openshift-config --from-file htpasswd=/home/student/DO280/labs/auth-provider/htpasswd
```

18. localuserのシークレットを削除

```
oc delete secret localusers -n openshift-config
```

19. すべてのユーザを削除

```
oc delete user --all
```

20. すべてのIDを削除

```
oc delete identity --all
```
