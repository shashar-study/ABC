











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



# 安装

## 基本组成

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



## Docker安装、卸载

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

## Docker的底层原理

Docker**是怎么工作的**？

Docker是一个Client-Server结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问！

Docker-Server接收到Docker-Client的指令，就会执行这个命令！

![image-20200515102949558](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTEwMjk0OTU1OC5wbmc?x-oss-process=image/format,png)

**为什么Docker比Vm快**
1、docker有着比虚拟机更少的抽象层。由于docker不需要Hypervisor实现硬件资源虚拟化,运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。
2、docker利用的是宿主机的内核,而不需要Guest OS。

```
GuestOS： VM（虚拟机）里的的系统（OS）

HostOS：物理机里的系统（OS）
123
```

![image-20200515104117329](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTEwNDExNzMyOS5wbmc?x-oss-process=image/format,png)

因此,当新建一个 容器时,docker不需要和虚拟机一样重新加载一个操作系统内核。仍而避免引导、加载操作系统内核返个比较费时费资源的过程,当新建一个虚拟机时,虚拟机软件需要加载GuestOS,返个新建过程是**分钟级别**的。而docker由于直接利用宿主机的操作系统,则省略了这个复杂的过程,因此新建一个docker容器只需要**几秒钟**。

## 阿里云镜像加速

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



# Docker的常用命令

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

# Docker部署

## Nginx部署

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



## Tomcat 部署

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

## elasticsearch+kibana部署

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





# Docker可视化管理工具Portainer