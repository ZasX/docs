# Stop an app

to stop an app via CLI: 

`midclt call chart.release.scale <appname> '{"replica_count": 0}'`

or:

`k3s kubectl scale deploy testwarden-vaultwarden -n ix-testwarden --replicas=0`