# Kubernetes Learning

# Environment preparation
+ install minikube
+ install kubectl 

https://kubernetes.io/docs/home/
https://github.com/kubernetes/minikube
https://yq.aliyun.com/articles/221687


# Start the environment
## start minikube
+ start minikube

        minikube start --vm-driver=xhyve --registry-mirror=https://registry.docker-cn.com  

+ start minikube with specified proxy info

        minikube start --vm-driver=xhyve --docker-env HTTP_PROXY=socks5://127.0.0.1:1086  --docker-env HTTPS_PROXY=socks5://127.0.0.1:1086 

## start the dashboard
    minikube dashboard

## start a service
+ Start a app

        kubectl run nginx-test --image=nginx --port=80

+ start a service

        kubectl expose deployment nginx-test --type="NodePort" --port 8080
+ visit the service

        curl $(minikube service nginx-test --url)

## start a ingress for the service
+ start ingress for minikube as addons

        minikube addons enable ingress

+ define a ingress

create a `nginx-test-ingress.yml` file with following content.
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: nginx-test-ingress
spec:
  backend:
    serviceName: nginx-test
    servicePort: 80
  rules:
  - host: nginx-test.com
    http:
      paths:
      - path: /
        backend:
          serviceName: nginx-test
          servicePort: 80
```

+ create a ingress

        kubectl create -f `nginx-test-ingress.yml`

+ check the ingress config

        kubectl describe ing nginx-test-ingress

+ append the domain into `/etc/hosts`
+ visit the pod through domain

        curl nginx-test.com:80