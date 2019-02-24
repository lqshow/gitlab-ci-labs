Skeleton project for Swagger

## Overview
为了确保我们所有的 CI 任务在本地都是可复制的，我们将使用 Makefile 代替所有的命令。

## Testing application with GNU make

1. 构建镜像

   ```bash
   make image-build -e IMAGE_NAME=lqshow/gilab-ci-nodejs
   ```

2. 运行容器

   ```bash
   make deploy-to-staging \
   	-e IMAGE_NAME=lqshow/gilab-ci-nodejs \
   	-e CONTAINER_NAME=gilab-ci-nodejs
   ```

3. 访问应用

   ```bash
   curl http://localhost:10010/hello
   ```