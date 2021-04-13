











# 概述

> 与虚拟机的区别

之前的虚拟机技术

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTE1Mzg1Mjk1NC5wbmc?x-oss-process=image/format,png)

1、 资源占用十分多

2、 冗余步骤多

3、 启动很慢！



容器化技术，不是模拟一个完整的操作系统

![image-20200515094336846](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTA5NDMzNjg0Ni5wbmc?x-oss-process=image/format,png)

比较Docker和虚拟机技术的不同：

- 传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
- 容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟我们的硬件，所以就轻便了
- 每个容器间是互相隔离，每个容器内都有一个属于自己的文件系统，互不影响



> 优点



**应用更快速的交付和部署**

传统：一对帮助文档，安装程序。

Docker：打包镜像发布测试一键运行。

**更便捷的升级和扩缩容**

使用了 Docker之后，我们部署应用就和搭积木一样，项目打包为一个镜像，扩展服务器A！服务器B

**更简单的系统运维**
在容器化之后，我们的开发，测试环境都是高度一致的

**更高效的计算资源利用**

Docker是内核级别的虚拟化，可以在一个物理机上可以运行很多的容器实例！服务器的性能可以被压榨到极致。



# 基础篇

## 安装

### 基本组成

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNDE5NTgwNTQwMC5wbmc?x-oss-process=image/format,png)

**镜像（image)：**

docker镜像就好比是一个目标，可以通过这个目标来创建容器服务，tomcat镜像==>run==>容器（提供服务器），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）。

**容器(container)：**

Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建的.
启动，停止，删除，基本命令
目前就可以把这个容器理解为就是一个简易的 Linux系统。

**仓库(repository)：**

仓库就是存放镜像的地方！
仓库分为公有仓库和私有仓库。(很类似git)
Docker Hub是国外的。
阿里云…都有容器服务器(配置镜像加速!)



### Docker安装、卸载

1. 连接服务器（我这里是阿里云服务器）
2. 前置工作，查看系统环境和系统版本

```bash
#服务器环境查看
uname -r
#系统版本
cat /etc/os-release
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210112103330526.png" alt="image-20210112103330526" style="zoom:50%;" />

可以看到，系统环境在3.10以上，符合内核3.0以上的要求，系统版本CentOS 7，也符合标准

3. 安装

```shell
#1. 卸载旧的版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
 #2.需要的安装包
yum install -y yum-utils

#3.设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
#上述方法默认是从国外的，不推荐

#推荐使用国内的
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    
#更新yum软件包索引
yum makecache fast

#4.安装docker相关的 docker-ce 社区版 而ee是企业版
yum install docker-ce docker-ce-cli containerd.io # 这里我们使用社区版即可

#5.启动docker
systemctl start docker

#6. 使用docker version查看是否按照成功
docker version
```

不知道为啥，我这边执行了命令1 ，但是还是可以直接使用命令6 ，之前应该是通过宝塔面板安装的，算了这个先不管，继续往下做

```shell
#7.查看已经下载的镜像(从这里可以查看已有镜像的id)
 docker images
```

![image-20210112111308251](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210112111308251.png)

这里查看时，只有这几个镜像，没有hello-world

于是在运行第八步时，出现了报错Unable to find image 'hello-world:latest' locally

```shell
#8.测试
docker run hello-world
```

出现报错之后，docker自动进行了更新如下

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210112111522078.png" alt="image-20210112111522078" style="zoom:50%;" />

此时我再次运行命令7和8，结果如下：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210112111615937.png" alt="image-20210112111615937" style="zoom:50%;" />



在这个过程中，Docker的run流程是这样的

![image-20200515102637246](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTEwMjYzNzI0Ni5wbmc?x-oss-process=image/format,png)

4. 卸载docker

```shell
#1. 卸载依赖
yum remove docker-ce docker-ce-cli containerd.io
#2. 删除资源
rm -rf /var/lib/docker
# /var/lib/docker 是docker的默认工作路径！
```

### Docker的底层原理

Docker**是怎么工作的**？

Docker是一个Client-Server结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问！

Docker-Server接收到Docker-Client的指令，就会执行这个命令！

![image-20200515102949558](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTEwMjk0OTU1OC5wbmc?x-oss-process=image/format,png)

**为什么Docker比Vim快**
1、docker有着比虚拟机更少的抽象层。由于docker不需要Hypervisor实现硬件资源虚拟化,运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。
2、docker利用的是宿主机的内核,而不需要Guest OS。

```
GuestOS： VM（虚拟机）里的的系统（OS）

HostOS：物理机里的系统（OS）
123
```

![image-20200515104117329](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTEwNDExNzMyOS5wbmc?x-oss-process=image/format,png)

因此,当新建一个 容器时,docker不需要和虚拟机一样重新加载一个操作系统内核。仍而避免引导、加载操作系统内核返个比较费时费资源的过程,当新建一个虚拟机时,虚拟机软件需要加载GuestOS,返个新建过程是**分钟级别**的。而docker由于直接利用宿主机的操作系统,则省略了这个复杂的过程,因此新建一个docker容器只需要**几秒钟**。

### 阿里云镜像加速

1. 登录阿里云服务器，在产品与服务的弹性计算中找到容器镜像服务，开启之后设置密码即可

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210112141429382.png" alt="image-20210112141429382" style="zoom:33%;" />

2. 在镜像中心找到镜像加速器，选择操作文档中的CentOS

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210112141702442.png" alt="image-20210112141702442" style="zoom:33%;" />

3. 配置使用,使用SSH工具



```shell
#1.创建一个目录
sudo mkdir -p /etc/docker

#2.编写配置文件
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://qubxnlmi.mirror.aliyuncs.com"]
}
EOF

#3.重启服务
sudo systemctl daemon-reload
sudo systemctl restart docker

```

第二步可能官网会有更新，以官网为准（反正我看了几个版本，每个版本都不一样）



## Docker的常用命令

### 1.帮助命令

```shell
docker version    #显示docker的版本信息。
docker info       #显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help #帮助命令
```

帮助文档的地址：https://docs.docker.com/engine/reference/commandline/build/

### 2.镜像命令

```shell
docker images #查看所有本地主机上的镜像 可以使用docker image ls代替

docker search #搜索镜像

docker pull 镜像名[:tag] #下载镜像，不写tag默认是最新版 docker pull  tomcat:8  

docker rmi #删除镜像 docker image rm
```

使用示例

```shell
[root@Shashar-study /]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              bf756fb1ae65        12 months ago       13.3kB
# 解释
#REPOSITORY			# 镜像的仓库源
#TAG				# 镜像的标签(版本)		---lastest 表示最新版本
#IMAGE ID			# 镜像的id
#CREATED			# 镜像的创建时间
#SIZE				# 镜像的大小

[root@Shashar-study /]# docker images --help
Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]
List images
Options:
  -a, --all             #列出所有镜像 
  -q, --quiet          # 只显示镜像的id
  


# --filter=STARS=3000 #过滤，搜索出来的镜像收藏STARS数量大于3000的
Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
      
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker search mysql --filter=STARS=3000
NAME        DESCRIPTION         STARS            OFFICIAL        AUTOMATED
mysql       MySQL IS ...        9520             [OK]                
mariadb     MariaDB IS ...      3456             [OK]   

[root@Shashar-study /]# docker pull mysql
Using default tag: latest #如果不屑tag,默认是最新版
latest: Pulling from library/mysql
6ec7b7d162b2: Pull complete #分层下载，docker image 的核心，联合文件系统
fedd960d3481: Pull complete 
7ab947313861: Pull complete 
64f92f19e638: Pull complete 
3e80b17bff96: Pull complete 
014e976799f9: Pull complete 
59ae84fee1b3: Pull complete 
ffe10de703ea: Pull complete 
657af6d90c83: Pull complete 
98bfb480322c: Pull complete 
6aa3859c4789: Pull complete 
1ed875d851ef: Pull complete 
Digest: sha256:78800e6d3f1b230e35275145e657b82c3fb02a27b2d8e76aac2f5e90c1c30873
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest  #真实地址


docker rmi -f 镜像id #删除指定id的镜像
docker rmi -f $(docker images -aq) #删除全部的镜像
```



### 3.容器命令

说明：我们有了镜像才可以创建容器，Linux，下载centos镜像来学习

##### 镜像下载

```shell
#docker中下载centos
docker pull centos


docker run 镜像id #新建容器并启动

docker ps 列出所有运行的容器 docker container list

docker rm 容器id #删除指定容器

docker start 容器id	#启动容器
docker restart 容器id	#重启容器
docker stop 容器id	#停止当前正在运行的容器
docker kill 容器id	#强制停止当前容器
```

##### 新建容器并启动

```shell
docker run [可选参数] image | docker container run [可选参数] image 
#参书说明
--name="Name"		#容器名字 tomcat01 tomcat02 用来区分容器
-d					#后台方式运行
-it 				#使用交互方式运行，进入容器查看内容
-p					#指定容器的端口 -p 8080(宿主机):8080(容器)
		-p ip:主机端口:容器端口
		-p 主机端口:容器端口(常用)
		-p 容器端口
		容器端口
-P(大写) 				随机指定端口
```

示例(完整的一套流程)

```bash
[root@Shashar-study /]# docker pull centos
Using default tag: latest
latest: Pulling from library/centos
7a0437f04f83: Pull complete 
Digest: sha256:5528e8b1b1719d34604c87e11dcd1c0a20bedf46e83b5632cdeac91b8c04efc1
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
[root@Shashar-study /]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mysql               latest              a347a5928046        3 weeks ago         545MB
centos              latest              300e315adb2f        5 weeks ago         209MB
[root@Shashar-study /]# docker run -it centos /bin/bash   #启动并进入容器内部
[root@504f17a67d8e /]# ls   #容器内部目录，同外部类似，因为是pull下载的小型centos
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@504f17a67d8e /]# exit   #从容器退回主机
exit
[root@Shashar-study /]# ls
bin  boot  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  patch  proc  root  run  sbin  srv  sys  tmp  usr  var  www
```

##### 列出所有运行的容器

```shell
docker ps 命令  		#列出当前正在运行的容器
  -a, --all     	 #列出当前正在运行的容器 + 带出历史运行过的容器
  -n=?, --last int   #列出最近创建的?个容器 ?为1则只列出最近创建的一个容器,为2则列出2个
  -q, --quiet        #只列出容器的编号
```

##### 退出容器

```shell
exit 		#容器直接退出
ctrl +P +Q  #容器不停止退出 ，想要再次进入使用命令docker attach 容器ID	---注意：这个很有用的操作
```

##### 删除容器

```shell
docker rm 容器id   				#删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm -rf
docker rm -f $(docker ps -aq)  	 #删除所有的容器
docker ps -a -q|xargs docker rm  #删除所有的容器
```

##### 启动和停止容器的操作

```shell
docker start 容器id	#启动容器
docker restart 容器id	#重启容器
docker stop 容器id	#停止当前正在运行的容器
docker kill 容器id	#强制停止当前容器
```

### 4.常用其他命令

##### 后台启动命令

```shell
# 命令 docker run -d 镜像名，后台运行
[root@Shashar-study /]# docker run -d centos
568ba4e855df7088bf2d62aa2160c8e97f0b8e4b8d499987c5bf99bad7bdb687
[root@Shashar-study /]# docker ps
CONTAINER ID    IMAGE     COMMAND  CREATED    STATUS     PORTS      NAMES

# 问题docker ps. 发现centos 停止了
# 常见的坑，docker容器使用后台运行，就必须要有要一个前台进程，docker发现没有应用，就会自动停止
# nginx，容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了
```

##### 查看日志

```shell
docker logs --help
Options:
      --details        Show extra details provided to logs 
*  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
*      --tail string    Number of lines to show from the end of the logs (default "all")
*  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
➜  ~ docker run -d centos /bin/sh -c "while true;do echo 6666;sleep 1;done" #模拟日志      
#显示日志
-tf		#显示日志信息（一直更新）
--tail number #需要显示日志条数
docker logs -t --tail n 容器id #查看n行日志
docker logs -ft 容器id #跟着日志
```

##### 查看容器中进程信息ps

```shell
# 命令 docker top 容器id
```

![image-20210113114931623](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210113114931623.png)



##### 查看镜像的元数据

```shell
# 命令
docker inspect 容器id
```

示例

```bash
[root@Shashar-study ~]# docker inspect b68d4872d19d
[
    {
        "Id": "b68d4872d19daa12f92e85c41da9d6ea6ec58eb213867b013819b3cfb4a1667a",
        "Created": "2021-01-13T03:48:20.416341627Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 670,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-01-13T03:48:20.8668921Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
        "ResolvConfPath": "/var/lib/docker/containers/b68d4872d19daa12f92e85c41da9d6ea6ec58eb213867b013819b3cfb4a1667a/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/b68d4872d19daa12f92e85c41da9d6ea6ec58eb213867b013819b3cfb4a1667a/hostname",
        "HostsPath": "/var/lib/docker/containers/b68d4872d19daa12f92e85c41da9d6ea6ec58eb213867b013819b3cfb4a1667a/hosts",
        "LogPath": "/var/lib/docker/containers/b68d4872d19daa12f92e85c41da9d6ea6ec58eb213867b013819b3cfb4a1667a/b68d4872d19daa12f92e85c41da9d6ea6ec58eb213867b013819b3cfb4a1667a-json.log",
        "Name": "/laughing_brattain",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/922283df5cb62067a53ee673d7ded8910191efd66ed5a7eab551b8e373ccd630-init/diff:/var/lib/docker/overlay2/8d6c7abda373c3589642ff2bdeeee8a6d6df5833340443262c0628699fd9ca21/diff",
                "MergedDir": "/var/lib/docker/overlay2/922283df5cb62067a53ee673d7ded8910191efd66ed5a7eab551b8e373ccd630/merged",
                "UpperDir": "/var/lib/docker/overlay2/922283df5cb62067a53ee673d7ded8910191efd66ed5a7eab551b8e373ccd630/diff",
                "WorkDir": "/var/lib/docker/overlay2/922283df5cb62067a53ee673d7ded8910191efd66ed5a7eab551b8e373ccd630/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "b68d4872d19d",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20201204",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "a371522731995bbe3ba8d2817e2333b7a3e57ef6565121cc0ed7e15cc59f9103",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/a37152273199",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "45b35431a6776ea8356635f3bfaf0156e4f27c56cda21f2a9f8e62388fb5c63c",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "87914e3a50ef81819030c67e1d37e40390ee6a828fda29e60e669b6befa81e82",
                    "EndpointID": "45b35431a6776ea8356635f3bfaf0156e4f27c56cda21f2a9f8e62388fb5c63c",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

##### 进入正在运行的容器

我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

```shell
#方式一
docker exec -it 容器id bashshell
# 方式二
docker attach 容器id

#docker exec #进入当前容器后开启一个新的终端，可以在里面操作。（常用）
#docker attach # 进入容器正在执行的终端
```



![image-20210113151305470](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210113151305470.png)



##### 从容器拷贝文件到主机上

这个容器可以是正在运行的，也可以是停止运行的，只要能在  docker ps -a 中能查询到容器ID即可

```shell
 docker cp 容器ID:容器内地址  需要粘贴的主机地址
```

完整示例如图：

```shell
#查看操作前主机目录
[root@Shashar-study ~]# cd /home
[root@Shashar-study home]# ls
redis  shasha  www

#进入容器
[root@Shashar-study home]# docker attach 906992b2485b
[root@906992b2485b /]# cd /home
[root@906992b2485b home]# ls

#在容器内新建测试文件
[root@906992b2485b home]# touch test.java
[root@906992b2485b home]# ls
test.java

#退出容器
[root@906992b2485b home]# exit
exit
[root@Shashar-study home]# docker ps
CONTAINER ID   IMAGE  COMMAND    CREATED    STATUS   PORTS   NAMES
[root@Shashar-study home]# docker ps -a
CONTAINER ID    IMAGE    COMMAND      CREATED       STATUS      PORTS                               NAMES
906992b2485b   centos   "/bin/bash"  3 minutes ago  Exited (0)  8 seconds ago                       pensive_goldberg

#复制文件到主机
[root@Shashar-study home]# docker cp 906992b2485b:/home/test.java  /home
[root@Shashar-study home]# ls
redis  shasha  test.java  www
```

### 命令小结

![image-20200514214313962](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNDIxNDMxMzk2Mi5wbmc?x-oss-process=image/format,png)
**命令大全**

```shell
  attach      Attach local standard input, output, and error streams to a running container
  #当前shell下 attach连接指定运行的镜像
  build       Build an image from a Dockerfile # 通过Dockerfile定制镜像
  commit      Create a new image from a container's changes #提交当前容器为新的镜像
  cp          Copy files/folders between a container and the local filesystem #拷贝文件
  create      Create a new container #创建一个新的容器
  diff        Inspect changes to files or directories on a container's filesystem #查看docker容器的变化
  events      Get real time events from the server # 从服务获取容器实时时间
  exec        Run a command in a running container # 在运行中的容器上运行命令
  export      Export a container's filesystem as a tar archive #导出容器文件系统作为一个tar归档文件[对应import]
  history     Show the history of an image # 展示一个镜像形成历史
  images      List images #列出系统当前的镜像
  import      Import the contents from a tarball to create a filesystem image #从tar包中导入内容创建一个文件系统镜像
  info        Display system-wide information # 显示全系统信息
  inspect     Return low-level information on Docker objects #查看容器详细信息
  kill        Kill one or more running containers # kill指定docker容器
  load        Load an image from a tar archive or STDIN #从一个tar包或标准输入中加载一个镜像[对应save]
  login       Log in to a Docker registry #
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```

## Docker部署

### Nginx部署

```shell
#1.搜索镜像
docker search nginx

#2.下载镜像
docker pull nginx

#3.查看已有镜像
docker images

#4. 运行测试
# -d 后台运行
# --name 给容器命名
# -p 宿主机端口：容器内部端口
docker run -d --name nginx01 -p 3344:80 nginx

#5. 查看正在启动的镜像
docker ps

#6. 进入容器
docker exec -it nginx01 /bin/bash #进入
whereis nginx	#找到nginx位置

#7. 退出容器
exit

#8.停止容器
docker stop 容器ID
```

在第4步之后，就可以访问nginx了，我用的是阿里云，必须先去阿里云安全组开放端口号3344（3344是上面自己配置的，可以改），然后通过地址    http://IP地址:3344   来访问，界面如下：

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210113165311631.png" alt="image-20210113165311631" style="zoom:67%;" />

在第八步之后，就不可以通过地址访问Nginx界面啦



### Tomcat 部署

```shell
# 下载 tomcat9.0
# 之前的启动都是后台，停止了容器，容器还是可以查到，docker ps -a可查
#docker run -it --rm 镜像名 一般是用来测试，用完就删除，docker ps -a不可查
docker run -it --rm tomcat:9.0
```

以上测试了命令docker run -it --rm 镜像名，接下来开始正常走流程

```shell
#1.下载镜像
docker pull tomcat
#2.启动镜像 
docker run -d -p 3344:8080 --name tomcat02  tomcat  #这里偷懒，不想多设置安全组，直接用了3344，这个在上文我已经配置过的端口号
```

启动之后，访问http://主机号:3344，出现404页面，这是因为webapps里没有内容

以及，Linux命令少了很多 ，这是 由于阿里云镜像的原因，阿里云默认是最小的镜像，所以不必要的都剔除掉，以保证最小的可运行环境。

```shell
#进入容器
[root@Shashar-study ~]#  docker exec -it tomcat02 /bin/bash

#查看容器内部目录，同之前配置的Tomcat
root@078bc0453d20:/usr/local/tomcat# ls
BUILDING.txt     LICENSE  README.md      RUNNING.txt  conf  logs            temp     webapps.dist
CONTRIBUTING.md  NOTICE   RELEASE-NOTES  bin          lib   native-jni-lib  webapps  work

#进入webapps并查看，可以发现，目录为空
root@078bc0453d20:/usr/local/tomcat# cd webapps
root@078bc0453d20:/usr/local/tomcat/webapps# ls
root@078bc0453d20:/usr/local/tomcat/webapps# 
```

*解决方案：* 将webapps.dist下的文件都拷贝到webapps下即可

```shell
root@078bc0453d20:/usr/local/tomcat# cp -r webapps.dist/* webapps
```

此时再刷新http://主机号:3344，就可以访问到Tomcat页面了

### elasticsearch+kibana部署

```shell
# es 暴露的端口很多！
# es 十分耗内存
# es 的数据一般需要放置到安全目录！挂载
# --net somenetwork ? 网络配置

# 1.  启动elasticsearch  
docker run -d --name elasticsearch  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.10.1
```

第一步就卡死了，服务器也掉线了，我…………

坚强的打工人无所畏惧，重启docker stop，然后重新启动容器

```shell
#可以添加内存的限制，修改配置文件 -e 环境配置修改,重新取个名
docker run -d --name elasticsearchmin -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2

#查看CPU状态
docker stats
```

![image-20210114164246206](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210114164246206.png)

```shell
# 测试一下es是否成功启动
curl localhost:9200
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210114164401038.png" alt="image-20210114164401038" style="zoom:50%;" />





## Docker可视化管理工具Portainer

- portainer(先用这个)

```shell
docker run -d -p 8080:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
12
```

- Rancher(CI/CD再用)
  **什么是portainer？**

Docker图形化界面管理工具！提供一个后台面板供我们操作！

```shell
#开启命令
docker run -d -p 3344:9000 > --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

因为懒，所以还是用了3344 这个端口号哈哈哈 

访问地址http://主机IP:3344即可进入面板

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210115142026762.png" alt="image-20210115142026762" style="zoom:50%;" />

选择本地的

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210115142246435.png" alt="image-20210115142246435" style="zoom:33%;" />

进入之后的面板

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210115142418673.png" alt="image-20210115142418673" style="zoom:33%;" />





## 镜像原理之联合文件系统

镜像是一种轻量级、可执行的独立软件保，用来打包软件运行环境和基于运行环境开发的软件，他包含运行某个软件所需的所有内容，包括代码、运行时库、环境变量和配置文件。

所有应用，直接打包docker镜像，就可以直接跑起来！

**如何得到镜像**

- 从远程仓库下载
- 别人拷贝给你
- 自己制作一个镜像 DockerFile



> UnionFs （联合文件系统）

UnionFs（联合文件系统）：Union文件系统（UnionFs）是一种分层、轻量级并且高性能的文件系统，他支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下（ unite several directories into a single virtual filesystem)。Union文件系统是 Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像
特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

> Docker镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。
boots(boot file system）主要包含 bootloader和 Kernel, bootloader主要是引导加 kernel, Linux刚启动时会加bootfs文件系统，在 Docker镜像的最底层是 boots。这一层与我们典型的Linux/Unix系统是一样的，包含boot加載器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由 bootfs转交给内核，此时系统也会卸载bootfs。
rootfs（root file system),在 bootfs之上。包含的就是典型 Linux系统中的/dev,/proc,/bin,/etc等标准目录和文件。 rootfs就是各种不同的操作系统发行版，比如 Ubuntu, Centos等等。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTE2MzA0OTk1OS5wbmc?x-oss-process=image/format,png)

平时我们安装进虚拟机的CentOS都是好几个G，为什么Docker这里才200M？

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTE2MzE0MDU1OS5wbmc?x-oss-process=image/format,png)

对于个精简的OS,rootfs可以很小，只需要包合最基本的命令，工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供rootfs就可以了。由此可见对于不同的Linux发行版， boots基本是一致的， rootfs会有差別，因此不同的发行版可以公用bootfs.

虚拟机是分钟级别，容器是秒级！

**分层理解**

> 分层的镜像

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层层的在下载

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTE2MzgzOTE4MC5wbmc?x-oss-process=image/format,png)

**思考：为什么Docker镜像要采用这种分层的结构呢？**

最大的好处，我觉得莫过于资源共享了！比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享。

查看镜像分层的方式可以通过docker image inspect 命令

```shell
➜  / docker image inspect redis          
[
    {
        "Id": "sha256:f9b9909726890b00d2098081642edf32e5211b7ab53563929a47f250bcdc1d7c",
        "RepoTags": [
            "redis:latest"
        ],
        "RepoDigests": [
            "redis@sha256:399a9b17b8522e24fbe2fd3b42474d4bb668d3994153c4b5d38c3dafd5903e32"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2020-05-02T01:40:19.112130797Z",
        "Container": "d30c0bcea88561bc5139821227d2199bb027eeba9083f90c701891b4affce3bc",
        "ContainerConfig": {
            "Hostname": "d30c0bcea885",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "REDIS_VERSION=6.0.1",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-6.0.1.tar.gz",
                "REDIS_DOWNLOAD_SHA=b8756e430479edc162ba9c44dc89ac394316cd482f2dc6b91bcd5fe12593f273"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"redis-server\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:704c602fa36f41a6d2d08e49bd2319ccd6915418f545c838416318b3c29811e0",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "18.09.7",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "REDIS_VERSION=6.0.1",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-6.0.1.tar.gz",
                "REDIS_DOWNLOAD_SHA=b8756e430479edc162ba9c44dc89ac394316cd482f2dc6b91bcd5fe12593f273"
            ],
            "Cmd": [
                "redis-server"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:704c602fa36f41a6d2d08e49bd2319ccd6915418f545c838416318b3c29811e0",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 104101893,
        "VirtualSize": 104101893,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/adea96bbe6518657dc2d4c6331a807eea70567144abda686588ef6c3bb0d778a/diff:/var/lib/docker/overlay2/66abd822d34dc6446e6bebe73721dfd1dc497c2c8063c43ffb8cf8140e2caeb6/diff:/var/lib/docker/overlay2/d19d24fb6a24801c5fa639c1d979d19f3f17196b3c6dde96d3b69cd2ad07ba8a/diff:/var/lib/docker/overlay2/a1e95aae5e09ca6df4f71b542c86c677b884f5280c1d3e3a1111b13644b221f9/diff:/var/lib/docker/overlay2/cd90f7a9cd0227c1db29ea992e889e4e6af057d9ab2835dd18a67a019c18bab4/diff",
                "MergedDir": "/var/lib/docker/overlay2/afa1de233453b60686a3847854624ef191d7bc317fb01e015b4f06671139fb11/merged",
                "UpperDir": "/var/lib/docker/overlay2/afa1de233453b60686a3847854624ef191d7bc317fb01e015b4f06671139fb11/diff",
                "WorkDir": "/var/lib/docker/overlay2/afa1de233453b60686a3847854624ef191d7bc317fb01e015b4f06671139fb11/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:c2adabaecedbda0af72b153c6499a0555f3a769d52370469d8f6bd6328af9b13",
                "sha256:744315296a49be711c312dfa1b3a80516116f78c437367ff0bc678da1123e990",
                "sha256:379ef5d5cb402a5538413d7285b21aa58a560882d15f1f553f7868dc4b66afa8",
                "sha256:d00fd460effb7b066760f97447c071492d471c5176d05b8af1751806a1f905f8",
                "sha256:4d0c196331523cfed7bf5bafd616ecb3855256838d850b6f3d5fba911f6c4123",
                "sha256:98b4a6242af2536383425ba2d6de033a510e049d9ca07ff501b95052da76e894"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```

**理解：**

所有的 Docker镜像都起始于一个基础镜像层，当进行修改或培加新的内容时，就会在当前镜像层之上，创建新的镜像层。

举一个简单的例子，假如基于 Ubuntu Linux16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python包，
就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创健第三个镜像层该像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点.

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTE2NTIzNDI3NC5wbmc?x-oss-process=image/format,png)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了一个简单的例子，每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTE2NDk1ODkzMi5wbmc?x-oss-process=image/format,png)

上图中的镜像层跟之前图中的略有区別，主要目的是便于展示文件
下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTE2NTE0ODAwMi5wbmc?x-oss-process=image/format,png)

文种情況下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中

Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统

Linux上可用的存储引撃有AUFS、 Overlay2、 Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于 Linux中对应的
件系统或者块设备技术，井且每种存储引擎都有其独有的性能特点。

Docker在 Windows上仅支持 windowsfilter 一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW [1]。

下图展示了与系统显示相同的三层镜像。所有镜像层堆并合井，对外提供统一的视图。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTE2NTU1NzgwNy5wbmc?x-oss-process=image/format,png)

> 特点

Docker 镜像都是只读的，当容器启动时，一个新的可写层加载到镜像的顶部！

这一层就是我们通常说的容器层，容器之下的都叫镜像层！

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTE2MTUwNTg5Ny5wbmc?x-oss-process=image/format,png)



## commit镜像

```shell
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[版本TAG]
```



案例测试 ：发布自己的 Tomcat镜像

在 Tomcat部署  章节中，做了将webapps.dist下的文件都拷贝到webapps下的操作，现在，我们准备发布这个镜像，然后通过docker images 命令，可以发现已经完成发布。

```shell
docker commit -m="test commit images" -a="Shashar" 078bc0453d20 mytomcat:1.0
```

![image-20210115154225580](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210115154225580.png)

如果你想要保存当前容器的状态，就可以通过commit来提交，获得一个镜像，就好比我们我们使用虚拟机的快照。





# 进阶篇 

## 容器数据卷

### 概念

**什么是容器数据卷**

将应用和环境打包成一个镜像！

数据？如果数据都在容器中，那么我们容器删除，数据就会丢失！需求：**数据可以持久化**

MySQL，容器删除了，删库跑路！需求：**MySQL数据可以存储在本地！**

容器之间可以有一个数据共享的技术！Docker容器中产生的数据，同步到本地！

这就是卷技术！目录的挂载，将我们容器内的目录，挂载到Linux上面！

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjEwNTI1ODQ1Ni5wbmc?x-oss-process=image/format,png)

**总结一句话：容器的持久化和同步操作！容器间也是可以数据共享的！**

### 使用容器数据卷

> 方式一：直接使用命令挂载 -v

-v类似于-p,-p命令将主机端口与容器内端口做了一个映射，访问主机端口就可以访问到对象的容器内端口，而-v将主机目录和容器内目录做了一个映射，在容器内进行的操作会同步到主机目录

```shell
-v, --volume list                    Bind mount a volume

docker run -it -v 主机目录:容器内目录  -p 主机端口:容器内端口

#示例
[root@Shashar-study home]# docker run -it -v /home/DockerTest:/home centos /bin/bash
#通过docker inspect 命令查看是否挂载成功
[root@Shashar-study DockerTest]# docker inspect b23f9e0db86f
```

![image-20210120154951179](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210120154951179.png)

可以看到，已经成功了

接下来，可以做一个测试，另开一个终端，我的命令执行顺序和结果如下：

```shell
#测试一：在容器启动时，在容器内部和宿主机内分别添加测试文件，查看同步情况
#1. 启动centos，cd 到home目录，查看该目录
[root@Shashar-study ~]# docker exec -it b23f9e0db86f /bin/bash
[root@b23f9e0db86f /]# ls 
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@b23f9e0db86f /]# cd /home
[root@b23f9e0db86f home]# ls  #空目录

#2.新开一个终端，查看对应的DockerTest目录
[root@Shashar-study ~]# cd /home/DockerTest
[root@Shashar-study DockerTest]# ls   #也是空目录

#3. 在容器中，新建 test.java
[root@b23f9e0db86f home]# touch test.java
[root@b23f9e0db86f home]# ls
test.java

#4.查主机中对应的DockerTest目录
[root@Shashar-study DockerTest]# ls
test.java  

#5.在主机中创建test1.java文件
[root@Shashar-study DockerTest]# touch test1.java
[root@Shashar-study DockerTest]# ls
test1.java  test.java

#6.查看容器目录
[root@b23f9e0db86f home]# ls
test.java  test1.java
```

接下来测试在容器停止时，主机修改文件，容器内部的同步情况

```shell
#测试二：
#1.退出 并关闭容器 
[root@b23f9e0db86f home]# exit
exit
[root@Shashar-study ~]# docker stop b23f9e0db86f
b23f9e0db86f
[root@Shashar-study ~]# docker ps
CONTAINER ID   IMAGE   COMMAND   CREATED   STATUS   PORTS    NAMES

#2.在主机上修改test.java，增加了一行文本
[root@Shashar-study DockerTest]# vim test.java

#3.启动容器，查看文件是否被修改
[root@Shashar-study home]# docker exec -it b23f9e0db86f /bin/bash
[root@b23f9e0db86f /]# cd /home
[root@b23f9e0db86f home]# ls
test.java  test1.java
[root@b23f9e0db86f home]# cat test.java
write for DockerTest
```

#### 实战：安装MySQL

```shell
# 参考官网hub 
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

#启动我们得
-d 后台运行
-p 端口映射
-v 卷挂载
-e 环境配置
-- name 容器名字

[root@Shashar-study /]# docker run -d -p 3344:3306 -v /home/DockerTest/mysql/conf:/etc/mysql/conf.d  -v  /home/DockerTest/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:latest
```



然后使用SQLyog、或者Navicat连接测试一下

这里，我连接出现了问题，报错2058 ,一番操作后解决

在本地创建数据库DockerTest ，并新建表格test01

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210121145233060.png" alt="image-20210121145233060" style="zoom:50%;" />

假设我们将包含mysql的容器删除时，

![image-20210121151204769](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210121151204769.png)

本地数据还是存在

![image-20210121151227320](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210121151227320.png)

这就实现了容器的持久化功能

#### 具名和匿名挂载

```shell
# 匿名挂载
-v 容器内路径!
$ docker run -d -P --name nginx01 -v /etc/nginx nginx

# 查看所有的volume(卷)的情况
$ docker volume ls    
DRIVER              VOLUME NAME # 容器内的卷名(匿名卷挂载)
local               21159a8518abd468728cdbe8594a75b204a10c26be6c36090cde1ee88965f0d0
local               b17f52d38f528893dd5720899f555caf22b31bf50b0680e7c6d5431dbda2802c
         
# 这里发现，这种就是匿名挂载，我们在 -v只写了容器内的路径，没有写容器外的路径！

# 具名挂载 -P:表示随机映射端口
$ docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
9663cfcb1e5a9a1548867481bfddab9fd7824a6dc4c778bf438a040fe891f0ee

# 查看所有的volume(卷)的情况
$ docker volume ls                  
DRIVER              VOLUME NAME
local               21159a8518abd468728cdbe8594a75b204a10c26be6c36090cde1ee88965f0d0
local               b17f52d38f528893dd5720899f555caf22b31bf50b0680e7c6d5431dbda2802c
local               juming-nginx #多了一个名字


# 通过 -v 卷名：查看容器内路径
# 查看一下这个卷
$ docker volume inspect juming-nginx
[
    {
        "CreatedAt": "2020-05-23T13:55:34+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data", #默认目录
        "Name": "juming-nginx",
        "Options": null,
        "Scope": "local"
    }
]
1234567891011121314151617181920212223242526272829303132333435363738
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjExMzU0NTc0Ni5wbmc?x-oss-process=image/format,png)

所有的docker容器内的卷，没有指定目录的情况下都是在**/var/lib/docker/volumes/自定义的卷名/_data**下，
**如果指定了目录，docker volume ls 是查看不到的**。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjExNDIzMTQzNS5wbmc?x-oss-process=image/format,png)

**区分三种挂载方式**

```shell
# 三种挂载： 匿名挂载、具名挂载、指定路径挂载
-v 容器内路径			#匿名挂载
-v 卷名：容器内路径		  #具名挂载
-v /宿主机路径：容器内路径 #指定路径挂载 docker volume ls 是查看不到的
1234
```

拓展：

```shell
# 通过 -v 容器内路径： ro rw 改变读写权限
ro #readonly 只读
rw #readwrite 可读可写
$ docker run -d -P --name nginx05 -v juming:/etc/nginx:ro nginx
$ docker run -d -P --name nginx05 -v juming:/etc/nginx:rw nginx

# ro 只要看到ro就说明这个路径只能通过宿主机来操作，容器内部是无法操作！
1234567
```





### 使用Dockerfile

> 方式二：使用Dockerfile

**Dockerfile 就是用来构建docker镜像的构建文件**！命令脚本！先体验一下！

通过这个**脚本可以生成镜像**，镜像是一层一层的，脚本是一个个的命令，每个命令都是一层！

```shell
# 创建一个dockerfile文件，名字可以随便 建议Dockerfile
# 文件中的内容： 指令(大写) + 参数
$ vim dockerfile01
    FROM centos 					# 当前这个镜像是以centos为基础的

    VOLUME ["volume01","volume02"] 	# 挂载卷的卷目录列表(多个目录)，匿名挂载

    CMD echo "-----end-----"		# 输出一下用于测试
    CMD /bin/bash					# 默认走bash控制台
    
# 这里的每个命令，就是镜像的一层！
# 构建出这个镜像 
-f dockerfile1 			# f代表file，指这个当前文件的地址(这里是当前目录下的dockerfile1)
-t shashar/centos 	# t就代表target，指目标目录(注意caoshipeng镜像名前不能加斜杠‘/’)
. 						# 表示生成在当前目录下
# 通过dockerfile01生成
 docker build -f /home/DockerTest/DockerVolume/dockerfile01 -t shashar/centos:1.0 .
```

![image-20210121160042835](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210121160042835.png)

```shell
#查看生成的镜像
docker images 
```

![image-20210121160405447](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210121160405447.png)



> 启动自己写的容器镜像

```shell
$ docker run -it f4a6b0d4d948 /bin/bash	# 运行自己写的镜像
$ ls -l 								# 查看目录
12
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjEyMTQ1OTAyNi5wbmc?x-oss-process=image/format,png)

这个卷和外部一定有一个同步的目录

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjEyMTUzMTYyNi5wbmc?x-oss-process=image/format,png)

查看一下卷挂载

```shell
# docker inspect 容器id
$ docker inspect ca3b45913df5
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjEyMTYzMDI5NS5wbmc?x-oss-process=image/format,png)

测试一下刚才的文件是否同步出去了！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200524154444736.png#pic_center)

这种方式使用的十分多，因为我们通常会构建自己的镜像！

假设构建镜像时候没有挂载卷，要手动镜像挂载 -v 卷名：容器内路径！

#### 数据卷容器

**多个mysql实现数据共享**

```shell
$ docker run -d -p 3306:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

$ docker run -d -p 3310:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volumes-from mysql01  mysql:5.7

# 这个时候，可以实现两个容器数据同步！
```

结论：

**容器之间的配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止**。

**但是一旦你持久化到了本地，这个时候，本地的数据是不会删除的**！



## DockerFile

#### DockerFile介绍

`dockerfile`是用来构建docker镜像的文件！命令参数脚本！

构建步骤：

1、 编写一个dockerfile文件

2、 docker build 构建称为一个镜像

3、 docker run运行镜像

4、 docker push发布镜像（DockerHub 、阿里云仓库)



#### DockerFile构建过程

**基础知识**：

1、每个保留关键字(指令）都是必须是大写字母

2、执行从上到下顺序

3、#表示注释

4、每一个指令都会创建提交一个新的镜像曾，并提交！

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjEzMTc1Njk5Ny5wbmc?x-oss-process=image/format,png)

Dockerfile是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件十分简单！

Docker镜像逐渐成企业交付的标准，必须要掌握！

DockerFile：构建文件，定义了一切的步骤，源代码

DockerImages：通过DockerFile构建生成的镜像，最终发布和运行产品。

Docker容器：容器就是镜像运行起来提供服务。

#### DockerFile的指令

```shell
FROM				# from:基础镜像，一切从这里开始构建
MAINTAINER			# maintainer:镜像是谁写的， 姓名+邮箱
RUN					# run:镜像构建的时候需要运行的命令
ADD					# add:步骤，tomcat镜像，这个tomcat压缩包！添加内容 添加同目录
WORKDIR				# workdir:镜像的工作目录
VOLUME				# volume:挂载的目录
EXPOSE				# expose:保留端口配置
CMD					# cmd:指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT			# entrypoint:指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD				# onbuild:当构建一个被继承DockerFile这个时候就会运行onbuild的指令，触发指令
COPY				# copy:类似ADD，将我们文件拷贝到镜像中
ENV					# env:构建的时候设置环境变量！
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200524154609624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70#pic_center)

#### 实战测试

scratch 镜像

```shell
FROM scratch
ADD centos-7-x86_64-docker.tar.xz /

LABEL \
    org.label-schema.schema-version="1.0" \
    org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS" \
    org.label-schema.license="GPLv2" \
    org.label-schema.build-date="20200504" \
    org.opencontainers.image.title="CentOS Base Image" \
    org.opencontainers.image.vendor="CentOS" \
    org.opencontainers.image.licenses="GPL-2.0-only" \
    org.opencontainers.image.created="2020-05-04 00:00:00+01:00"

CMD ["/bin/bash"]
123456789101112131415
```

**Docker Hub 中 99%的镜像都是从这个基础镜像过来的 FROM scratch**，然后配置需要的软件和配置来进行构建。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200524154740467.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70#pic_center)

> 创建一个自己的centos

```shell
# 1./home/DockerTest下新建dockerfile目录
$ mkdir dockerfile

# 2. dockerfile目录下新建mydockerfile-centos文件
$ vim mydockerfile01

# 3.编写Dockerfile配置文件
FROM centos							# 基础镜像是官方原生的centos
MAINTAINER sha<7890@qq.com> 	# 作者

ENV MYPATH /usr/local				# 配置环境变量的目录 
WORKDIR $MYPATH						# 将工作目录设置为 MYPATH

RUN yum -y install vim				# 给官方原生的centos 增加 vim指令
RUN yum -y install net-tools		# 给官方原生的centos 增加 ifconfig命令

EXPOSE 80							# 暴露端口号为80

CMD echo $MYPATH					# 输出下 MYPATH 路径
CMD echo "-----end----"				
CMD /bin/bash						# 启动后进入 /bin/bash
```

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210122142629490.png" alt="image-20210122142629490" style="zoom:50%;" />

```shell
# 4.通过这个文件构建镜像
# 命令： docker build -f 文件路径 -t 镜像名:[tag] .
$ docker build -f mydockerfile-centos -t mycentos:0.1 .

# 5.出现下图后则构建成功
```

![image-20210122143206224](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210122143206224.png)



```shell
# 6. 测试运行
[root@Shashar-study dockerfile]# docker images
REPOSITORY   TAG    IMAGE ID       CREATED     SIZE
mycentos    1.0  08a9fa6689a1    2 minutes ago   282MB
[root@Shashar-study dockerfile]# docker run -it mycentos:1.0
[root@e18c3737bf39 local]# pwd
/usr/local
[root@e18c3737bf39 local]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.2  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:02  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

[root@e18c3737bf39 local]# ls  
bin  etc  games  include  lib  lib64  libexec  sbin  share  src
[root@e18c3737bf39 local]# vim test
[root@e18c3737bf39 local]# ls
bin  etc  games  include  lib  lib64  libexec  sbin  share  src  test
```

**测试cmd**

```shell
# 编写dockerfile文件
$ vim dockerfile-test-cmd
FROM centos
CMD ["ls","-a"]					# 启动后执行 ls -a 命令

# 构建镜像
$ docker build  -f dockerfile-test-cmd -t cmd-test:0.1 .

# 运行镜像
$ docker run cmd-test:0.1		# 由结果可得，运行后就执行了 ls -a 命令
.
..
.dockerenv
bin
dev
etc
home

# 想追加一个命令  -l 成为ls -al：展示列表详细数据
$ docker run cmd-test:0.1 -l
docker: Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "exec: \"-l\":
executable file not found in $PATH": unknown.
ERRO[0000] error waiting for container: context canceled 

# cmd的情况下 -l 替换了CMD["ls","-l"] 而 -l  不是命令所以报错
12345678910111213141516171819202122232425
```

**测试ENTRYPOINT**

```shell
# 编写dockerfile文件
$ vim dockerfile-test-entrypoint
FROM centos
ENTRYPOINT ["ls","-a"]

# 构建镜像
$ docker build  -f dockerfile-test-entrypoint -t cmd-test:0.1 .

# 运行镜像
$ docker run entrypoint-test:0.1
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found ...

# 我们的命令，是直接拼接在我们得ENTRYPOINT命令后面的
$ docker run entrypoint-test:0.1 -l
total 56
drwxr-xr-x   1 root root 4096 May 16 06:32 .
drwxr-xr-x   1 root root 4096 May 16 06:32 ..
-rwxr-xr-x   1 root root    0 May 16 06:32 .dockerenv
lrwxrwxrwx   1 root root    7 May 11  2019 bin -> usr/bin
drwxr-xr-x   5 root root  340 May 16 06:32 dev
drwxr-xr-x   1 root root 4096 May 16 06:32 etc
drwxr-xr-x   2 root root 4096 May 11  2019 home
lrwxrwxrwx   1 root root    7 May 11  2019 lib -> usr/lib
lrwxrwxrwx   1 root root    9 May 11  2019 lib64 -> usr/lib64 ....

12345678910111213141516171819202122232425262728293031323334
```

Dockerfile中很多命令都十分的相似，我们需要了解它们的区别，我们最好的学习就是对比他们然后测试效果！



#### 实战：Tomcat镜像

1. 准备需要的文件

apache-tomcat-9.0.41.tar.gz  

jdk-8u281-linux-x64.tar.gz

2. 编写Dockerfile文件，官方的命名为==dockerfile==，如果使用这个名字，build时会自动寻找这个文件，可以不用-f指定

```shell
#以下为dockerfile的内容
FROM centos

MAINTAINER sha<2890@qq.com>

COPY readme.md /usr/local/readme.md
ADD apache-tomcat-9.0.41.tar.gz /usr/local
ADD jdk-8u281-linux-x64.tar.gz /usr/local
RUN yum -y install vim
ENV MYPATH /usr/local 
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_281
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.41
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.41

ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin 

EXPOSE 8080
CMD /usr/local/apache-tomcat-9.0.41/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.41/logs/catalina.out
```

4. 构建镜像

```shell
#因为使用了默认命名dockerfile,所以不需要再使用-f指定文件夹
docker build -t mytomcat:0.1 .
```

5. 启动镜像

```shell
#启动镜像，-d后台运行 -p端口映射 --name自定义名称 -v目录映射挂载
docker run -d -p 3344:8080 --name diytomcat  -v /home/DockerTest/dockerfile/mytomcat/test:/usr/local/apache-tomcat-9.0.41/webapps/test -v /home/DockerTest/dockerfile/mytomcat/tomcatlogs:/usr/local/apache-tomcat-9.0.41/logs  mytomcat:0.1
```

6. 访问测试

```shell
#方式一，本机访问
curl localhost:3344

#方式二，网址访问
服务器网址：3344
```

#### 发布自己的镜像 

> 发布到DockerHub

这个和GitHub是类似的，首先需要注册DockerHub账号，官网地址是：

 https://hub.docker.com/

然后需要在服务器上登录

```shell
#1. 登录
docker login -u DockerID -p password

#2. 上传
docker push 镜像名：版本
```

在这里我遇到了报错，具体如下：

denied: requested access to the resource is denied

这是由于没有前缀，会默认提交到官方的库中，所以需要加上自己的DockerHub名称，有两种解决方式：

第一种：

```shell
#错误denied: requested access to the resource is denied解决方式
#1. docker tag IMAGE_ID  DockerHub_ID/IMAGE_NAME:版本号
docker tag 08a9fa6689a1 shashar910/mycentos:1.0
#2.再次使用docker push 命令
 docker push shashar910/mycentos:1.0
```

然后成功发布，结果如下：

![image-20210126161318827](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210126161318827.png)

![image-20210126161335248](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210126161335248.png)

第二种：

就是在build的时候就加上自己的dockerhub用户名,例如

docker build -t DockerHub_Name/Image_name:版本号



#### 发布到阿里云镜像仓库

登录阿里云，找到容器镜像服务，创建命名空间

之后，选择镜像仓库，创建自己的仓库，这里选择了本地仓库

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210128152113091.png" alt="image-20210128152113091" style="zoom:33%;" />

<img src="C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210128152152338.png" alt="image-20210128152152338" style="zoom:33%;" />

点击仓库，尽可以进行仓库基本信息，里面有详细的操作流程，包括登录、拉取、推送镜像等。

```shell
#1. 登录阿里云镜像仓库，登录的用户名为阿里云账号全名，密码为开通服务时设置的密码
docker login --username=用户名 registry.cn-hangzhou.aliyuncs.com

#2.生成镜像版本号
docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/shashar-docker/docker-test:[镜像版本号]

#3.push镜像
docker push registry.cn-hangzhou.aliyuncs.com/shashar-docker/docker-test:[镜像版本号]
```

2.3两个命令都是直接在阿里云上复制的，修改对应的[ImageId] 和[镜像版本号]即可使用



## Docker网络

##### 理解Docker 0

学习之前**清空下前面的docker 镜像、容器**

```shell
# 删除全部容器
$ docker rm -f $(docker ps -aq)

# 删除全部镜像
$ docker rmi -f $(docker images -aq)
```

查看IP地址的方式 

```shell
# Windows和Linux 可以通过ipconfig，Linux还可以通过ip addr
ip addr
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTIyMzIzNjc3Mi5wbmc?x-oss-process=image/format,png)

**三个网络**

> 问题： docker 是如果处理容器网络访问的？

```shell
#1. 测试  运行一个tomcat
docker run -d -P --name tomcat01 tomcat
#2. 查看容器内部网络地址
docker exec -it tomcat01 ip addr
```

运行结果如下：

```shell
[root@Shashar-study ~]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
64: eth0@if65: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
```

可以发现，docker内部分配了一个网卡64: eth0@if65: ==ip地址，docker分配！==

尝试在Linux上新开一个会话ping 172.17.0.2，结果如下：

```shell
[root@Shashar-study ~]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.050 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.052 ms
^C
--- 172.17.0.2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 0.050/0.051/0.052/0.001 ms
```

> 原理

1、我们每启动一个docker容器，docker就会给docker容器分配一个ip，我们只要按照了docker，就会有一个docker0桥接模式，使用的技术是veth-pair技术！

再次使用命令ip addr 

```shell
[root@Shashar-study ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:16:3e:08:54:29 brd ff:ff:ff:ff:ff:ff
    inet 172.31.90.70/20 brd 172.31.95.255 scope global dynamic eth0
       valid_lft 300515090sec preferred_lft 300515090sec
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:00:58:3a:7f brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
65: vethb61656b@if64: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether aa:b3:ba:d5:a9:ee brd ff:ff:ff:ff:ff:ff link-netnsid 0
```

对照前两次ip addr（Linux和docker容器内部），可以发现，多了一个vethb61656b@if64，和上文的可以对照。。



2 、再启动一个容器测试，发现又多了一对网络

```shell
[root@Shashar-study ~]# docker run -d -P --name tomcat02 tomcat
2d42a8ea6615edd1e470c0a8213ab893f6b1e9dbe4cfc8cc3f276a3acfc92f73
[root@Shashar-study ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:16:3e:08:54:29 brd ff:ff:ff:ff:ff:ff
    inet 172.31.90.70/20 brd 172.31.95.255 scope global dynamic eth0
       valid_lft 300514831sec preferred_lft 300514831sec
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:00:58:3a:7f brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
65: vethb61656b@if64: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether aa:b3:ba:d5:a9:ee brd ff:ff:ff:ff:ff:ff link-netnsid 0
67: veth2f66ea5@if66: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether aa:82:84:d5:a6:83 brd ff:ff:ff:ff:ff:ff link-netnsid 1
```

可以发现：

我们发现这个容器带来网卡，都是一对对的（64,65） （66,67）

veth-pair 就是一对的虚拟设备接口，他们都是成对出现的，一端连着协议，一端彼此相连

正因为有这个特性 veth-pair 充当一个桥梁，连接各种虚拟网络设备的

OpenStac,Docker容器之间的连接，OVS的连接，都是使用evth-pair技术



###### 网络之间的互ping汇总

```shell
#预备工作
#启动两个Tomcat，分别是tomcat01、tomcat02
docker run -d -P --name tomcat01 tomcat
docker run -d -P --name tomcat02 tomcat
#分别查询本机，Tomcat01、Tomcat02的ip
#本机的
[root@Shashar-study ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:16:3e:08:54:29 brd ff:ff:ff:ff:ff:ff
    inet 172.31.90.70/20 brd 172.31.95.255 scope global dynamic eth0
       valid_lft 300279814sec preferred_lft 300279814sec
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:00:58:3a:7f brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
65: vethb61656b@if64: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether aa:b3:ba:d5:a9:ee brd ff:ff:ff:ff:ff:ff link-netnsid 0
67: veth2f66ea5@if66: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether aa:82:84:d5:a6:83 brd ff:ff:ff:ff:ff:ff link-netnsid 1

#tomcat01 的
[root@Shashar-study ~]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
64: eth0@if65: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
#tomcat02 的
[root@Shashar-study ~]# docker exec -it tomcat02 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
66: eth0@if67: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.3/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
```

到这一步，我们可以发现：

```shell
#主机的ip： 172.31.90.70
#tomcat01的ip：172.17.0.2
#tomcat02的ip：172.17.0.3
#接下来，测试他们之间的连接

#主机连接tomcat01 ，即主机ping Docker 容器内部，可以连接
[root@Shashar-study ~]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.072 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.058 ms
^C
--- 172.17.0.2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 0.058/0.065/0.072/0.007 ms

#tomcat01 ping 主机，即Docker容器内部ping主机，可以联通
[root@Shashar-study ~]# docker exec -it tomcat01 ping 172.31.90.70
PING 172.31.90.70 (172.31.90.70) 56(84) bytes of data.
64 bytes from 172.31.90.70: icmp_seq=1 ttl=64 time=0.058 ms
64 bytes from 172.31.90.70: icmp_seq=2 ttl=64 time=0.053 ms
^C
--- 172.31.90.70 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 11ms
rtt min/avg/max/mdev = 0.053/0.055/0.058/0.007 ms

#tomcat01 ping tomcat02 容器之间的互联
[root@Shashar-study ~]# docker exec -it tomcat01 ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3) 56(84) bytes of data.
64 bytes from 172.17.0.3: icmp_seq=1 ttl=64 time=0.083 ms
64 bytes from 172.17.0.3: icmp_seq=2 ttl=64 time=0.040 ms
^C
--- 172.17.0.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.040/0.061/0.083/0.022 ms
```

==结论==：容器和主机、容器之间都可以互相ping通！

思考：直接使用ping ip可以ping 通，那么使用ping 名称呢？比如tomcat01 ping tomcat02?

```shell
#测试使用名称来ping
[root@Shashar-study ~]# docker exec -it tomcat01 ping tomcat02
ping: tomcat02: Name or service not known

#失败，不可以直接通过容器名称来ping
#解决方式：使用--link 做映射
# 运行一个tomcat03 --link tomcat02 
[root@Shashar-study ~]# docker run -d -P --name tomcat03 --link tomcat02 tomcat
c77d555453e970e638c6c0e614191d98390feb7c001717d18d20b82babff99c5
[root@Shashar-study ~]# docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.096 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.074 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=3 ttl=64 time=0.056 ms
^C
--- tomcat02 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 5ms
rtt min/avg/max/mdev = 0.056/0.075/0.096/0.017 ms

#tomcat03做完--link 映射tomcat02之后可以通过名称ping通tomcat02
#测试反向ping，即没有做过--link tomcat03 的tomcat02 ping tomcat03
[root@Shashar-study ~]# docker exec -it tomcat02 ping tomcat03
ping: tomcat03: Name or service not known
#发现使用了--link 来做映射之后，可以Ping通 了
#原因如下：
[root@Shashar-study ~]# docker exec -it tomcat03 cat /etc/hosts
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
172.17.0.3      tomcat02 2d42a8ea6615
172.17.0.4      c77d555453e9

#可以发现，hosts中有了tomcat02--172.17.0.3的映射
```

–link 本质就是在hosts配置中添加映射

现在使用Docker已经不建议使用–link了！

自定义网络，不适用docker0！

docker0问题：不支持容器名连接访问！

###### **网络模型图**

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE3NDI0ODYyNi5wbmc?x-oss-process=image/format,png)

结论：tomcat01和tomcat02公用一个路由器，docker0。

所有的容器不指定网络的情况下，都是docker0路由的，docker会给我们的容器分配一个默认的可用ip。

> 小结

Docker使用的是Linux的桥接，宿主机是一个Docker容器的网桥 docker0

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE3NDcwMTA2My5wbmc?x-oss-process=image/format,png)

Docker中所有网络接口都是虚拟的，虚拟的转发效率高（内网传递文件）

只要容器删除，对应的网桥一对就没了！





##### 自定义网络

在网络之间的互ping汇总中，我们使用了--link来实现容器之间的使用容器名称来ping

还有另一种处理方式 ，就是自定义网络。

```shell
docker network
connect     -- Connect a container to a network
create      -- Creates a new network with a name specified by the
disconnect  -- Disconnects a container from a network
inspect     -- Displays detailed information on a network
ls          -- Lists all the networks created by the user
prune       -- Remove all unused networks
rm          -- Deletes one or more networks
12345678
```

> 查看所有的docker网络

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MDMxNjA3My5wbmc?x-oss-process=image/format,png)

**网络模式**

bridge ：桥接 docker（默认，自己创建也是用bridge模式）

```shell
# 我们直接启动的命令 --net bridge,而这个就是我们得docker0
# bridge就是docker0
$ docker run -d -P --name tomcat01 tomcat
等价于 => docker run -d -P --name tomcat01 --net bridge tomcat

# docker0，特点：默认，域名不能访问。 --link可以打通连接，但是很麻烦！
```

none ：不配置网络，一般不用

host ：和所主机共享网络

container ：容器网络连通（用得少！局限很大）



> 自定义网络

```shell
#预备工作，首先删除之前创建的3个容器tomcat01,02,03,使得网络恢复初始的状态，方便操作
[root@Shashar-study ~]# docker rm -f $(docker ps -aq)
c77d555453e9
2d42a8ea6615
ffc1f16640aa
[root@Shashar-study ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:16:3e:08:54:29 brd ff:ff:ff:ff:ff:ff
    inet 172.31.90.70/20 brd 172.31.95.255 scope global dynamic eth0
       valid_lft 300255312sec preferred_lft 300255312sec
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:00:58:3a:7f brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

使用docker network 自定义网络

```shell
#创建网络的命令是docker network create

[root@Shashar-study ~]# docker network create --help

Usage:  docker network create [OPTIONS] NETWORK

Create a network

Options:
      --attachable           Enable manual container attachment
      --aux-address map      Auxiliary IPv4 or IPv6 addresses used by Network driver (default map[])
      --config-from string   The network from which copying the configuration
      --config-only          Create a configuration only network
  -d, --driver string        Driver to manage the Network (default "bridge")
      --gateway strings      IPv4 or IPv6 Gateway for the master subnet
      --ingress              Create swarm routing-mesh network
      --internal             Restrict external access to the network
      --ip-range strings     Allocate container ip from a sub-range
      --ipam-driver string   IP Address Management Driver (default "default")
      --ipam-opt map         Set IPAM driver specific options (default map[])
      --ipv6                 Enable IPv6 networking
      --label list           Set metadata on a network
  -o, --opt map              Set driver specific options (default map[])
      --scope string         Control the network's scope
      --subnet strings       Subnet in CIDR format that represents a network segment
```

测试使用如下：

```shell
#创建网络  
#--driver  bridge
#--subnet 192.168.0.0/16
# --gateway 192.168.0.1 
[root@Shashar-study ~]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1  mynet
eef34fe50d6b5643a943982ccb3a25943ce0e9299d40104eced357ad7e09bf93

#查看已有的网络
[root@Shashar-study ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
87914e3a50ef        bridge              bridge              local
b2e8391d4019        host                host                local
eef34fe50d6b        mynet               bridge              local
4c794269c6ca        none                null                local

#查看自己创建的网络详情，config里有我们配置的内容，Containers里面是空的
[root@Shashar-study ~]#  docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "eef34fe50d6b5643a943982ccb3a25943ce0e9299d40104eced357ad7e09bf93",
        "Created": "2021-02-02T15:56:53.0534899+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```



在自己创建的网络里启动tomcat 

```shell
docker run -d -P --name tomcat-mynet-01 --net mynet tomcat
docker run -d -P --name tomcat-mynet-02 --net mynet tomcat
```

此时，重新使用命令docker network inspect查看网络情况

```shell
[root@Shashar-study ~]# docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "eef34fe50d6b5643a943982ccb3a25943ce0e9299d40104eced357ad7e09bf93",
        "Created": "2021-02-02T15:56:53.0534899+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "98244422a8e38e6fd33a4ecffcf6b653dd2c2e2e22f5aa8e115b4e340298c9d3": {
                "Name": "tomcat-mynet-01",
                "EndpointID": "092f2ac14554df2e37bdab9425f6d29f7c016a372190ae40c017c03dd8e67152",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            },
            "b5e3bff2006d6364d323e35eb4c6ee3d6fd87395951394f9af77205c2c3fec15": {
                "Name": "tomcat-mynet-02",
                "EndpointID": "7b32bddcc3e02e6593bcc00d0ff682243b130793fe44f655c94c287ff45cf7c8",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

可以看到，Containers中有了刚才启动的两个容器

接下来测试这两个tomcat之间的联通性，是否可以通过名称ping 通，答案是可以

```shell
[root@Shashar-study ~]# docker exec -it tomcat-mynet-01 ping tomcat-mynet-02
PING tomcat-mynet-02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat-mynet-02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.067 ms
64 bytes from tomcat-mynet-02.mynet (192.168.0.3): icmp_seq=2 ttl=64 time=0.060 ms
^C
--- tomcat-mynet-02 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 8ms
rtt min/avg/max/mdev = 0.060/0.063/0.067/0.008 ms
[root@Shashar-study ~]# docker exec -it tomcat-mynet-02 ping tomcat-mynet-01
PING tomcat-mynet-01 (192.168.0.2) 56(84) bytes of data.
64 bytes from tomcat-mynet-01.mynet (192.168.0.2): icmp_seq=1 ttl=64 time=0.036 ms
^C
--- tomcat-mynet-01 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.036/0.036/0.036/0.000 ms
```

我们自定义的网络docker当我们维护好了对应的关系，推荐我们平时这样使用网络！

好处：

redis -不同的集群使用不同的网络，保证集群是安全和健康的

mysql-不同的集群使用不同的网络，保证集群是安全和健康的

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MjUwNDM2Ny5wbmc?x-oss-process=image/format,png)

#### 网络连通

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MzI0MzE0Ni5wbmc?x-oss-process=image/format,png)

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MzI1OTE4NS5wbmc?x-oss-process=image/format,png)

```shell
# 测试两个不同的网络连通  再启动两个tomcat 使用默认网络，即docker0
$ docker run -d -P --name tomcat01 tomcat
$ docker run -d -P --name tomcat02 tomcat
# 此时ping不通
```

![image-20210202164803001](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210202164803001.png)

![image-20210202164835182](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210202164835182.png)

```shell
# 要将tomcat01 连通 tomcat—net-01 ，连通就是将 tomcat01加到 mynet网络
docker network connect 网络名 容器名
# 一个容器两个ip（tomcat01）
```

![image-20210202164852889](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210202164852889.png)

```shell
# 01连通 ，加入后此时，已经可以tomcat01 和 tomcat-01-net ping通了
# 02是依旧不通的
```

结论：假设要跨网络操作别人，就需要使用docker network connect 连通！

##### 实战：部署Redis集群

目标：六个容器，其中三个Redis主机，每个主机配备一个从机

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5NDQxOTQ3MS5wbmc?x-oss-process=image/format,png)



> 步骤

```shell
#1. 创建redis网络
docker network create redis --subnet 172.38.0.0/16
# 通过脚本创建六个redis配置，直接复制回车即可
for port in $(seq 1 6);\
do \
mkdir -p /home/DockerTest/RedisTest/node-${port}/conf
touch /home/DockerTest/RedisTest/node-${port}/conf/redis.conf
cat << EOF >>  /home/DockerTest/RedisTest/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done

# 通过脚本运行六个redis
for port in $(seq 1 6);\
do \
docker run -p 637${port}:6379 -p 1667${port}:16379 --name redis-${port} \
-v /home/DockerTest/RedisTest/node-${port}/data:/data \
-v /home/DockerTest/RedisTest/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
done


```

到这里，6 个Redis已经启动，我们可以用命令docker ps查询

![image-20210203144136091](C:\Users\shasha\AppData\Roaming\Typora\typora-user-images\image-20210203144136091.png)

```shell
#接下来，配置集群
docker exec -it redis-1 /bin/sh   #redis中没有bash
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379  --cluster-replicas 1
```

关于这个命令

```
 redis-cli --cluster help
Cluster Manager Commands:
  create         host1:port1 ... hostN:portN
                 --cluster-replicas <arg>
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjIwMjkwMjI0MS5wbmc?x-oss-process=image/format,png)



```shell
#连接集群的客户端
/data # redis-cli -c    #-c指集群客户端
127.0.0.1:6379> cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:3204
cluster_stats_messages_pong_sent:3183
cluster_stats_messages_sent:6387
cluster_stats_messages_ping_received:3178
cluster_stats_messages_pong_received:3204
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:6387

127.0.0.1:6379> cluster nodes
1db1f1d8ddb7a1084d6b4530ec3c97532ef66fc6 172.38.0.11:6379@16379 myself,master - 0 1612336866000 1 connected 0-5460
ff05a6389a38ce0dd447193659aab6d981b48831 172.38.0.15:6379@16379 slave 1db1f1d8ddb7a1084d6b4530ec3c97532ef66fc6 0 1612336865773 5 connected
a70ad5accc36b7b964f679c34ad8229fe050659c 172.38.0.13:6379@16379 master - 0 1612336866274 3 connected 10923-16383
b205ce4da669f6c78f81600955e12459f370d835 172.38.0.16:6379@16379 slave 659a08710503f380e19ed30b7f6839f23a283e9d 0 1612336865573 6 connected
659a08710503f380e19ed30b7f6839f23a283e9d 172.38.0.12:6379@16379 master - 0 1612336864271 2 connected 5461-10922
eebb31ecfeb33cc638b99ea12075fc1bbdd1057f 172.38.0.14:6379@16379 slave a70ad5accc36b7b964f679c34ad8229fe050659c 0 1612336865000 4 connected
```

## SpringBoot微服务打包Docker镜像