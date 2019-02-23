Skeleton project for Swagger

## 使用 GitLab CI 构建项目

### 本地测试应用流程
1. 构建镜像

   ```bash
   docker build -t lqshow/gilab-ci-nodejs:0.01 .
   ```

2. 运行容器

   ```bash
   docker run -d --name gilab-ci-nodejs \
   	--restart always \
   	--env NODE_ENV=test \
   	-p 10010:10010 \
   	lqshow/gilab-ci-nodejs:0.01
   ```

3. 访问应用

   ```bash
   curl http://localhost:10010/hello
   ```

