---
title: Explore Builds
description: 了解如何在 Docker Desktop 中使用 Builds 视图
keywords: Docker Dashboard, manage, gui, dashboard, builders, builds
---

![Docker Desktop 中的 Builds 视图](../images/builds-view.webp)

**Builds** 视图是一个简单的界面，让你可以查看构建历史记录并使用 Docker Desktop 管理 builders。

打开 Docker Desktop 中的 **Builds** 视图会显示已完成构建的列表。默认情况下，列表按日期排序，最近的构建显示在顶部。你可以切换到 **Active builds** 以查看任何正在进行的构建。

![Build UI 截图活动构建](../images/build-ui-active-builds.webp)

如果你通过 [Docker Build Cloud](../../build/cloud/_index.md) 连接到云 builder，Builds 视图还会列出其他团队成员连接到相同云 builder 的所有活动或已完成的云构建。

## 显示构建列表

在 Docker Dashboard 中选择 **Builds** 视图以打开构建列表。

构建列表显示你已完成和正在进行的构建。**Build history** 选项卡显示已完成的历史构建，从这里你可以检查构建日志、依赖关系、跟踪等。**Active builds** 选项卡显示当前正在运行的构建。

列表显示你活动、正在运行的 builders 的构建。它不列出非活动 builders 的构建：你已从系统中移除的 builders 或已停止的 builders。

### Builder 设置

右上角显示当前选定 builder 的名称，**Builder settings** 按钮让你在 Docker Desktop 设置中[管理 builders](#manage-builders)。

### 导入构建

> **测试功能**
>
> 导入构建目前处于[测试](../../release-lifecycle.md#Beta)阶段。
{ .experimental }

**Import builds** 按钮让你可以导入其他人或 CI 环境中的构建记录。导入构建记录后，你可以直接在 Docker Desktop 中完全访问构建的日志、跟踪和其他数据。`docker/build-push-action` 和 `docker/bake-action` GitHub Actions 的[构建摘要](../../build/ci/github-actions/build-summary.md)包含下载构建记录的链接，用于在 Docker Desktop 中检查 CI 任务。

## 检查构建

要检查构建，请在列表中选择要查看的构建。检查视图包含多个选项卡。

**Info** 选项卡显示有关构建的详细信息。

如果你在检查多平台构建，右上角的下拉菜单可以让你将信息过滤到特定平台：

![平台过滤器](../images/build-ui-platform-menu.webp?w=400)

**Source details** 部分显示有关前端[frontend](../../build/dockerfile/frontend.md)的信息，如果有的话，还显示用于构建的源代码库。

### 构建时间

Info 选项卡的 **Build timing** 部分包含显示从不同角度进行构建执行的时间分解的图表。

- **Real time** 指的是完成构建所用的实际时间。
- **Accumulated time** 显示所有步骤的总 CPU 时间。
- **Cache usage** 显示构建操作缓存的程度。
- **Parallel execution** 显示构建执行时间中有多少是并行运行步骤所用。

![构建时间图表](../images/build-ui-timing-chart.webp)

图表颜色和图例键描述了不同的构建操作。构建操作定义如下：

| 构建操作            | 描述                                                                                                                                           |
| :------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------- |
| 本地文件传输         | 传输本地文件从客户端到 builder 所花费的时间。                                                                                                   |
| 文件操作             | 任何涉及在构建中创建和复制文件的操作。例如，Dockerfile 前端中的 `COPY`、`WORKDIR`、`ADD` 指令都涉及文件操作。                                |
| 镜像拉取             | 拉取镜像所花费的时间。                                                                                                                         |
| 执行                 | 容器执行，例如 Dockerfile 前端中定义为 `RUN` 指令的命令。                                                                                       |
| HTTP                 | 使用 `ADD` 进行的远程工件下载。                                                                                                                |
| Git                  | 与 **HTTP** 类似，但用于 Git URLs。                                                                                                            |
| 结果导出             | 导出构建结果所花费的时间。                                                                                                                     |
| SBOM                 | 生成[SBOM 证明](../../build/attestations/sbom.md)所花费的时间。                                                                                |
| 空闲                 | 构建工作器的空闲时间，如果你配置了[最大并行度限制](../../build/buildkit/configure.md#max-parallelism)，可能会发生空闲时间。                    |

### 构建依赖

**Dependencies** 部分显示构建期间使用的镜像和远程资源。此处列出的资源包括：

- 构建过程中使用的容器镜像
- 使用 `ADD` Dockerfile 指令包含的 Git 仓库
- 使用 `ADD` Dockerfile 指令包含的远程 HTTPS 资源

### 参数、秘密和其他参数

Info 选项卡的 **Configuration** 部分显示传递给构建的参数：

- 构建参数，包括解析的值
- 秘密，包括它们的 ID（但不包括它们的值）
- SSH sockets
- 标签
- [附加上下文](/reference/cli/docker/buildx/build/#build-context)

### 输出和工件

**Build results** 部分显示生成的构建工件的摘要，包括镜像清单详情、证明和构建跟踪。

证明是附加到容器镜像的元数据记录。元数据描述了关于镜像的某些内容，例如它是如何构建的或包含哪些软件包。有关证明的更多信息，请参见[构建证明](../../build/attestations/_index.md)。

构建跟踪捕获了 Buildx 和 BuildKit 中构建执行步骤的信息。跟踪有两种格式：OTLP 和 Jaeger。你可以通过打开操作菜单并选择要下载的格式，从 Docker Desktop 下载构建跟踪。

#### 使用 Jaeger 检查构建跟踪

使用 Jaeger 客户端，你可以从 Docker Desktop 导入并检查构建跟踪。以下步骤向你展示如何从 Docker Desktop 导出跟踪并在 [Jaeger](https://www.jaegertracing.io/) 中查看：

1. 启动 Jaeger UI：

   ```console
   $ docker run -d --name jaeger -p "16686:16686" jaegertracing/all-in-one
   ```

2. 打开 Docker Desktop 中的 Builds 视图，选择一个已完成的构建。

3. 导航到 **Build results** 部分，打开操作菜单并选择 **Download as Jaeger format**。

   <video controls>
     <source src="/assets/video/build-jaeger-export.mp4" type="video/mp4" />
   </video>

4. 在浏览器中访问 <http://localhost:16686> 以打开 Jaeger UI。

5. 选择 **Upload** 选项卡并打开刚才导出的 Jaeger 构建跟踪。

现在你可以使用 Jaeger UI 分析构建跟踪：

![Jaeger UI 截图](../images/build-ui-jaeger-screenshot.png "Jaeger UI 中的构建跟踪截图")

### Dockerfile 源代码和错误

在检查成功完成的构建或正在进行的活动构建时，**Source** 选项卡显示用于创建构建的[frontend](../../build/dockerfile/frontend.md)。

如果构建失败，**Error** 选项卡会替代 **Source** 选项卡显示。错误信息会内联显示在 Dockerfile 源代码中，指出失败发生的位置和原因。

![在 Dockerfile 中内联显示的构建错误](../images/build-ui-error.webp)

### 构建日志

**Logs** 选项卡显示构建日志。对于活动构建，日志会实时更新。

你可以在 **列表视图** 和 **纯文本视图** 之间切换构建日志。

- **列表视图** 以可折叠格式呈现所有构建步骤，并提供沿时间轴导航日志的时间线。

- **纯文本视图** 以纯文本显示日志。

**复制** 按钮可以让你将纯文本版本的日志复制到剪贴板。

### 构建历史

**History** 选项卡显示有关已完成构建的统计数据。

时间序列图显示相关构建的持续时间、构建步骤和缓存使用情况的趋势，帮助你识别构建操作随时间变化的模式和变化。例如，构建持续时间的显著峰值或缓存未命中的高数量可能表示优化 Dockerfile 的机会。

![构建历史图表](../images/build-ui-history.webp)

你可以通过在图表中选择相关构建或使用图表下方的 **Past builds** 列表导航并检查相关构建。

## 管理 builders

Docker Desktop 设置中的 **Builder settings** 视图让你可以：

- 检查活动 builders 的状态和配置
- 启动和停止 builder
- 删除构建历史
- 添加或

删除 builders（对于云 builders，连接和断开连接）

![Builder 设置下拉菜单](../images/build-ui-manage-builders.webp)

有关管理 builders 的更多信息，请参见：

- [更改设置，Windows](../settings/windows.md#builders)
- [更改设置，Mac](../settings/mac.md#builders)
- [更改设置，Linux](../settings/linux.md#builders)
