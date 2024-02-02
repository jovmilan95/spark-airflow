### Prerequisites

```
    Install kubectl
    Install helm
```



### Installing the Nginx Ingress Chart
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update ingress-nginx
helm upgrade --install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx --create-namespace

```


### Setup etc hosts for ingress (Windows)
Add ip and hostname in `C:\Windows\System32\drivers\etc\hosts` file
```
    <set-your-ip> spark.local airflow.local
```
