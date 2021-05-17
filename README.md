# ghost-helm
Ghost blog running as a Kubernetes application managed by Helm chart.

# Getting started
## Clone
Clone this repo to your local machine
```
git clone git@github.com:wwrzesien/ghost-helm.git
```

## Setup
Open VM VirtualBox and start minikube cluster
```
minikube start
```

Make sure ingress addon and metrics are enabled
```
minikube addons list
```

If not:
```
minikube addons enable ingress
minikube addons enable metrics-server
```


Check minikube IP
```
minikube ip
```

Pass minikube IP and host domain, defined in values file, into host file
```
echo '192.168.99.101 ghost.info' | sudo tee -a /etc/hosts
```
## Deployment
Having defined all parameters in `values.yaml` file, start application
```
helm install ghost-blog ghost-blog
```

## Simple test autoscale
```
kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://ghost.info; done"
kubectl get hpa -w
kubectl get deployments -w
```

