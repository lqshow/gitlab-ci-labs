
## 怎么才能同时并发执行多个 Job ？

默认情况下只允许一个 Job 在运行，其他 Job 都处于 pending 状态。

直到当前 Job 执行完成后，后续  Job 才能一个个开始执行。

#### 解决方案

通过修改 Gitlab Runner 的全局配置参数。

| setting    | desc                                       |
| ---------- | ------------------------------------------ |
| concurrent | 默认值为1，自行调整成需要的并发数，比如 30 |

##### 例子
```toml
concurrent = 30
```

##### 文件位置

由于之前 Gitlab Runner 是通过 docker 安装的，并将配置 mount 到了宿主主机上，我们可以直接通过修改主机上文件完成。

| source    | file path                                    |
| --------- | -------------------------------------------- |
| host      | /mnt/docker/gitlab-runner/config/config.toml |
| container | /etc/gitlab-runner/config.toml               |

## 怎么才能在 GitLab CI/CD 中使用 ssh 私钥？

通过配置 .gitlab-ci.yml 将 SSH 密钥注入到构建环境中，这是一种可与任何类型的执行程序（Docker，shell等）一起使用的解决方案。

#### 解决方案
参考: [GitLab CI/CD中使用SSH私钥](https://github.com/lqshow/notes/issues/30)

## 怎么设置动态环境变量到构建环境中？

```yaml
before_script:
- eval export VERSION=$(git describe --tags --always --dirty)
```

## References

- [Advanced configuration](https://docs.gitlab.com/runner/configuration/advanced-configuration.html)