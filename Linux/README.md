# Linux



## 1. vim编辑器



### 切换模式

命令行下：vim xxx  ----  进入一般模式  ----  一般模式按**a**或**i**  ----  编辑模式  ----  编辑模式按**esc**  ----  回到一般模式  ----  一般模式按 **:或/**  ----  命令模式  ----  按esc  ----  回到一般模式



### 快捷键

正常模式

- yy：拷贝当前行
- 5yy：拷贝当前行在内向下的5行
- p：粘贴
- dd：删除当前行
- 5dd：删除当前行在内向下的5行
- G：定位最末行
- gg：定位最首行
- u：撤销之前的动作
- 先输入行号再按 shift + g：定位到指定行（在显示行号模式下）

命令模式

- 先输入 /关键词 再按回车：查找关键词，按n查找下一个
- :set nu 或 :set nonu：显示/关闭行号



## 2. 用户管理



### 添加用户

- **useradd 用户名 -- 自动创建同名的家目录，默认创建同名组**

- **useradd -d 指定目录 用户名 -- 创建指定名称的家目录**

- **useradd -g 用户组 用户名 -- 指定用户的组，组不存在则创建**



### 修改密码

- **passwd 用户名 -- root和本用户才有权限**



### 移除用户

- **userdel 用户名 -- 不删除家目录**

- **userdel -r 用户名 -- 删除家目录**



### 查询用户信息

- **id 用户名**

​		有用户：返回用户信息（uid（name）、gid(name)、组（name））

​		无用户：无此用户



### 切换用户

- **su - 用户名 -- 切换用户（注意中间都有空格）**

- **exit -- 返回上一个用户**

​		权限高到权限低不必输入密码

​		权限不足时会提示



### 查看用户

- **whoami**



### 创建组

- **groupadd 组名**



### 删除组

- **groupdel 组名**



### 修改用户的组

- **usermod -g 用户组 用户名**



### 配置文件

- **用户信息配置文件 -- /etc/passwd**
  - 行含义：用户名：口令（密文）：用户id：组id：注释性描述（可选）：家目录：登录shell
- **组配置文件 -- /etc/group**
- **口令配置文件（加密，存密码和登录信息）-- /etc/shadow**
  - 行含义：组名：口令（密文）：组id：组内用户列表



## 3. 实用指令



### 关机、重启和注销

- **init 0** 
- **shutdown：关机或重启**
  - shutdown -h now ：立即关机
  - shutdown -h 1 ：1分钟后关机
  - shutdown -r now ：立即重启
- **halt：停机**
- **reboot ：重启**
- **sync：内存同步到磁盘（重要）**
- **logout：注销（图形界面（级别5）下无效）**



### 指定运行级别（含关机、找回root密码方法）

**init [012356]**

- 七个运行级别
  - 0：关机
  - 1：单用户级别（用于**找回密码**）
  - 2：多用户无网络
  - **3*：多用户有网络（最常用）**
  - 4：保留
  - 5：图形界面
  - 6：重启
- 配置文件：**/etc/inittab**
- **找回root密码**：
  - 引导时输入一个回车
  - 在界面中输入e
  - 新界面中选中第二行（编辑内核）
  - 再输入一个e
  - 在最后一行输入一个回车
  - 再输入一个b
  - 进入单用户模式
  - 用passwd root修改密码
- 修改默认的运行级别：修改配置文件/etc/inittab，修改最后一行（id:5:initdefault），改为对应的运行级别



### 帮助指令

​	**man 需要查看的命令**

​		在文档中可以使用回车翻页

​	**help 需要查看的命令**



### 文件目录类指令

------

#### 注意：文件目录后面都需要带一个/



#### 显示信息

- **pwd -- 显示当前工作目录的绝对路径**
- **ls [选项] [目录或文件] -- 显示目录或文件信息**
  - -l ： 列表方式显示
  - -a：显示隐藏文件
  - -h：显示文件大小时按单位显示
  - 可以组合使用



#### 切换目录

- **cd [参数] -- 切换目录**
  - 绝对路径：从根目录/开始定位
  - 相对路径：从当前工作目录开始定位
  - cd ~ ：回到自己的**家**目录
  - cd .. ：回到**上一级**目录



#### 管理目录

- **mkdir [选项] 目录 -- 创建目录**
  - 默认只能创建一级（上级必须存在）
  - -p：创建多级目录
- **rmdir [选项] 要删除的空目录 -- 删除空目录**
  - 默认只能删除空目录
  - -rf：删除非空目录



#### 管理文件

- **touch 文件名1 文件名2 ... 文件名n -- 创建空文件**
- **cp [选项] 源文件 目的地址 -- 拷贝文件（夹）到指定目录**
  - -r：递归复制整个文件夹
  - 前面加反斜杠\，表示**强制覆盖文件**，不要提示：\cp -r aaa/ bbb/
  - 实例1：cp aaa.txt bbb/ **（注意最后的/）**
  - 实例2：cp -r aaa/ bbb/

- **rm [选项] 文件或目录 -- 移除文件（夹）**
  - -r：删除整个文件夹
  - -rf：删除一个目录
  - -f：强制删除不提示

- **mv 原文件名 新文件名 -- 移动文件（夹）或重命名**

  - 若移动的目的中存在该文件，则重命名

    mv aaa.txt bbb.txt

  - 若移动的目的中不存在该文件，则移动

    mv /temp/aaa.txt /home

  - 若移动的目的中不存在该文件，但使用mv时指定了新文件名 ，则移动加重命名

    mv /temp/aaa.txt /home/bbb.txt



#### 查看文件

- **cat [选项] 要查看的文件 -- 只读方式打开文件**

  - -n：显示行号
  - | more ：分页显示
    - 实例：cat -n aaa.txt | more

- **more 文件名 -- more方式查看文件**

  在more显示状态下：

  - 空格：翻页
  - 回车：翻行
  - ctrl + b ：看上一页
  - ctrl + f ：看下一页

- **less 文件名 -- 分屏显示，分次加载（大型文件）**

  在less显示状态下：

  - 空格：翻页
  - 回车：翻行
  - pgdn：向下翻
  - pgup：向上翻
  - /字符串：向下查找
  - ?字符串：向上查找



#### 命令行输出至文件

- **> -- 输出重定向：将原来的文件内容覆盖**
  - 把其前面命令的输出内容写到文件中
    - 实例：ls -l > a.txt
- **>> -- 追加：追加到文件末尾**
  - **追加**方式把其前面命令的输出内容写到文件中
    - 实例：ls -l >> a.txt
- **使用实例**
  - **cat 文件1 > 文件2** ：将文件1的内容覆盖至文件2
  - **echo 内容 > 文件**：用内容覆盖文件1（完全覆盖）



#### 输出内容至命令行

- **echo [选项] [输出内容] -- 把内容输出至控制台**
  - echo $PATH ：查看环境变量
  - echo "hello"：输出一个hello
- **head [选项] 文件名 -- 输出指定文件的前几行，默认10 **
  - -n 行数：指定输出前几行，如 head -n 5 /etc/profile
- **tail [选项] 文件名 -- 输出指定文件的最后几行，默认10**
  - -n 行数：指定输出后几行，如 tail-n 5 /etc/profile
  - **-f ：实时监控文件变化，有变化就会看到（重要）**



#### 链接文件

- **ln -s [源文件/目录] [软链接名] -- 为源文件创建一个软链接（类似windows的快捷方式）**

  - 添加软链接：ln -s /root linkToRoot ---- **cd linkToRoot/ （注意最后要带/）会到root目录，但是此时pwd显示的还是linkToRoot所在的目录**

  - 删除软链接：**rm -rf linkToRoot （千万不要带最后的/，否则提示资源忙）**



#### 查看指令

- **history -- 显示执行过的历史指令**
  - 默认显示所有的历史指令
  - history 10 ：显示最后10个执行的历史指令
  - !178：执行历史编号178的指令



### 日期时间类指令

------



#### 显示时间

- **date **
  - **date**：显示当前时间
  - **date +%Y**：显示年（m月d日H时M分S秒）
  - **date "+%Y-%m-%d - %H:%M:%S"**：按引号内的格式显示时间信息

#### 显示日历

- **cal -- 显示当前月历**
- **cal 年份 -- 显示指定年历**



### 搜索查找类指令

------



#### find：普通搜索

- find 搜索范围 选项
  - **find /home -name 文件名**：按文件名查找
  - **find /opt -user 用户名**：按文件用户查找
  - **find / -size +20M**：按文件容量查找，+表示大于，-表示小于，直接写大小表示等于，+n，-n，n 与 +，-，不写 同样作用
  - **find / -name *.txt**：按通配符查找



#### locate与updatedb：快速搜索

- 在使用locate之前必须要使用updatedb创建locate数据库
- locate 文件名



#### grep与|管道符

- 管道符|表示把前面指令的结果输出给后面的指令使用
- **grep [选项] 搜索内容：查找指定内容**
  - -n：显示行号
  - 默认区分大小写
  - -i：忽略大小写

- 实例：cat hello.txt | grep -in yes



### 压缩类指令

------



#### gzip和gunzip：压缩文件

- **gzip 文件名1 文件名2 ... 文件名n**：压缩指定文件，压缩后**不保留源文件**
- **gunzip 文件名**：解压缩指定文件



#### zip和unzip：压缩文件（夹）

- **zip [选项] XXX.zip 要打包的文件名**
  - **zip -r ：打包文件目录**，如 zip -r mypackage.zip /home/

- **unzip [-d 目标路径] 要解压的文件**
  - 不指定时默认解压到压缩时的文件夹



#### tar：打包指令

打包好的文件为tar.gz格式

- **tar 选项 打包后的文件名 要打包的文件1 文件2 ... 文件n**
  - **tar -zcvf a.tar.gz a1.txt a2.txt ：打包文件**
  - **tar -zcvf a.tar.gz /home/ ：打包文件夹**
  - **tar -zxvf a.tar.gz ：解压文件到当前目录**
  - **tar -zxvf a.tar.gz -C /opt ：解压文件到指定目录（要求文件目录存在）**



## 4. 组管理和权限管理



### 组管理

------

#### 查看文件所有者、所在组

- ls -ahl

#### 修改文件所有者 、所在组

- **chown 新的所有者 文件名**：改变所有者，不改变文件所在组
- **chgrp 新的组名 文件名**：改变所在组，不改变所有者



### 权限管理

------

#### 权限介绍

-rw-r--r--. 1 所有者名 所在组名 文件大小 最后修改时间 文件名

- 第0位：文件类型
  - l：软链接
  - c：字符设备
  - -：文件
  - d：文件目录
  - b：块文件、磁盘
- **第1-3位：文件所有者权限（重要）**
  - r：读 4
    - 对文件：可读
    - 对文件目录：可读，可ls查看目录内容
  - w：写 2
    - 对文件：可以修改，但是不能删除，只有同时拥有对文件所在目录的w权限的用户才能删除
    - 对文件目录：可以修改，可以在目录内创建+删除+重命名文件
  - x：可执行 1
    - 对文件：可以执行
    - 对文件目录：可以进入目录
  - rx可以对文件目录进行进入和读取
- 第4-6位：文件所在组的其他用户权限
- 第7-9位：文件其他组的用户权限
- 最后一位：对文件，为硬链接个数；对文件目录，为子目录个数
  - **注意：.和..也是目录（隐藏），分别代表本目录和上级目录**
- 大小位：文件大小，**如果是文件目录，则固定为4096**



#### 权限管理

- **chmod 修改权限表达式 文件/目录名 -- 修改文件权限**
  - u：所有者
  - g：所在组
  - o：其他人
  - a：所有人
  - +：增加权限
  - -：减少权限
  - =：赋予权限
  - 实例：
    - chmod u=rwx,g=rx,o=rw abc.txt  权限之间用逗号隔开
    - chmod u-x,g+w abc
  - 数字方式的权限：4r 2w 1x
    - 实例：chmod 751 abc 相当于 u=rwx,g=rx,o=x

- **chown --- 修改文件所有者** 
  - **chown -R 新的用户名 文件目录名**：将该目录下所有的文件和子目录的所有者递归地改成新的用户
  - **chown 用户名:组名 文件名**：同时改变用户名和组名

- **chgrp -- 修改文件所在组**
  - **chgrp -R 新的组名文件目录名**：将该目录下所有的文件和子目录的所在组递归地改成新的组



## 5. 任务调度

核心文件：/etc/crontab 

核心命令：crontab



### 添加定时任务 crontab -e 

- crontab -e 进入命令编辑
- 实例：
  - */1 * * * * ls -l  /etc >> a.txt ： 每隔一分钟将/etc的文件信息追加记录到a.txt中，没有则创建
- 占位符说明
  - 第一位：分钟 0-59
  - 第二位：小时 0-23
  - 第三位：天数 1-31
  - 第四位：月份 1-12
  - 第五位：星期几 0-7（0/7都是周日）

- 特殊符号说明
  - *：所有
  - ,：分隔几个时间
    - 0 * 2，12 * * ：每个月的2号、12号的每个整点执行
  - -：连续的时间（1-10表示从1到10）
  - */n：表示每隔多久执行一次

- **使用shell脚本**
  - 新建脚本文件：xxx.sh，将指令写入该文件
  - 修改脚本文件的权限，添加可执行权限
  - 使用crontab -e命令
  - 添加 * * * * * /home/xxx.sh（全路径）
  - 保存即可



### 其他

------

#### 移除全部定时任务 crontab -r

#### 列出全部定时任务 crontab -l

#### 重启任务调度 service crond restart



## 6. 磁盘管理



### 磁盘分区和挂载

------

#### 磁盘分区类型

- mbr分区
  - 最多支持四个主分区
  - 系统只能安装在主分区
  - 扩展分区要占一个主分区
  - 最大支持2TB
  - 兼容性好
- gtp分区
  - 支持无限多个分区（操作系统可能限制，比如win限制128）
  - 最多支持18EB的容量（1EB = 1024 PB = 1024 * 1024 TB）
  - win7之后支持gtp



#### Linux的分区和挂载磁盘

- Linux的分区
  - **lsblk   或   lsblk -f** ：查看系统分区和挂载情况
    - 分区情况 分区类型 uuid 挂载点
  - ![linux分区](C:\Users\yanglin\Downloads\linux分区.png)

- 增加一块硬盘
  - 添加硬盘
  - 分区
    - fdisk /dev/sdb（sdb为新添加 的硬盘）
      - m
      - n
      - t
      - 1
      - 1
      - w
  - 格式化
    - mkfs -t ext4 /dev/sdb
  - 挂载
    - 先创建一个目录
    - mount /dev/sdb /home/newdisk（临时挂载）
  - 设置永久挂载（重启系统后仍然可以挂载）
    - vim /etc/fstab
    - 在该文件中添加：
      - /dev/sdb1               /home/newdisk          ext4         defaults       0   0   
    - mount -a
  - 卸载
    - umount /dev/sdb1 或 umount /newdisk



### 磁盘情况查询

------

#### 查询磁盘整体情况

- **df -l 或 df -lh**



#### 查询指定目录的磁盘使用情况

- **du -h 目录名** 
  - -c：显示总用量情况
  - -a：包含文件的相关信息
  - -h：带计量单位
  - -s：指定目录占用大小汇总
  - --max-depth=1：指定最大子目录的深度
  - 实例：
    - du -ach --max-depth=1 /opt



#### 实用指令（重要）

- 统计文件个数

  ls -l /home | grep “^-” | wc-l

- 统计目录个数

  ls -l /home | grep “^d” | wc-l

- 统计文件个数（含子目录）

  ls -lR /home | grep “^-” | wc-l

- 统计目录个数（含子目录）

  ls -lR /home | grep “^d” | wc-l

- 树状图显示目录结构（需要先 yum install tree 安装指令）

  tree



## 7. 网络配置

![linux网络](C:\Users\yanglin\Downloads\linux网络.png)

### 为Linux指定固定的ip地址

- 修改**/etc/sysconfig/network-scripts/ifcfg-eth0**
  - ONBOOT=yes
  - BOOTPROTO=static
  - IPADDR=指定的ip地址
  - GATEWAY=网关
  - DNS1=网关
- 重启网络服务
  - service network restart0



## 8. 进程管理（重要）



### 进程介绍

------

- 每个进程都有固定的id
- 每个进程都有自己的父进程，最顶的进程是init
- 父进程可以复制多个子进程
- 每个进程都可能以前台和后台两种形式存在



### 显示进程 ps

- ps -aux：a-显示所有进程 u-以用户格式显示 x-显示后台进程的运行参数

  ![image-20210128144312929](C:\Users\yanglin\AppData\Roaming\Typora\typora-user-images\image-20210128144312929.png)

  含义分析：

  - 从左到右：用户名   **进程号PID**   进程占用CPU的百分比   进程占用内存的百分比   进程占用的虚拟内存大小   进程占用的物理内存的大小   **终端名称TTY**   进程状态   启动时间  **占用CPU总时间TIME   启动过程使用的命令和参数CMD**（过长会被截断显示）
  - 进程状态中
    - S：睡眠
    - s：会话的先导进程
    - N：进程比普通进程优先级更低
    - R：正在运行
    - D：短期等待
    - Z：僵死进程
    - T：被跟踪或被停止

  - ps -aux | grep xxx：过滤显示的进程
  - ps -ef：显示全格式进程并查看父进程（PPID就是父进程号，0表示没有父进程）



### 终止进程 kill和killall

- kill 选项 进程号
  - -9：强制进程立即停止（包括系统的终端，包括自己）
  - 踢出非法用户：
    - ps -aux | grep 用户名
    - kill pid
  - 终止sshd
    - ps -aux | grep sshd
    - kill pid
- killall 进程名称
  - 同时杀掉父进程及其全部子进程



### 进程树显示 pstree

- -p：显示pid
- -u：显示所属用户

![20210128150317](C:\Users\yanglin\Documents\Scrshot\20210128150317.png)



### 服务（守护进程）管理

某些服务会在后台监听一个端口，这种监听端口的进程叫服务（如mysql、tomcat等）



#### 控制服务

- **service 服务名称 [start|restart|status|stop|reload]（CentOS 7.0后为systemctl status xxx.service）**
  - iptables：防火墙（可以通过win的telnet指令检查linux的某个端口是否打开并可以访问：**telnet ip 端口号**）
  - 这种操作是**临时**的，立即生效，重启后恢复



#### 查看服务名

- 方式一：setup
- 方式二：**ls -l /etc/init.d/**
- 7.0之后：**systemctl list-unit-files --type=service**



#### 服务的运行级别 chkconfig

- **vim /etc/inittab** 可以设置运行级别
- 开机的流程
  - 开机----BIOS----/boot----init进程----读取运行级别----**运行级别对应的服务启动**

- **chkconfig 每个服务可以设置自己在各个级别是否自启动，重启时生效**

  - chkconfig --list：查看全部服务的全部级别的启动

    ![20210128152622](C:\Users\yanglin\Documents\Scrshot\20210128152622.png)

  - chkconfig 服务名 --list：查看指定服务的全部级别

  - chkconfig --level 5 服务名 on/off：设置某个服务在某个级别的自启动，不指定level时默认修改所有级别



### 进程的监控

------

#### 动态监控 top

![image-20210128154110541](C:\Users\yanglin\AppData\Roaming\Typora\typora-user-images\image-20210128154110541.png)

top 选项 ---- 可以动态地监控进程的执行情况

- 选项
  - -p：监控指定PID的进程
  - -d 秒数：指定刷新时间，默认3s
  - -i：不显示僵死进程
- 交互操作
  - u：查看指定用户的进程
  - k：杀死指定PID的进程
  - N：按pid大小排序
  - P：按CPU使用率排序
  - M：按内存使用排序
  - q：退出



#### 网络进程监控 netstat

- **netstat -anp：查看所有的网络服务**
  - -p：显示哪个进程正在调用
  - 与管道符和grep指令配合使用，可以查看指定的网络服务



## 9. RPM和YUM

互联网下载包的打包与安装工具



### RPM包的管理

------

#### 查询RPM包

- 已安装的RPM列表：**rpm -qa**
  - 与| grep xxx组合使用
- 查询是否安装：**rpm -q 软件包名**
- 查询安装的软件信息：**rpm -qi 软件包名**

![image-20210128155835118](C:\Users\yanglin\AppData\Roaming\Typora\typora-user-images\image-20210128155835118.png)

- 查询软件安装的位置和所有的文件：**rpm -ql 包名**
- 查询文件所述的软件包：**rpm -qf 文件名**



#### 卸载RPM包

- **rpm -e 包名**
- 可能存在的依赖，强制删除需要用**rpm -e - -nodeps 包名**



#### 安装RPM包

- **rpm -ivh 安装包**
  - i：安装
  - v：提示
  - h：进度条
- 安装步骤：
  - 挂载centos的iso文件
  - cd到media
  - 从media中cp需要的包到/opt
  - rpm -ivh 包名即可



### YUM包的管理（可以自动去服务器安装，可以自动处理依赖）

前端软件包管理器，要联网，类似maven？

- **yum list | grep xxx**：查询yum包列表
- **yum install xxx**：安装