# Rabbit MQ集群

当RabbitMQ单机部署节点负载达到上线时，可用采用集群策略，通过添加更多的节点来线性扩展消息的吞吐量，在集群策略下RabbitMQ允许消费者和生产者在RabbitMQ单节点崩溃的情况下继续运行，当失去一个RabbitMQ节点时，客户端能够重新连接到集群中的任何其他节点继续生产或者消费，保证RabbitMQ的高可用。



注意：RabbitMQ集群并不能保证消息的不丢失，当RabbitMQ的一个节点进行宕机时，该节点上的所有队列中的消息将会丢失。（镜像队列可以解决该问题）

## Rabbit MQ的负载均衡

当使用集群策略时，对于客户端而言，并不需要和集群中所有节点建立连接，而是挑选其中一个节点建立连接，在引入负载均衡后，各个客户端可以均匀的分摊到集群中的各个节点。

目前RabbitMQ负载均衡的主流方案如下：

- 客户端内部实现负载均衡
- 使用HAProxy+Keepalived实现高可用负载均衡
- LVS

### 客户端负载均衡

RabbitMQ的SDK中可以在客户端连接时简单的使用负载均衡算法实现负载均衡：

- 轮询
- 加权轮旋
- 随机
- 加权随机
- 源地址哈希法
- 最小连接数法

### 使用HAProxy实现负载均衡

HAProxy提供高可用、负载均衡以及基于TCP和HTTP应用的代理。

剩下的TODO

### 使用Keepalived实现高可靠负载均衡

如果单单使用HAProxy实现负载均衡，可能出现以下情况：

HAProxy突然宕机，RabbitMQ集群正常运行，但是对于外部的客户端来说所有的连接将断开，这种情况是毁灭性的。

所以这种情形下引入Keepalived，Keepalived采用VRRP以软件的形式实现服务的热备功能，通常将两台Linux服务器组成一个热备组（Master和Backup），同一时间内热备组只有一个主服务器Master提供服务，同事Master会虚拟一个VIP，这个VIP只存在在于Master上并对外提供服务。

当Keepalived检测到Master宕机或者服务故障，备份服务器Backup会自动接管VIP并成为Master，Keepalived将原Master从热备组中移除。

当原Master回复后，会自动加入到热备组，然后再抢占成为Master，起到故障转移的功能。

### LVS实现高可靠负载均衡

TODO