1. 設定情報読み込み

```
source /usr/local/etc/ocp4.config
```

2. OpenShiftにログイン

```
oc login -u kubeadmin -p ${RHT_OCP4_KUBEADM_PASSWD} https://api.ocp4.example.com:6443
```

3. Project作成

```
oc new-project install-storage
```

4. ストレージクラス確認

```
oc get storageclass
```

5. PostgreSQLを作成

```
oc new-app --name postgresql-persistent --docker-image registry.redhat.io/rhel8/postgresql-13:1-7 -e POSTGRESQL_USER=redhat -e POSTGRESQL_PASSWORD=redhat123 -e POSTGRESQL_DATABASE=persistentdb
```

6. ボリュームの作成とアプリに追加

```
oc set volumes deployment/postgresql-persistent --add --name postgresql-storage --type pvc --claim-class nfs-storage --claim-mode rwo --claim-size 10Gi --mount-path /var/lib/pgsql --claim-name postgresql-storage
```

7. PVCの確認

```
oc get pvc
```

8. PVの確認

```
oc get pv -o custom-columns=NAME:.metadata.name,CLAIM:.spec.claimRef.name
```

9. PostgreSQLのアプリを削除

```
oc delete all -l app=postgresql-persistent
```

10. PVCの削除

```
oc delete pvc/postgresql-storage
```
