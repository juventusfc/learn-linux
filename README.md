# Linux 学习笔记

## 安装

1. 下载并安装 VirtualBox

2. 安装 Ubuntu。可以选择 desktop 版和 server 版的，学习时可以先使用 desktop 版过度下。

   在安装时，选择 RAM 和硬盘要稍微大点，不然会出现安装时键盘无法输入的情况。安装时，若出现窗口太小问题，使用`Alt+F7`拖动窗口。Linux 系统的分区主要有：

   1. 启动分区`/boot`，用于启动 Linux 系统
   2. swap 分区，主要当作虚拟内存
   3. 根分区`/`

## 虚拟机的网络模式

![network](./images/network.png)

网络模式主要有桥接模式/NAT 模式/主机模式。各个模式的区别主要见上图。上图中，本地网络是 192.168.0.xx 网段，李四/王五/陆六分别创建了 Linux 虚拟机。根据网络模式的区别，虚拟机与主机即本地网络的网络连接行为不一致。

主要区别见下图：

|                   | NAT | 桥接 | Internal       | Host-Only    |
| ----------------- | --- | ---- | -------------- | ------------ |
| 虚拟机 → 主机     | √   | √    | ×              | 默认不需设置 |
| 主机 → 虚拟机     | ×   | √    | ×              | 默认不需设置 |
| 虚拟机 → 其他主机 | √   | √    | ×              | 默认不需设置 |
| 其他主机 → 虚拟机 | ×   | √    | ×              | 默认不需设置 |
| 虚拟机之间        | ×   | √    | 同网络名下可以 | √            |

## 虚拟机的配置

### 窗口自适应

1. Click `Devices`
2. Click `Insert guest Additions CD Image`

### 虚拟机和主机之间的复制粘贴

1. Click `Devices`
2. Click `Shared Clipboard`
3. Choose `Bidirectional`

### 虚拟机和主机之间的文件拖拽

1. Click `Devices`
2. Click `Drag and Drop`
3. Choose `Bidirectional`

### 虚拟机和主机之间的文件(夹)共享

1. Click `Devices`
2. Click `Shared Folders`
3. 根据提示选择共享文件(夹)

## Linux 文件系统目录结构

Linux 不像 Windows 一样有 C 盘/D 盘等，Linux 有且只有一个根目录，即`/`目录。

在 Linux 中，一切皆文件。比如，cpu 是文件，目录是文件，硬盘也是文件。

主要的目录包括：

- /bin：bin 是 Binary 的缩写, 这个目录存放着最经常使用的命令。
- /boot：这里存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件。
- /dev ：dev 是 Device(设备)的缩写, 该目录下存放的是 Linux 的外部设备，在 Linux 中访问设备的方式和访问文件的方式是相同的。
- /etc：这个目录用来存放所有的系统管理所需要的配置文件和子目录。
- /home：用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。
- /lib：这个目录里存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要用到这些共享库。
- /lost+found：这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。
- /media linux 系统会自动识别一些设备，例如 U 盘、光驱等等，当识别后，linux 会把识别的设备挂载到这个目录下。
- /mnt：系统提供该目录是为了让用户临时挂载别的文件系统的。
- /opt：这是给主机额外安装软件所摆放的目录。比如你安装一个 ORACLE 数据库则就可以放到这个目录下。默认是空的。
- /proc：这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的 ping 命令，使别人无法 ping 你的机器：`echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all`
- /root：该目录为系统管理员，也称作超级权限者的用户主目录。
- /sbin：s 就是 Super User 的意思，这里存放的是系统管理员使用的系统管理程序。
- /selinux：这个目录是 Redhat/CentOS 所特有的目录，Selinux 是一个安全机制，类似于 windows 的防火墙，但是这套机制比较复杂，这个目录就是存放 selinux 相关的文件的。
- /srv：该目录存放一些服务启动之后需要提取的数据。
- /sys：这是 linux2.6 内核的一个很大的变化。该目录下安装了 2.6 内核中新出现的一个文件系统 sysfs 。sysfs 文件系统集成了下面 3 种文件系统的信息：针对进程信息的 proc 文件系统、针对设备的 devfs 文件系统以及针对伪终端的 devpts 文件系统。该文件系统是内核设备树的一个直观反映。当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。
- /tmp：这个目录是用来存放一些临时文件的。
- /usr：这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与 windows 下的 program files 目录。
- /usr/bin：系统用户使用的应用程序。
- /usr/sbin：超级用户使用的比较高级的管理程序和系统守护程序。
- /usr/src：内核源代码默认的放置目录。
- /var：这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。

## 远程访问

实际工作中，真实环境是部署在远程的，程序员需要在本地远程访问服务器。远程访问服务器需要：

1. 被访问的 Linux 开启 sshd 服务
2. 访问的机器安装
   1. XShell(收费)或者 PuTTY(免费)：远程登录软件。在 Linux 中使用`ifconfig`指令查找 Linux 的网络地址，然后使用 XShell 连接至这个 Linux 的网络地址。
   2. XFtp：远程上传和下载软件
