## Practice Questions

* Perform ETCD backup and restore
* Add an init container which should create a file. If that file exists, container should run
    -  Answer: https://github.com/hub-kubernetes/cka-exam/blob/master/Module%20-%201/pods/initcontainers/initcontainer.yaml
*  Create secret from literals (username=admin, password=dummy)
    - Answer: $ kubectl create secret generic --from-literal username=admin --from-literal password=dummy
*  Use the above secret as an env variable in pod
    - Answer: as env variable (https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables)

## References
- WeMake - YouTube Channel
