# Databases restore

## Restoring databases for TrueCharts apps

```bash title="tcdbrestore.sh"
#!/bin/bash

# Read the variable from command line argument
app=$1
folder="./dumps/"
file="$app.sql"
ns="ix-$app"

# Check if the file exists
if [ -f "$folder$file" ]; then
    echo "The file $file exists."
else
    echo "The file $file does not exist."
    exit 1
fi

# Ask the user if they want to continue
read -p "Are you sure you want to restore the database for $app? (y/N): " answer

if [ "$answer" != "y" ]; then
    echo "Exiting script."
    exit 0
fi

# If the user chooses to continue, execute the rest of the script here
echo "Restoring database for $app."

# Scale down deployment to avoid inconsistencies in DB
k3s kubectl scale deploy "$app" -n "$ns" --replicas=0
while true; do k3s kubectl get pods -n "$ns" | grep -i -q terminating || break; done;

k3s kubectl cp -n "$ns" -c "$app"-postgresql "$folder$file" "$app-postgresql-0:tmp/$file"
k3s kubectl exec -n "$ns" -c "$app"-postgresql "$app"-postgresql-0 -- bash -c 'PGPASSWORD=$POSTGRES_PASSWORD pg_restore -U $POSTGRES_USER -d $POSTGRES_DB --clean /tmp/'$file

# Scale deployment back up
k3s kubectl scale deploy "$app" -n "$ns" --replicas=1

echo "Restoring database for $app finished."
```