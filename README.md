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

