# 简介
RabbitMQ 是一个消息中间件.最主要的思想非常简单：接受并转发消息。你可以把它想象成一个邮局：当你把一封信放在邮箱里，你会非常肯定邮递员最终将把这封信交到你的收件人手里。RabbitMQ 就像上面的例子一样，扮演了邮箱、邮局、邮递员的角色。

RabbitMQ 和邮局最主要的区别就是，RabbitMQ 不用纸，使用 “消息” 这种二进制的数据来替代纸张进行接收、存储、转发。

RabbitMQ 和消息传递在通常情况下有如下的术语：

+ *生产者* 无非就是发送的意思。发送消息的的程序就是生产者，画出图形来就是下面这个样子，用大写字母 "P" 代表。
![alt text][producer]

# Hello World! (使用 Python 的 pika 0.9.8 作为客户端演示)
# 发送
# 接收
# 总结

[producer]: http://www.rabbitmq.com/img/tutorials/producer.png "Producer"
