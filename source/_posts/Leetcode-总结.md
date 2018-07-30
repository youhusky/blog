---
title: Leetcode 总结
comments: true
date: 2018-05-14 21:36:13
updated: 2018-07-28 21:36:13
categories: Leetcode
tags: 
top: 8
---

# 缘由
在整个找工作的期间，每天刷一亩三分地和Leetcode，而且刷题的过程中，有一些比较有价值的网站值得参考，其中的解法分析重点需要掌握。目前在Google的Product Infrastructure组工作，可以提供内推。

# 我的总结
## 套路
**大部分来自天纯的pdf笔记  **  
如果问最短，最少，BFS  如果问连通性，静态就是 DFS,BFS，动态就 UF  如果问依赖性就 topo sort  DAG 的问题就 dfs+memo  矩阵和 Array 通常都是 DP  问数量的通常都是 DP  问是否可以，也很有可能 DP  求所有解的，基本 backtracking  排序总是可以想一想的  
万事总可以想HashMap  
找规律试试Stack
<!--more-->
## 基础数据结构
1. [Longest Substring系列](http://joshuablog.herokuapp.com/Longest-Substring%E7%B3%BB%E5%88%97.html) 
[Two Pointer问题总结](http://joshuablog.herokuapp.com/Two-Pointer%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.html)
[String总结](http://joshuablog.herokuapp.com/String%E6%80%BB%E7%BB%93.html)  
2. [LinkedList系列](http://joshuablog.herokuapp.com/LinkedList%E7%B3%BB%E5%88%97.html)  
3. [Stack总结](http://joshuablog.herokuapp.com/Stack%E6%80%BB%E7%BB%93.html)  
4. [Backtracking总结](http://joshuablog.herokuapp.com/Backtracking%E6%80%BB%E7%BB%93.html)  
5. [Heap总结](http://joshuablog.herokuapp.com/Heap%E6%80%BB%E7%BB%93.html)  
6. [BFS-Board类型总结](http://joshuablog.herokuapp.com/BFS-Maze%E7%B1%BB%E5%9E%8B%E6%80%BB%E7%BB%93.html)
7. [DFS and Dijkstra](http://joshuablog.herokuapp.com/DFS-BFS-%E6%80%BB%E7%BB%93.html)
8. [Tree总结](http://joshuablog.herokuapp.com/Tree%E6%80%BB%E7%BB%93.html)
9. [DP总结](http://joshuablog.herokuapp.com/DP%E6%80%BB%E7%BB%93.html)

## 稍难数据结构
[TopLogicalSort 总结](http://joshuablog.herokuapp.com/TopLogicalSort-%E6%80%BB%E7%BB%93.html)  
[Trie 类型总结](http://joshuablog.herokuapp.com/Trie-%E7%B1%BB%E5%9E%8B%E6%80%BB%E7%BB%93.html)  
[Union-Find总结](http://joshuablog.herokuapp.com/Union-Find%E6%80%BB%E7%BB%93.html)  
[Design 问题](http://joshuablog.herokuapp.com/Design-%E9%97%AE%E9%A2%98.html)  
[System Desgin](http://joshuablog.herokuapp.com/System-Desgin.html)

# 参考资料

## 找工作
1. [Leetcode](https://leetcode.com/)
2. [1point3acre.com](http://www.1point3acres.com/bbs/)
3. [Linkedin (encourage to purchase premium member)](https://www.linkedin.com/)
4. [Indeed](https://www.indeed.com/)
5. [AngelList](https://angel.co/)
6. [Glassdoor](https://www.glassdoor.com/index.htm)
7. [LingOffer (Refer)](https://lingoffer.com/resource)
8. [Hackerrank](https://www.hackerrank.com/)
9. [某一次Bittiger分享的公司和HR邮箱PPT](https://bittigerfiles.s3.amazonaws.com/Company%20Slides%20for%20Career%20Meetup.pdf?nsukey=ITLZW1c%2BEgWMA0N%2BJBaXv%2FTqZfXkpCvREiPlHgVeBcN7soSoXYRWytE327M0R6ViNZGxVxz4LfMcE2hmJ6AvzeEtV9DgVfjU7qsgBq04n9qO0o032Ohq0bV%2BdG91IBWmq2w2vNgSuFQOtYWka9zmt5vN5L4WSfgI8EjHhSQQfLBBiv0KDTDXhWsX40T1gx3a)


## 算法
### Python 党
1. [kamyu104 带有时间复杂度的总结](https://github.com/kamyu104/LeetCode)
2. [细语呢喃 (思路清晰)](https://www.hrwhisper.me/)

### Java 党
1. [一个按类总结的博客](https://www.zybuluo.com/Yano/note/253217)
2. [Ethan Li 的技术专栏 思路清晰](https://segmentfault.com/a/1190000003922961)
3. [一个适合刷基础数据结构的博客](https://www.kancloud.cn/kancloud/data-structure-and-algorithm-notes/72897)
4. [分类总结2（值得看）](https://liut2.gitbooks.io/crazystuff/content/trie.html)
5. [分类总结3](https://mnmunknown.gitbooks.io/algorithm-notes/content/)
6. [算法珠玑——一个最精简的题库](https://soulmachine.gitbooks.io/algorithm-essentials/content/java/)

## 知识
### Software Engineer
1. [Python小技巧大全](https://taizilongxu.gitbooks.io/stackoverflow-about-python/content/80/README.html)
2. [小土刀总结](http://wdxtub.com/2016/01/22/programmer-startline-1/)
3. [CMU-15619 Cloud Computing](http://www.cs.cmu.edu/~msakr/15619-s16/index.html)
4. [Java API 小总结](http://wiki.jikexueyuan.com/project/java/object-classes.html)
5. [菜鸟教程！](http://www.runoob.com/)
6. [廖雪峰教程](https://www.liaoxuefeng.com/)
7. [数盟](http://dataunion.org/tag/python)
8. [结构之法 算法之道](http://blog.csdn.net/v_july_v?viewmode=contents)

### Data Science
1. [fullstackml](https://fullstackml.com/)
2. [Apache Pig Intro] (http://www.tutorialspoint.com/apache_pig/apache_pig_distinct_operator.htm)
3. [Cassandra Tutorial](http://www.tutorialspoint.com/cassandra/index.htm)
4. [qwiklabs](https://qwiklabs.com/)
5. [dive-into-machine-learning](https://github.com/hangtwenty/dive-into-machine-learning)

## 系统设计
1. [不太全的总结](http://systemdesigns.blogspot.com/?view=classic)
2. [设计模式](http://novoland.github.io/%E8%AE%BE%E8%AE%A1/2015/04/02/%E5%B8%B8%E7%94%A8%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E7%9A%84%E6%80%BB%E7%BB%93.html)
3. [教你如何迅速秒杀掉：99%的海量数据处理面试题](http://blog.csdn.net/v_july_v/article/details/7382693)
4. [基础知识包括系统设计--面试题](https://hellohell.gitbooks.io/java-/content/url_shorten_design.html)
5. [hiredintech](https://www.hiredintech.com/classrooms/system-design/lesson/104)


## Project
1. [利用 Python 练习数据挖掘](http://python.jobbole.com/83563/)
2. [The Open Source Data Science Masters](http://datasciencemasters.org/)
3. [Bittiger项目](https://slack-files.com/T0GUEMKEZ-F0J4G9QTT-274d3bc97e)
4. [voting-app](http://sahatyalkabov.com/create-a-character-voting-app-using-react-nodejs-mongodb-and-socketio/)
5. [Interactive Data Visualization for the Web](http://chimera.labs.oreilly.com/books/1230000000345/index.html)

## 杂
1. [北美（湾区）CS工作进阶攻略－求职篇](http://www.1point3acres.com/bbs/thread-104824-1-1.html)
2. [一个台湾人写的小总结](https://medium.com/@KenjiChao/2016-17-%E7%BE%8E%E5%9C%8B%E8%BB%9F%E9%AB%94%E5%B7%A5%E7%A8%8B%E5%B8%AB%E6%B1%82%E8%81%B7%E5%BF%83%E5%BE%97-a5c00427fa73)
3. [复杂度表](http://bigocheatsheet.com/)
4. [USC学长的Blog](http://yansu.org/)
5. [面试总结](https://zhuanlan.zhihu.com/p/30218471)
6. [Leetcode 分类顺序表](https://docs.google.com/spreadsheets/d/17ZXOm9P7hK7cTmSy_FslOB7nbctbGBFRPR5jQg4aJ7s/edit#gid=1222314994)
7. [小土刀简历总结](https://github.com/wdxtub/WDXpeak/blob/master/3.0/Interview/Code%20Complete/resume-and-introduction.md)
8. [简历的自我推销](https://aaronk9.gitbooks.io/programmer/content/ru-he-dong-shou-zhun-bei-project.html)