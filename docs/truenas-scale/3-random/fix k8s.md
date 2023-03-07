# Fix K8s

```
zfs set mountpoint=legacy nvme-storage/ix-applications/k3s/kubelet
mount -t zfs nvme-storage/ix-applications/k3s/kubelet /var/lib/kubelet
zfs set mountpoint=legacy $(zfs list -o name | grep ix-applications | grep pvc)
midclt call service.stop kubernetes
midclt call service.start kubernetes
```