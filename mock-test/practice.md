## Practice Questions

* Perform ETCD backup and restore
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
            -   create a static pod manifest in master node. Check kubelet config file and 'staticPodPath' configuration path (usually /etc/kubernetes/manifests)
            -   create a static pod on worker node. Create pod manifest in master node. Do scp to worker node. check the configuration path for staticPodPath, update if required. Restart kubelet service (enable). $ kubectl get pods -w on master, you should be able to see static pods.
        -   
## References
- WeMake - YouTube Channel
