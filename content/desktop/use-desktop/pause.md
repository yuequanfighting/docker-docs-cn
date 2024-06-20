---
description: 了解暂停 Docker Dashboard 的含义
keywords: Docker Dashboard, manage, containers, gui, dashboard, pause, user manual
title: 暂停 Docker Desktop
---

当 Docker Desktop 被暂停时，运行 Docker 引擎的 Linux 虚拟机也会被暂停，所有容器的当前状态保存在内存中，所有进程都会被冻结。这将减少 CPU 和内存的使用，有助于延长笔记本电脑的电池寿命。

你可以通过选择 Docker 菜单 {{< inline-image src="../images/whale-x.svg" alt="whale menu" >}} 并选择 **Pause** 来手动暂停 Docker Desktop。要手动恢复 Docker Desktop，请在 Docker 菜单中选择 **Resume** 选项，或运行任何 Docker CLI 命令。

当你手动暂停 Docker Desktop 时，Docker 菜单和 Docker Dashboard 上会显示暂停状态。你仍然可以访问 **Settings** 和 **Troubleshoot** 菜单。

>**提示**
>
> Docker Desktop 4.24 及更高版本中可用的 Resource Saver 功能默认启用，提供比手动暂停功能更好的 CPU 和内存节省效果。更多信息请参见[这里](resource-saver.md)。
{ .tip }
