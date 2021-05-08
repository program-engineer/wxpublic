 

## ***一、简介***

NFS（Network File System）即网络文件系统，它允许网络中的计算机之间通过网络共享资源。将NFS主机分享的目录，挂载到本地客户端当中，本地NFS的客户端应用可以透明地读写位于远端NFS服务器上的文件，在客户端端看起来，就像访问本地文件一样。 

RPC，基于C/S模型。程序可以使用这个协议请求网络中另一台计算机上某程序的服务而不需知道网络细节，甚至可以请求对方的系统调用。 

对于Linux而言，文件系统是在内核空间实现的，即文件系统比如ext3、ext4等是在Kernel启动时，以内核模块的身份加载运行的。 

## ***二、原理***

NFS本身的服务并没有提供数据传递的协议，而是通过使用RPC（远程过程调用 Remote Procedure Call）来实现。当NFS启动后，会随机的使用一些端口，NFS就会向RPC去注册这些端口。RPC就会记录下这些端口，RPC会开启111端口。通过client端和sever端端口的连接来进行数据的传输。在启动nfs之前，首先要确保rpc服务启动。 

具体过程如下： 

![img](https://i.loli.net/2021/05/08/fwBZFuvgd3WH8LO.jpg)

1、本地用户要访问nfs服务器中文件，先向内核发起请求，内核处理调用nfs模块及rpc client 

2、rpc client向rpc server发起连接 

3、在连接之前，NFS服务除了启动nfsd本身监听的端口2049/tcp和2049/udp，还会启动其它进程（如mountd，statd，rquotad等）以完成文件共享，这些进程的端口是不固定的；是每次NFS服务启动时向RPC服务注册的，RPC服务会随机分配未使用的端口 

4、完成连接，接受访问请求 

5、nfs应用程序向内核发起请求 

6、内核调用文件系统 

7、然后client端通过获取的NFS端口来建立和server端的NFS连接并进行数据的传输。

 

以下为启动***各服务的作用*** 

***rpc：***远程过程调用协议，是实现本地调用远程主机实现系统调用的协议。 

***portmapper：***负责分配rpc server的端口，并在client端请求时，负责响应目的rpc server端口返回给client端，工作在tcp与udp的111端口上。 

***mountd：***是nfs服务的认证服务的守护进程，client在收到返回的真正端口时，就会去连接mountd，认证取得令牌。 

***nfsd：***nfs的守护进程，负责接收到用户的调用请求后与内核发出请求并得到调用结果响应给用户，工作在tcp和udp的2049端口。 

attempt：***是NFS的一个程序，用来负责远程client端创建文件后的权限问题。 

***quotad：***用于实现磁盘配额，当client端挂载nfs后可以限制磁盘空间的大小。 

 

## ***\*三、NFS服务配置安装\****

 

![img](https://i.loli.net/2021/05/08/4NdV9keXw6PYqKt.jpg)

 

***相关配置文件及命令的使用*** 

/etc/exports 

/path/to/somedir CLIENT_LIST 

  多个客户之间使用空白字符分隔 

每个客户端后面必须跟一个小括号，里面定义了此客户访问特性，如访问权限等

172.16.0.0/16(ro,async) 192.16.0.0/24(rw,sync) *(ro) 

权限属性： 

  ro:只读 

  rw:读写 

  sync:同步，数据同步写到内存与硬盘中 

  async:异步，数据先暂存内存 

  root_squash: 将root用户映射为来宾账号 

  no_root_squash: 有root的权限，不建议使用 

  all_squash: 全部映射为来宾账号 

  anonuid, anongid: 指定映射的来宾账号的UID和GID 

 

exportfs命令： 

  -a：跟-r或-u选项同时使用，表示重新挂载所有文件系统或取消导出所有文件系统； 

  -r: 重新导出 

  -u: 取消导出 

  -v: 显示详细信息 

showmount命令： 

showmount -e NFS_SERVER: 查看NFS服务器"导出"的各文件系统 

showmount -a NFS_SERVER: 查看NFS服务器所有被挂载的文件系统及其挂载的客户端对应关系列表 

showmount -d NFS_SERVER: 显示NFS服务器所有导出的文件系统中被客户端挂载了文件系统列表 

rpcinfo  -p hostname(orIP) 

-p ：显示所有的 port 与 program 的信息！ 

如果要让mountd和quotad等进程监听在固定端口，编辑配置文件/etc/sysconfig/nfs 

客户端使用mount命令挂载 

mount -t nfs NFS_SERVER:/PATH/TO/SOME_EXPORT /PATH/TO/SOMEWHRERE 

***安装配置*** 

环境准备： 

server端：192.168.1.222 centos 7 

client端：192.168.1.200 centos 6.5 

　　1.在服务端安装nfs， 

　　# yum install nfs-utils rpcbind -y 

  ![img](https://i.loli.net/2021/05/08/l6t7b1HyRM2mN4z.jpg)

　　2.编辑/etc/exports，并启动nfs 

　![img](https://i.loli.net/2021/05/08/gdCu3ZHkSFn4Yiw.jpg)

　　#systemctl start nfs 

　　3.客户端同样安装nfs-utils和rpcbind并启动，必须先启动rpcbind，否则报错（注意防火墙等） 

　![img](https://i.loli.net/2021/05/08/HxIfWiTsJn73pcu.jpg)

　　4.挂载并查看挂载信息 

　　# mount -t nfs 192.168.1.222:/var/nfs /mnt 

　　#showmount –e 192.168.1.222 

![img](https://i.loli.net/2021/05/08/TQM8enk4CKxgNYq.jpg)

　1、在服务器端/var/nfs创建目录或文件，并在客户端/mnt查看即可。 

将所有用户映射为来宾账号实验 

2、在服务器端添加用户hot，并修改配置文件并重新挂载文件系统 

3、添加用户 

useradd –u 520 hot 

修改/etc/exports 

/var/nfs  192.168.1.0/24(rw,async,all_squash,anonuid=520) 

重新挂载导出 

exportfs –ra 

![img](https://i.loli.net/2021/05/08/3lNwdyqSHftFebr.jpg)

在客户端上添加用户code，分别在code用户和root用户下创建文件，查看文件属性 

![img](https://i.loli.net/2021/05/08/ZfGIgULKzwxuqDN.jpg)

![img](https://i.loli.net/2021/05/08/bsLGiI9ZOCjyNYw.jpg)

可以看到文件属主都为服务器端设置好的来宾账号hot的uid 

让mountd和quotad等进程监听在固定端口，编辑配置文件/etc/sysconfig/nfs，取消注释 

![img](https://i.loli.net/2021/05/08/zqSRfKiFD5ewjYs.jpg)

重启nfs，查看端口 

![img](https://i.loli.net/2021/05/08/nDZ8omscrEXjYBW.jpg)

## ***四、固定端口及防火墙设置***

1)服务器开启111和2049端口，111为rpcbind对应端口，2049为nfsd对应端口

```
firewall-cmd --permanent --add-port=2049/tcp
firewall-cmd --permanent --add-port=2049/udp
firewall-cmd --permanent --add-port=111/tcp
firewall-cmd --permanent --add-port=111/udp
firewall-cmd --reload
```

2)编辑/etc/sysconfig/nfs 修改默认端口，在末尾添加以下内容：

```
RQUOTAD_PORT=30001
LOCKD_TCPPORT=30002
LOCKD_UDPPORT=30002
MOUNTD_PORT=30003
STATD_PORT=30004
```

3)开启添加的端口

```
firewall-cmd --permanent --add-port=30001/tcp
firewall-cmd --permanent --add-port=30001/udp
firewall-cmd --permanent --add-port=30002/tcp
firewall-cmd --permanent --add-port=30002/udp
firewall-cmd --permanent --add-port=30003/udp
firewall-cmd --permanent --add-port=30003/tcp
firewall-cmd --permanent --add-port=30004/tcp
firewall-cmd --permanent --add-port=30004/udp
firewall-cmd --reload
```

4)重启服务

```
systemctl restart nfs
```

5)客户端查看服务器共享目录

```
[root@nfs-client ~]# showmount -e 192.168.172.190
Export list for 192.168.172.190:
/data 192.168.172.0/24
```


