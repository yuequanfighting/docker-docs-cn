---
description: "使用 Docker CLI 运行和配置容器"
keywords: "docker, run, cli"
aliases:
- /reference/run/
title: 运行容器
---

Docker 在隔离的容器中运行进程。容器是运行在主机上的一个进程。主机可以是本地或远程的。当你执行 `docker run` 时，容器进程是隔离运行的，它有自己的文件系统、自己的网络以及独立于主机的进程树。

本页面详细介绍如何使用 `docker run` 命令运行容器。

## 基本形式

一个 `docker run` 命令的基本形式如下：

```console
$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

`docker run` 命令必须指定一个用于创建容器的[镜像引用](#image-references)。

### 镜像引用

镜像引用是镜像的名称和版本。你可以使用镜像引用来创建或运行基于镜像的容器。

- `docker run IMAGE[:TAG][@DIGEST]`
- `docker create IMAGE[:TAG][@DIGEST]`

镜像标签是镜像的版本，当省略时默认为 `latest`。使用标签可以运行特定版本的镜像。例如，要运行 `ubuntu` 镜像的 `23.10` 版本：`docker run ubuntu:23.10`。

#### 镜像摘要

使用 v2 或更高版本镜像格式的镜像具有一个内容可寻址的标识符，称为摘要。只要生成镜像的输入保持不变，摘要值就是可预测的。

以下示例使用 `sha256:9cacb71397b640eca97488cf08582ae4e4068513101088e9f96c9814bfda95e0` 摘要运行 `alpine` 镜像的容器：

```console
$ docker run alpine@sha256:9cacb71397b640eca97488cf08582ae4e4068513101088e9f96c9814bfda95e0 date
```

### 选项

`[OPTIONS]` 允许你为容器配置选项。例如，你可以给容器指定一个名称（`--name`），或者将其作为后台进程运行（`-d`）。你还可以设置控制资源约束和网络的选项。

### 命令和参数

你可以使用 `[COMMAND]` 和 `[ARG...]` 位置参数来指定容器启动时运行的命令和参数。例如，你可以指定 `sh` 作为 `[COMMAND]`，结合 `-i` 和 `-t` 标志，在容器中启动一个交互式 shell（如果你选择的镜像在 `PATH` 中有一个 `sh` 可执行文件）。

```console
$ docker run -it IMAGE sh
```

> **注意**
>
> 根据你的 Docker 系统配置，可能需要在 `docker run` 命令前加 `sudo`。为了避免在使用 `docker` 命令时必须使用 `sudo`，系统管理员可以创建一个名为 `docker` 的 Unix 组并将用户添加到该组中。有关此配置的更多信息，请参阅操作系统的 Docker 安装文档。

## 前台和后台

默认情况下，当你启动一个容器时，容器在前台运行。如果你希望容器在后台运行，可以使用 `--detach`（或 `-d`）标志。这会启动容器而不会占用你的终端窗口。

```console
$ docker run -d <IMAGE>
```

当容器在后台运行时，你可以使用其他 CLI 命令与容器交互。例如，`docker logs` 允许你查看容器的日志，`docker attach` 可以将其带回前台。

```console
$ docker run -d nginx
0246aa4d1448a401cabd2ce8f242192b6e7af721527e48a810463366c7ff54f1
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS     NAMES
0246aa4d1448   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 1 second   80/tcp    pedantic_liskov
$ docker logs -n 5 0246aa4d1448
2023/11/06 15:58:23 [notice] 1#1: start worker process 33
2023/11/06 15:58:23 [notice] 1#1: start worker process 34
2023/11/06 15:58:23 [notice] 1#1: start worker process 35
2023/11/06 15:58:23 [notice] 1#1: start worker process 36
2023/11/06 15:58:23 [notice] 1#1: start worker process 37
$ docker attach 0246aa4d1448
^C
2023/11/06 15:58:40 [notice] 1#1: signal 2 (SIGINT) received, exiting
...
```

有关 `docker run` 与前台和后台模式相关的标志的更多信息，请参见：

- [`docker run --detach`](https://docs.docker.com/reference/cli/docker/container/run/#detach): 在后台运行容器
- [`docker run --attach`](https://docs.docker.com/reference/cli/docker/container/run/#attach): 附加到 `stdin`、`stdout` 和 `stderr`
- [`docker run --tty`](https://docs.docker.com/reference/cli/docker/container/run/#tty): 分配一个伪终端
- [`docker run --interactive`](https://docs.docker.com/reference/cli/docker/container/run/#interactive): 即使未附加也保持 `stdin` 打开

有关重新附加到后台容器的更多信息，请参见 [`docker attach`](https://docs.docker.com/reference/cli/docker/container/attach/)。

## 容器标识

你可以通过三种方式标识容器：

| 标识符类型            | 示例值                                                                  |
|:----------------------|:-----------------------------------------------------------------------|
| UUID 长标识符         | `f78375b1c487e03c9438c729345e54db9d20cfa2ac1fc3494b6eb60872e74778`      |
| UUID 短标识符         | `f78375b1c487`                                                         |
| 名称                  | `evil_ptolemy`                                                         |

UUID 标识符是由守护进程分配给容器的随机 ID。

守护进程会自动为容器生成一个随机字符串名称。你也可以使用 [`--name` 标志](https://docs.docker.com/reference/cli/docker/container/run/#name)定义一个自定义名称。定义 `name` 可以方便地为容器添加意义。如果你指定了 `name`，可以在引用用户定义网络中的容器时使用该名称。这适用于后台和前台 Docker 容器。

容器标识符与镜像引用不同。镜像引用指定运行容器时使用的镜像。你不能运行 `docker exec nginx:alpine sh` 来打开基于 `nginx:alpine` 镜像的容器中的 shell，因为 `docker exec` 期望的是容器标识符（名称或 ID），而不是镜像。

虽然容器使用的镜像不是容器的标识符，但你可以使用 `--filter` 标志查找使用某个镜像的容器的 ID。例如，以下 `docker ps` 命令获取基于 `nginx:alpine` 镜像的所有运行中的容器的 ID：

```console
$ docker ps -q --filter ancestor=nginx:alpine
```

有关使用过滤器的更多信息，请参见 [过滤](https://docs.docker.com/config/filter/)。

## 容器网络

容器默认启用网络，并且可以发出外部连接。如果你运行多个需要相互通信的容器，可以创建一个自定义网络并将容器附加到该网络。

当多个容器附加到同一个自定义网络时，它们可以使用容器名称作为 DNS 主机名进行通信。以下示例创建一个名为 `my-net` 的自定义网络，并运行两个附加到该网络的容器。

```console
$ docker network create my-net
$ docker run -d --name web --network my-net nginx:alpine
$ docker run --rm -it --network my-net busybox
/ # ping web
PING web (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.326 ms
64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.257 ms
64 bytes from 172.18.0.2: seq=2 ttl=64 time=0.281 ms
^C
--- web ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.257/0.288/0.326 ms
```

有关容器网络的更多信息，请参见 [网络概述](https://docs.docker.com/network/)。

我们没有设置任何内存限制，这意味着容器中的进程可以根据需要使用尽可能多的内存和交换内存。

```console
$ docker run -it -m 300M --memory-swap -1 ubuntu:22.04 /bin/bash
```

我们设置了内存限制并禁用了交换内存限制，这意味着容器中的进程可以使用300M内存以及根据需要使用尽可能多的交换内存（如果主机支持交换内存）。

```console
$ docker run -it -m 300M ubuntu:22.04 /bin/bash
```

我们只设置了内存限制，这意味着容器中的进程可以使用300M内存和300M交换内存。默认情况下，总虚拟内存大小（--memory-swap）将设置为内存的两倍，在这种情况下，内存+交换内存将为2*300M，因此进程也可以使用300M的交换内存。

```console
$ docker run -it -m 300M --memory-swap 1G ubuntu:22.04 /bin/bash
```

我们设置了内存和交换内存，因此容器中的进程可以使用300M内存和700M交换内存。

内存保留是一种内存软限制，允许更大程度的内存共享。在正常情况下，容器可以根据需要使用尽可能多的内存，并且只受 `-m`/`--memory` 选项设置的硬限制约束。当设置了内存保留时，Docker会检测内存争用或低内存并强制容器将其消耗限制在保留限额内。

始终将内存保留值设置为低于硬限制，否则硬限制优先。保留值为0等同于不设置保留。默认情况下（未设置保留），内存保留与硬内存限制相同。

内存保留是一种软限制功能，并不保证限制不会被超越。相反，该功能试图确保在内存高度争用时，内存的分配基于保留提示/设置。

以下示例将内存限制（`-m`）设置为500M，并将内存保留设置为200M。

```console
$ docker run -it -m 500M --memory-reservation 200M ubuntu:22.04 /bin/bash
```

在此配置下，当容器消耗超过200M但少于500M的内存时，下一次系统内存回收尝试将容器内存缩减到200M以下。

以下示例将内存保留设置为1G而没有硬内存限制。

```console
$ docker run -it --memory-reservation 1G ubuntu:22.04 /bin/bash
```

容器可以根据需要使用尽可能多的内存。内存保留设置确保容器不会长时间消耗过多的内存，因为每次内存回收都会将容器的消耗量缩减到保留值。

默认情况下，如果发生内存不足（OOM）错误，内核会杀死容器中的进程。要更改此行为，请使用 `--oom-kill-disable` 选项。只有在设置了 `-m/--memory` 选项的容器上禁用OOM杀手。如果未设置 `-m` 标志，这可能会导致主机内存耗尽并需要杀死主机的系统进程以释放内存。

以下示例将内存限制为100M并禁用该容器的OOM杀手：

```console
$ docker run -it -m 100M --oom-kill-disable ubuntu:22.04 /bin/bash
```

以下示例展示了使用该标志的危险方法：

```console
$ docker run -it --oom-kill-disable ubuntu:22.04 /bin/bash
```

该容器具有无限制的内存，这可能导致主机内存耗尽并需要杀死系统进程以释放内存。可以更改 `--oom-score-adj` 参数以选择系统内存不足时将被杀死的容器的优先级，负分数使其不太可能被杀死，正分数则更可能被杀死。

### 内核内存约束

内核内存与用户内存根本不同，因为内核内存无法交换出去。无法交换会导致容器消耗过多内核内存而阻塞系统服务。内核内存包括：

- 栈页
- Slab页
- 套接字内存压力
- TCP内存压力

您可以设置内核内存限制以约束这些类型的内存。例如，每个进程都会消耗一些栈页。通过限制内核内存，您可以在内核内存使用过高时阻止新进程的创建。

内核内存从未完全独立于用户内存。相反，您在用户内存限制的上下文中限制内核内存。假设 "U" 是用户内存限制，"K" 是内核限制。有三种可能的设置方式：

<table>
  <thead>
    <tr>
      <th>选项</th>
      <th>结果</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="no-wrap"><strong>U != 0, K = 无限</strong> (默认)</td>
      <td>
        这是使用内核内存之前已经存在的标准内存限制机制。内核内存被完全忽略。
      </td>
    </tr>
    <tr>
      <td class="no-wrap"><strong>U != 0, K &lt; U</strong></td>
      <td>
        内核内存是用户内存的一个子集。这种设置在每个cgroup的总内存量超额分配的部署中很有用。
        不建议超额分配内核内存限制，因为系统仍可能耗尽不可回收的内存。
        在这种情况下，您可以配置K，使得所有组的总和永远不大于总内存。然后，可以自由地设置U，但以牺牲系统服务质量为代价。
      </td>
    </tr>
    <tr>
      <td class="no-wrap"><strong>U != 0, K &gt; U</strong></td>
      <td>
        由于内核内存费用也会传递给用户计数器，并且对于两种内存，都会触发容器的回收。
        这种配置为管理员提供了统一的内存视图。它对于只想跟踪内核内存使用情况的人也很有用。
      </td>
    </tr>
  </tbody>
</table>

示例：

```console
$ docker run -it -m 500M --kernel-memory 50M ubuntu:22.04 /bin/bash
```

我们设置了内存和内核内存，因此容器中的进程可以总共使用500M内存，其中可以最多使用50M的内核内存。

```console
$ docker run -it --kernel-memory 50M ubuntu:22.04 /bin/bash
```

我们在没有设置 **-m** 的情况下设置了内核内存，因此容器中的进程可以根据需要使用尽可能多的内存，但只能使用50M的内核内存。

### 交换性约束

默认情况下，容器的内核可以交换一定比例的匿名页。要为容器设置此比例，请指定一个介于0和100之间的 `--memory-swappiness` 值。值为0会关闭匿名页交换。值为100会设置所有匿名页为可交换。默认情况下，如果不使用 `--memory-swappiness`，内存交换性值将从父进程继承。

例如，您可以设置：

```console
$ docker run -it --memory-swappiness=0 ubuntu:22.04 /bin/bash
```

设置 `--memory-swappiness` 选项有助于保留容器的工作集并避免交换性能的损失。

### CPU共享约束

默认情况下，所有容器获得相同比例的CPU周期。可以通过更改容器的CPU共享权重来修改相对于所有其他正在运行的容器的权重。

要将比例从默认的1024修改为2或更高的权重值，请使用 `-c` 或 `--cpu-shares` 标志。如果设置为0，系统将忽略该值并使用默认值1024。

比例仅在运行CPU密集型进程时适用。当一个容器中的任务空闲时，其他容器可以使用剩余的CPU时间。实际的CPU时间量会根据系统上运行的容器数量而变化。

例如，考虑三个容器，一个的cpu-share为1024，另外两个的cpu-share设置为512。当三个容器中的进程尝试使用100%的CPU时，第一个容器将获得总CPU时间的50%。如果添加一个cpu-share为1024的第四个容器，第一个容器仅获得33%的CPU，剩余的容器分别获得16.5%、16.5%和33%的CPU。

在多核系统上，CPU时间的份额分布在所有CPU核心上。即使一个容器被限制在小于

100%的CPU时间，它也可以使用多个核心的资源，而不是一个单独的核心。

下面的示例运行一个使用25%的CPU时间的容器。

```console
$ docker run -it --cpu-shares 256 ubuntu:22.04 /bin/bash
```

在高负载场景下，所有CPU核心的CPU周期将在所有容器之间共享。

### CPU排他性约束

对容器执行CPU排他性约束意味着仅为该容器分配特定的CPU（通常称为CPU亲和力）。可以为容器的任务绑定一个或多个处理器，确保进程只在指定的CPU上运行。这通过提高缓存命中率和减少跨CPU切换成本来提高性能。

```console
$ docker run -it --cpuset-cpus 0,1 ubuntu:22.04 /bin/bash
```

在此示例中，容器中的进程将绑定到CPU0和CPU1。

### CPU优先级约束

可以指定一个相对优先级，以表示容器相对于其他容器的优先级。CPU周期的分配由 `cpu.shares` 的值决定。

```console
$ docker run -it --cpu-shares 1024 ubuntu:22.04 /bin/bash
```

此示例中，容器将获得与其他默认容器相同的CPU优先级。

默认情况下，Docker 容器是“非特权”的，无法在 Docker 容器内运行 Docker 守护进程。这是因为默认情况下容器不允许访问任何设备，但“特权”容器可以访问所有设备（参见[cgroups 设备](https://www.kernel.org/doc/Documentation/cgroup-v1/devices.txt)文档）。

`--privileged` 标志为容器提供了所有权限。当操作员执行 `docker run --privileged` 时，Docker 允许访问主机上的所有设备，并重新配置 AppArmor 或 SELinux，使容器几乎具有与在主机上运行的进程相同的访问权限。请谨慎使用此标志。有关 `--privileged` 标志的更多信息，请参见 [`docker run` 参考](https://docs.docker.com/reference/cli/docker/container/run/#privileged)。

如果你希望限制对特定设备或设备的访问，可以使用 `--device` 标志。它允许你指定一个或多个设备，这些设备将在容器内可访问。

```console
$ docker run --device=/dev/snd:/dev/snd ...
```

默认情况下，容器将能够 `read`、`write` 和 `mknod` 这些设备。你可以使用第三个 `:rwm` 选项来覆盖每个 `--device` 标志：

```console
$ docker run --device=/dev/sda:/dev/xvdc --rm -it ubuntu fdisk /dev/xvdc

Command (m for help): q
$ docker run --device=/dev/sda:/dev/xvdc:r --rm -it ubuntu fdisk /dev/xvdc
You will not be able to write the partition table.

Command (m for help): q

$ docker run --device=/dev/sda:/dev/xvdc:w --rm -it ubuntu fdisk /dev/xvdc
    crash....

$ docker run --device=/dev/sda:/dev/xvdc:m --rm -it ubuntu fdisk /dev/xvdc
fdisk: unable to open /dev/xvdc: Operation not permitted
```

除了 `--privileged`，操作员还可以使用 `--cap-add` 和 `--cap-drop` 来细粒度地控制能力。默认情况下，Docker 保留了一个默认的能力列表。以下表格列出了默认允许并可以删除的 Linux 能力选项。

| 能力键                | 能力描述                                                                                                         |
|:----------------------|:----------------------------------------------------------------------------------------------------------------|
| AUDIT_WRITE           | 将记录写入内核审计日志。                                                                                        |
| CHOWN                 | 对文件的 UID 和 GID 进行任意更改（参见 chown(2)）。                                                              |
| DAC_OVERRIDE          | 绕过文件读取、写入和执行权限检查。                                                                              |
| FOWNER                | 绕过对操作的权限检查，这些操作通常需要进程的文件系统 UID 与文件的 UID 相匹配。                                  |
| FSETID                | 修改文件时不清除 set-user-ID 和 set-group-ID 权限位。                                                            |
| KILL                  | 绕过发送信号的权限检查。                                                                                        |
| MKNOD                 | 使用 mknod(2) 创建特殊文件。                                                                                    |
| NET_BIND_SERVICE      | 将套接字绑定到互联网域特权端口（端口号小于 1024）。                                                             |
| NET_RAW               | 使用 RAW 和 PACKET 套接字。                                                                                      |
| SETFCAP               | 设置文件能力。                                                                                                  |
| SETGID                | 对进程 GID 和补充 GID 列表进行任意操作。                                                                         |
| SETPCAP               | 修改进程能力。                                                                                                  |
| SETUID                | 对进程 UID 进行任意操作。                                                                                       |
| SYS_CHROOT            | 使用 chroot(2)，更改根目录。                                                                                    |

下表列出了默认不授予且可以添加的能力。

| 能力键                | 能力描述                                                                                                         |
|:----------------------|:----------------------------------------------------------------------------------------------------------------|
| AUDIT_CONTROL         | 启用和禁用内核审计；更改审计过滤规则；检索审计状态和过滤规则。                                                |
| AUDIT_READ            | 允许通过多播 netlink 套接字读取审计日志。                                                                        |
| BLOCK_SUSPEND         | 允许防止系统挂起。                                                                                              |
| BPF                   | 允许创建 BPF 映射，加载 BPF 类型格式（BTF）数据，检索 BPF 程序的 JITed 代码，等等。                              |
| CHECKPOINT_RESTORE    | 允许检查点/恢复相关操作。引入于内核 5.9。                                                                        |
| DAC_READ_SEARCH       | 绕过文件读取权限检查和目录读取和执行权限检查。                                                                  |
| IPC_LOCK              | 锁定内存（mlock(2)，mlockall(2)，mmap(2)，shmctl(2)）。                                                        |
| IPC_OWNER             | 绕过 System V IPC 对象的操作权限检查。                                                                          |
| LEASE                 | 在任意文件上建立租约（参见 fcntl(2)）。                                                                          |
| LINUX_IMMUTABLE       | 设置 FS_APPEND_FL 和 FS_IMMUTABLE_FL i-node 标志。                                                              |
| MAC_ADMIN             | 允许 MAC 配置或状态更改。为 Smack LSM 实现。                                                                    |
| MAC_OVERRIDE          | 覆盖强制访问控制（MAC）。为 Smack Linux 安全模块（LSM）实现。                                                  |
| NET_ADMIN             | 执行各种网络相关操作。                                                                                           |
| NET_BROADCAST         | 创建套接字广播并监听多播。                                                                                       |
| PERFMON               | 允许使用 perf_events、i915_perf 和其他内核子系统进行系统性能和可观察性特权操作。                                |
| SYS_ADMIN             | 执行一系列系统管理操作。                                                                                        |
| SYS_BOOT              | 使用 reboot(2) 和 kexec_load(2)，重新启动并加载要稍后执行的新内核。                                             |
| SYS_MODULE            | 加载和卸载内核模块。                                                                                            |
| SYS_NICE              | 提高进程 nice 值（nice(2)，setpriority(2)）并更改任意进程的 nice 值。                                          |
| SYS_PACCT             | 使用 acct(2)，开关进程会计。                                                                                    |
| SYS_PTRACE            | 使用 ptrace(2) 追踪任意进程。                                                                                   |
| SYS_RAWIO             | 执行 I/O 端口操作（iopl(2) 和 ioperm(2)）。                                                                     |
| SYS_RESOURCE          | 覆盖资源限制。                                                                                                  |
| SYS_TIME              | 设置系统时钟（settimeofday(2)，stime(2)，adjtimex(2)）；设置实时（硬件）时钟。                                   |
| SYS_TTY_CONFIG        | 使用 vhangup(2)；对虚拟终端使用各种特权 ioctl(2) 操作。                                                         |
| SYSLOG                | 执行特权 syslog(2) 操作。                                                                                       |
| WAKE_ALARM            | 触发唤醒系统的操作。                                                                                             |

进一步的参考信息可以在[capabilities(7) - Linux man page](https://man7.org/linux/man-pages/man7/capabilities.7.html)和[Linux kernel source code](https://github.com/torvalds/linux/blob/124ea650d3072b005457faed69909221c2905a1f/include/uapi/linux/capability.h)中找到。

这两个标志都支持 `ALL` 值，因此要允许容器使用除 `MKNOD` 之外的所有能力：

```console
$ docker run --cap-add=ALL --cap-drop=MKNOD ...
```

`--cap-add` 和 `--cap-drop` 标志接受带有 `CAP_` 前缀的能力指定。因此，以下示例是等效的：

```console
$ docker run --cap-add=SYS_ADMIN ...
$ docker run --cap-add=CAP_SYS_ADMIN ...
```

为了与网络堆栈交互，而不是使用 `--privileged`，他们应该使用 `--cap-add=NET_ADMIN` 来修改网络接口。

```console
$ docker run -it --rm ubuntu:22.04 ip link add dummy0 type dummy

RTNETLINK answers: Operation not permitted

$ docker run -it --rm --cap-add=NET_ADMIN ubuntu:22.04 ip link add dummy0 type dummy
```

要挂载基于 FUSE 的文件系统，你需要结合使用 `--cap-add` 和 `--device`：

```console
$ docker run --rm -it --cap-add SYS_ADMIN sshfs sshfs sven@10.10.10.20:/home/sven /mnt

fuse: failed to open /dev/fuse: Operation not permitted

$ docker run --rm -it --device /dev/fuse sshfs sshfs sven@10.10.10.20:/home/sven /mnt

fusermount: mount failed: Operation not permitted

$ docker run --rm -it --cap-add SYS_ADMIN --device /dev/fuse sshfs

# sshfs sven@10.10.10.20:/home/sven /mnt
The authenticity of host '10.10.10.20 (10.10.10.20

)' can't be established.
```

最终，你将需要告诉 Docker 为何需要在 `Dockerfile` 中指定所有这些标志，因此最好在 `docker-compose` 文件中指定它们。

### 环境变量

Docker 在创建 Linux 容器时会自动设置一些环境变量。Docker 在创建 Windows 容器时不会设置任何环境变量。

对于 Linux 容器，会设置以下环境变量：

| 变量       | 值                                                                                                    |
|:-----------|:-----------------------------------------------------------------------------------------------------|
| `HOME`     | 根据 `USER` 的值设置                                                                                  |
| `HOSTNAME` | 与容器关联的主机名                                                                                    |
| `PATH`     | 包含常用目录，例如 `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin`                     |
| `TERM`     | 如果容器分配了伪终端，则为 `xterm`                                                                    |

此外，您可以使用一个或多个 `-e` 标志在容器中设置任何环境变量。您甚至可以覆盖上述变量，或使用 Dockerfile 的 `ENV` 指令在构建映像时定义的变量。

如果只命名环境变量而不指定值，则主机上该变量的当前值会传播到容器的环境中：

```console
$ export today=Wednesday
$ docker run -e "deep=purple" -e today --rm alpine env

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=d2219b854598
deep=purple
today=Wednesday
HOME=/root
```

```powershell
PS C:\> docker run --rm -e "foo=bar" microsoft/nanoserver cmd /s /c set
ALLUSERSPROFILE=C:\ProgramData
APPDATA=C:\Users\ContainerAdministrator\AppData\Roaming
CommonProgramFiles=C:\Program Files\Common Files
CommonProgramFiles(x86)=C:\Program Files (x86)\Common Files
CommonProgramW6432=C:\Program Files\Common Files
COMPUTERNAME=C2FAEFCC8253
ComSpec=C:\Windows\system32\cmd.exe
foo=bar
LOCALAPPDATA=C:\Users\ContainerAdministrator\AppData\Local
NUMBER_OF_PROCESSORS=8
OS=Windows_NT
Path=C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Users\ContainerAdministrator\AppData\Local\Microsoft\WindowsApps
PATHEXT=.COM;.EXE;.BAT;.CMD
PROCESSOR_ARCHITECTURE=AMD64
PROCESSOR_IDENTIFIER=Intel64 Family 6 Model 62 Stepping 4, GenuineIntel
PROCESSOR_LEVEL=6
PROCESSOR_REVISION=3e04
ProgramData=C:\ProgramData
ProgramFiles=C:\Program Files
ProgramFiles(x86)=C:\Program Files (x86)
ProgramW6432=C:\Program Files
PROMPT=PPG
PUBLIC=C:\Users\Public
SystemDrive=C:
SystemRoot=C:\Windows
TEMP=C:\Users\ContainerAdministrator\AppData\Local\Temp
TMP=C:\Users\ContainerAdministrator\AppData\Local\Temp
USERDOMAIN=User Manager
USERNAME=ContainerAdministrator
USERPROFILE=C:\Users\ContainerAdministrator
windir=C:\Windows
```

### 健康检查

以下 `docker run` 命令的标志允许您控制容器健康检查的参数：

| 选项                      | 描述                                                                                     |
|:--------------------------|:----------------------------------------------------------------------------------------|
| `--health-cmd`            | 用于检查健康状态的命令                                                                  |
| `--health-interval`       | 两次检查之间的时间间隔                                                                  |
| `--health-retries`        | 报告不健康状态所需的连续失败次数                                                        |
| `--health-timeout`        | 允许单次检查运行的最长时间                                                              |
| `--health-start-period`   | 在开始健康检查倒计时之前，容器初始化的时间                                              |
| `--health-start-interval` | 在启动期间运行检查的时间间隔                                                            |
| `--no-healthcheck`        | 禁用任何容器指定的 `HEALTHCHECK`                                                        |

示例：

```console
$ docker run --name=test -d \
    --health-cmd='stat /etc/passwd || exit 1' \
    --health-interval=2s \
    busybox sleep 1d
$ sleep 2; docker inspect --format='{{.State.Health.Status}}' test
healthy
$ docker exec test rm /etc/passwd
$ sleep 2; docker inspect --format='{{json .State.Health}}' test
{
  "Status": "unhealthy",
  "FailingStreak": 3,
  "Log": [
    {
      "Start": "2016-05-25T17:22:04.635478668Z",
      "End": "2016-05-25T17:22:04.7272552Z",
      "ExitCode": 0,
      "Output": "  File: /etc/passwd\n  Size: 334       \tBlocks: 8          IO Block: 4096   regular file\nDevice: 32h/50d\tInode: 12          Links: 1\nAccess: (0664/-rw-rw-r--)  Uid: (    0/    root)   Gid: (    0/    root)\nAccess: 2015-12-05 22:05:32.000000000\nModify: 2015..."
    },
    {
      "Start": "2016-05-25T17:22:06.732900633Z",
      "End": "2016-05-25T17:22:06.822168935Z",
      "ExitCode": 0,
      "Output": "  File: /etc/passwd\n  Size: 334       \tBlocks: 8          IO Block: 4096   regular file\nDevice: 32h/50d\tInode: 12          Links: 1\nAccess: (0664/-rw-rw-r--)  Uid: (    0/    root)   Gid: (    0/    root)\nAccess: 2015-12-05 22:05:32.000000000\nModify: 2015..."
    },
    {
      "Start": "2016-05-25T17:22:08.823956535Z",
      "End": "2016-05-25T17:22:08.897359124Z",
      "ExitCode": 1,
      "Output": "stat: can't stat '/etc/passwd': No such file or directory\n"
    },
    {
      "Start": "2016-05-25T17:22:10.898802931Z",
      "End": "2016-05-25T17:22:10.969631866Z",
      "ExitCode": 1,
      "Output": "stat: can't stat '/etc/passwd': No such file or directory\n"
    },
    {
      "Start": "2016-05-25T17:22:12.971033523Z",
      "End": "2016-05-25T17:22:13.082015516Z",
      "ExitCode": 1,
      "Output": "stat: can't stat '/etc/passwd': No such file or directory\n"
    }
  ]
}
```

健康状态也会显示在 `docker ps` 输出中。

### 用户

容器内的默认用户是 `root`（uid = 0）。您可以使用 Dockerfile 的 `USER` 指令设置运行第一个进程的默认用户。启动容器时，可以通过传递 `-u` 选项覆盖 `USER` 指令。

```text
-u="", --user="": 设置指定命令使用的用户名或 UID，并可选地设置组名或 GID。
```

以下示例都是有效的：

```text
--user=[ user | user:group | uid | uid:gid | user:gid | uid:group ]
```

> **注意**
>
> 如果传递数字用户 ID，必须在 0-2147483647 的范围内。如果传递用户名，该用户必须存在于容器中。

### 工作目录

在容器中运行二进制文件的默认工作目录是根目录（`/`）。镜像的默认工作目录通过 Dockerfile 的 `WORKDIR` 指令设置。您可以使用 `-w`（或 `--workdir`）标志为 `docker run` 命令覆盖镜像的默认工作目录：

```text
$ docker run --rm -w /my/workdir alpine pwd
/my/workdir
```

如果目录在容器中尚不存在，则会创建。
