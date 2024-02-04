
### Create git sync credential secret
```
kubectl create ns  basic-airflow
kubectl apply -f ./2-setup-basic-airflow-cluster/git-credentials-secret.yaml -n  basic-airflow
```


### Installing the Airflow Chart
```
    helm repo add apache-airflow https://airflow.apache.org
    helm repo update apache-airflow
    helm upgrade --install basic-airflow  apache-airflow/airflow \
        --set ingress.web.enabled=true \
        --set ingress.web.host=airflow.local \
        --set ingress.web.ingressClassName=nginx \
        --set images.airflow.repository=apache/airflow \
        --set images.airflow.tag=spark \
        --set executor=KubernetesExecutor \
        --set dags.gitSync.enabled=true \
        --set dags.gitSync.repo='https://github.com/jovmilan95/spark-airflow.git' \
        --set dags.gitSync.branch=master \
        --set dags.gitSync.subPath="dags/" \
        --set dags.gitSync.credentialsSecret=git-credentials \
        --wait \
        --timeout=20m \
        --create-namespace --namespace basic-airflow
```
### Build spark container 
```
    docker build -t milan/spark:test -f Dockerfile.Spark .
```
### Create spark service account with cluster-admin privlages
### Create worker.yaml
### Copy config content to /opt/bitnami/spark/config
### And run command
```
KUBECONFIG=/opt/bitnami/spark/config spark-submit \
    --conf spark.kubernetes.container.image=milan/spark:test \
    --master k8s://https://kubernetes.default.svc:443 \
    --conf spark.kubernetes.driverEnv.SPARK_MASTER_URL=spark://basic-spark-master-svc:7077 \
    --conf spark.kubernetes.context=docker-desktop \
    --deploy-mode cluster \
    --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
    --conf spark.kubernetes.namespace=basic-spark \
  --conf spark.executor.instances=2 \
    local:///opt/bitnami/spark/examples/src/main/python/pi.py 10

```

KUBECONFIG=/opt/bitnami/spark/config spark-submit \
    --conf spark.kubernetes.container.image=milan/spark:test \
    --master k8s://https://kubernetes.default.svc:443 \
    --conf spark.kubernetes.driverEnv.SPARK_MASTER_URL=spark://basic-spark-master-svc:7077 \
    --conf spark.kubernetes.context=docker-desktop \
    --deploy-mode cluster \
    --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
    --conf spark.kubernetes.authenticate.submission.caCertFile=
    --conf spark.kubernetes.namespace=basic-spark \
  --conf spark.executor.instances=2 \
    local:///opt/bitnami/spark/examples/src/main/python/pi.py 10