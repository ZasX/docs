# Find apps with broken secret

```bash
for NS in $(k3s kubectl get ns -o name | cut -d "/" -f 2 | grep "ix-"); 
do
  HELM_REVISION=$(k3s kubectl get deployment -n $NS -o json | jq -r '.items[0].metadata.labels["helm-revision"]')
  LATEST_SECRET_VERSION=$(k3s kubectl get secrets -n $NS -o name | grep "sh.helm.release" | tail -n 1 | rev | cut -d "." -f 1 | rev | cut -d "v" -f 2)
  
  if [ "$HELM_REVISION" -lt "$LATEST_SECRET_VERSION" ]; then
    echo "$NS has helm revision $HELM_REVISION and newer secret version $LATEST_SECRET_VERSION"
  fi
done;
```