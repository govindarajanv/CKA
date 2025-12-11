## Import kubectl commands

- To switch from one namespace to another namespace </br> $ kubectl config set-context $(kubectl config current context) --namespace=dev
- To create pod using pod definition in other namespace </br> $ kubectl create -f pod-definition.yml -n mynamespace
- To create/replace k8s objects use --force
- Update image in a deployment </br> $ kubectl set image deployment nginx nginx:1.18
- To create pod and expose service </br> $ kubectl run httpd -o yaml --dry-run=client --image=httpd:alpine --port=80 --expose 
- To search for 5 lines before and after matching expression </br> $ kubectl explain po --recursive |grep -i -A 5 -B5 affinity
- view the events to get which scheduler picked it up </br> $ kubectl get events
- To view logs </br> $ kubectl logs -f event-simulator <container name>
- deployments $ kubectl rollout statis deployment/deployment-name </br> $ kubectl rollout history deployment/deployment-name </br> $ kubectl rollout undo deployment/myapp-deploymentname
- To create Config Map </br> $ kubectl create configmap app-config --from-literal=APP_COLOR=blue </br> $ kubectl create configmap app-config --from-file=app-config.properities
- To check certificate details </br> $ openssl x509 -in file-path.crt -text -noout
