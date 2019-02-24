Skeleton project for Swagger

## Overview
使用 GitLab CI 将应用部署到 Kubernetes 中，搭建一个持续集成开发环境。
这里用 nodejs 作为项目样例。

## Deploy app using GitLab CI with Kubernetes

### 本地测试应用流程

> 本地访问 k8s 集群的配置参考: [lab23-creating-kubeconfig-for-cluster](https://github.com/lqshow/k8s-labs/tree/master/lab23-creating-kubeconfig-for-cluster)

1. 构建和发布镜像

   ```bash
   make release -e IMAGE_NAME=gilab-ci-k8s-nodejs
   ```

2. 部署应用到 k8s cluster

   ```bash
   make deploy-to-staging -e IMAGE_NAME=gilab-ci-k8s-nodejs
   ```

3. 访问应用

   ```bash
   kubectl port-forward svc/gilab-ci-k8s-nodejs 10010
   curl http://localhost:10010/hello
   ```

### Gitlab CI 和 Kubernetes 集成流程

创建 Dockerfile 文件用于和 k8s cluster 通讯的 base ci  镜像

#### 需要准备的事项

1. KUBE_APISERVER
2. CA_CRT_FILE
3. USER_TOKEN

```dockerfile
FROM lqshow/docker-kubectl

RUN mkdir -p /data/kubeconfig
COPY resource/${CA_CRT_FILE} /data/kubeconfig/dev-ci-ca.crt

# Configure Access to Clusters
RUN kubectl config set-cluster cluster-staging \
	--certificate-authority=/data/kubeconfig/dev-ci-ca.crt \
	--embed-certs=true \
	--server=${KUBE_APISERVER}

# Sets a user entry in kubeconfig
RUN kubectl config set-credentials dev-user \
    --token=${USER_TOKEN}

# Sets a context entry in kubeconfig
RUN kubectl config set-context dev-ci-staging \
    --cluster=cluster-staging \
    --user=dev-user \
    --namespace=dev

# Sets the current-context in a kubeconfig file
RUN kubectl config use-context dev-ci-staging
```

创建镜像，后续 gitlab-ci 中部署使用该镜像就行

```bash
docker build -t ci-kubectl .
```

## References
- [GitLab CI/CD && Kubernetes](https://medium.com/nosebit/gitlab-ci-cd-kubernetes-65eec29d0555)
-  [lab23-creating-kubeconfig-for-cluster](https://github.com/lqshow/k8s-labs/tree/master/lab23-creating-kubeconfig-for-cluster)
- [k8s 利用 Service Account + RBAC 访问资源](https://github.com/lqshow/notes/issues/45)
