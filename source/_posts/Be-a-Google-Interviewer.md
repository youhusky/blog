---
title: Be a Google Interviewer
comments: true
date: 2019-03-24 21:43:54
updated: 2019-03-24 21:43:54
categories: Google	
tags: interview
---

> 此篇文章不代表谷歌官方Hiring Process，只是我最近的感受  
> 一切内容均可在公开论坛上搜到，不含机密信息

# Tricky Question？
No and it is banned from the Google Interview Question List.

# 题目来源

* 谷歌内部有题库，但Intern没有权限访问  
* 题目数量比较多，但质量参差不齐
* 如果题目的完整叙述被发到了网上，题目很有可能被Banned

# 面试流程

> 只讨论技术/算法论

一般的面试者（L5之下）会有至少4轮的算法，各个面试官之间并无任何信息交流，仅凭借一张checklist来记录上一位面试官出的题，所以也可能出现一天中被面了好多Tree/ Dynamic Programming/ String 类型的题

# Rubic

每个面试官会根据candidate的表现打分（Strong No hire, No hire, Linear hire, Hire, Strong Hire），然后送到Hiring Committee（L6+）那里去评价。

# 我的感受

<!--more-->

## 准备

当我得知能面试别人的时候还有有点兴奋的，就是换到面试官的角度来看待面试者。但实际上却是：

* 怎么出题呀，是不是太简单了，是不是太难了，是不是在banned list上！
* 是不是自己还不会做！
* 是不是有一题多解，自己却只能implement一个解法 

## 面试

当你急匆匆的从上一个meeting走出来，看了看自己设置的buffer time，温习一下题目，看一下candidate的简历，房间位置是在自身的楼还是隔壁楼，一切确认后，到达现场，等待上一个面试官推门而去。然后你面带笑容的走进房间：

* Hi xxx, nice to met you. My name is Joshua and I will be your next interviewer.  
* 问candidate要不要上厕所/喝水/休息一下.
* 一般开场5分钟是介绍一下自己/让candidate介绍一下自己
* 开始正式出题，一番简明扼要的描述完题目之后，对方一脸疑惑的看着，你就会想是自己没说清楚？
	* 但实际上面试者只是在思考怎么做，这时候你就自然而然的引出一个例子并且给出一个答案。

### Be
* 引导对方开始不要在Corner Case上太费时间 
* 在对方开始或者结束Coding的时候询问时间空间复杂度（直接影响到data structure的评价）
* 在恰当的时候给出hint，指出一条道路而不是任由对方在错误的道路上不断徘徊尝试
* A Timer，把控时间
* Involved
* 记录对方讨论的思路

### Not to be
* 进行需要集中注意力的Work Coding
* 刷微信朋友圈
* 想着周末去哪玩/约谁

### 优秀者

当对方水平达到Hiring/ Strong Hiring的时候，很有可能他的解法你没有见过/思路清奇但却能完成任务，这时候你应该默默的感谢对方而不是因为不对我的路子去怀疑。  

### Follow Up

一般能进行到这一步的candidate说明基本问题已经能较好的打上来,(满足时间空间复杂度要求)这时候你就会把问题展开化变成一个更加普世的， 更加General， 更加Open的情况。Candidate只需要指出哪一步需要添加条件/改变数据结构即可。

## 面试后
当你终于离开了房间，have a good luck with the candidate后，心想这个Candidate（分如下）：

* 怎么表现这么差，phone是怎么放过来的
* 就是有点紧张，时间上来不及，快结束的时候终于想出来正确思路了
* Coding没问题，就是嵌套IF，各种条件揉在一起
* 表现不错，Follow Up没答上来是因为没时间了
* 给我上了一课！TA的算法我要记下来！

然后就要给出一个Inital的评分，和详细评价（多于100字）。这时候就需要你回忆那时候candidate的表现，无论是你走之前拍了照还是当时记录在Docs上的信息，这时候都会是有用的材料！  
所以别看一个session只有45分钟，本着对candidate但负责的角度，实际上你的时间成本大于90分钟

# 个人Rubic
做出题来是能否达到Linear Hire的标准，答对Follow Up是Strong Hire的标准。

# 个人Tips
## Interviewer

* 选择Onsite而不是Phone，这样能更好的和candidate交流。
* 题目难度要大于等于Medium
* 面试频率不要过高
* 制造轻松的氛围

## Interviewee

* 选择白板
* 多讨论少闷头自己琢磨
* 先把常见的corner case明确并写出（Null/ 0/ -1/ itself/ etc…），再写common case的方案，最后讨论完善——最优解。