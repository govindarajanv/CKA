## Control plane failure

- Check the status of nodes
- Check the status of pods
- if pods are in pending state without any issues with those pods, there could issue with scheduler
- Note: static pods will have node name appended (kube-scheduler if run as static pod will have its node name appended in its pod name)
- check the service configuration /etc/systemd/system/kubelet.service.d/10.kubeadm.conf
    - you will get config and args from this file
    - search for 'staticPodPath' in config.yml -> gives the default path for storing configurations related to static pods
- if the scaling does not work, the issue could be with kube-controller-manager

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
