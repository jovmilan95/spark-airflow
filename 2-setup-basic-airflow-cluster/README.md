
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


spark-submit --master spark://basic-spark-master-0.basic-spark-headless.basic-spark.svc.cluster.local:7077 --py-files dags/repo/dags/pi.py --name arrow-spark --deploy-mode cluster examples/src/main/python/pi.py 10