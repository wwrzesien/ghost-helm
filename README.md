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

## Monitoring
Install prometheus-operator 
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo update
```
In separate namespace install prometheus chart
```
kubectl create namespace prometheus
helm install prometheus prometheus-community/kube-prometheus-stack
```

Open prometheus pod to external world
```
kubectl port-forward deployment/prometheus-grafana 3000
```
Log into grafana
```
https://localhost:3000
```
Import dashboard
```
https://grafana.com/grafana/dashboards/6417
```

## Simple test autoscale
```
kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://ghost.info; done"
```
In a new terminal monitor application status
```
kubectl get hpa -w
kubectl get deployments -w
```

