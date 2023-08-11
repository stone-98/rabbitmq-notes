# RabbitMQ的AMQP协议介绍

## AMQP概述

AMQP是一种消息传递的标准协议，定义了消息队列系统应该如何进行消息的传递和交换。

## RabbitMQ概述

RabbitMQ时一个可靠，灵活可扩展的消息队列，提供了可靠的消息传递机制。

## 两者的关系

RabbitMQ和AMQP的关系类比于实现类和接口的关系，AMQP定义了消息如何进行交互，RabbitMQ对AMQP进行实现。当然RabbitMQ还支持STOMP、MQTT等协议。RabbitMQ中的交换器、交换器类型等都遵循AMQP协议中的相应的概念。

## AMQP相关概念详细介绍

### Message （消息）

消息是AMQP中传输的介质，消息一般包含两个部分：消息体和标签。

- 消息体：具体的业务数据。
- 标签：用来表述这个消息，例如交换器的名称、路由键等。（消息的标签在消息的路由过程中将会丢弃）

### Producer（生产者）

生产者就是生成消息的一方，将消息生成然后发布到RabbitMQ中。

### Broker （RabbitMQ节点）

一个RabbitMQ Broker可以简单的看作一个RabbitMQ的服务节点。

### Consumer（消费者）

消费者就是消费消息的一方，它连接到RabbitMQ，并且订阅到队列上，当消息消费一条消息时，只是消费消息的消息体。

### Queue（队列）

Queue是RabbitMQ的内部对象，用于存储消息。一个RabbitMQ实例可以有多个队列。

### Exchange

交换器：生产者将消息发送到Exchange，交换器通过对应的规则，将消息路由到一个或多个队列中，如果路由不到，或许会返回给生产者，或许会直接丢弃。

### RoutingKey（路由键）

生产者将消息发送给交换器时，一般会指定这个RoutingKey，而

### Binding（绑定）

RabbitMQ中通过Binding将交换机与队列关联起来。

### BindingKey （绑定键）

在上述进行Binding的过程中，一般会指定一个BindingKey，这样交换机就知道要将哪些消息路由到对应的队列了。











