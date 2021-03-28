## ETCDCTL

- $ kubectl describe pod <etdcd pod> -n kube-system </br>
- $ ETCDCTL_API=3</br>
- $ etcdctl --endpoints=https://127.0.0.1:2379 --cert=<path> --cacert=<path> --key=<key> snapshot save <target path> </br>
- $ ETCDCTL_API=3 etcdctl  --data-dir=<new-data-dir> snapshot restore <path of snapshot file> </br>
- modify /etc/kubernetes/manifests/etcd.yaml to --data-dir and etcd-data in volumes at two places </br>
- check $ watch "docker ps |grep etcd" before refiring kubectl
  
