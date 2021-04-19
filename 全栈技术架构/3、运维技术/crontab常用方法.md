Crontab是Linux系统中在固定时间执行某一个程序的工具，类似于Windows系统中的任务计划程序。一般格式如下：

```bash
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```

一、安装crontab（一般情况下系统均自带）

```bash
chkconfig crond on     #设为开机启动，安装chkconfig（yum install chkconfig） ###设置启动方式
service crond start  #启动
service crond stop  #停止
```

或者

```bash
/etc/rc.d/init.d/crond  start/stop/restart/reload
/etc/rc.d/init.d/crond  reload  #不中断服务，重新载入配置
```



二、crontab 参数的含义

【一般流程】 先crontab -l 查看当前 crontab 任务，删除或者新增都用  crontab -e


cron服务提供crontab命令来设定cron服务的,详细参数如下

crontab -u  //设定某个用户的cron服务，一般sh用户在执行这个命令的时候需要此参数

crontab -l   //列出某个用户cron服务的详细内容

crontab -e  //编辑某个用户的cron服务

查看crontab 日志

一般在 /var/log/cron ,用 tail  cron 查看


三、设置任务计划

/home/mvp/osyunwei.sh    #要自动执行的脚本程序路径

chmod +x /home/mvp/osyunwei.sh   #对脚本文件添加执行权限，否则不能执行

新增 crontab 

30 1 * * * sh /home/mvp/osyunwei.sh #表示每天凌晨1点30执行备份

:wq! #保存退出


service crond stop


service crond start


*/1 * * * * /   每分钟执行一次


crontab文件的格式：

minute    hour    day    month    weekday    username     command

minute：分，值为0-59

hour：小时，值为1-23

day：天，值为1-31

month：月，值为1-12

weekday：星期，值为0-6（0代表星期天，1代表星期一，以此类推）

username：要执行程序的用户，一般设置为sh

command：要执行的程序路径（设置为绝对路径）例如：/home/mvp/osyunwei.sh

*/2 每隔2

1-6 1到6

1,2,3,4  1、2、3、4

附crontab规则详细实例

1、每天6:00执行

0 6 * * * sh /home/mvp/osyunwei.sh

2、每周六凌晨4:00执行

0 4 * * 6 sh /home/mvp/osyunwei.sh

3、每周六凌晨4:05执行

5 4 * * 6 sh /home/mvp/osyunwei.sh 

4、每周六凌晨4:15执行

15 4 * * 6 sh /home/mvp/osyunwei.sh

5、每周六凌晨4:25执行

25 4 * * 6 sh /home/mvp/osyunwei.sh

6、每周六凌晨4:35执行

35 4 * * 6 sh /home/mvp/osyunwei.sh

7、每周六凌晨5:00执行

5 * * 6 sh /home/mvp/osyunwei.sh

8、每天8:40执行

40 8 * * * sh /home/mvp/osyunwei.sh

9、每天8:30执行

30 8 * * * sh /home/mvp/osyunwei.sh

10、每周一到周五的11:41开始，每隔10分钟执行一次    #值得借鉴

41,51 11 * * 1-5   sh /home/mvp/osyunwei.sh

或者

1-59/10 12-23 * * 1-5   sh /home/mvp/osyunwei.sh

11、在每天的10:31开始，每隔2小时重复一次

31 10-23/2 * * * sh   /home/mvp/osyunwei.sh

12、每天15:00执行

0 15 * * *  sh /home/mvp/osyunwei.sh

13、每天的10:30开始，每隔2小时重复一次

30 10-23/2 * * * sh  /home/mvp/osyunwei.sh

14、每天15:30执行

30 15 * * *  sh /home/mvp/osyunwei.sh

15、每天17:50执行

50 17 * * *  sh /home/mvp/osyunwei.sh

16、每天8:00执行

0 8 * * *  sh  /home/mvp/osyunwei.sh

17、每天18:00执行

0 18 * * *  sh  /home/mvp/osyunwei.sh

18、每天8:30执行

30 8 * * *  sh  /home/mvp/osyunwei.sh

19、每天20:30

30 20 * * *  sh /home/mvp/osyunwei.sh

20、每周一到周五2:00

0 2 * * 1-5 sh /home/mvp/osyunwei.sh

21、每周一到周五9:30

30 9 * * 1-5 sh /home/mvp/osyunwei.sh

22、每周一到周五8:00，每周一到周五9:00

0 8,9 * * 1-5  sh /home/mvp/osyunwei.sh

23、每天23:59

59 23 * * *  sh  /home/mvp/osyunwei.sh

24、每周六23:59

59 23 * * 6  sh    /home/mvp/osyunwei.sh

25、每天0:30

30 0 * * *  sh  /home/mvp/osyunwei.sh

26、每周一到周五9:25到11:35之间、13:00到15:00之间，每隔10分钟运行一次

分区段写值得借鉴

25,35,45,55  9 * * 1-5  sh   /home/mvp/osyunwei.sh

5-59/10  10 * * 1-5  sh   /home/mvp/osyunwei.sh

5,15,25,35  11 * * 1-5  sh   /home/mvp/osyunwei.sh

*/10  13-15 * * 1-5  sh   /home/mvp/osyunwei.sh

27、每周一到周五8:30、8:50、9:30、10:00、10:30、11:00、11:30、13:30、14:00、14:30、5:00分别执行一次

30,50 8 * * 1-5  sh  /home/mvp/osyunwei.sh

30 9 * * 1-5  sh  /home/mvp/osyunwei.sh

*/30 10-11 * * 1-5  sh  /home/mvp/osyunwei.sh

30 13 * * 1-5  sh  /home/mvp/osyunwei.sh

0,30 14-15 * * 1-5  sh  /home/mvp/osyunwei.sh

28、每天23:50执行

50 23 * * *  sh  /home/mvp/osyunwei.sh

29、每天10:00、16:00执行

0 10,16 * * *  sh /home/mvp/osyunwei.sh

30、每天5:30执行

30 5 * * *  sh  /home/mvp/osyunwei.sh

31、每周一到周五9:30执行

30 9 * * 1-5  sh  /home/mvp/osyunwei.sh

32、每周一到周五13:00执行

0 13 * * 1-5  sh  /home/mvp/osyunwei.sh

33、每天7:51执行

51 7 * * *  sh /home/mvp/osyunwei.sh

34、每天7:53、12:40分别执行一次

53 7 * * *  sh /home/mvp/osyunwei.sh

40 12 * * *  sh /home/mvp/osyunwei.sh

35、每天7:55执行

55 7 * * *  sh  /home/mvp/osyunwei.sh

36、每天8:10、16:00、20:00分别执行一次

10 8 * * *  sh  /home/mvp/osyunwei.sh

0 16 * * *  sh  /home/mvp/osyunwei.sh

0 20 * * *  sh  /home/mvp/osyunwei.sh

37、每天7:57、8:00分别执行一次

57 7 * * *  sh  /home/mvp/osyunwei.sh

0 8 * * *  sh  /home/mvp/osyunwei.sh