# Migrating

## Goal
Migrating an existing app to a new app, while still having the old app available. So if something goes wrong, you haven't lost the old app.
## Prerequisites
Make sure you have `pgadmin` installed, as we'll be using it to make a backup and to restore the database. It's a TrueCharts app from the stable train.
Also make sure you have the database info script available on your server.
## Create new app with a different name
In this guide, I'll be using the app `vaultwarden` as an example. I chose the name `testwarden` for the new app install.
I installed the new app with mostly default settings, just changed service type to `ClusterIP` and setup a new, temporary, ingress.
## Configure database connections in pgadmin
run the tcdbinfo.sh script to see the connection details for both the old and the new database, and set them up in pgadmin.
## Scale down both apps
```bash
k3s kubectl scale deploy testwarden-vaultwarden -n ix-testwarden --replicas=0
k3s kubectl scale deploy vaultwarden -n ix-vaultwarden --replicas=0
```
## Create database backup
In `pgadmin`, right click `vaultwarden->Databases->vaultwarden` and click `Backup...`. Give the file a name (e.g. `vaultwarden.sql`) and click `Backup`.
## Restore database backup
In `pgadmin`, right click `testwarden->Databases->vaultwarden` and click `Restore...`. Select the sql file (`vaultwarden.sql`).
On the 2nd tab page, select the first 3 options (`Pre-data`, `Data` and `Post-data`). On the last tab, select `Clean before restore`. Now click `Restore`.
## Migrate the PVC
The following command will return the data PVC's for the old and the new install
```bash
k3s kubectl get pvc -A | grep warden-data | sort -u | awk '{print $1, $2, $4}' | column -t
zfs list | grep warden | grep legacy
```
Destroy the PVC of the new app and replicate the PVC of the old app to the new location.
```bash
zfs destroy new-pvc
zfs snapshot old-pvc@migrate
zfs send old-pvc@migrate | zfs recv new-pvc@migrate
zfs set mountpoint=legacy new-pvc
```
The `new-pvc` will look something like `poolname/ix-applications/releases/testwarden/volumes/pvc-32341f93-0647-4bf9-aab1-e09b3ebbd2b3`.
The `old-pvc` will look something like `poolname/ix-applications/releases/vaultwarden/volumes/pvc-40275e0e-5f99-4052-96f1-63e26be01236`.
## Scale up both apps
```bash
k3s kubectl scale deploy testwarden-vaultwarden -n ix-testwarden --replicas=1
k3s kubectl scale deploy vaultwarden -n ix-vaultwarden --replicas=1
```
## Reflection
You should now be able to log in on the new install.