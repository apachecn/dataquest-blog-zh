# 如何在 Ubuntu 上安装和配置 Docker Swarm

> 原文：<https://www.dataquest.io/blog/install-and-configure-docker-swarm-on-ubuntu/>

February 15, 2018Docker Swarm is a clustering tool that turns a group of Docker hosts into a single virtual server. Docker Swarm ensures availability and high performance for your application by distributing it over the number of Docker hosts inside a cluster. Docker Swarm also allows you to increase the number of container instance for the same application. Clustering is an important feature of container technology for redundancy and high availability. You can manage and control clusters through a swarm manager. The swarm manager allows you to create a primary manager instance and multiple replica instances in case the primary instance fails. Docker Swarm exposes standard Docker API, meaning that any tool that you used to communicate with Docker (Docker CLI, Docker Compose, Krane, and Dokku) can work equally well with Docker Swarm. Features of Swarm Mode:

*   提供 Docker 引擎 CLI 来创建一群 Docker 引擎，您可以在其中部署应用程序服务。你不需要任何额外的软件工具来创建或管理一个群体。
*   群管理器为群中的每个服务分配一个唯一的 DNS 名称。因此，您可以通过 DNS 名称轻松查询 swarm 中运行的每个容器。
*   群中的每个节点强制执行 TLS 相互认证和加密，以确保自身和所有其他节点之间的通信安全。您也可以使用自签名根证书或来自自定义根 CA 的证书。
*   允许您以增量方式将服务更新应用到节点。
*   允许您为服务指定覆盖网络，以及如何在节点之间分发服务容器。

在本帖中，我们将介绍如何在 Ubuntu 16.04 服务器上安装和配置 Docker Swarm 模式。我们将:

*   安装一个服务发现工具，并在所有节点上运行 swarm 容器。
*   安装 Docker 并配置 swarm manager。
*   将所有节点添加到 Manager 节点(下一节将详细介绍节点)。

为了充分利用这篇文章，你应该:

*   Ubuntu 和 Docker 的基础知识。
*   安装了 ubuntu 16.04 的两个节点。
*   在两个节点上都设置了具有 sudo 权限的非 root 用户。
*   在管理节点和工作节点上配置的静态 IP 地址。这里，我们将对管理器节点使用 IP 192.168.0.103，对工作者节点使用 IP 192.168.0.104。

我们先来看节点。

## 管理者节点和工作者节点

Docker Swarm 由两个主要组件组成:

*   管理器节点
*   工作节点

**管理器节点**管理器节点用于处理集群管理任务，例如维护集群状态、调度服务和服务群模式 HTTP API 端点。群体模式下 Docker 最重要的特性之一就是管理者定额。manager quorum 存储有关集群的信息，信息的一致性通过 Raft 共识算法达成共识。如果任何一个管理节点意外死亡，另一个管理节点可以接管任务并将服务恢复到稳定状态。Raft 最多允许(N-1)/2 次失败，并且需要(N/2)+1 个成员的多数或法定人数来同意向集群提议的值。这意味着在运行 Raft 的 5 个管理器的集群中，如果 3 个节点不可用，系统将无法处理更多的请求来调度额外的任务。现有任务继续运行，但如果管理器集不健康，调度程序无法重新平衡任务来应对故障。**工人节点**工人节点用于执行容器。工作节点不参与 Raft 分布式状态，不做调度决策。您可以创建一个由一个管理器节点组成的群组，但是您不能拥有一个没有至少一个管理器节点的工作器节点。当管理器节点脱机进行维护时，也可以将工作节点升级为管理器。正如引言中提到的，我们在本文中使用了两个节点——一个作为管理者节点，另一个作为工作者节点。

## 入门指南

在开始之前，您应该用最新版本更新您的系统存储库。您可以使用以下命令更新它:

更新存储库后，重启系统以应用所有更新。

### 安装 Docker

您还需要在两个节点上安装 Docker 引擎。默认情况下，Docker 在 Ubuntu 16.04 资源库中不可用。因此，您需要首先建立一个 Docker 存储库。您可以使用以下命令安装所需的软件包:

`sudo apt-get install apt-transport-https software-properties-common ca-certificates -y`接下来，为 Docker 添加 GPG 键:`wget https://download.docker.com/linux/ubuntu/gpg && sudo apt-key add gpg`然后，添加 Docker 存储库并更新包缓存:`sudo echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable" >> /etc/apt/sources.list` `sudo apt-get update -y`最后，使用以下命令安装 Docker 引擎:`sudo apt-get install docker-ce -y`一旦安装了 Docker，启动 Docker 服务并使其在引导时启动:`sudo systemctl start docker && sudo systemctl enable docker`默认情况下，Docker 守护进程总是以 root 用户身份运行，其他用户只能使用 sudo 访问它。如果想在不使用 sudo 的情况下运行`docker`命令，那么创建一个名为 docker 的 Unix 组，并向其中添加用户。您可以通过运行下面的命令来做到这一点:`sudo groupadd docker && sudo usermod -aG docker dockeruser`接下来，注销并使用 dockeruser 重新登录到您的系统，以便重新评估您的组成员资格。注意:记住在两个节点上运行上述命令。

### 配置防火墙

您需要为一个群集群配置防火墙规则，以便在两个节点上正常工作。使用 UFW 防火墙和以下命令允许端口 7946、4789、2376、2376、2377 和 80:

`sudo ufw allow 2376/tcp && sudo ufw allow 7946/udp && sudo ufw allow 7946/tcp && sudo ufw allow 80/tcp && sudo ufw allow 2377/tcp && sudo ufw allow 4789/udp`接下来，重新加载 UFW 防火墙并使其在引导时启动:`sudo ufw reload && sudo ufw enable`重新启动 Docker 服务以影响 Docker 规则:`sudo systemctl restart docker`

## 创建 Docker 群集群

首先，您需要用 IP 地址初始化集群，以便您的节点充当管理节点。在 Manager 节点上，运行以下命令来通告 IP 地址:

您应该会看到以下输出:

```
 Swarm initialized: current node (iwjtf6u951g7rpx6ugkty3ksa) is now a manager.

To add a worker to this swarm, run the following command:
    docker swarm join --token SWMTKN-1-5p5f6p6tv1cmjzq9ntx3zmck9kpgt355qq0uaqoj2ple629dl4-5880qso8jio78djpx5mzbqcfu 192.168.0.103:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

上面输出中显示的令牌将用于在下一步中向集群添加工作节点。Docker 引擎根据您提供给`docker swarm join`命令的 join-token 加入集群。节点仅在加入时使用令牌。如果您随后轮换令牌，它不会影响现有的群节点。现在，使用以下命令检查 Manager 节点的状态:`docker node ls`如果一切正常，您应该会看到以下输出:

```
 ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
iwjtf6u951g7rpx6ugkty3ksa *   Manager-Node        Ready               Active              Leader
```

你也可以检查 Docker Swarm 集群的状态:code>docker info

您应该会看到以下输出:

```
 Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 17.09.0-ce
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: bridge host macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file logentries splunk syslog
Swarm: active
 NodeID: iwjtf6u951g7rpx6ugkty3ksa
 Is Manager: true
 ClusterID: fo24c1dvp7ent771rhrjhplnu
 Managers: 1
 Nodes: 1
 Orchestration:
  Task History Retention Limit: 5
 Raft:
  Snapshot Interval: 10000
  Number of Old Snapshots to Retain: 0
  Heartbeat Tick: 1
  Election Tick: 3
 Dispatcher:
  Heartbeat Period: 5 seconds
 CA Configuration:
  Expiry Duration: 3 months
  Force Rotate: 0
 Autolock Managers: false
 Root Rotation In Progress: false
 Node Address: 192.168.0.103
 Manager Addresses:
  192.168.0.103:2377
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: 06b9cb35161009dcb7123345749fef02f7cea8e0
runc version: 3f2f8b84a77f73d38244dd690525642a72156c64
init version: 949e6fa
Security Options:
 apparmor
 seccomp
  Profile: default
Kernel Version: 4.4.0-45-generic
Operating System: Ubuntu 16.04.1 LTS
OSType: linux
Architecture: x86_64
CPUs: 1
Total Memory: 992.5MiB
Name: Manager-Node
ID: R5H4:JL3F:OXVI:NLNY:76MV:5FJU:XMVM:SCJG:VIL5:ISG4:YSDZ:KUV4
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false 
```

## 将工作节点添加到群集群

管理器节点现在已经正确配置，是时候将工作者节点添加到群集群中了。首先，复制上一步中“swarm init”命令的输出，然后将该输出粘贴到 Worker 节点上以加入 swarm 集群:

`docker swarm join --token SWMTKN-1-5p5f6p6tv1cmjzq9ntx3zmck9kpgt355qq0uaqoj2ple629dl4-5880qso8jio78djpx5mzbqcfu 192.168.0.103:2377`

您应该会看到以下输出:

```
This node joined a swarm as a worker.
```

现在，在 Manager 节点上，运行以下命令来列出 Worker 节点:`docker node ls`

您应该在以下输出中看到 Worker 节点:

```
 ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
iwjtf6u951g7rpx6ugkty3ksa *   Manager-Node        Ready               Active              Leader
snrfyhi8pcleagnbs08g6nnmp     Worker-Node         Ready               Active 
```

## 在 Docker Swarm 中启动 web 服务

Docker Swarm 集群现在已经启动并运行，是时候在 Docker Swarm 模式中启动 web 服务了。在 Manager 节点上，运行以下命令来部署 web 服务器服务:`docker service create --name webserver -p 80:80 httpd`

上面的命令将创建一个 Apache web 服务器容器，并将其映射到端口 80，这样您就可以从远程系统访问 Apache web 服务器。现在，您可以使用下面的命令检查正在运行的服务:`docker service ls`您应该会看到下面的输出:

```
 ID                  NAME                
MODE                REPLICAS            IMAGE               PORTS
nnt7i1lipo0h        webserver           replicated          0/1                 apache:latest       *:80->80/tcp
```

接下来，使用下面的命令跨两个容器扩展 web 服务器服务:`docker service scale webserver=2`

然后，使用以下命令检查 web 服务器服务的状态:`docker service ps webserver`您应该会看到以下输出:

```
 ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                  ERROR               PORTS
7roily9zpjvq        webserver.1         httpd:latest        Worker-Node         Running             Preparing about a minute ago                       
r7nzo325cu73        webserver.2         httpd:latest        Manager-Node        Running             Preparing 58 seconds ago 
```

## 测试码头工人群

Apache web 服务器现在运行在管理器节点上。现在，您可以通过将 web 浏览器指向管理器节点 IP 来访问 web 服务器

或者工作者节点 IP 如下所示:![Screenshot-of-docker-swarm-apache](img/0f9aa45e582d546edfec5fddd6eed6c1.png) Apache web 服务器服务现在分布在两个节点上。Docker Swarm 还为您的服务提供高可用性。如果 web 服务器在 Worker 节点上关闭，那么新的容器将在 Manager 节点上启动。要测试高可用性，只需停止 Worker 节点上的 Docker 服务:`sudo systemctl stop docker`在 Manager 节点上，使用以下命令运行 web 服务器服务 status:`docker service ps webserver`您应该看到 Manager 节点上启动了一个新的容器:

```
 ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
ia2qc8a5f5n4        webserver.1         httpd:latest        Manager-Node        Ready               Ready 1 second ago                           
7roily9zpjvq         \_ webserver.1     httpd:latest        Worker-Node         Shutdown            Running 15 seconds ago                       r7nzo325cu73        webserver.2         httpd:latest        Manager-Node        Running             Running 23 minutes ago 
```

## 结论

恭喜你！您已经在 Ubuntu 16.04 上成功安装并配置了 Docker Swarm 集群。现在，您可以轻松地将应用程序扩展到一千个节点和五万个容器，而不会降低性能。现在你已经有了一个基本的集群设置，去看看 Docker Swarm 文档了解更多关于 Swarm 的信息。您可能希望根据组织的高可用性要求，为您的集群配置一个以上的管理器节点。