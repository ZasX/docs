# Databases info

## How to list database login info for TrueCharts apps

You can use these details to login to e.g. `pgadmin`, one of the TC apps that lets you manage databases.

```bash title="tcdbinfo.sh"
#!/bin/bash

# get namespaces
namespaces=$(k3s kubectl get pods -A | grep postgres | awk '{print $1}')

# iterate over namespaces
( printf "Application | Username | Password | Address | Port\n"
for ns in $namespaces; do
    # extract application name
    app_name=$(echo "$ns" | sed 's/^ix-//')

    # get username, password, addresspart, and port
    creds=$(k3s kubectl get secret/dbcreds --namespace "$ns" -o jsonpath='{.data.url}' | base64 --decode)
    username=$(echo "$creds" | awk -F '//' '{print $2}' | awk -F ':' '{print $1}')
    password=$(echo "$creds" | awk -F ':' '{print $3}' | awk -F '@' '{print $1}')
    addresspart=$(echo "$creds" | awk -F '@' '{print $2}' | awk -F ':' '{print $1}')
    port=$(echo "$creds" | awk -F ':' '{print $4}' | awk -F '/' '{print $1}')

    # construct full address
    full_address="${addresspart}.${ns}.svc.cluster.local"

    # print results with aligned columns
    printf "%s | %s | %s | %s | %s\n" "$app_name" "$username" "$password" "$full_address" "$port"
done ) | column -t -s "|"
```