# Mosquitto

Inside the mosquitto shell, you'll want to:

\- navigate to /mosquitto/configinc

```
cd /mosquitto/configinc
```

\- create password file

```
mosquitto_passwd -c passwordfile username
```

\- create config file to use password

```
echo "password_file /mosquitto/configinc/passwordfile" > passwordconfig.conf
```