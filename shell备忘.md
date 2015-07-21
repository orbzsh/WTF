##**shell**的配置文件
-	./bash_profile 用户登录的时候读取,其中包含的命令被执行
-	./bashrc 启动新的shell时被读取,并执行
-	./bash_logout shell登出时被读取

##**shell**初始化过程	
	if [ -f /etc/profile ];then
		/bin/bash /etc/profile
	elif [ -f ./bash_profile ];then
		/bin/bash ./bash_profile
	elif [ -f ./bash_login ];then
		/bin/bash ./bash_login
	elif [ -f ./profile ];then
		/bin/bash ./profile
	fi
	
###**set**选项介绍
-	**set -o errexit** 设置这个选项后,当一个命令执行失败时,shell立即退出
-	**set -o noexec** 当设置后,shell读取命令,但不会执行它们,可以用来检测shell脚本是否有语法错误
-	**set -o strace** 当设置后,对于每一条要执行的命令,在执行之前,输出trace到stderr

###**trap**