1. 設定ファイル読み込み

```
source /usr/local/etc/ocp4.config
```

2. ログイン

```
oc login -u admin -p redhat https://api.ocp4.example.com:6443
```

3. クラスターロールバインディングからself-provisionerを検索

```
oc get clusterrolebinding -o wide | grep -E 'NAME|self-provisioners'
```

4. self-provisionerの詳細を表示

```
oc describe clusterrolebindings self-provisioners
```

5. 管理者でself-provisionerからsystem:authenticated:oauthのグループをクラスターロールから削除

```
oc adm policy remove-cluster-role-from-group self-provisioner system:authenticated:oauth
```

6. 管理者でleaderユーザにadminロール権限のポリシーを追加

```
oc policy add-role-to-user admin leader
```

7. dev-groupのグループを作成し、developerユーザをdev-groupに追加

```
oc adm groups new dev-group
oc adm groups add-users dev-group developer
```

8. dev-groupにedit権限を追加

```
oc policy add-role-to-group edit dev-group
```

9 グループ一覧を表示

```
oc get groups
```
