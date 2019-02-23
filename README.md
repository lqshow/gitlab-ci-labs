## Overview

GitLab CI 是 GitLab 提供的持续集成服务。

需在仓库根目录创建一个.gitlab-ci.yml 文件， 并为该项目指派一个 GitLab Runner。

每次 commit 或者 merge request 代码到 git 仓库， GitLab Runner 就会自动运行 pipeline，开始自动化集成。

## Why use gitlab-ci

1. 安装方便，只需配置 GitLab Runner 即可。
2. 不需要配置 webhook 回调地址，只需在项目中配置 .gitlab-ci.yml 文件（**开发需要做的**）。
3. 整个构建流程可以直接在 gitlab 中清晰可见。

## References

- [本地基于 Docker + Gitlab + Gitlab CI 搭建持续集成环境](https://github.com/lqshow/notes/issues/29)
- [GitLab Runner](https://docs.gitlab.com/runner/)
- [Configuration of your pipelines with .gitlab-ci.yml](https://docs.gitlab.com/ce/ci/yaml/README.html)


