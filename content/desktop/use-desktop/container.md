---
description: 了解如何在 Docker Dashboard 中使用 Containers 视图
keywords: Docker Dashboard, manage, containers, gui, dashboard, images, user manual
title: 探索 Docker Desktop 中的 Containers 视图
---

**Containers** 视图列出了所有正在运行的容器和应用程序。你必须有正在运行或已停止的容器和应用程序才能看到它们在列表中显示。

## 容器操作

使用 **Search** 字段来搜索任何特定的容器。

在 **Containers** 视图中，你可以执行以下操作：
- 暂停/恢复
- 停止/启动/重启
- 查看镜像包和 CVE（漏洞）
- 删除
- 在 VS Code 中打开应用程序
- 在浏览器中打开容器暴露的端口
- 复制 `docker run` 命令。这使你可以共享容器运行的详细信息或修改某些参数。

## 资源使用情况

在 **Containers** 视图中，你可以监控容器的 CPU 和内存使用情况。这可以帮助你了解容器是否存在问题或是否需要分配更多资源。

当你[检查容器](#inspect-a-container)时，**Stats** 标签显示有关容器资源使用情况的更多信息。你可以看到容器随时间的 CPU、内存、网络和磁盘使用情况。

## 检查容器

选择容器时，你可以获取有关容器的详细信息。

在这里，你可以使用快速操作按钮执行各种操作，如暂停、恢复、启动或停止，或浏览 **Logs**、**Inspect**、**Bind mounts**、**Exec**、**Files** 和 **Stats** 标签。

### Logs

选择 **Logs** 以查看容器的日志。你还可以：

- 使用 `Cmd + f`/`Ctrl + f` 打开搜索栏并查找特定条目。搜索匹配项以黄色突出显示。
- 按 `Enter` 或 `Shift + Enter` 分别跳转到下一个或上一个搜索匹配项。
- 使用右上角的 **Copy** 图标将所有日志复制到剪贴板。
- 通过突出显示日志的几行或一部分，自动复制日志内容。
- 使用右上角的 **Clear terminal** 图标清除日志终端。
- 选择并查看日志中可能存在的外部链接。

### Inspect

选择 **Inspect** 查看有关容器的低级信息。它显示本地路径、镜像版本号、SHA-256、端口映射等详细信息。

### 集成终端

从 **Exec** 标签，你可以在 Docker Desktop 中直接使用集成终端在运行的容器上运行命令。你可以快速在容器内运行命令，以便了解其当前状态或在出现问题时进行调试。

使用集成终端与运行以下命令相同：

- `docker exec -it <container-id> /bin/sh`
- `docker exec -it <container-id> cmd.exe` （访问 Windows 容器时）
- `docker debug <container-id>` （使用调试模式时）

集成终端：

- 如果你导航到 Docker Dashboard 的另一个部分然后返回，它会保留你的会话和 **Debug mode** 设置。
- 支持复制、粘贴、搜索和清除会话。
- 在不使用调试模式时，它会自动检测容器镜像 Dockerfile 中指定的默认用户。如果没有指定用户，或者你使用的是调试模式，它将默认使用 `root`。

#### 打开集成终端

要打开集成终端，可以：

- 将鼠标悬停在运行中的容器上，在 **Actions** 列下选择 **Show container actions** 菜单。从下拉菜单中选择 **Open in terminal**。
- 或者，选择容器，然后选择 **Exec** 标签。

要使用外部终端，请导航到 **Settings** 中的 **General** 标签，并在 **Choose your terminal** 下选择 **System default** 选项。

#### 在调试模式下打开集成终端

> **Beta 功能**
>
> 调试模式功能目前处于 [Beta](../../release-lifecycle.md/#beta) 阶段。
{ .experimental }

调试模式需要 [Pro、Team 或 Business 订阅](/subscription/details/)。调试模式有几个优势，例如：

- 一个可定制的工具箱。工具箱预装了许多标准的 Linux 工具，例如 `vim`、`nano`、`htop` 和 `curl`。有关详细信息，请参见 [`docker debug` CLI 参考](/reference/cli/docker/debug/)。
- 能够访问没有 shell 的容器，例如精简或无发行版容器。

要在调试模式下打开集成终端：

1. 使用具有 Pro、Team 或 Business 订阅的帐户登录 Docker Desktop。
2. 登录后，可以：

   - 将鼠标悬停在运行中的容器上，在 **Actions** 列下选择 **Show container actions** 菜单。从下拉菜单中选择 **Use Docker Debug**。
   - 或者，选择容器，然后选择 **Debug** 标签。如果 **Debug** 标签不可见，请选择 **Exec** 标签，然后启用 **Debug mode** 设置。

要默认使用调试模式访问集成终端，请导航到 **Settings** 中的 **General** 标签，并选择 **Enable Docker Debug by default** 选项。

### Files

选择 **Files** 以浏览运行中或已停止容器的文件系统。你还可以：

- 查看最近添加、修改或删除的文件
- 直接从内置编辑器编辑文件
- 在主机和容器之间拖放文件和文件夹
- 右键单击文件删除不必要的文件
- 将文件和文件夹从容器下载到主机

## 其他资源

- [什么是容器](../../guides/docker-concepts/the-basics/what-is-a-container.md)
- [运行多容器应用程序](../../guides/docker-concepts/running-containers/multi-container-applications.md)
