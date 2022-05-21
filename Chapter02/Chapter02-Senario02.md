1. 設定情報読み込み

```
source /usr/local/etc/ocp4.config
```

2. OpenShiftにログイン

```
oc login -u kubeadmin -p ${RHT_OCP4_KUBEADM_PASSWD} https://api.ocp4.example.com:6443
```

3. master01ノードの端末操作

```
oc debug node/master01
chroot /host
```

4. kubelet のシステムステータス確認

```
systemctl status kubelet
```

5. cri-oのシステムステータス確認

```
systemctl status cri-o
```

6. openvswitchのステータス確認

```
crictl ps --name openvswitch
```
