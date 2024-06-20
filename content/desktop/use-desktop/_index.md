---
description: 了解如何在 Docker Desktop 中使用 Docker Dashboard，包括快速搜索、Docker 菜单等功能
keywords: Docker Dashboard, manage, containers, gui, dashboard, images, user manual, whale menu
title: 探索 Docker Desktop
aliases:
- /desktop/dashboard/
---

打开 Docker Desktop 时，会显示 Docker Dashboard。

![容器视图中的 Docker Dashboard](../images/dashboard.webp)

**Containers** 视图提供了所有容器和应用程序的运行时视图。它允许你与容器和应用程序进行交互，并直接从你的机器管理应用程序的生命周期。此视图还提供了一个直观的界面来执行常见操作，以检查、交互和管理包括容器和基于 Docker Compose 的应用程序在内的 Docker 对象。有关更多信息，请参见[探索运行中的容器和应用程序](container.md)。

**Images** 视图显示你的 Docker 镜像列表，并允许你将镜像作为容器运行、从 Docker Hub 拉取最新版本的镜像以及检查镜像。它还显示镜像漏洞的摘要。此外，**Images** 视图包含清理选项，可以从磁盘中删除不需要的镜像以回收空间。如果你已登录，还可以看到你和你的组织在 Docker Hub 上共享的镜像。有关更多信息，请参见[探索你的镜像](images.md)。

**Volumes** 视图显示卷列表，并允许你轻松创建和删除卷以及查看哪些卷正在使用中。有关更多信息，请参见[探索卷](volumes.md)。

**Builds** 视图让你检查构建历史并管理 builders。默认情况下，它显示所有正在进行和已完成的构建列表。[探索构建](builds.md)。

此外，Docker Dashboard 还允许你：

- 导航到 **Settings** 菜单以配置 Docker Desktop 设置。在 Dashboard 头部选择 **Settings** 图标。
- 访问 **Troubleshoot** 菜单进行调试和执行重启操作。在 Dashboard 头部选择 **Troubleshoot** 图标。
- 在 **Notifications center** 中收到新版本的通知、安装进度更新等。在 Docker Dashboard 的右下角选择铃铛图标以访问通知中心。
- 从 Dashboard 头部访问 **Learning center**。它帮助你快速入门，通过应用内快速演练提供其他学习 Docker 的资源。

  有关更详细的入门指南，请参见[开始使用](../../guides/getting-started/_index.md)。
- 访问 [Docker Scout](../../scout/_index.md) dashboard。
- 检查 Docker 服务的状态。

## 快速搜索

在 Docker Dashboard 中，你可以使用位于 Dashboard 头部的快速搜索来搜索：

- 本地系统上的任何容器或 Compose 应用程序。你可以看到关联环境变量的概览或执行快速操作，如启动、停止或删除。

- 公共 Docker Hub 镜像、本地镜像和来自远程仓库的镜像（你所在组织的私有仓库中的镜像）。根据你选择的镜像类型，你可以按标签拉取镜像、查看文档、前往 Docker Hub 获取更多详细信息，或使用该镜像运行新容器。

- 扩展。从这里，你可以了解更多关于扩展的信息并一键安装它。或者，如果你已经安装了扩展，可以直接从搜索结果中打开它。

- 任何卷。从这里你可以查看关联的容器。

- 文档。直接从 Docker Desktop 查找 Docker 的官方文档中的帮助。

## Docker 菜单

Docker Desktop 还提供了一个易于访问的托盘图标，该图标出现在任务栏中，被称为 Docker 菜单 {{< inline-image src="../../assets/images/whale-x.svg" alt="whale menu" >}}。

要显示 Docker 菜单，请选择 {{< inline-image src="../../assets/images/whale-x.svg" alt="whale menu" >}} 图标。它显示以下选项：

- **Dashboard**。这会带你到 Docker Dashboard。
- **登录/注册**
- **设置**
- **检查更新**
- **故障排除**
- **提供反馈**
- **切换到 Windows 容器**（如果你使用的是 Windows）
- **关于 Docker Desktop**。包含有关你正在运行的版本的信息，以及订阅服务协议的链接。
- **Docker Hub**
- **文档**
- **扩展**
- **Kubernetes**
- **重启**
- **退出 Docker Desktop**
