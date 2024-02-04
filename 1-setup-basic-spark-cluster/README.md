### Spark helm chart 
```
https://artifacthub.io/packages/helm/bitnami/spark
```

### Installing the Chart
```
    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm repo update bitnami
    helm upgrade --install basic-spark bitnami/spark \
        --set ingress.enabled=true \
        --set ingress.hostname=spark.local \
        --set ingress.ingressClassName=nginx \
        --set master.configOptions="-Dspark.ui.reverseProxy=true,-Dspark.ui.reverseProxyUrl=https://spark.local" \
        --set worker.configOptions="-Dspark.ui.reverseProxy=true,-Dspark.ui.reverseProxyUrl=https://spark.local" \
        --wait \
        --timeout=20m \
        --create-namespace --namespace basic-spark
```
