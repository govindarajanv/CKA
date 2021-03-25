## Practice Questions

1. Perform ETCD backup and restore
    -   ETCDCTL_API=3 etcdctl — endpoints=[ENDPOINT] — cacert=[CA CERT] — cert=[ETCD SERVER CERT] — key=[ETCD SERVER KEY] snapshot save [BACKUP FILE NAME]
    -   Method 1
        -   if etcd runs as a pod
            - $ export ETCDCTL_API=3
            - $ etcdctl snapshot save /path-to-backup.db --cacert=etcd-ca.pem --cert=etcd-server.crt --endpoints=https://ip:2379 --key=etcd-server.key # Get these values fromthe running pod or static pod configurations
    -   Method 2
        - if etcd is a separate server and running as a service
            - $ export ETCDCTL_API=3
            - $ etcdctl member list --cacert=etcd-ca.pem --cert=etcd-server.crt --endpoints=https://ip:2379 --key=etcd-server.key # To get the members
            - $ etcdctl snapshot get cluster.name --cacert=etcd-ca.pem --cert=etcd-server.crt --endpoints=https://ip:2379 --key=etcd-server.key # To get the members
            - $ etcdctl snapshot save /path-to-backup.db --cacert=etcd-ca.pem --cert=etcd-server.crt --endpoints=https://ip:2379 --key=etcd-server.key # Get these values fromthe running pod or static pod configurations
            - $ sudo systemctl stop etcd
            - $ sudo rm -rf /var/lib/etcd
            - $ etcdctl snapshot restore /path-to-backup.db --data-dir="/var/lib/etcd"
            - $ sudo chown -R etcd:etcd /var/lib/etcd
            - $ sudo systemctl start etcd
            - $ etcdctl snapshot get cluster.name --cacert=etcd-ca.pem --cert=etcd-server.crt --endpoints=https://ip:2379 --key=etcd-server.key # To get the members
            
1. Setup 2 nodes k8s clusters
    -   https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
3. Upgrade the current version of kubernetes from 1.18 to 1.19.0 exactly using the kubeadm utility. Make sure that the upgrade is carried out one node at a time starting with the master node. 
    -   Refer https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/
4. We should use Init Container to create a file named “sharedfile.txt” under the “work” directory and the application container should check if the file exists and sleep for a while. If the file does not exist the application container should exit.
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
        name: init-container-test
    spec:
        containers:
        - name: application-container
          image: alpine
          command: ['sh', '-c', 'if [ -f /work/sharedfile.txt ]; then sleep 99999; else exit; fi']
          volumeMounts:
          - name: workdir-volume
            mountPath: /work
    ```
    -  Answer: https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables
1.  Create a new secret from literals (username=admin, password=dummy) and use the above secret as an env variable in pod
    - Answer: 
        - $ kubectl create secret generic --from-literal username=admin --from-literal password=dummy
        - as env variable (https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables)
1.   Create a new secret from file and map the created secret to a volume and map it to the pod
    - Answer: https://kubernetes.io/docs/concepts/configuration/secret/
1. Create a pod nginx with below configurations <br> Limits: cpu 200m, Memory 300Mi, requests: cpu 100m, memory 200Mi and label: team=core-dev in dev namespace
    - Answer: </br> $ kubectl create ns dev </br> $ kubectl run nginx -o yaml --dry-run=client --image=nginx --requests "cpu=100m,memory=200Mi" --limits "cpu=200m,memory=300Mi" -l team=core-dev --namespace=dev
1. Create a pod with multi-containers in it  nginx, tomcat, redis 
    - Answer: 
        - $ kubectl run nginx -o yaml --dry-run=client --image=nginx > multipod.yml
        - Append additional entries of second and third containers and do kubectl apply
1. Create a deployment webapp with image nginx and replicas 6 and place the file in /tmp directory
    - Scale down the deployments to 4
    - Update the image of the above deployment to nginx:1.18.0 and try to record the deployments
    - Check rollout history
    - Rollback to original nginx image in the deployment
    - Expose the deployment as a service on port 80
1. Create a pod with image redis and expose the pod as a service. 
    - Also expose the service for the deployment in the previous question with type nodeport on port 30080
    - Create a pod with image redis with port and target port as 6379
    - Get the list of pods related to the service redis
1. Check DNS record of redis pod and store in a file /tmp/dns_redis_pods.txt
    -   $ kubectl get pod redis -n dev -o wide  # Get ip address 10.1.15.5
    -   $ kubectl run busybox --image=busybox --restart=Never --rm -it -- nslookup 10-1-15-5.default.pod
1. Static Pod creation by kubelet
    -   Create a static pod on worker node
        -   Answer: 
            -   Sometimes there will be a few hints about static pod if there is no explicit mention like a pod on worker node by placing in a path like /etc/kubernetes/manifests
            -   $ ps -aux|grep kubelet # gives config file /var/lib/kubelet/config.yaml
            -   create a static pod manifest in master node. Check kubelet config file and 'staticPodPath' configuration path (usually /etc/kubernetes/manifests)
            -   create a static pod on worker node. Create pod manifest in master node. Do scp to worker node. check the configuration path for staticPodPath, update if required. Restart kubelet service (enable). $ kubectl get pods -w on master, you should be able to see static pods.
1. Daemonsets with a twist due to Taint and Tolerations. 
    -   if daemonsets need to be created on all nodes (toleration for the taint on master node is required)
    -   if daemonsets need to be created only on worker nodes (no toleration is required to be set up)
3. Create a pod with image redis with “temporary” directory that shares the pod’s lifetime on a given path /share/redis.
    -   Answer: Temporary here means empty directory. Create a hostpath persistent volume, create PVC, create pod with that PVC mounted on to it.
4. Create a network policy that denies access to the payroll pod in the accounting namespace **beware of namespace**
    -   Answer: Refer documentation, add egress and Ingress with pod selector based on labels
5. Create a namespace called finance, create a network policy, that blocks all traffic to pods in finance namespace except for traffic from pods in the same namespace on port 8080
    -   Answer: **label namespace** (as we would be using namespace label selector. To select all pods in the namespace, pod selector should have empty braces {}. Use namespace selector in ingress or egress (ipblock, pod selector and namespace selector are the options available)
    -   podSelector: {} means all pods will be selected
6. Make a worker node unschedulable and drain the pods
    -   $ kubectl cordon worker01
    -   $ kubectl drain worker01 --ignore-daemonsets
7. JSONPATH Exercises
    -   Print the image of containers used by all pods across namespace
        -   $ kubectl get pod -o jsonpath=’{.items[*].metadata.name}{.items[*].status.conditions[?(@.type==”Ready”)].lastTransitionTime}’
    -   List all pods name, lastProbeTime from status where the type is ready.
        -   $ kubectl get nodes -o jsonpath=”{range .items[*]}{.metadata.name} {.spec.taints[*].effect}{\”\n\”}{end}” | grep -v NoSchedule
        -   $ kubectl get node -o custom-columns=NAME:.metadata.name,TAINT:.spec.taints[*].effect | grep -v NoSchedule
    -   Get all persistence volume from kube-system namespace ordered with capacity
        -   $ kubectl get pv -n kube-system — sort-by=.spec.capacity.storage
    -   Tips: 
        -   To display every node in an individual row, we have to use a loop to process node by node. Use {range .items[*]} …… {end} to loop through the list of items.
        -   Within the loop, directly refer the items from the looping node. For example, use {.metadata.name} under {range .items[*]} for referring name. Should not use {.items[*].metadata.name}.
        -   At the end of every iteration use {\”\n\”} for new line.
8. Troubleshooting
    -   Worker node in 'Not Ready' state
        -   kubelet might have been stopped and disabled
    -   Controlplane
        -   CNI plugin not installed
        -   Core DNS pods in error so name resolution does not happen
        -   Replication does not work as Controller Manager pods not in running state
        -   Pods in 'Pending' state due to kube-scheduler pods not in running state
    -   Pods
        -   kubectl describe and kubectl logs can help 
            -   Incorrect Commands
            -   Incorrect Image
            -   Incorrect namespace
    -   Service
        -   Incorrect configuration including pod selector
        -   Incorrect namespace
1.  Create a busybox container to do name resolution on a service and check connectivity to that service on the related port
    -   $ kubectl run name-resolver --image=busybox:1.28 
    -   $ kubectl exec name-resolver -- nslookup \<service name\>
    -   $ kubectl exec name-resolver -- nc -vz \<service name\> \<port\>
1.  Use certificate and key for the user to create CSR, approve the certificate. Create the required role and rolebinding
    -   $ cat abc.csr | base64| tr -d "\n" => requests key in CertificateSigningRequest
    -   $ kubectl certificate approve \<csr-name\>
    -   $ kubectl create role rolename --resource=pods --verb=create,list,update,delete --namespace=my-ns
    -   $ kubectl create rolebinding rolebindingname --role=rolename --user=abc --namespace=my-ns
1. Create a new service account with the name pvaccess. Grant this Service account access to list all PersistentVolumes in the cluster by creating an appropriate cluster role called pvaccess-role and ClusterRoleBinding called pvaccess-role-binding.
Next, create a pod called pvaccess with the image: redis and serviceAccount: pvaccess in the default namespace
    -   $ kubectl create sa pvaccess
    -   $ kubectl create clusterrole pvaccess-role --resource=persistentvolumes --verb=list
    -   $ kubectl create clusterrolebinding pvaccess-role-binding --clusterrole=pvaccess-role --serviceaccount=default:pvaccess
    -   $ create a pod yaml and add serviceaccount in yml and create the pod
    -   whenever a pod is created, secret will be created with sa account. Inspec the pod to confirm
## References
- WeMake - YouTube Channel
- Sagar Reddy - YouTube Channel
- https://medium.com/@imarunrk/certified-kubernetes-administrator-cka-tips-and-tricks-part-5-869d947412c0
- Linux Academy
- Mumshad (KodeKloud)

