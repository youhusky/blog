---
title: RabbitMQ
comments: true
date: 2018-04-24 21:43:46
updated: 2018-04-24 21:43:46
categories: Interview
tags: RabbitMQ
---

# 应用场景
- 信息的发送者和接收者如何维持这个连接，如果一方的连接中断，这期间的数据如何方式丢失？
- 如何降低发送者和接收者的耦合度？
- 如何让Priority高的接收者先接到数据？
- 如何做到load balance？有效均衡接收者的负载？
- 如何有效的将数据发送到相关的接收者？也就是说将接收者subscribe 不同的数据，如何做有效的filter。
- 如何做到可扩展，甚至将这个通信模块发到cluster上？
- 如何保证接收者接收到了完整，正确的数据？

<!--more-->
# 架构
## 基础
![](https://upload-images.jianshu.io/upload_images/5401760-16c4239d5197c238.png)
RabbitMQ Server： 也叫broker server，它不是运送食物的卡车，而是一种传输服务。原话是RabbitMQisn’t a food truck, it’s a delivery service. 他的角色就是维护一条从Producer到Consumer的路线，保证数据能够按照指定的方式进行传输。但是这个保证也不是100%的保证，但是对于普通的应用来说这已经足够了。当然对于商业系统来说，可以再做一层数据一致性的guard，就可以彻底保证系统的一致性了。

Client P： 也叫Producer，数据的发送方。createmessages and publish (send) them to a broker server (RabbitMQ).一个Message有两个部分：payload（有效载荷）和label（标签）。payload顾名思义就是传输的数据。label是exchange的名字或者说是一个tag，它描述了payload，而且RabbitMQ也是通过这个label来决定把这个Message发给哪个Consumer。AMQP仅仅描述了label，而RabbitMQ决定了如何使用这个label的规则。

Client C: 也叫Consumer，数据的接收方。Consumersattach to a broker server (RabbitMQ) and subscribe to a queue。把queue比作是一个有名字的邮箱。当有Message到达某个邮箱后，RabbitMQ把它发送给它的某个订阅者即Consumer。当然可能会把同一个Message发送给很多的Consumer。在这个Message中，只有payload，label已经被删掉了。对于Consumer来说，它是不知道谁发送的这个信息的。就是协议本身不支持。但是当然了如果Producer发送的payload包含了Producer的信息就另当别论了。

**Exchanges** are where producers publish their messages.

**Queues** are where the messages end up and are received by consumers

**Bindings** are how the messages get routed from the exchange to particular queues.  
**Routing Key**： 路由关键字，exchange根据这个关键字进行消息投递。
## Message acknowledgment
在实际应用中，可能会发生消费者收到Queue中的消息，但没有处理完成就宕机（或出现其他意外）的情况，这种情况下就可能会导致消息丢失。为了避免这种情况发生，我们可以要求消费者在消费完消息后发送一个回执给RabbitMQ，RabbitMQ收到消息回执（Message acknowledgment）后才将该消息从Queue中移除；如果RabbitMQ没有收到回执并检测到消费者的RabbitMQ连接断开，则RabbitMQ会将该消息发送给其他消费者（如果存在多个消费者）进行处理。

## Message durability
如果我们希望即使在RabbitMQ服务重启的情况下，也不会丢失消息，我们可以将Queue与Message都设置为可持久化的（durable）
## Exchange
生产者将消息发送到Exchange（交换器，下图中的X），由Exchange将消息路由到一个或多个Queue中（或者丢弃）
### fanout
![](https://upload-images.jianshu.io/upload_images/5401760-48523262e0628ef1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/329)
### direct
完全匹配
![](https://upload-images.jianshu.io/upload_images/5401760-98077aa4aff11976.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/423)
### topic
前面讲到direct类型的Exchange路由规则是完全匹配binding key与routing key，但这种严格的匹配方式在很多情况下不能满足实际业务需求。topic类型的Exchange在匹配规则上进行了扩展，它与direct类型的Exchage相似，也是将消息路由到binding key与routing key相匹配的Queue中

![](https://upload-images.jianshu.io/upload_images/5401760-8aa34283824172fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/424)
以上图中的配置为例，routingKey=”quick.orange.rabbit”的消息会同时路由到Q1与Q2，routingKey=”lazy.orange.fox”的消息会路由到Q1，routingKey=”lazy.brown.fox”的消息会路由到Q2，routingKey=”lazy.pink.rabbit”的消息会路由到Q2（只会投递给Q2一次，虽然这个routingKey与Q2的两个bindingKey都匹配）；routingKey=”quick.brown.fox”、routingKey=”orange”、routingKey=”quick.orange.male.rabbit”的消息将会被丢弃，因为它们没有匹配任何bindingKey。


# 引用
作者：高广超
链接：https://www.jianshu.com/p/24f464f9161c
來源：简书
