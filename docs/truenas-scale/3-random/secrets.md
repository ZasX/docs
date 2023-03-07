# List secrets for namespace:

```bash
k3s kubectl get secrets --namespace ix-vaultwarden
```

# List data of named secret:

```bash
k3s kubectl get secret/dbcreds --namespace ix-vaultwarden -o jsonpath='{.data}'
```

# List data of named secret and decode base64:

```bash
k3s kubectl get secret/dbcreds --namespace ix-vaultwarden -o json | jq '.data[] |= @base64d'
```

# Just get the db pw

```bash
k3s kubectl get secret/dbcreds --namespace ix-vaultwarden -o jsonpath='{.data.postgresql-password}' | base64 --decode
```
