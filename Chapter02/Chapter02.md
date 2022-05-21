
# 設定情報・ログイン

## 設定情報表示

```
cat /usr/local/etc/ocp.config
```

## OpenShift ログイン

```
oc login -u kubeadmin -p ${RHT_OCP4_KUBEADM_PASSWD} https://api.ocp4.example.com:6443
```

## Console URL表示

```
oc whoami --show-console
```

## Red Hat Container Catalog にログイン

```
podman login registry.redhat.io
```

# Openshiftのステータス

```
oc status
```

# リソース表示

## Node 一覧

```
oc get nodes
```

## Pod 一覧

```
oc get pods
```

## Devloyment一覧

```
oc get deployment
```

## イベント一覧

```
oc get events
```

## ストレージクラス

```
oc get storageclass
```

## Persistent Volume Claim

```
 oc get pvc
```

## Persistent Volume

```
 oc get pv
```

## Cluster Version

```
oc get clusterversion
```

## Cluster Operator

```
oc get clusteroperator
```

# 管理 oc adm

## 各NodesのCPU使用率とメモリーの使用率確認

```
oc adm top nodes
```

# 端末を操作 oc debug

```
oc debug [node/node-name]
```

# イメージレジストリ

## 内部レジストリーサーバのログ

```
oc get pod -n openshift-image-registry
oc logs --tail 3 -n openshift-image-registry cluster-image-registry-operator-*
```

# Kubeletのログ

```
oc adm node-logs --tail 1 -u kubelet master01
```

# システムサービス ステータス

## Kubelet Serviceのステータス

```
oc debug node/master01
sh-4.4# chroot /host
systemctl status kubelet
```

## CRI-O Serviceのステータス

```
oc debug node/master01
sh-4.4# chroot /host
systemctl status cri-o
```

## openvswitchのステータス

```
oc debug node/master01
sh-4.4# chroot /host
crictl ps --name openvswitch
```

# PVC

## Add

```
oc set volumes deployment/postgresql-persistent 
--add 
--name postgresql-storage 
--type pvc 
--claim-class nfs-storage 
--claim-mode rwo 
--claim-size 10Gi 
--mount-path /var/lib/pgsql 
--claim-name postgresql-storage

 oc get pvc
```

# Application

## PostgreSQL

```
podman login registry.redhat.io
oc new-app --docker-image registry.redhat.io/rhel8/postgresql-13:1-7 -e POSTGRESQL_USER=redhat -e POSTGRESQL_PASSWORD=redhat123 -e POSTGRESQL_DATABASE=persistentdb --name postgresql-persistent 
```
