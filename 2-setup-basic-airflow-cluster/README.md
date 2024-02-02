
### Create PV for DAGs
```
kubectl apply -f ./2-setup-basic-airflow-cluster/dags-persistance-pv.yaml
```


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
        --set ingress.enabled=true \
        --set ingress.web.enabled=true \
        --set ingress.web.host=airflow.local \
        --set ingress.web.ingressClassName=nginx \
        --set executor=KubernetesExecutor \
        --set dags.gitSync.enabled=true \
        --set dags.gitSync.repo=https://github.com/jovmilan95/spark-airflow.git \
        --set dags.gitSync.branch=main \
        --set dags.gitSync.subPath=dags/ \
        --set dags.gitSync.credentialsSecret=git-credentials \
        --set dags.persistence.enabled=true \
        --set dags.persistence.enabled=true \
        --set dags.persistence.size=1Gi \
        --set dags.persistence.storageClassName=dags \
        --wait \
        --timeout=20m \
        --create-namespace --namespace basic-airflow
```
