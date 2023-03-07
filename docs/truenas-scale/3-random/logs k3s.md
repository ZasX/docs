```
k3s kubectl logs -n NAMESPACE PODNAME
```

NAMESPACE is often ix-APPNAME. PODNAME can be found with:

```
k3s kubectl get pods -n NAMESPACE
```