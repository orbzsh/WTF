###**shell**的配置文件
-	./bash_profile 用户登录的时候读取,其中包含的命令被执行
-	./bashrc 启动新的shell时被读取,并执行
-	./bash_logout shell登出时被读取

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
	
###**set**选项介绍
-	**set -o errexit** 设置这个选项后,当一个命令执行失败时,shell立即退出
-	**set -o noexec** 当设置后,shell读取命令,但不会执行它们,可以用来检测shell脚本是否有语法错误
-	**set -o strace** 当设置后,对于每一条要执行的命令,在执行之前,输出trace到stderr

###**trap**

###判断
1.	字符串判断

	str1 = str2		当两个串有相同内容、长度时为真
	str1 != str2	当串str1和str2不等时为真
	-n str1　　　　 当串的长度大于0时为真(串非空)
	-z str1　　　　 当串的长度为0时为真(空串)
	str1　　　　    当串str1为非空时为真
2.	数字判断

	int1 -eq int2　　　　两数相等为真 
	int1 -ne int2　　　　两数不等为真 
	int1 -gt int2　　　　int1大于int2为真 
	int1 -ge int2　　　　int1大于等于int2为真 
	int1 -lt int2　　　　int1小于int2为真 
	int1 -le int2　　　　int1小于等于int2为真
3.	文件判断

	-r file　　　　　用户可读为真 
	-w file　　　　　用户可写为真 
	-x file　　　　　用户可执行为真 
	-f file　　　　　文件为正规文件为真 
	-d file　　　　　文件为目录为真 
	-c file　　　　　文件为字符特殊文件为真 
	-b file　　　　　文件为块特殊文件为真 
	-s file　　　　　文件大小非0时为真 
	-t file　　　　　当文件描述符(默认为1)指定的设备为终端时为真
4.	逻辑判断

	-a 　 　　　　　 与 
	-o　　　　　　　 或 
	!　　　　　　　　非