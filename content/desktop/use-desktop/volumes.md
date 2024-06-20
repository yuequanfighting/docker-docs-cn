---
description: 了解如何在 Docker Dashboard 的 Volumes 视图中操作
keywords: Docker Dashboard, manage, containers, gui, dashboard, volumes, user manual
title: 探索 Volumes 视图
---

**Volumes** 视图让你可以在不使用 CLI 的情况下管理 Docker 卷。默认情况下，它会显示本地磁盘上的所有 Docker 卷列表。

## 查看卷

你可以查看有关卷的以下信息：

- 名称：卷的名称。
- 状态：卷是否被容器使用。
- 创建时间：卷创建的时间。
- 大小：卷的大小。

默认情况下，**Volumes** 视图会显示所有卷的列表。

你可以通过以下操作筛选和排序卷以及修改显示的列：

- 按名称筛选卷：使用 **Search** 字段。
- 按状态筛选卷：在搜索栏右侧，通过 **In use** 或 **Unused** 筛选卷。
- 排序卷：选择列名以排序卷。
- 自定义列：在搜索栏右侧，选择要显示的卷信息。

## 克隆卷

克隆卷会创建一个新卷，并复制被克隆卷中的所有数据。当克隆被一个或多个运行中的容器使用的卷时，容器会在克隆数据时暂时停止，并在克隆过程完成后重新启动。

克隆卷步骤如下：

1. 登录 Docker Desktop。你必须登录才能克隆卷。
2. 在要克隆的卷的 **Actions** 列中选择 **Clone** 图标。
3. 在 **Clone a volume** 模态窗口中，指定 **Volume name**，然后选择 **Clone**。

## 创建卷

你可以使用以下步骤创建一个空卷。或者，如果你[启动带有卷的容器](../../storage/volumes.md#start-a-container-with-a-volume)且该卷不存在，Docker 会为你创建该卷。

创建卷步骤如下：

1. 选择 **Create** 按钮。
2. 在 **New Volume** 模态窗口中，指定卷名，然后选择 **Create**。

要将卷与容器一起使用，请参阅[使用卷](../../storage/volumes.md#start-a-container-with-a-volume)。

## 删除一个或多个卷

删除卷会删除卷及其所有数据。当容器正在使用卷时，你无法删除该卷，即使容器已停止。你必须先停止并删除任何使用该卷的容器，然后才能删除该卷。

删除卷步骤如下：

1. 在要删除的卷的 **Actions** 列中选择 **Show volume actions** 图标。
2. 选择 **Delete volume**。
3. 在 **Delete volume?** 模态窗口中，选择 **Delete forever**。

删除多个卷步骤如下：

1. 选中要删除的所有卷旁边的复选框。
2. 选择 **Delete**。
3. 在 **Delete volumes?** 模态窗口中，选择 **Delete forever**。

## 清空卷

清空卷会删除卷中的所有数据，但不会删除卷。当清空被一个或多个运行中的容器使用的卷时，容器会在清空数据时暂时停止，并在清空过程完成后重新启动。

清空卷步骤如下：

1. 登录 Docker Desktop。你必须登录才能清空卷。
2. 在要清空的卷的 **Actions** 列中选择 **Show volume actions** 图标。
3. 选择 **Empty volume**。
4. 在 **Empty a volume?** 模态窗口中，选择 **Empty**。

## 导出卷

> **测试功能**
>
> 卷导出功能目前处于[测试阶段](../../release-lifecycle.md/#beta)。
{ .experimental }

你可以将卷的内容导出到本地文件、本地镜像或 Docker Hub 上的镜像。当导出一个或多个运行中的容器使用的卷的内容时，容器会在导出内容时暂时停止，并在导出过程完成后重新启动。

导出卷步骤如下：

1. 登录 Docker Desktop。你必须登录才能导出卷。
2. 在要导出内容的卷的 **Actions** 列中选择 **Export** 图标。
3. 在 **Export content** 模态窗口中，选择要导出内容的位置，然后根据你的选择指定以下附加详细信息：

   - **本地文件**：指定文件名并选择文件夹。
   - **本地镜像**：选择要导出内容到的本地镜像。任何现有数据将被导出的内容替换。
   - **新镜像**：指定新镜像的名称。
   - **注册表**：指定 Docker Hub 仓库。注意，Docker Hub 仓库可以公开访问，这意味着你的数据可能会公开访问。有关更多详情，请参阅[将仓库从公开更改为私有](../../docker-hub/repos/#change-a-repository-from-public-to-private)。

4. 选择 **Export**。

## 导入卷

> **测试功能**
>
> 卷导入功能目前处于[测试阶段](../../release-lifecycle.md/#beta)。
{ .experimental }

你可以从本地文件、本地镜像或 Docker Hub 镜像导入卷。导入的内容将替换卷中的任何现有数据。当导入一个或多个运行中的容器使用的卷的内容时，容器会在导入内容时暂时停止，并在导入过程完成后重新启动。

导入卷步骤如下：

1. 登录 Docker Desktop。你必须登录才能导入卷。
2. 可选地，[创建](#create-a-volume)一个新卷以导入内容。
3. 在要导入内容的卷的 **Actions** 列中选择 **Import** 图标。
4. 在 **Import content** 模态窗口中，选择内容的来源，然后根据你的选择指定以下附加详细信息：

   - **本地文件**：选择包含内容的文件。
   - **本地镜像**：选择包含内容的本地镜像。
   - **注册表**：指定包含内容的 Docker Hub 镜像。

5. 选择 **Import**。

## 检查卷

要查看特定卷的详细信息，请从列表中选择一个卷。这会打开详细视图。

**In Use** 选项卡显示使用该卷的容器名称、镜像名称、容器使用的端口号和目标。目标是容器内的路径，提供对卷中文件的访问。

**Data** 选项卡显示卷中的文件和文件夹以及文件大小。要保存文件或文件夹，请右键单击文件或文件夹以显示选项菜单，选择 **Save as...**，然后指定下载文件的位置。

要从卷中删除文件或文件夹，请右键单击文件或文件夹以显示选项菜单，选择 **Delete**，然后再次选择 **Delete** 以确认。

## 其他资源

- [持久化容器数据](../../guides/docker-concepts/running-containers/persisting-container-data.md)
- [使用卷](../../storage/volumes.md)
