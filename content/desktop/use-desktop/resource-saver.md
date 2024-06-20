---
description: 了解什么是 Docker Desktop 资源节省模式及其配置方法
keywords: Docker Dashboard, resource saver, manage, containers, gui, dashboard, user manual
title: Docker Desktop 的资源节省模式
---

资源节省（Resource Saver）是 Docker Desktop 4.24 及更高版本中提供的一项新功能。它通过在没有容器运行一段时间后自动停止 Docker Desktop Linux 虚拟机，大幅减少 Docker Desktop 在主机上的 CPU 和内存使用，减少 2 GB 或更多。默认时间设置为 5 分钟，但可以根据需要进行调整。

通过资源节省模式，当 Docker Desktop 处于空闲状态时，它使用的系统资源最少，从而延长笔记本电脑的电池寿命并改善多任务体验。

## 如何配置资源节省模式

资源节省模式默认启用，但可以通过导航到 **Settings** 中的 **Resources** 选项卡来禁用。你也可以按照下图所示配置空闲计时器。

![Resource Saver Settings](../images/resource-saver-settings.png)

如果可用的值不能满足你的需求，可以通过更改 Docker Desktop `settings.json` 文件中的 `autoPauseTimeoutSeconds` 配置为任何大于 30 秒的值：

- Mac: `~/Library/Group Containers/group.com.docker/settings.json`
- Windows: `C:\Users\[USERNAME]\AppData\Roaming\Docker\settings.json`
- Linux: `~/.docker/desktop/settings.json`

重新配置后无需重启 Docker Desktop。

当 Docker Desktop 进入资源节省模式时：
- 在 Docker Desktop 状态栏以及系统托盘中的 Docker 图标上显示一个叶子图标。下图显示了资源节省模式开启时 Linux 虚拟机的 CPU 和内存使用率为零。

  ![Resource Saver Status Bar](../images/resource-saver-status-bar.png)

- 不运行容器的 Docker 命令，例如列出容器镜像或卷的命令，不一定会触发退出资源节省模式，因为 Docker Desktop 可以在不唤醒 Linux 虚拟机的情况下处理这些命令。

> **注意**
>
> Docker Desktop 需要时会自动退出资源节省模式。导致退出资源节省模式的命令执行时间稍长（约 3 到 10 秒），因为 Docker Desktop 需要重新启动 Linux 虚拟机。在 Mac 和 Linux 上通常较快，在使用 Hyper-V 的 Windows 上较慢。一旦 Linux 虚拟机重启，后续的容器运行会像往常一样立即进行。

## 资源节省模式与暂停功能

资源节省模式的优先级高于较旧的[暂停](pause.md)功能，这意味着当 Docker Desktop 处于资源节省模式时，无法手动暂停 Docker Desktop（也没有意义，因为资源节省模式实际上停止了 Docker Desktop Linux 虚拟机）。一般来说，我们建议保持资源节省模式启用，而不是禁用它并使用手动暂停功能，因为它能带来更好的 CPU 和内存节省效果。

## Windows 上的资源节省模式

在 Windows 上使用 WSL 时，资源节省模式的工作方式有所不同。它仅暂停 `docker-desktop` WSL 发行版内的 Docker 引擎，而不是停止 WSL 虚拟机。这是因为在 WSL 中，所有 WSL 发行版共享一个 Linux 虚拟机，因此 Docker Desktop 不能停止 Linux 虚拟机（即，WSL Linux 虚拟机不归 Docker Desktop 所有）。因此，资源节省模式在 WSL 上减少了 CPU 使用，但不减少 Docker 的内存使用。

要减少 WSL 上的内存使用，我们建议用户启用 WSL 的 `autoMemoryReclaim` 功能，具体请参见[Docker Desktop WSL 文档](../wsl/_index.md)。最后，由于 Docker Desktop 不会停止 WSL 上的 Linux 虚拟机，退出资源节省模式是即时的（没有退出延迟）。

## 反馈

要提供反馈或报告可能发现的任何错误，请在相应的 Docker Desktop GitHub 存储库中创建一个 issue：

- [for-mac](https://github.com/docker/for-mac)
- [for-win](https://github.com/docker/for-win)
