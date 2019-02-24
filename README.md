## Overview

GitLab CI 是 GitLab 提供的持续集成服务。

需在仓库根目录创建一个.gitlab-ci.yml 文件， 并为该项目指派一个 GitLab Runner。

每次 commit 或者 merge request 代码到 git 仓库， GitLab Runner 就会自动运行 pipeline，开始自动化集成。

## Why use gitlab-ci

1. 安装方便，只需配置 GitLab Runner 即可。
2. 不需要配置 webhook 回调地址，只需在项目中配置 .gitlab-ci.yml 文件（**开发需要做的**）。
3. 整个构建流程可以直接在 gitlab 中清晰可见。

## Step by step  play with GitLab CI

* [00-concept](./00-concept)：介绍 GitLab CI 的基本概念。
* [01-deploy](./01-deploy)：如何通过 docker compose 快速部署一套 GitLab 以及 GitLab Runner 本地环境？
* [02-runner](./02-runner)：介绍 GitLab Runner 安装流程。
* [03-troubleshooting](./03-troubleshooting)：使用 GitLab CI 中碰到的一些常见问题以及解决方案。
* [04-basic-usage](./04-basic-usage)：通过一个简单的 Node.js Express + GitLab 的例子，构建一个可持续集成开发环境。
* [05-use-makefile-2-wrap-scripts](./05-use-makefile-2-wrap-scripts)：如何通过 GitLab CI 搭配  Makefile 部署项目？
* [06-docker-compose](./06-docker-compose)：如何通过 GitLab CI 搭配  Docker Compose 部署项目？
* [07-deploy-with-kubernetes](./07-deploy-with-kubernetes)：如何通过 GitLab CI 搭配  Kubernetes 部署项目？
* [08-deploy-with-helm](./08-deploy-with-helm)：如何通过 GitLab CI 搭配 Hlem 部署项目？

## References

- [本地基于 Docker + Gitlab + Gitlab CI 搭建持续集成环境](https://github.com/lqshow/notes/issues/29)
- [GitLab Runner](https://docs.gitlab.com/runner/)
- [Configuration of your pipelines with .gitlab-ci.yml](https://docs.gitlab.com/ce/ci/yaml/README.html)


