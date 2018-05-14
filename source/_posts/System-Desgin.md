---
title: System Desgin
comments: true
date: 2018-04-24 21:30:32
updated: 2018-04-24 21:30:32
categories: Interview
tags: System Design
---
# 系统设计
## 结构
来源  
> https://www.jianshu.com/p/f7cfd9dbcd5d

Scenario - Necessary - Application - Kilobit - Evolve 

先说哪里用得到，再说我们需要解决问题多大规模。然后说基本解里头Application里面都有啥，然后说说相对应的数据放哪里怎么放。最后这些都说完了（20-25分钟左右）来具体谈怎么让我的基本解在哪些方面做的更好。
### Scenario 场景
1. 问清楚自己要做哪些功能（也就是说，45分钟内不聊哪些功能）

2. 问清楚或者说清楚自己要handle多大用户量，面试官起码得给你确认这么几个信息，否则聊不下去。

- 一个是你平均每天handle多少用户

- 一个是你峰值（最多？不太精确但是形容一下）每天handle多少用户

3. 自己把自己要算的东西都算出来， QPS啊，存储size啊，不非得一口气全部算完，但是记住最基本的用户量，然后再说然后的。

<!--more-->
### Implementation 实现
4. 搭架子，我的系统要干嘛，为了做这件事情，我们需要什么组件，怎么安排。这里一切最简单，保证这个东西可以work，不要有明显的优化还不做。

5. 按照架子一个一个实现具体功能，如果发现有问题了，改改架子。记住。改架子的时候一定要想想别的东西动没动，动了，赶紧拿个纸记下来。数据放哪可以这里说可以分开说，这都不要紧。

6. 架子流程实现完了想想数据放哪里怎么放，那么些个数据库呢，好好挑挑。

### Enhancement  提高
7.根据确认的问题或者优化点慢慢优化。这里的话，不懂就说不懂。为啥？基本分已经有了，大好局面来之不易，千万不要不懂装懂暴露问题。比如你之前说的都挺好，然后到优化了把load balancer放到dispatch service还要靠前端的地方，这不扯么？

## CAP
不同要求，A vs C，聊天系统通常是C
# 解法
## 想不到怎么办
![](https://pic4.zhimg.com/80/cc2a92e324587867c8cfba3022ea9cca_hd.jpg)

- Cache：缓存，万金油，哪里不行优先考虑 - **Latency**
- Queue：消息队列，常见使用Linkedin的kafkaAsynchronized：批处理＋异步，减少系统IO瓶颈
- Load Balance: 负载均衡，可以使用一致性hash技术做到尽量少的数据迁移
- Parallelization：并行计算，比如MapReduce
- Replication：提高可靠性，如HDFS，基于位置感知的多块拷贝 
- Partition：数据库 sharding，通过hash取摸

作者：董飞
链接：https://www.zhihu.com/question/26312148/answer/32627282
来源：知乎

# Link
[系統設計救星! 一天內手把手教你面試](http://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=210147&extra=page%3D1%26filter%3Dtypeid%26typeid%3D200%26typeid%3D200)
