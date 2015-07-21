###**shell**的配置文件
-	./bash_profile 用户登录的时候读取,其中包含的命令被执行
-	./bashrc 启动新的shell时被读取,并执行
-	./bash_logout shell登出时被读取
------
###**shell**初始化过程	
	if [ -f /etc/profile ];then
		/bin/bash /etc/profile
	elif [ -f ./bash_profile ];then
		/bin/bash ./bash_profile
	elif [ -f ./bash_login ];then
		/bin/bash ./bash_login
	elif [ -f ./profile ];then
		/bin/bash ./profile
	fi
------	
###**set**选项介绍
-	**set -o errexit** 设置这个选项后,当一个命令执行失败时,shell立即退出
-	**set -o noexec** 当设置后,shell读取命令,但不会执行它们,可以用来检测shell脚本是否有语法错误
-	**set -o strace** 当设置后,对于每一条要执行的命令,在执行之前,输出trace到stderr
------
###**trap**
####trap对信号的处理方式
1.	trap 'commands' signal 当接收到信号时,执行引号中的命令
2.	trap signal 不指定命令,接收信号的默认操作,默认是结束进程
3.	trap '' signal 指定一个空命令串,忽视信号

-	**trap对同种signal只会响应最后一个信号**
-	**trap不必要放在第一行**

####信号在什么时候执行
	在一个终端执行如下脚本
	#/bin/bash
	trap 'echo "interrupted...";exit' SIGINT
	sleep 10

	在本终端按下CTRL+C,会输出interrupted...
	但是如果在另一个终端执行**kill -SIGINT pid**,会看到原先终端没有任何反应,等待10秒后原先终端输出interrupted...

>	Bash 终端的默认行为是这样的,当按下CTRL+C的时候,会向当前的整个进程组发出SIGINT信号,而这个脚本的sleep是由当前脚本调用的,是这个脚本的子进程,默认是在同一个进程组的,所以也会收到SIGINT信号后并停止执行,返回主进程以后trap捕捉到信号
------
###判断
字符串判断
```
	str1 = str2		当两个串有相同内容、长度时为真
	str1 != str2	当串str1和str2不等时为真
	-n str1　　　　 当串的长度大于0时为真(串非空)
	-z str1　　　　 当串的长度为0时为真(空串)
	str1　　　　    当串str1为非空时为真
```
数字判断
```
	int1 -eq int2　　　　两数相等为真 
	int1 -ne int2　　　　两数不等为真 
	int1 -gt int2　　　　int1大于int2为真 
	int1 -ge int2　　　　int1大于等于int2为真 
	int1 -lt int2　　　　int1小于int2为真 
	int1 -le int2　　　　int1小于等于int2为真
```
文件判断
```
	-r file　　　　　用户可读为真 
	-w file　　　　　用户可写为真 
	-x file　　　　　用户可执行为真 
	-f file　　　　　文件为正规文件为真 
	-d file　　　　　文件为目录为真 
	-c file　　　　　文件为字符特殊文件为真 
	-b file　　　　　文件为块特殊文件为真 
	-s file　　　　　文件大小非0时为真 
	-t file　　　　　当文件描述符(默认为1)指定的设备为终端时为真
```
逻辑判断
```
	-a 　 　　　　　 与 
	-o　　　　　　　 或 
	!　　　　　　　　非
```
------