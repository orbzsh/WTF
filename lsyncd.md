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
**settings**里面是全局配置,--开头表示注释,下面是常用选项说明:
```
+ **logfile** 定义日志文件
+ **statusFile** 定义状态文件
+ nodaemon = true 表示不启用守护模式,默认
+ statusInterval 
```