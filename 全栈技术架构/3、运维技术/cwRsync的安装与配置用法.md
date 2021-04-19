## 1.简介

 		cwRsync是基于cygwin平台的rsync软件包，支持windows对windows、windows对Linux、Linux对windows高效文件同步。由于CwRsync已经集成了cygwin类库，因此安装的时候可以省去cygwin包。Cwrsync还集成了OpenSSH for windows，可以实现Linux 下Rsync一模一样的操作。使用 cwRsync 来同步文件后，只需要对一台主服务器进行文件修改，其他镜像服务器可以自动同步，包括文件的更新、删除、重命名等。

## 2.安装

​		cwRsync的安装全部都是默认安装，对于服务器版本的安装，在安装过程中会提示要求我们输入Service account以及密码，如果我们不指定的话会使用SvcCWRSYNC这个账户，密码是随机生成的，所以要记住这个密码。但是在我后面的配置中，并没有用到这个Service account。

![img](https://images0.cnblogs.com/blog/80896/201310/11103143-f027e467c47a44c8b5c32af43789c03d.png)