## Application Troubleshooting

- get all objects under a given namespace to get a glance on k8s objects deployed </br> $ kubectl get all -n alpha
- check if there are any restarts
- check if service name and ports are correct by describing the services
- check if selectors used by svc are matching with pods selectors
- check if env variable in deployments and pods are correct
- check logs of pods for any error

## Possible errors

- service name could be wrong
- service port could be wrong
- service might not have endpoints (like when no matching pods available)
- service might have wrong pod selectors
- connection string or env variables could be wrong in deployments or pods
- pods might be down due to errors
