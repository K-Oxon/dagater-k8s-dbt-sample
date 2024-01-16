# dagater-k8s-dbt-sample

## 公式

https://docs.dagster.io/deployment/guides/kubernetes/deploying-with-helm

## Web serverデプロイ

```bash
kubectl config use-context dagster
helm repo add dagster https://dagster-io.github.io/helm
helm install dagster-test dagster/dagster \
    --namespace dagster --create-namespace

# 公式の出力ではlabelがおかしいので要修正 (component=dagster-webserverが正しい)
export DAGSTER_WEBSERVER_POD_NAME=$(kubectl get pods --namespace dagster -l "app.kubernetes.io/name=dagster,app.kubernetes.io/instance=dagster-test,component=dagster-webserver" -o jsonpath="{.items[0].metadata.name}")

kubectl --namespace dagster port-forward $DAGSTER_WEBSERVER_POD_NAME 8080:80

```

## User Code デプロイ

https://dev.classmethod.jp/articles/dagster-with-kubernetes/

```bash
cd user_code
docker build -t oxon/dagster-k8s-dbt-sample:[tag] .

# 挙動確認
docker run -it --rm  oxon/dagster-k8s-dbt-sample:[tag] /bin/bash

# try bellow in running container
# dbt debug --profiles-dir .
# dagster api grpc -h 0.0.0.0 -p 3030 -f src/repo.py
```

```bash
helm upgrade --install dagster-test dagster/dagster -f values.yaml \
    --namespace dagster --create-namespace
```
