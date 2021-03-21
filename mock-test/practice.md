## Practice Questions

* Perform ETCD backup and restore
* Add an init container which should create a file. If that file exists, container should run
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
* Create a pod with image redis and expose the pod as a service. Also expose the service for the deployment in the previous question with type nodeport on port 30080
* 
## References
- WeMake - YouTube Channel
