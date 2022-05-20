# OpenShift Nodesのステータス確認

`oc get nodes`

# 各NodesのCPU使用率とメモリーの使用率確認

`oc adm top nodes`

# ClusterVersionの確認

`oc get clusterversion`

# Cluster Operator の確認

`oc get clusteroperator`

# Node端末を操作

`oc debug [node/node-name]`

# OpenShift Console表示

`oc whoami --show-console`

# OpenSifit 設定情報表示

`cat /usr/local/etc/ocp.config`

# OpenShift にログイン

`oc login -u kubeadmin -p ${RHT_OCP4_KUBEADM_PASSWD} https://api.ocp4.example.com:6443`

# 内部レジストリーサーバのログ

`oc get pod -n openshift-image-registry`

# Operator Podのログ

`oc logs --tail 3 -n openshift-image-registry cluster-image-registry-operator-*`

# Kubeletのログ

`oc adm node-logs --tail 1 -u kubelet master01`

# Kubelet Serviceのステータス

`oc debug node/master01
sh-4.4# chroot /host
systemctl status kubelet
`

# CRI-O Serviceのステータス

`oc debug node/master01
sh-4.4# chroot /host
systemctl status cri-o
`

# openvswitchのステータス

`oc debug node/master01
sh-4.4# chroot /host
crictl ps --name openvswitch
`

# Openshiftのステータス

`oc status`

# Openshiftのイベント一覧

`oc get events`

# Red Hat Container Catalog にログイン

`podman login registry.redhat.io`

# Devloyment一覧確認

`oc get deployment`
