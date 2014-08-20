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
| ------------- |:-------------|:-----|
|RABBITMQ_NODE_IP_ADDRESS | 空字符串代表绑定到所有网络接口上，也就是 0.0.0.0 |当你想让 Server 只绑定到特定的网口上的时候, 可以使用此参数设定|
|RABBITMQ_NODE_PORT | 5672 |默认使用 5672 端口服务 |
|RABBITMQ_DIST_PORT | RABBITMQ_NODE_PORT + 20000 | 设置集群模式的端口, 但如果你在配置文件中设定了inet_dist_listen_min 或者 inet_dist_listen_max 的值, 此参数的配置可能会被忽略|
|RABBITMQ_NODENAME | Linux:rabbit@$HOSTNAME  Windows:rabbit@%COMPUTERNAME%| 当在集群模式中运行多个 node 的时候, 此参数可以配置 node name, 每一个 node name 必须是唯一的|
|RABBITMQ_SERVICENAME | Windows Service: RabbitMQ | The name of the installed service. This will appear in services.msc.|
|RABBITMQ_CONSOLE_LOG |Windows Service:| Set this variable to new or reuse to redirect console output from the server to a file named%RABBITMQ_SERVICENAME%.debug in the default RABBITMQ_BASE directory.  If not set, console output from the server will be discarded (default).  new A new file will be created each time the service starts.  reuse The file will be overwritten each time the service starts.|
|RABBITMQ_CTL_ERL_ARGS | None | Parameters for the erl command used when invoking rabbitmqctl. This should be overridden for debugging purposes only.|
|RABBITMQ_SERVER_ERL_ARGS |Linux: "+K true +A30 +P 1048576 -kernel inet_default_connect_options [{nodelay,true}]"
Windows: None|Standard parameters for the erl command used when invoking the RabbitMQ Server. This should be overridden for debugging purposes only.|
|RABBITMQ_SERVER_START_ARGS | None | Extra parameters for the?erl?command used when invoking the RabbitMQ Server. This will not overrideRABBITMQ_SERVER_ERL_ARGS.|

另外还有一些环境变量可以设置:[数据库、日志、插件、配置文件的位置等信息](http://www.rabbitmq.com/relocate.html)
## 自定义配置
RabbitMQ 的配置文件路径是:/etc/rabbitmq/rabbitmq.config　
### The rabbitmq.config File
RabbitMQ 的配置文件可以配置 RabbitMQ Server 、Erlang 的服务以及 RabbitMQ 的插件等内容。这是一个标准的 erlang 配置文件, 更多 erlang 配置的内容参见[这里](http://www.erlang.org/doc/man/config.html)

配置文件举例:
```erlang
[
    {mnesia, [{dump_log_write_threshold, 1000}]},
    {rabbit, [{tcp_listeners, [5673]}]}
  ].
```
这个配置文件中主要修改了两项内容:
- dump_log_write_threshold 默认值是 100 , 增加到了 1000
- tcp_listeners 默认是 5672 改成了 5673
### RabbitMQ 的配置文件和环境变量文件一般保存在如下位置
在 Unix-base system (包括 Linux, MacOSX) RabbitMQ 的环境变量可以通过 rabbitmq-env.conf 来定义.这个文件的位置在:
* Linux - $RABBITMQ_HOME/etc/rabbitmq/
* Debian - /etc/rabbitmq/
* RPM - /etc/rabbitmq/
* Mac OS X (Macports) - ${install_prefix}/etc/rabbitmq/, the Macports prefix is usually /opt/local
* Windows - %APPDATA%\RabbitMQ\
如果 rabbitmq-env.conf  文件不存在你可以手动的创建文件, 但在 windows 中 rabbitmq-env.conf 是不被使用的.
如果 rabbitmq.config 文件不存在, 你可以手动创建文件. 你也可以通过在 rabbitmq-env.conf 里面设置 RABBITMQ_CONFIG_FILE 环境变量改变 rabbitmq.config 的默认位置, 配置文件修改后重启 RabbitMQ  服务新的配置即可生效

### 示例配置文件
RabbiMQ 默认提供了一个示例配置文件, 这个配置文件叫做: rabbitmq.config.example . 大多数的配置参数都可以在这个配置文件中看到配置方法.并在配置文件中我们添加了很多注释帮助你来理解每一个配置项的意义.
在大多数的发行版本中示例配置文件都在默认的配置文件的位置处, 但是 Debian 和 RPM 打包的 RabbitMQ 程序不能这样做, 所以通过这两个方式安装的用户需要在 /usr/share/doc/rabbitmq-server/ 或 /usr/share/doc/rabbitmq-server-3.3.5/ 中找到示例文件, 注意把 3.3.5 换成你安装的版本号.

### RabbitMQ 可以配置的参数
可配置的参数列举如下

| Key|  Description|
| -------------|:-----|
| tcp_listeners| List of ports on which to listen for AMQP connections (without SSL). Can contain integers (meaning "listen on all interfaces") or tuples such as {"127.0.0.1", 5672} to listen on a single interface.  Default: [5672] |
| ssl_listeners| As above, for SSL connections.  Default: []|
| ssl_options| SSL configuration. See the [SSL documentation](http://www.rabbitmq.com/ssl.html#enabling-ssl).  Default: []
| vm_memory_high_watermark|Memory threshold at which the flow control is triggered. See the [memory-based flow control](http://www.rabbitmq.com/memory.html) documentation.  Default: 0.4|
| vm_memory_high_watermark_paging_ratio | Fraction of the high watermark limit at which queues start to page messages out to disc to free up memory. See the [memory-based flow control](http://www.rabbitmq.com/memory.html) documentation.  Default: 0.5|
| disk_free_limit | Disk free space limit of the partition on which RabbitMQ is storing data. When available disk space falls below this limit, flow control is triggered. The value may be set relative to the total amount of RAM (e.g. {mem_relative, 1.0}). The value may also be set to an integer number of bytes. By default free disk space must exceed 50MB. See the [memory-based flow control](http://www.rabbitmq.com/memory.html) documentation.  Default: 50000000 |
| log_levels | Controls the granularity of logging. The value is a list of log event category and log level pairs.  The level can be one of 'none' (no events are logged), 'error' (only errors are logged), 'warning' (only errors and warning are logged), or 'info' (errors, warnings and informational messages are logged).  At present there are three categories defined. Other, currently uncategorised, events are always logged.  The categories are: 1) connection - for all events relating to network connections 2) mirroring - for all events relating to [mirrored queues](http://www.rabbitmq.com/ha.html) 3) federation - for all events relating to [federation](http://www.rabbitmq.com/federation.html)
Default: [{connection, info}] |



## 参考文献
* [RabbitMQ-configure](http://www.rabbitmq.com/configure.html)
