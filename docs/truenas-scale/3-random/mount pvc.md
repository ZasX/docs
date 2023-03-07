# Mounting a PVC

### **ALWAYS MAKE SURE THE APP IS STOPPED WHILE MOUNTING THE PVC**

###### To get the PVCNAME:
```bash
k3s kubectl get pvc -n ix-APPNAME
```

###### To get the PVCPATH:

```bash
zfs list | grep legacy | grep APPNAME
```

###### If you want to mount the PVC content:

```bash
zfs set mountpoint=/temp PVCPATH
```

Your PVC will be mounted under `/mnt/temp`

###### and when you're done editing:

```bash
zfs set mountpoint=legacy PVCPATH
```
