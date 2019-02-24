Skeleton project for Swagger

## Overview
使用 GitLab CI 结合 Helm 将应用部署到 Kubernetes 中，搭建一个持续集成开发环境。
这里用 nodejs 作为项目样例。

## Deploy app using GitLab CI with Helm

### 本地测试应用流程

1. 构建和发布镜像

   ```bash
   make release -e IMAGE_NAME=gilab-ci-k8s-nodejs
   ```

2. 部署应用到 k8s cluster

   ```bash
   make run -e IMAGE_NAME=gilab-ci-k8s-nodejs
   ```

3. 访问应用

   ```bash
   kubectl port-forward svc/gilab-ci-k8s-nodejs 10010
   curl http://localhost:10010/hello
   ```
