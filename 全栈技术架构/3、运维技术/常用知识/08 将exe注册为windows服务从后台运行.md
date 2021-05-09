# 方法：使用windows自带的命令sc

首先我们要打开cmd，下面的命令在cmd中运行，最好使用管理员运行cmd

### 注册服务：

```bash
 sc create ceshi binpath= D:\ceshi\ceshi.exe type= own start= auto displayname= ceshi
```

**binpath**：你的应用程序所在的路径。

**displayname**：服务显示的名称

注意：在"binpath=" 与“start=” 后面有空格。

### 如何判断服务是否注册成功：

在cmd中输入`services.msc`打开系统服务，查看是否出现`ceshi`名称的服务（即`displayname=`后面的参数，我这里是`ceshi`）

or

按下面的方式尝试启动服务

### 启动服务

```undefined
net start ceshi
```

### 停止服务

```undefined
net stop ceshi
```

### 删除服务

```cpp
sc delete "ceshi"
```



## 附: sc命令详解

SC命令详解(一个很有用的command)

作为一个命令行工具，SC.exe可以用来测试你自己的系统，你可以设置一个批处理文件来使用不同的参数调用 SC.exe来控制服务。

**一.SC使用这样的语法：**

```
SC [Servername] command Servicename [Optionname= Optionvalues] 
SC [command] 
```

这里使用第一种语法使用SC，使用第二种语法显示帮助。
下面介绍各种参数。 

```
Servername 
可选择：可以使用双斜线，如\\myserver，也可以是\\IP来操作远程计算机。如果在本地计算机上操作就不用添加任何参数。 
Command 
下面列出SC可以使用的命令。 
config----改变一个服务的配置。（长久的） 
continue--对一个服务送出一个继续控制的要求。 
control----对一个服务送出一个控制。 
create----创建一个服务。（增加到注册表中） 
delete----删除一个服务。（从注册表中删除） 
EnumDepend--列举服务的从属关系。 
GetDisplayName--获得一个服务的显示名称。 
GetKeyName--获得一个服务的服务键名。 
interrogate--对一个服务送出一个询问控制要求。 
pause----对一个服务送出一个暂停控制要求。 
qc----询问一个服务的配置。 
query----询问一个服务的状态，也可以列举服务的状态类型。 
start----启动一个服务。 
stop----对一个服务送出一个停止的要求。
```



Servicename  在注册表中为service key制定的名称。注意这个名称是不同于显示名称的（这个名称可以用net start和服务控制面板看到），而SC是使用服务键名来鉴别服务的。 

Optionname  这个optionname和optionvalues参数允许你指定操作命令参数的名称和数值。注意，这一点很重要在操作名称和等号之间是没有空格的。

optionvalues可以是0，1，或者是更多的操作参数名称和数值对。 如果你想要看每个命令的可以用的optionvalues，你可以使用sc command这样的格式。这会为你提供详细的帮助。

Optionvalues 为optionname的参数的名称指定它的数值。有效数值范围常常限制于哪一个参数的optionname。如果要列表请用 sc command来询问每个命令。

Comments 很多的命令需要管理员权限，在你操作这些东西的时候最好是管理员。当你键入SC而不带任何参数时，SC.exe会显示帮助信息和可用的命令。当你键入SC紧跟着命令名称时，你可以得 到一个有关这个命令的详细列表。比如，键入sc create可以得到和create有关的列表。 但是除了一个命令，sc query，这会导出该系统中当前正在运行的所有服务和驱动程序的状态。 当你使用start命令时，你可以传递一些参数（arguments）给服务的主函数，但是不是给服务进程的主函数。

**二.SC create**

这个命令可以在注册表和服务控制管理数据库建立一个入口。 

语法1 

这里的servername，servicename，optionname，optionvalues和上面的一样，这里就不多说了。这里我们详细说明一下optionname和optionvalues。


 Optionname--Optionvalues 

描述 

type=----own, share, interact, kernel, filesys 关于建立服务的类型，选项值包括驱动程序使用的类型，默认是share。 

start=----boot, sys tem, auto, demand, disabled 关于启动服务的类型，选项值包括驱动程序使用的类型，默认是demand（手动）。

 error=----normal, severe, critical, ignore 当服务在导入失败错误的严重性，默认是normal。

binPath=--(string) 服务二进制文件的路径名，这里没有默认值，这个字符串是必须设置的。 

group=----(string) 这个服务属于的组，这个组的列表保存在注册表中的ServiceGroupOrder下。默认是nothing。 

tag=----(string) 如果这个字符串被设置为yes，sc可以从CreateService call中得到一个tagId。然而，SC并不显示这个标签，所以使用这个没有多少意义。默认是nothing 

depend=----(space separated string)有空格的字符串。 在这个服务启动前必须启动的服务的名称或者是组。

obj=----(string) 账号运行使用的名称，也可以说是登陆身份。默认是localsys tem 

Displayname=--(string) 一个为在用户界面程序中鉴别各个服务使用的字符串。 

password=--(string) 一个密码，如果一个不同于localsystem的账号使用时需要使用这个。 
