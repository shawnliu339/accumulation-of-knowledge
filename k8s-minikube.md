1. Install minikube
```
brew install minikube
```

2. Raise resource
```
minikube config set memory 2000
minikube config set cpus 4
minikube config view
```

3. Set docker resource by desktop docker


4. Build cluster
```
minikube start --nodes 3 -p test --extra-config scheduler.logging-format=json --extra-config kubelet.logging-format=json --extra-config apiserver.logging-format=json --extra-config controller-manager.logging-format=json --addons metrics-server
```

5. Application deploy
```
kubectl config use-context test
kubectl apply -f file
```

6. Explore ip to external
Explore all services
```
minikube tunnel -p test
```
Explore the specified service
```
minikube service <name> -n <namespace> --url -p test
```

7. Start dashboard
```
minikube dashboard -p test
```

8. Stop minikube
```
minikube profile list
minikube stop -p test
minikube stop --all
```
