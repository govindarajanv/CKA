## Practice Questions

* Perform ETCD backup and restore
* Add an init container which should create a file. If that file exists, container should run
    -  Answer: https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables
*  Create secret from literals (username=admin, password=dummy)
    - Answer: $ kubectl create secret generic --from-literal username=admin --from-literal password=dummy
*  Use the above secret as an env variable in pod
    - Answer: as env variable (https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables)
* Create a pod nginx with below configurations <br> Limits: cpu 200m, Memory 300Mi, requests: cpu 100m, memory 200Mi and label: team=core-dev in dev namespace
    - Answer: </br> $ kubectl create ns dev </br> $ kubectl run nginx -o yaml --dry-run=client --image=nginx --requests "cpu=100m,memory=200Mi" --limits "cpu=200m,memory=300Mi" -l team=core-dev
## References
- WeMake - YouTube Channel
