## Overview

考虑到 GitLab Runner 的资源消耗和安全问题，不建议和 Gitlab 安装在同一台机器上。

Runner 和 GitLab 是通过 API 进行通信， 只要 Runnes 能访问网络就能与 GitLab 通信。

所以一个 Gitlab 可以同时安装多个 Gitlab Runner，用来解决资源消耗问题。

## Install

### Run GitLab Runner in a container

```bash
docker run -d --name docker-gitlab-runner --restart always \
  -v /mnt/docker/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /usr/bin/docker:/usr/bin/docker \
  gitlab/gitlab-runner:latest
```

### Register Runner（Docker）

| arguments            | desc                                                         |
| -------------------- | ------------------------------------------------------------ |
| --url                | Gitlab host 地址                                             |
| --registration-token | Runner 的注册 token<br />token 的来源决定了该 Runner 用于指定运行某一个项目，还是共享运行所有未分配的项目 |
| --executor           | 选择执行器，这里采用 docker                                  |
1. 进入gitlab runner容器

   ```bash
   docker exec -it docker-gitlab-runner /bin/bash
   ```

2. 进入容器后，在容器内执行注册命令

   ```bash
   # 1. 普通模式
   
   gitlab-runner register -n \
      --url http://xx.xx.xx.xx/ \
      --registration-token PROJECT_REGISTRATION_TOKEN \
      --executor docker \
      --description "Runner description" \
      --docker-image "docker:latest" \
      --docker-volumes /var/run/docker.sock:/var/run/docker.sock
   ```
   
   ```bash
   # 2. 自签名证书模式

   # 如果服务器地址是：https://my.gitlab.server.com
   # 对应创建证书文件需命名为：/etc/gitlab-runner/certs/my.gitlab.server.com.crt
   
   
   gitlab-runner register -n \
      --url https://my.gitlab.server.com/ \
      --registration-token REGISTRATION_TOKEN \
      --executor docker \
      --description "Runner Certification" \
      --docker-image "docker:latest" \
      --docker-memory 4096M \
      --docker-cpus 4 \
      --tls-ca-file /etc/gitlab-runner/certs/my.gitlab.server.com.crt \
      --docker-volumes /var/run/docker.sock:/var/run/docker.sock
   ```
    
3. 修改 config.toml 配置，将旧的 runner 配置为自签名证书模式

    ```toml
    [[runners]]
      name = "Runner description"
      url = "https://my.gitlab.server.com/"
      tls-ca-file = "/etc/gitlab-runner/certs/my.gitlab.server.com.crt"
    ```

## References

- [Run GitLab Runner in a container](https://gitlab.com/gitlab-org/gitlab-runner/blob/master/docs/install/docker.md)
- [The self-signed certificates or custom Certification Authorities](https://docs.gitlab.com/runner/configuration/tls-self-signed.html)
