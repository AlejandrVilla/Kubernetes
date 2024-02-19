# HPA, Kustomize and Ingress with Kubernetes

## Setup de environment
1. Clone the repo
```
git clone https://github.com/AlejandrVilla/advanced-kubernetes.git
```
2.  Install Ingress Nginx Controller in your cluster
[Ingress Nginx Controller](https://kubernetes.github.io/ingress-nginx/deploy/ "Ingress Nginx Controller")

3. Check Ingress Nginx pods
```
kubectl get pods -n ingress-nginx
```

## Run deployments, services and use Ingress Nginx

1.  Run the deployments
```
kubectl apply -f deployment-v1.yaml
kubectl apply -f deployment-v2.yaml
```

2. Run the services
```
kubectl apply -f service-v1.yaml
kubectl apply -f service-v2.yaml
```

3. Run ingress-nginx.yaml
	- Ingress Nginx will provide a public IP Adress for the services
```
kubectl apply -f ingress-nginx.yaml
```

4. Check the services using Ingress
```
curl http://[IP-Address]/v1
curl http://[IP-Address]/v2
```

## Use Kustomize to scale pods in application 1
1. Check the changes
```
kubectl kustomize .
kubectl get deployment hello-v1 -o yaml
```
2. Run kustomize file
	 - The following command will scale hello-v1 app from 1 to 3 pods and change the requests and limits resources
```
kubectl apply -k .
```

## Use HPA (Horizontal Pod Autoscaling)
1. Run the deployment, service and pha php application
```
kubectl apply -f php-apache-deployment.yaml
kubectl apply -f php-apache-service-node-port.yaml
kubectl apply -f php-apache-hpa.yaml
```
2. Check the service using the public IP address
```
curl http://[IP address]:80
```
3. Change the public IP address in the stress.sh script
```bash
#! /bin/bash
while true;
do
 wget -q -O- http://[IP address]:80; 
done
```
4. After a minutes check how the cluster scales the pods and resources used
```
kubectl get all
```
or
```
kubectl get hpa
```