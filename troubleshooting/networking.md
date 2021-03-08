## Networking related troubleshooting

- CNI plugin is located at /etc/cni/net.d/  , other way to check is $ kubectl get pods
- if it is not installed, install from the link provided in the documentation (search for 'cloud.weave.works')
- Refer the below configuration of kubelet (/var/lib/kubelet)
  - cni-bin-dir:   Kubelet probes this directory for plugins on startup

  - network-plugin: The network plugin to use from cni-bin-dir. It must match the name reported by a plugin probed from the plugin directory.
- check if coreDNS pods are in running status if not
  - check if CNI plugin is installed
- if CoreDNS pods and the kube-dns service are working fine, check the kube-dns service has valid endpoints $ kubectl -n kube-system get ep kube-dns
  - If there are no endpoints for the service, inspect the service and make sure it uses the correct selectors and ports
- In a cluster configured with kubeadm, you can find kube-proxy as a daemonset
  - Get status of kube-proxy
  - Check kube-proxy logs
  - Check configmap is correctly defined and the config file for running kube-proxy binary is correct.
  - Get the configuration file from command displayed by $ kubectl describe ds kube-proxy -n kube-system
  - *Remember:* kube-proxy configurations are fed through config Maps which is not listed by $ k get all -n kube-system
- check if name resolution works from within pod $nslookup <dnsname>
- check if endpoints of a service has all ip addresses of related pods
    - $ wget -q0- <pod ip>:<port>  # this gives out hostname
- Login to kube-proxy $ kubectl exec -it <kube proxy pod> -n kube-system -- sh  
    - $ iptables-save | grep hostname
