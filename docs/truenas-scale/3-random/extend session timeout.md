# Extend session timeout

This command sets the session timeout of Scale to 7 days, replacing the default 5 minutes.

Run it in the Scale shell.

```bash
sed -i 's/auth.generate_token",\[300/auth.generate_token",\[604800/g'  /usr/share/truenas/webui/*.js
```

