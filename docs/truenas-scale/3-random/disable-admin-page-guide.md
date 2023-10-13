# Disabling the Admin Interface

:::caution Backup Reminder

Before proceeding, ensure that you have backed up important configurations, especially when making changes to configuration files or secrets. It's also advisable to back up the Vaultwarden database to prevent potential data loss.

:::

## Modify the Host Secret

To start with the deactivation, you must first modify the secret on the host's shell. Execute the following command:

```sh
k3s kubectl patch secret vaultwarden-vaultwardensecret -n ix-vaultwarden --type='json' -p='[{"op": "remove", "path": "/data/ADMIN_TOKEN"}]'
```

## Update Container Config

Next, while inside the Vaultwarden container, run the command below to modify the `config.json` file:

```sh
sed -i.bak '/admin_token/d' /data/config.json
```

:::info Command Explanation

- The `sed` command is used to search and delete the line containing `admin_token` from the `config.json` file.
- A backup of the original `config.json` is created with the `.bak` extension before making the change.

:::

## Adjust the App Configuration

Finally, head to the Vaultwarden app's configuration:

1. Find and disable the admin interface option (if it is still enabled).
2. Click "Save" at the bottom to apply the changes.
