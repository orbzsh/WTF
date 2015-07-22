###服务发现consul
------
####安装
下载并解压到/usr/bin目录
[consul下载地址](https://www.consul.io/downloads.html)

####架构图
![consul架构图](https://www.consul.io/assets/images/consul-arch-b2478674.png)
####简单测试
server
	
	consul agent -server -bootstrap-expect 1 -data-dir /tmp -node=agent-server -bind=192.168.1.10

client
	
	consul agent -data-dir /tmp -join=192.168.1.10 -bind=192.168.1.20 -node=agent-client -ui-dir=/tmp/ui -client=0.0.0.0

####参数说明

-	/etc/consul.d 		配置文件放置位置
-	-bind				改地址是集群内部的通讯地址,集群内的所有节点到地址都必须是可达的,默认是0.0.0.0
-	-client 			consul绑定在哪个client地址上,这个地址提供HTTP、DNS、RPC等服务,默认是127.0.0.1
-	-config-dir			配置文件目录,里面所有以.json结尾的文件都会被加载
-	-config-file		明确的指定要加载哪个配置文件
-	-dc 				该标记控制agent允许的datacenter的名称,默认是dc1
-	-join				加入一个已经启动的agent的ip地址,可以多次指定多个agent的地址(类似向服务器注册)
-	-node				节点在集群中的名称,在一个集群中必须是唯一的,默认是该节点的主机名
-	-server 			定义agent运行在server模式,每个集群至少有一个server
-	-ui-dir 			提供存放web ui资源的路径,该目录必须是可读的
-	-bootstrap-expect	在一个datacenter中期望提供的server节点数目,当该值提供的时候,consul一直等到达到指定sever数目的时候才会引导整个集群
-	-bootstrap 			用来控制一个server是否在bootstrap模式,在一个datacenter中只能有一个server处于bootstrap模式,当一个server处于bootstrap模式时,可以自己选举为raft leader
-	-data-dir			提供一个目录用来存放agent的状态,所有的agent允许都需要该目录,该目录必须是稳定的,系统重启后都继续存在

####配置写入到文件中,json格式

	{
		"datacenter":"DC1",
		"data-dir":"/tmp/consul",
		"log_level": "INFO",
		"node_name":"agent1",
		"server":true,
		"watches":[
			{
				"type":"checks",
				"handler":"/usr/local/bin/check-handler.sh"
			}
		]
	}

配置文件参数
-	acl_datacenter:
-	acl_default_policy:
-	acl_down_policy:
-	acl_token:
-	acl_ttl:
-	acl_master_token:
-	addresses:
-	bootstrap:
-	bootstrap_expect:
-	bind_addr:
-	client_addr:
-	datacenter:
-	data_dir: