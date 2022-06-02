1. ログイン

```
oc login -u developer -p developer https://api.ocp4.example.com:6443
```

2. プロジェクト作成

```
oc new-project authorization-secrets
```

3. 環境情報のシークレット作成

```
oc create secret generic mysql --from-literal user=myuser --from-literal password=redhat123 --from-literal database=test_secrets --from-literal hostname=mysql
```

4. MYSQL アプリをデプロイ

```
 oc new-app --name mysql --docker-image registry.redhat.io/rhel8/mysql-80:1
```

5. MYSQLアプリにシークレットを設定

```
oc set env deployment/mysql --from secret/mysql --prefix MYSQL_
```

6. ボリュームをアプリに追加

```
oc set volume deployment/mysql --add --type secret --mount-path /run/secrets/mysql --secret-name mysql
```

7. Quotes アプリケーションをデプロイ

```
 oc new-app --name quotes  --docker-image quay.io/redhattraining/famous-quotes:2.1
```

8. Quotes アプリにシークレットを設定

```
oc set env deployment/quotes --from secret/mysql --prefix QUOTES_
```

9. サービスを公開

```
oc expose service quotes --hostname quotes.apps.ocp4.example.com
```
