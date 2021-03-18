## Import kubectl commands

- To switch from one namespace to another namespace </br> $ kubectl config set-context $(kubectl config current context) --namespace=dev
- To create pod using pod definition in other namespace </br> $ kubectl create -f pod-definition.yml -n mynamespace
- To create/replace k8s objects use --force
