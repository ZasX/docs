k3s kubectl get pods -n ix-nextcloud | egrep 'nextcloud-[0-9a-g]{10}-[0-9a-z]{5}' | cut -d " " -f 1

k3s kubectl exec -n ix-nextcloud --stdin --tty $(k3s kubectl get pods -n ix-nextcloud | egrep -o 'nextcloud-[0-9a-g]{10}-[0-9a-z]{5}') -- runuser -u www-data -- php /var/www/html/occ preview:pre-generate

k3s kubectl logs -n ix-APPNAME $(k3s kubectl -n ix-APPNAME get pods | tail -n 1 | cut -d " " -f 1)




k3s kubectl get pods -n ix-nextcloud -o json

jq '.items[] | select(.status.containerStatuses[].started | not) | [{name: .metadata.name, statuses: .status.containerStatuses}]'


jq '.items[].metadata | select(.labels."app.kubernetes.io/name" = "nextcloud") | [.metadata.name]'



k3s kubectl get pods -n ix-nextcloud -o json | jq '.items[].metadata | select(.labels."app.kubernetes.io/name" == "nextcloud") | .name'



k3s kubectl get pods -n ix-nextcloud -l "app.kubernetes.io/name"=nextcloud -o name


k3s kubectl exec -n ix-nextcloud --stdin --tty $(k3s kubectl get pods -n ix-nextcloud -l "app.kubernetes.io/name"=nextcloud -o name) -c nextcloud -- runuser -u www-data -- php /var/www/html/occ preview:pre-generate

k3s kubectl exec -n ix-nextcloud --stdin --tty nextcloud-7d649c665b-tkgq4 -- runuser -u www-data -- echo "test"
