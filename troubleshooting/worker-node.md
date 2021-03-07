## Worker node troubleshooting

- check if nodes are ready
- if not ready $ kubectl describe node <node>
  - if status is unknown, worker node failed to communicate with master due to issues, there is a possible loss of that node
  - get last heartbeat time to know when the node has crashed
  - check the node 
    - $ top for CPU  
    - $df -h disk
    - $ service kubelet status
    - $ sudo journalctl -u kubelet or $ journalctl -u kubelet -f
    - $ cat /var/lib/kubelet/config.yml and $ cat /etc/kubernetes/kubelet.conf
  - Check the kubelet certificates to ensure they are not expired
    - $ openssl -x509 -in /var/lib/kubelet/worker-1.crt -text 
