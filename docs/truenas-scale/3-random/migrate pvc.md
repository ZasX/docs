Basically You can do:

* `k3s kubectl get pvc -A`
* note the VOLUME name of the old and new PVC locations (where new is copy target and old is copy source in this example)
* `zfs list`
* Find the volumes under `tank/ix-applications/releases/` etc.

Then either:

A.

* `zfs destroy new-pvc`
* `zfs rename old-pvc new-pvc`

B.

* `zfs destroy new-pvc`
* `zfs snapshot old-pvc@migrate`
* `zfs send old-pvc@migrate | zfs recv new-pvc@migrate`