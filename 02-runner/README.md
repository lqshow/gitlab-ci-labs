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
   gitlab-runner register -n \
      --url http://xx.xx.xx.xx/ \
      --registration-token PROJECT_REGISTRATION_TOKEN \
      --executor docker \
      --description "Runner description" \
      --docker-image "docker:latest" \
      --docker-volumes /var/run/docker.sock:/var/run/docker.sock
   ```

## References

- [本地基于Docker+Gitlab+Gilab CI搭建持续集成环境](https://github.com/lqshow/notes/issues/29)
- [Run GitLab Runner in a container](https://gitlab.com/gitlab-org/gitlab-runner/blob/master/docs/install/docker.md)

