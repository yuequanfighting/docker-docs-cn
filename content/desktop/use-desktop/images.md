---
description: 了解如何在 Docker Dashboard 中使用 Images 视图
keywords: Docker Dashboard, manage, containers, gui, dashboard, images, user manual
title: 探索 Docker Desktop 中的 Images 视图
---

**Images** 视图让你无需使用 CLI 即可管理 Docker 镜像。默认情况下，它显示本地磁盘上所有的 Docker 镜像列表。

登录 Docker Hub 后，你还可以查看 Hub 上的镜像。这使你可以与团队协作，并直接通过 Docker Desktop 管理你的镜像。

**Images** 视图允许你执行核心操作，如将镜像作为容器运行、从 Docker Hub 拉取最新版本的镜像、将镜像推送到 Docker Hub 以及检查镜像。

它还显示关于镜像的元数据，如：
- 标签
- 镜像 ID
- 创建日期
- 镜像大小

对于运行和停止的容器所使用的镜像，旁边会显示一个 **In Use** 标签。你可以通过选择搜索栏右侧的 **More options** 菜单，使用切换开关根据你的偏好选择要显示的信息。

**Images on disk** 状态栏显示镜像的数量、镜像所用的磁盘空间总量以及上次刷新的时间。

## 管理你的镜像

使用 **Search** 字段搜索任何特定的镜像。

你可以按以下方式排序镜像：

- 使用中
- 未使用
- 悬空

## 将镜像作为容器运行

在 **Images** 视图中，将鼠标悬停在一个镜像上并选择 **Run**。

当被提示时，你可以：

- 选择 **Optional settings** 下拉菜单来指定名称、端口、卷、环境变量，然后选择 **Run**
- 不指定任何可选设置直接选择 **Run**

## 检查镜像

要检查镜像，请选择镜像行。检查镜像会显示有关镜像的详细信息，例如：

- 镜像历史
- 镜像 ID
- 镜像创建日期
- 镜像大小
- 构成镜像的层
- 使用的基础镜像
- 发现的漏洞
- 镜像内的包

[Docker Scout](../../scout/index.md) 提供此漏洞信息。
有关此视图的更多信息，请参见 [Image details view](../../scout/image-details-view.md)。

## 从 Docker Hub 拉取最新镜像

从列表中选择镜像，选择 **More options** 按钮，然后选择 **Pull**。

> **注意**
>
> 仓库必须在 Docker Hub 上存在才能拉取镜像的最新版本。你必须登录才能拉取私有镜像。

## 将镜像推送到 Docker Hub

从列表中选择镜像，选择 **More options** 按钮，然后选择 **Push to Hub**。

> **注意**
>
> 你只能将镜像推送到 Docker Hub，如果该镜像属于你的 Docker ID 或你的组织。也就是说，镜像标签中必须包含正确的用户名/组织名，才能将其推送到 Docker Hub。

## 删除镜像

> **注意**
>
> 要删除一个被运行或停止的容器使用的镜像，必须先删除相关的容器。

未使用的镜像是不被任何运行或停止的容器使用的镜像。当你使用相同标签构建镜像的新版本时，镜像会变成悬空状态。

要删除单个镜像，请选择垃圾桶图标。

## Docker Hub 仓库

**Images** 视图还允许你管理和与 Docker Hub 仓库中的镜像进行交互。
默认情况下，当你进入 Docker Desktop 中的 **Images** 时，你会看到本地镜像存储中的镜像列表。
顶部的 **Local** 和 **Hub** 标签可以在查看本地镜像存储中的镜像和你有访问权限的远程 Docker Hub 仓库中的镜像之间切换。

切换到 **Hub** 标签会提示你登录 Docker Hub 帐户，如果你尚未登录。
登录后，它会显示你有访问权限的 Docker Hub 组织和仓库中的镜像列表。

从下拉菜单中选择一个组织，以查看该组织的仓库列表。

如果你在仓库上启用了 [Docker Scout](../../scout/_index.md)，图像标签旁会显示图像分析结果。

将鼠标悬停在镜像标签上会显示两个选项：

- **Pull**：从 Docker Hub 拉取镜像的最新版本。
- **View in Hub**：打开 Docker Hub 页面并显示有关镜像的详细信息。

## 其他资源

- [什么是镜像？](../../guides/docker-concepts/the-basics/what-is-an-image.md)
