## ETCDCTL

- $ kubectl describe pod <etdcd pod> -n kube-system </br>
- $ etcdctl --endpoints=https://127.0.0.1:2379 --cert=<path> --cacert=<path> --key=<key> snapshot save <target path> </br>
- $ etcdctl  --data-dir=<new-data-dir> snapshot restore <path of snapshot file> </br>
- modify /etc/kubernetes/manifests/etcd.yaml to --data-dir </br>
- check $ watch "docker ps |grep etcd" before refiring kubectl
  
