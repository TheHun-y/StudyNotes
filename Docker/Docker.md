# Docker



## 1. 概述



### 容器虚拟化技术



### Docker与虚拟机技术



### Docker的三要素

- 镜像（类）
- 容器（对象）
- 仓库（类库）



## 2. Docker命令



### 2.1 帮助命令

- docker version
- docker info
- docker --help



### 2.2 镜像命令

- **查看所有镜像**

  ```shell
  docker images
  ```

  ![image-20210128173331096](C:\Users\yanglin\AppData\Roaming\Typora\typora-user-images\image-20210128173331096.png)

  - -a：列出所有镜像，含**中间镜像层**
  - -q：只显示id
  - --digests：显示镜像的说明
  - --no-trunc：显示全长id，不要截取

- **去docker hub查询镜像文件**

  ```shell
  docker search xxx	
  ```

  - -s 数值：只显示**收藏数不小于**指定值的镜像
  - --no-trunc：显示全长，不要截取
  - --automated：只显示automated的镜像

- **下载镜像**

  ```shell
  docker pull xxx[:version]
  ```

  - 不加版本号默认latest

- **删除镜像（单个/多个）**

  ```shell
  docker rmi [-f] xxx[:version] yyy[:version]
  ```

  - -f：强制删除
  - 不加版本号默认latest

- **删除全部镜像**

  ```shell
  docker rmi [-f] $(docker images -qa)
  ```

  

