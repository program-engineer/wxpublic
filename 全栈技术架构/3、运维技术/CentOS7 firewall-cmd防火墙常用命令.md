Centos7 使用的是filewall（-cmd）
iptables 用于过滤数据包，属于网络层防火墙。
firewall 能够允许哪些服务可用，那些端口可用...属于更高一层的防火墙。

1.firewalld的基本使用
启动：  systemctl start firewalld
查看状态：systemctl status firewalld 
停止：  systemctl disable firewalld
禁用：  systemctl stop firewalld
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed

2.配置firewalld-cmd
查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看防火墙规则： firewall-cmd --list-all 
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
查看区域信息:  firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic

3.通过firewall-cmd 开放端口
firewall-cmd --zone=public --add-port=80/tcp --permanent   #作用域是public，开放tcp协议的80端口，一直有效
firewall-cmd --zone=public --add-port=80-90/tcp --permanent #作用域是public，批量开放tcp协议的80-90端口，一直有效
firewall-cmd --zone=public --add-port=80/tcp  --add-port=90/tcp --permanent #作用域是public，批量开放tcp协议的80、90端口，一直有效
firewall-cmd --zone=public --add-service=http --permanent #开放的服务是http协议，一直有效
firewall-cmd --reload    # 重新载入，更新防火墙规则，这样才生效。通过systemctl restart firewall 也可以达到
firewall-cmd --zone= public --query-port=80/tcp  #查看tcp协议的80端口是否生效
firewall-cmd --zone= public --remove-port=80/tcp --permanent  # 删除
firewall-cmd --list-services
firewall-cmd --get-services
firewall-cmd --add-service=<service>
firewall-cmd --delete-service=<service>
在每次修改端口和服务后/etc/firewalld/zones/public.xml文件就会被修改,所以也可以在文件中之间修改,然后重新加载
使用命令实际也是在修改文件，需要重新加载才能生效。

 

4.使用备忘
firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.0.4/24" service name="http" accept'    //设置某个ip访问某个服务
firewall-cmd --permanent --zone=public --remove-rich-rule='rule family="ipv4" source address="192.168.0.4/24" service name="http" accept' //删除配置
firewall-cmd --permanent --add-rich-rule 'rule family=ipv4 source address=192.168.0.1/2 port port=80 protocol=tcp accept'     //设置某个ip访问某个端口
firewall-cmd --permanent --remove-rich-rule 'rule family=ipv4 source address=192.168.0.1/2 port port=80 protocol=tcp accept'     //删除配置

firewall-cmd --query-masquerade  # 检查是否允许伪装IP
firewall-cmd --add-masquerade    # 允许防火墙伪装IP
firewall-cmd --remove-masquerade # 禁止防火墙伪装IP

firewall-cmd --add-forward-port=port=80:proto=tcp:toport=8080   # 将80端口的流量转发至8080
firewall-cmd --add-forward-port=proto=80:proto=tcp:toaddr=192.168.1.0.1 # 将80端口的流量转发至192.168.0.1
firewall-cmd --add-forward-port=proto=80:proto=tcp:toaddr=192.168.0.1:toport=8080 # 将80端口的流量转发至192.168.0.1的8080端口

Centos7以前命令备忘
1.开放80，22，8080 端口
/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 22 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
2.保存
/etc/rc.d/init.d/iptables save
3.查看打开的端口
/etc/init.d/iptables status
4.关闭防火墙 
1） 永久性生效，重启后不会复原
开启： chkconfig iptables on
关闭： chkconfig iptables off
2） 即时生效，重启后复原
开启： service iptables start
关闭： service iptables stop