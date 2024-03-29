使用VMware克隆一个新虚拟机，启动系统后，虚拟网卡ens33的ip地址可能无法使用，尝试重启网络后，会发生如下错误：
```
Job for network.service failed because the control process exited with error code
network.service - LSB: Bring up/down networking
Loaded: loaded (/etc/rc.d/init.d/network; bad; vendor preset: disabled)
Active: failed (Result: exit-code) since 五 2017-07-14 19:01:47 CST; 1min 16s ago
Docs: man:systemd-sysv-generator(8)
Process: 4681 ExecStart=/etc/rc.d/init.d/network start (code=exited, status=1/FAILURE)
。。。。。。。
```

原因(从网上找到的回答)：
在CentOS系统上，目前有NetworkManager和network两种网络管理工具。
如果两种都配置会引起冲突，而且NetworkManager在网络断开的时候，会清理路由，
如果一些自定义的路由，没有加入到NetworkManager的配置文件中，路由就被清理掉，网络连接后需要自定义添加上去。

解决：
```
systemctl stop NetworkManager
systemctl disable NetworkManager
systemctl restart network
service network restart
```
设置主机名(hostname)：
```
hostnamectl  set-hostname NEW_NAME
```
NEW_NAME替换成你的新主机名
