## Control plane failure

- Check the status of nodes
- Check the status of pods

### Control plane components as pods

- Check the status of pods in kube-system namespace
- $ kubectl logs kube-apiserver-master -n kube-system
- $ kubectl logs kube-apiserver-master -n kube-system
### Control plane components as services

- Check the status of services on master node
  - $ service kube-apiserver status
  - $ service kube-controller-manager status
  - $ service kube-scheduler status
  - $ sudo journalctl -u kube-apiserver 
- Check the status of services on worker nodes
  - $ service kubelet status
  - $ service kube-proxy status
