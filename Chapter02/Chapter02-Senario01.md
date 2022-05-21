1. 設定情報読み込み

```
source /usr/local/etc/ocp4.config
```

2. OpenShiftにログイン

```
oc login -u kubeadmin -p ${RHT_OCP4_KUBEADM_PASSWD} https://api.ocp4.example.com:6443
```

3. Node一覧を確認

```
oc get nodes
```

4. Nodeのリソースを確認

```
oc adm top node
```

5. master ノードの詳細を確認

```
oc describe node master01
```
