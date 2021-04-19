## 1.简介

 		cwRsync是基于cygwin平台的rsync软件包，支持windows对windows、windows对Linux、Linux对windows高效文件同步。由于CwRsync已经集成了cygwin类库，因此安装的时候可以省去cygwin包。Cwrsync还集成了OpenSSH for windows，可以实现Linux 下Rsync一模一样的操作。使用 cwRsync 来同步文件后，只需要对一台主服务器进行文件修改，其他镜像服务器可以自动同步，包括文件的更新、删除、重命名等。

## 2.安装

​		cwRsync的安装全部都是默认安装，对于服务器版本的安装，在安装过程中会提示要求我们输入Service account以及密码，如果我们不指定的话会使用SvcCWRSYNC这个账户，密码是随机生成的，最好自己设置一个密码，在后面可能会用到。

​		注意的两个点是：在服务启动时，如果失败，需要将启动账户密码更新一下，就是上面提到的。

![img](https://i.loli.net/2021/04/19/uDP3YrmVOGMzEt7.png)