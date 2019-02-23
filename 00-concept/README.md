## Concept

### Pipeline(PipeLine as code)

一次 Pipeline 相当于一次构建任务，里面可以包含多个流程，如安装依赖、运行测试、编译、部署测试服务器、部署生产服务器等流程。

### Stages

表示构建阶段，即流程。我们可以在一次 Pipeline 中定义多个 Stages。

1. 所有 Stages 会按照顺序运行，即当一个 Stage 完成后，下一个 Stage 才会开始。
2. 只有当所有 Stages 完成后，该 Pipeline  才会成功。
3. 如果任何一个 Stage 失败，那么后面的 Stages 不会执行，该 Pipeline 失败。

### Jobs

表示构建工作，表示某个 Stage 里面执行的工作。我们可以在 Stage 里面定义多个 Jobs

1. 相同 Stage 中的 Jobs 会并行执行。
2. 相同 Stage 中的 Jobs 都执行成功时，该 Stage 才会成功。
3. 如果任何一个 Job 失败，那么该 Stage 失败，即该 Pipeline 失败。