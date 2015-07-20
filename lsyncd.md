##lsyncd同步文件工具
------
###安装

	安装ali的repo源,直接yum -y install lsyncd
------
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
	
	-- 本地目录同步,direct: cp/rm/mv。适用：500+万文件,变动不大
	sync {
		default.direct,
		source = "/tmp/src",
		target = "/tmp/dest",
		delay = 1,
		maxProcesses = 1
	}

	-- 本地目录同步,rsync模式:rsync
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
			bwlimit = 2000,
		}
	}

	-- 远程目录同步,rsync模式+rsyncd daemon
	sync {
		default.rsync,
		source = "/tmp/src",
		target = "user@192.168.1.10::module",
		delete = "running",
		exclude = { ".*", ".tmp"},
		delay = 30,
		init = false,
		rsync = {
			binary = "/usr/bin/rsync",
			archive = true,
			compress = true,
			verbose = true,
		}
	}

	--远程目录同步, rsync模式+ssh shell
	sync {
		default.rsync,
		source = "/tmp/src",
		target = "192.168.1.10:/tmp/dest",
		maxDelay = 5,
		delay = 30,
		init = true,
		rsync = {
			binary = "/usr/bin/rsync",
			archive = true,
			compress = true,
			verbose = true,
		}
	}

	--远程目录同步,rsync模式+rsyncssh
	sync {
		default.rsync,
		source = "/tmp/src",
		host = "192.168.1.10",
		targetdir = "/tmp/dest",
		maxDelay = 5,
		delay = 30,
		init = true,
		rsync = {
			binary = "/usr/bin/rsync",
			archive = true,
			compress = true,
			verbose = true,
		}
		ssh = {
			port = 57522
		}
	}
------
###lsyncd.conf配置文件说明
> ####**settings**里面是全局配置,--开头表示注释,下面是常用选项说明:
>
> 	+ **logfile** 定义日志文件
> 	+ **statusFile** 定义状态文件
> 	+ **nodaemon = true** 表示不启用守护模式,默认
> 	+ **statusInterval** 将lsyncd的状态写入上面的statusFile的间隔,默认10秒
> 	+ **maxProcesses** 同步进程的最大个数,假如同时有20个文件需要同步,而 **maxProcesses = 8**,则最大能看到有8个rsync进程
> 	+ **maxDelays** 累计到多少监控的事件激活一次同步,即使后面的**delay** 延迟时间还未到

> ####**sync**
>	里面是定义同步参数,可以继续使用**maxDelays**来重写**settings**的全局变量,一般第一个参数指定**lsyncd**以什么模式执行:**rsync**、**rsyncssh**、**direct**三种模式

>	+ **default.rsync**: 本地目录间同步,使用**rsync**,也可以达到使用**ssh**形式的远程**rsync**效果，或**daemon**方式连接远程**rsyncd**进程
	+ **default.direct**: 本地目录间同步,使用**cp**、**rm**、等命令完成差异文件备份
	+ **default.rsyncssh**: 同步到远程主机目录,rsync的ssh模式,需要使用key认证

>	+ **source** 同步的源目录,使用绝对路径
>	+ **target** 定义目的地址,对应几种写法:
	+ **/tmp/dest**:本地同步目录,可用于**direct**和**rsync**模式
	+ **192.168.1.10:/tmp/dest**: 同步到远程服务器目录,可用于**rsync**和**rsyncssh**模式
	+ **192.168.1.10:module**: 同步到远程服务器目录,用于**rsync**模式

>	+ **init**这是一个优化选项,当**init = true**,只同步进程启动后发生改动事件的文件,原有的目录即使有差异也不会同步.默认是**true**
>	+ **delay**累计事件,等待rsync同步延迟时间,默认是15秒,也就是15s内监控目录下发生的改动，会累积到一次rsync同步，避免过于频繁的同步
>	+ **excludeFrom**排除选项,后面指定排除的列表文件,如:**excludeFrom = "/etc/lsyncd.d/exclude.lst"**,这里的排除规则写法与原生rsync有点不同
	+ 监控路径里的任何部分匹配到一个文本,都会被排除,例如**/bin/foo/bar**可以匹配规则**foo**
	+ 如果规则以斜线/开头,则从头开始要匹配全部
	+ 如果规则以/结尾,则要匹配监控路径的末尾
	+ ?匹配任何字符,但不包括/
	+ *匹配0或多个字符,但不包括/
	+ **匹配0或多个字符,可以是/
>	+ **delete** 为了保持**target**和**source**完全同步,Lsyncd默认会**delete = true**来允许同步删除

> ####**rsync**
>	+ **bwlimit** 限速，单位**kb/s**,与**rsync**相同
>	+ **compress** 压缩传输默认为**true**.在带宽与**cpu**负载之间权衡,本地目录同步可以考虑把它设为**false**
>	+ **perms** 默认保留文件权限
> ####**lsyncd.conf**可以有多个**sync**,各自的**source**,各自的**target**,各自的模式,互不影响

------
###启动lsyncd
####使用命令行加载配置文件,启动守护进程,自动同步目录
	lsyncd -log Exec /etc/lsyncd.conf

------
###lsyncd的其他功能
**官方手册**[Lsyncd ManPage](https://github.com/axkibe/lsyncd/wiki/Manual%20to%20Lsyncd%202.1.x)