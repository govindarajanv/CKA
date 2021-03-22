## Practice Questions

* Perform ETCD backup and restore
    -   ETCDCTL_API=3 etcdctl — endpoints=[ENDPOINT] — cacert=[CA CERT] — cert=[ETCD SERVER CERT] — key=[ETCD SERVER KEY] snapshot save [BACKUP FILE NAME]
* Setup 2 nodes k8s clusters
* Upgrade node
* We should use Init Container to create a file named “sharedfile.txt” under the “work” directory and the application container should check if the file exists and sleep for a while. If the file does not exist the application container should exit.
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
*  Create secret from literals (username=admin, password=dummy)
    - Answer: $ kubectl create secret generic --from-literal username=admin --from-literal password=dummy
*  Use the above secret as an env variable in pod
    - Answer: as env variable (https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables)
*   Create a new secret from file and map the created secret to a volume and map it to the pod
    - Answer: https://kubernetes.io/docs/concepts/configuration/secret/
* Create a pod nginx with below configurations <br> Limits: cpu 200m, Memory 300Mi, requests: cpu 100m, memory 200Mi and label: team=core-dev in dev namespace
    - Answer: </br> $ kubectl create ns dev </br> $ kubectl run nginx -o yaml --dry-run=client --image=nginx --requests "cpu=100m,memory=200Mi" --limits "cpu=200m,memory=300Mi" -l team=core-dev --namespace=dev
* Create a pod with multi-containers in it  nginx, tomcat, redis 
    - Answer: 
        - $ kubectl run nginx -o yaml --dry-run=client --image=nginx > multipod.yml
        - Append additional entries of second and third containers and do kubectl apply
* Create a deployment webapp with image nginx and replicas 6 and place the file in /tmp directory
    - Scale down the deployments to 4
    - Update the image of the above deployment to nginx:1.18.0 and try to record the deployments
    - Check rollout history
    - Rollback to original nginx image in the deployment
* Create a pod with image redis and expose the pod as a service. 
    - Also expose the service for the deployment in the previous question with type nodeport on port 30080
    - Create a pod with image redis with port and target port as 6379
    - Get the list of pods related to the service redis
* Check DNS record of redis pod and store in a file /tmp/dns_redis_pods.txt
    -   $ kubectl get pod redis -n dev -o wide  # Get ip address 10.1.15.5
    -   $ kubectl run busybox --image=busybox --restart=Never --rm -it -- nslookup 10-1-15-5.default.pod
* Static Pod creation by kubelet
    -   Create a static pod on worker node
        -   Answer: 
            -   Sometimes there will be a few hints about static pod if there is no explicit mention like a pod on worker node by placing in a path like /etc/kubernetes/manifests
            -   create a static pod manifest in master node. Check kubelet config file and 'staticPodPath' configuration path (usually /etc/kubernetes/manifests)
            -   create a static pod on worker node. Create pod manifest in master node. Do scp to worker node. check the configuration path for staticPodPath, update if required. Restart kubelet service (enable). $ kubectl get pods -w on master, you should be able to see static pods.
* Daemonsets with a twist due to Taint and Tolerations. Create a pod with image redis with “temporary” directory that shares the pod’s lifetime on a given path /share/redis.
    -   Answer: Temporary here means empty directory. Create a hostpath persistent volume, create PVC, create pod with that PVC mounted on to it.
* Create a network policy that denies access to the payroll pod in the accounting namespace
    -   Answer: Refer documentation, add egress and Ingress with pod selector
* Create a namespace called finance, create a network policy, that blocks all traffic to pods in finance namespace except for traffic from pods in the same namespace on port 8080
    -   Answer: label namespace (as we would be using namespace selector. To select all pods in the namespace, pod selector should have empty braces {}. Use namespace selector in ingress or egress (ipblock, pod selector and namespace selector are the options available)
* JSONPATH Exercises
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
## References
- WeMake - YouTube Channel
- Sagar Reddy - YouTube Channel
- https://medium.com/@imarunrk/certified-kubernetes-administrator-cka-tips-and-tricks-part-5-869d947412c0
