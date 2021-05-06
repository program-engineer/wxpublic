## 1.使用GitHub

​		用GitHub作为免费的远程仓库，如果是个人的开源项目，放到GitHub上是完全没有问题的。其实GitHub还是一个开源协作社区，通过GitHub，既可以让别人参与你的开源项目，也可以参与别人的开源项目。

​		在GitHub出现以前，开源项目开源容易，但让广大人民群众参与进来比较困难，因为要参与，就要提交代码，而给每个想提交代码的群众都开一个账号那是不现实的，因此，群众也仅限于报个bug，即使能改掉bug，也只能把diff文件用邮件发过去，很不方便。

​		但是在GitHub上，利用Git极其强大的克隆和分支功能，广大人民群众真正可以第一次自由参与各种开源项目了。

​		一定要从自己的账号下clone仓库，这样你才能推送修改。如果从作者的仓库地址``克隆，因为没有权限，你将不能推送修改。

​		如果你想修复一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送。

​		如果你希望官方库能接受你的修改，你就可以在GitHub上发起一个pull request。当然，对方是否接受你的pull request就不一定了。



### 1.1 配置SSH连接GitHub

1：检查本机是否有ssh key设置

$ cd ~/.ssh 或cd .ssh

如果没有则提示： No such file or directory ，有的话我们可以ls查看ssh文件。

在ssh下存在3个文件，其中id_rsa和id_rsa.pub是我们需要的密钥了。

id_rsa是私钥，id_rsa.pub是公钥。

2：创建密钥，生成ssh key

如果没有ssh，通过ssh-keygen -t rsa -C "输入你的邮箱" 创建密钥。

如果你有了还要创建密码，git会提示你是否需要覆盖（y/n）

当生成ssh key时

Enter file in which to save the key (/c/Users/diyvc/.ssh/id_rsa):   #不填直接回车

Enter passphrase (empty for no passphrase):   #输入密码（可以为空）

Enter same passphrase again:   #再次确认密码（可以为空）

生成如上图所示标识生成成功了。其存放路径为：.ssh/下，查看ssh文件夹下，会发现id_rsa和id_rsa_pub。

3：将ssh key添加到github中。

登录GitHub系统；点击右上角账号头像的→Settings→SSH and GPG keys→New SSH key。

![img](https://i.loli.net/2021/05/06/xvDmw4ajg1V6bSc.png)

4：配置账户

$ git config --global user.name “your_username”  #设置用户名

$ git config --global user.email “your_registered_github_Email”  #设置邮箱地址(建议用注册giuhub的邮箱)

### 1.2 从GitHub远程克隆

远程库已经准备好了，下一步是用命令git clone克隆一个本地库：

```bash
$ git clone git@github.com:mutihinking/gits.git
Cloning into 'gits'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.
```

要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

Git支持多种协议，包括https，但ssh协议速度最快。

### 1.3 本地修改后提交到Github

1.若远程存在相应的仓库：

**git add .** ：会监控工作区的状态树，使用它会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。

**git add -u** ：仅监控已经被add的文件（即tracked file），会将被修改的文件提交到暂存区。add -u 不会提交新文件（untracked file）。（git add –update的缩写）

**git add -A** ：是上面两个功能的合集（git add –all的缩写）

**git commit**  -m "添加test.md和testgit文件"  #提交记录说明 

提交到github中

**$ git push -u origin main/master**

刷新我们的github，就可以看见我们提交的项目和文件了。

2.若远程不存在相应的仓库：

#初始化

$ git init

#创建test.md文件

提交到本地

$ git add .   #提交当前目录下所以文件

$ git commit -m "添加test.md和testgit文件"   #提交记录说明提交到github中

$ git remote add origin ‘粘贴复制test ssh key的ssh路径’  

$ git push -u origin master

## 2.Git常用命令

![git速查表下载app](https://i.loli.net/2021/05/06/bBjYSfv1WgVsJ9l.png)

