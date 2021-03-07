## Networking related troubleshooting

- Refer the below configuration of kubelet (/var/lib/kubelet)
  - cni-bin-dir:   Kubelet probes this directory for plugins on startup

  - network-plugin: The network plugin to use from cni-bin-dir. It must match the name reported by a plugin probed from the plugin directory.
- check if coreDNS pods are in running status if not
  - check if CNI plugin is installed
- if CoreDNS pods and the kube-dns service are working fine, check the kube-dns service has valid endpoints $ kubectl -n kube-system get ep kube-dns
  - If there are no endpoints for the service, inspect the service and make sure it uses the correct selectors and ports
- In a cluster configured with kubeadm, you can find kube-proxy as a daemonset
  - Get status of kueb-proxy
  - Check kube-proxy logs
  - Check configmap is correctly defined and the config file for running kube-proxy binary is correct.
  - Get the configuration file from command displayed by $ kubectl describe ds kube-proxy -n kube-system