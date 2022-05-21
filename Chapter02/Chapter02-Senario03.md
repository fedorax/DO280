1. 設定情報読み込み

```
source /usr/local/etc/ocp4.config
```

2. OpenShiftにログイン

```
oc login -u kubeadmin -p ${RHT_OCP4_KUBEADM_PASSWD} https://api.ocp4.example.com:6443
```

3. Pod一覧を確認

```
oc get pod
```

4. Openshiftのステータスを確認

```
oc status
```

5. イベントを確認

```
oc get events
```

6. registry.redhat.ioにログイン

```
podman login registry.redhat.io
```

7. コンテナイメージを確認

```
skopeo inspect docker://registry.redhat.io/rhel8/postgresql-13:1 
```

8. deployments一覧を確認

```
oc get deployments
```

9. deploymentを編集

```
oc edit deployment/psql
```

10. ステータスの変更を確認

```
oc status
```

11. Podの状態を確認

```
oc get pods
```
