## 1.简介

cwRsync是基于cygwin平台的rsync软件包，支持windows对windows、windows对Linux、Linux对windows高效文件同步。由于CwRsync已经集成了cygwin类库，因此安装的时候可以省去cygwin包。Cwrsync还集成了OpenSSH for windows，可以实现Linux 下Rsync一模一样的操作。使用 cwRsync 来同步文件后，只需要对一台主服务器进行文件修改，其他镜像服务器可以自动同步，包括文件的更新、删除、重命名等。



## 2.安装

cwRsync的安装全部都是默认安装，对于服务器版本的安装，在安装过程中会提示要求我们输入Service account以及密码，如果我们不指定的话会使用SvcCWRSYNC这个账户，密码是随机生成的，最好自己设置一个密码，在后面可能会用到。

注意的两个点是：在服务启动时，如果失败，需要将启动账户SvcCWRSYNC和密码更新一下，就是上面提到的。

![img](https://i.loli.net/2021/04/19/uDP3YrmVOGMzEt7.png)

## 配置

### 1.服务端

cwRsync的架构很简单，有一个Server和多个Client组成。安装server版的cwRsync以后，在服务器上面启动cwRsync服务，然后在客户端上面执行文件同步命令即可实现文件同步功能。如果我们将文件同步命令添加到windows计划任务当中，就可实现定义同步的功能。

服务器端配置：

在cwRsync的安装目录下，可以找到一个rsyncd.conf的配置文件，下面为配置文件的修改方法：

```bash
use chroot = false
strict modes = false
hosts allow = *
log file = rsyncd.log
pid file = rsyncd.pid 
port = 8173 #默认端口873 
uid = 0 #不指定uid，不加这一行将无法使用任何账户 
gid = 0 #不指定gid 
max connections = 10 #最大连接数10 


# Module definitions
# Remember cygwin naming conventions : c:\work becomes /cygwin/c/work
#

[config]
path = /cygdrive/d/xxx/config #表示文件目录
read only = false
transfer logging = yes
lock file = rsyncd.lock
#auth users = service-scada #认证用户名
#secrets file = rsync.password #认证用户的用户名和密码存储位置
```

在配置完毕以后，我们接下来就需要启动cwRsync的服务，我们将此服务设定为自动启动，如下图所示：

![img](https://i.loli.net/2021/04/19/61nAdVGBwDrkyEQ.png)

还有，在上面我们指定RsyncServer的端口是873，我们可以通过netstat -an这个命令来检查873端口是否被监听，如下图所示：

![img](https://i.loli.net/2021/04/19/2lqCKayhgXmswLB.png)



![image-20210421211348589](https://i.loli.net/2021/04/21/uC9MoimqN1Ff8Er.png)

### 2.客户端

在安装完cwRsync的客户端以后，我们看到默认的安装目录是C:\Program Files\cwRsync，我们记下这个安装目录，后面会用到这个安装目录。

在客户端上新建一个记事本，在记事本中输入以下信息：

```
1 c:
2 cd C:\Program Files\cwRsync\bin
3 rsync -av rsync://服务端IP:873/config  /cygdrive/d/xxx/config 
```

然后再将此记事本重命名为config_rsync.bat，就形成了一个批处理文件。在批处理文件中，之所以需要添加第1、2行，是因为在安装cwRsync客户端的时候，并没有将cwRsync的程序目录添加到path这个环境变量当中，如果在环境变量path当中添加C:\Program Files\cwRsync\bin，则不需要在批处理中添加第1、2行。

第三行"rsync -av rsync://10.138.16.54:8173/config /cygdrive/d/xxx/config"的含义是从服务器同步config文件，同步到本地的D:\xxx\config目录下面。同步会以server上面的版本为准，如果在fe上面存在同名文件会被替换。



<h3>注意</h3>

在Windows系统内若rsyncd.conf 配置文件内有中文路径，需要将文件的格式转为UTF-8格式，否则可能出现错误。



