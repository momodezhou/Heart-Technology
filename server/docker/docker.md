# 初识docker

## 这是什么东西

- 集装箱（单间) 或者书虚拟机环境理论

|                      |
| :------------------: |
|       Applications   |
|      Runtime Library |
|               Kernel |
|     Operating System |
| CPU Memory IO Device |


|    特性    |      虚拟机      |       容器       |
| :--------: | :--------------: | :--------------: |
|  隔离级别  |    操作系统级    |      进程级      |
|  隔离策略  |    hypervisor    |     cGroups      |
|  系统资源  |      5~15%       |       ~5%        |
|  启动时间  |      分钟级      |       秒级       |
|  镜像存储  |      GB-TB       |        MB        |
|  集群规模  |       上百       |       上万       |
| 高可用策略 | 备份、容灾、迁移 | 弹性、动态、负载 |

- 由镜像（Image)、容器（Container）和镜像托管注册中心（Repository）构成的生态
        镜像:
            1.只是一个虚拟的概念，其实际体现并非由一个文件组成，而是由一组文件系统组成，或者说，由多层文件系统联合组成【为何要多层？】。
            2.上一层无法操作下层【就像建楼房】
        容器:
            1.容器进程运行于属于自己的独立的`命名空间`,因此有着类似操作系统的资源（文件系统、自己的网络配置、自己的进程空间等）
            2.以镜像为基础层，在其上创建一个当前容器的存储层，即容器存储层（看得出来，镜像和容器的关系，就像代码和程序，一静一动）
        镜像托管注册中心：
            1.可以理解为存放镜像的云端服务，作用类似github
-------

## 解决了什么问题

- 独立隔离的运行环境（Build once，Run anywhere）
 ```
  以php为例，自己机器上如何同时运行php7和php5的项目？
  1.本地拉取项目
  2.安装php,nginx
  3.修改nginx.conf、php-fpm.conf（分配端口)
```

- 一次构建镜像，从仓库拉到本地即可运行（Build, Ship and Run）
```
  一个项目如何快速部署多个机器？
  处理机器环境程序安装、环境配置
```
-------



## 怎么实现的功能

- 为何docker能够做到每一个容器可以拥有独立的系统环境？
```
docker资源管控（主要依赖linux：Namespaces、Control groups、UnionFS）

Namespaces(命名空间，主要起隔离作用):
pid namespace：进程隔离
net namespace：网络管理
ipc namespace：IPC（进程间通信）资源管理
mnt namespace：文件系统挂载管理
uts namespace：内核和版本标识符隔离。

Control groups(依赖技术cgroups):
控制组：限制容器对物理资源的使用，比如cpu、内存。

UnionFS:
联合文件系统：为容器提供块功能，也就是文件系统。

```

- 为何docker参照linux设计，别的系统可以运用？
```
   - 模拟linux环境
   - 此环境仅是能够满足基础组件原型，远小于linu操作系统量级
```

- 为何docker能够让服务流量打到对应项目
```
    - 端口映射监听
    - 路由映射配置
    - 项目路径映射
```

- 【思考】docker设计之初是参照linux，为何其他平台可以使用

-------

## 怎么用
- docker环境搭建
```
sudo yum install docker-ce docker-ce-cli containerd.io

sudo groupadd docker

sudo usermod -aG docker ${USER}  // 注销后生效

sudo systemctl start docker

sudo docker run hello-world

```
- docker镜像获取
    
        docker pull ubuntu:18.04
- docke运行

        docker run -it --rm ubuntu:18.04 bash
- 镜像生成

   1.commit[作用类似git commit]
   
   2.[dockerfile](./dockerfie/dockerfile.md)

------


## 出入源码
- docker架构

```
Client: Docker Engine - Community
 Version:           19.03.8
	...

Server: Docker Engine - Community
 Engine:
	...


├── cli  // client
├── engine  // server
└── packaging


```

- 生命周期
![docker生命周期](https://upload-images.jianshu.io/upload_images/14944275-584653328788d81d.png "docker容器生命周期")
----

## 参考

- [docker 源码入门](https://www.jianshu.com/p/4a1f712185c4)
- [docker 基础架构](https://www.jianshu.com/p/66b31607dc3d)