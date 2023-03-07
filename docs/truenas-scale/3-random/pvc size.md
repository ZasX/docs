# PVC Size

```
zfs list $(cli -c 'app kubernetes config' | grep -E "pool\s\|" | awk -F '|' '{print $3}' | tr -d " \t\n\r")/ix-applications/releases -r | grep pvc | awk '{print $1, $2}' | sort -hr -k2 | column -t
```