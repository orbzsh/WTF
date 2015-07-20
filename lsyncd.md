##lsyncd同步文件工具
------
###安装

	安装ali的repo源,直接yum -y install lsyncd

###lsyncd同步配置

	#mkdir -p /var/log/lsyncd
	#mkdir -p /etc/lsyncd.d
	vim /etc/lsyncd.conf
	settings {
		logfile = "/var/log/lsyncd/lsyncd.log",
		statusFile = "/var/log/lsyncd/lsyncd.status",
		inotifiMode = "CloseWrite",
		maxProcesses = 7,
		-- nodaemon = true,
	}
	sync{
	default.rsync,
	source = "/tmp/src",
	target = "/tmp/dest",
	-- exludeFrom = "/etc/lsyncd.d/rsync_exclude.lst",
	rsync= {
			binary = "/usr/bin/rsync",
			archive = true,
			compress = true,
			verbose = true,
		}
	}

###lsyncd.conf配置文件说明
> **settings**里面是全局配置,--开头表示注释,下面是常用选项说明:
>
> 	+ **logfile** 定义日志文件
> 	+ **statusFile** 定义状态文件
> 	+ **nodaemon = true** 表示不启用守护模式,默认
> 	+ **statusInterval** 将lsyncd的状态写入上面的statusFile的间隔,默认10秒
> 	+ **maxProcesses** 同步进程的最大个数,假如同时有20个文件需要同步,而 **maxProcesses = 8**,则最大能看到有8个rsync进程
> 	+ **maxDelays** 累计到多少监控的事件激活一次同步,即使后面的**delay** 延迟时间还未到

> **sync**
>	里面是定义同步参数,可以继续使用**maxDelays**来重写**settings**的全局变量,一般第一个参数指定**lsyncd**以什么模式执行:**rsync**、**rsyncssh**、**direct**三种模式

>	+ **default.rsync**: 本地目录间同步,使用**rsync**,也可以达到使用**ssh**形式的远程**rsync**效果，或**daemon**方式连接远程**rsyncd**进程
	  **default.direct**: 本地目录间同步,使用**cp**、**rm**、等命令完成差异文件备份
	  **default.rsyncssh**: 同步到远程主机目录,rsync的ssh模式,需要使用key认证
>	+ **source** 同步的源目录,使用绝对路径
>	+ **target** 定义目的地址,