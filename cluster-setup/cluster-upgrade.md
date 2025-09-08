## Upgrading cluster to latest version

There are two ways. One is to sequentially upgrade using kubeadm but leap upgradation from one older version to directly newer version is not possible

- $ kubectl get nodes # get node's k8s version
- $ kubeadm version
- $ kubeadm upgrade plan
- $ kubeadm upgrade apply <version>

Another way is to directly upgrade  kubeadm to a newer version but this needs to be done by downloading kubeadm explicitly

- $ kubeadm version
- $ apt install kubeadm=1.19.0-00 -y
- $ kubeadm upgrade apply v1.19.0
- $ apt install kubelet=1.19.0-00  # use the same version of kubelet and do not prefix 'v' before the version
- $ systemctl enable kubelet && systemctl restart kubelet
