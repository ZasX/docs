# Databases

## Setting up the files

In Truenas Scale, create a dataset that can house the script files. Put the `.sh` files in that dataset. Navigate to the dataset from the host shell. Make the files executable.

```bash
cd /mnt/tank/scripts/databases
chmod +x tcdbinfo.sh tcdbbackup.sh tcdbrestore.sh
```

## Creating a database backup

To create a backup of a database, run the `tcdbbackup.sh` script. This script will create a backup of all databases running in Truenas Scale.

```bash
./tcdbbackup.sh
```

This script will create a `.sql` file for each database, and store them in the `./dumps` directory.

## Restoring a database from backup

To restore a database from a backup, run the `tcdbrestore.sh` script with the name of the database as the argument.

```bash
./tcdbrestore.sh <app_name>
```

This script will restore the specified database from the `.sql` backup file located in the `./dumps` directory.

Note that the `tcdbrestore.sh` script will prompt for confirmation before proceeding with the restore operation.

## Prerequisites

The user running the scripts must have permissions to access the Truenas Scale host shell and execute `kubectl` commands.
