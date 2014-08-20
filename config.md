# 配置文件

rabbitmq 内置了一些默认配置, 这些默认配置就可以启动 server 服务了。当然你也可以通过配置的方式设置个性的 server。RabbitMQ 有三种当时可以配置 Server：
* 通过环境变量控制
通过环境变量可以设置启动的端口、相关文件的位置以及文件名等信息。配置方法： 1) 通过 shell 指定 2)通过 rabbitmq-env.conf 文件指定
* 通过配置文件控制
通过配置文件可以实现 Server 的插件管理、权限管理、各种资源的使用管理以及集群管理等功能。
* 运行时控制
运行时控制是在 Server 过程中通过 rabbitctl 进行实时控制， 比如设置集群的属性等

## RabbitMQ 环境变量配置文件位置
在 Unix-base system (包括 Linux, MacOSX) RabbitMQ 的环境变量可以通过 rabbitmq-env.conf 来定义.这个文件的位置在:
* Linux - $RABBITMQ_HOME/etc/rabbitmq/
* Debian - /etc/rabbitmq/
* RPM - /etc/rabbitmq/
* Mac OS X (Macports) - ${install_prefix}/etc/rabbitmq/, the Macports prefix is usually /opt/local
* Windows - %APPDATA%\RabbitMQ\
rabbitmq-env.conf 文件的配置是固定的, 一定是上面叙述的几个位置之一, 它不像 rabbitmq.config 一样可以自定义位置
```ini
#example rabbitmq-env.conf file entries
#Rename the node
NODENAME=bunny@myhost
#Config file location and new filename bunnies.config
CONFIG_FILE=/etc/rabbitmq/testdir/bunnies
```
[更多关于环境变量的配置参考这里](http://www.rabbitmq.com/man/rabbitmq-env.conf.5.man.html)

## RabbitMQ 环境变量列举
RabbitMQ 环境变量一般带有 RABBITMQ_ 这样的一个前置, 比如: RABBITMQ_var_name 这是环境变量的名字, RabbitMQ 有三种方式制定环境变量:
* 通过 shell 方式制定的环境变量都以 RABBITMQ_var_ 开头, 比如: RABBITMQ_var_name
* 在 rabbitmq-env.conf 中定义的环境变量都以 var_ 开头, 比如: var_name
* Server 内置的默认值
这三种变量的优先级是: shell 方式指定的变量 > rabbitmq-env.conf 配置的变量 > 默认环境变量值, 并且相同名称的变量高优先级的变量会覆盖低优先级的变量值.  RabbitMQ 可用的环境变量如下:

| Name        | Default | Description|
| ------------- |:-------------:| -----:|
|RABBITMQ_NODE_IP_ADDRESS | the empty string - meaning bind to all network interfaces. | Change this if you only want to bind to one network interface.|
|RABBITMQ_NODE_PORT | 5672 | |
|RABBITMQ_DIST_PORT | RABBITMQ_NODE_PORT + 20000 | Port to use for clustering. Ignored if your config file setsinet_dist_listen_min or inet_dist_listen_max|
|RABBITMQ_NODENAME | Linux:rabbit@$HOSTNAME  Windows:rabbit@%COMPUTERNAME%| "The node name should be unique per erlang-node-and-machine combination. To run multiple nodes|
|RABBITMQ_SERVICENAME | Windows Service: RabbitMQ | The name of the installed service. This will appear in services.msc.|
|RABBITMQ_CONSOLE_LOG |Windows Service:| Set this variable to new or reuse to redirect console output from the server to a file named%RABBITMQ_SERVICENAME%.debug in the default RABBITMQ_BASE directory.  If not set, console output from the server will be discarded (default).  new A new file will be created each time the service starts.  reuse The file will be overwritten each time the service starts.|
|RABBITMQ_CTL_ERL_ARGS | None | Parameters for the erl command used when invoking rabbitmqctl. This should be overridden for debugging purposes only.|
|RABBITMQ_SERVER_ERL_ARGS |Linux: "+K true +A30 +P 1048576 -kernel inet_default_connect_options [{nodelay,true}]"
Windows: None|Standard parameters for the erl command used when invoking the RabbitMQ Server. This should be overridden for debugging purposes only.|
|RABBITMQ_SERVER_START_ARGS | None | Extra parameters for the?erl?command used when invoking the RabbitMQ Server. This will not overrideRABBITMQ_SERVER_ERL_ARGS.|

## 自定义配置
RabbitMQ 的配置文件路径是:/etc/rabbitmq/rabbitmq.config　

## 参考文献
* [RabbitMQ-configure](http://www.rabbitmq.com/configure.html)
