---
title: LinkedList系列
comments: true
date: 2017-07-10 23:46:55
updated: 2017-07-17 09:46:55
categories: Leetcode
tags: [LinkedList, TwoPointer]
---
# LinkedList系列总结
24/27  
[x] Easy  
[x] Medium  
[x] Hard  
这种题，多画图，一步步来，确定哪个node指向哪个node就会好一点,之后把图放上来，会更容易复习！
## 基础
### dummyNode
适用于头节点需要进行操作，增删改，亦或者保存头节点，不被后续操作改变

```python
dummy = ListNode(0)
dummy.next = head
curr = head
```

### reverseList
Iterative版本，简要来说就是记录下一节点，当前节点指向上一节点，同步移动上一节点和当前节点。最后的curr为空，prev为头也就是最初链表的最后一个元素

```python
prev = None
curr = head
while curr:
    nextNode = curr.next
    curr.next = prev
    prev = curr
    curr = nextNode
return prev
```

Recursive版本的，一直到底，然后倒叙指针

```python
second = head.next
head.next = None
rest = self.reverseList(second)
second.next = head
return rest
```

变种1 92. Reverse Linked List II  
除了移动节点之外，关键是链接头和尾

```python
pre.next.next = curr //pre.next 为最初的头，.next则链接后来的尾和最初的尾 1-2-3-4-5，1-4-3-2-5，2-5
pre.next = newHead // 1-4
```
<!--more-->
### 快慢指针
用于检测环和找中点,见于  
 141. Linked List Cycle  
 142. Linked List Cycle II   
 ·234. Palindrome Linked List

```python
fast, slow = head, head
while fast and fast.next:
    fast = fast.next.next
    slow = slow.next
```

## Medium版本
·369. Plus One Linked List    
·445. Add Two Numbers II  
本质上用stack保存节点信息，然后不断在前方添加节点

```python
node.val = add_value % 10
carry = ListNode(add_value / 10)
carry.next = node
node = carry
add_value /= 10
```

Merge，Move系列  
·21. Merge Two Sorted Lists  
·328. Odd Even Linked List  
·86. Partition List  

```python
curr = dummy
while l1 and l2:
    if l1.val < l2.val:
        curr.next = l1
        l1 = l1.next
    else:
        curr.next = l2
        l2 = l2.next
    curr = curr.next
```
保证两个list都存在，然后剩余的append在后面；然后移动节点的过程中要数好步伐`while even and even.next:`  
·160. Intersection of Two Linked Lists  
·19. Remove Nth Node From End of List

`61. Rotate List  
trick的地方是找到最后一个node，并且链接第一个，使用常用模版，不过稍加改动，因为要找到最后一个node而不是长度，所以要提前终止循环 
 
```python
length = 1
while curr.next:
    curr = curr.next
    length += 1
curr.next = head
move = length-1-k%length
```

综合   
`143. Reorder List  
结合 以上多种方法，快慢指针找中点，反转，merge

```python
mid = self.findMiddle(head)
tail = self.reverse(mid.next)
mid.next = None

self.merge(head, tail)
```

`23. Merge k Sorted Lists  
一种方法是利用merge two list然后不断divide and conquer，另外一种比较简洁的是利用PriorityQueue，然后不断put和poll()进而每一个node都是所有优先队列中最小的一个

```python
q = PriorityQueue()
for node in lists:
    if node: ## empty
        q.put((node.val, node))
while q.qsize():
    curr.next = q.get()[1]
    curr = curr.next
    if curr.next:
        q.put((curr.next.val, curr.next))
```

`82. Remove Duplicates from Sorted List II  
因为要移除所有重复的node，所以势必要prev保存上一节点，然后如果中间因为重复节点而curr！= prev.next，要把prev节点的next放到curr的next节点，因为curr为重复节点的最后一个

```python
while curr:
	while curr.next and curr.val == curr.next.val: ## [1]
		curr = curr.next
		if prev.next != curr:
			prev.next = curr.next
			curr = prev.next
		else:
			prev = prev.next
			curr = curr.next
```

`109. Convert Sorted List to Binary Search Tree 
用helperfunction帮助，每一步找出子链表的中点，然后分别递归left和right节点。

```python
while fast!= tail and fast.next != tail:
    fast = fast.next.next
    slow = slow.next
root = TreeNode(slow.val)
root.left = self.helper(head, slow)
root.right = self.helper(slow.next, tail)
```

·148. Sort List  
分治法，然后分别对子链表merge

```python
prev,fast, slow =  None, head, head
while fast and fast.next:
    prev = slow
    slow = slow.next
    fast = fast.next.next
    
prev.next = None ## cut the middle
    
l1 = self.sortList(head)
l2 = self.sortList(slow)
    
return self.merge(l1, l2)
```

·24. Swap Nodes in Pairs
这道题勤画图，一步步来就好，iterative的方法比较烦，不过是`Reverse Nodes in k-Group`的简化版，那道题是LinkedList集大成者

```python
while curr.next and curr.next.next:
    first = curr.next   # 1
    second = curr.next.next #2
    
    first.next = second.next # 1-3
    curr.next = second  #-2
    curr.next.next = first  #2-1
    curr = curr.next.next # 1
```
    
    
`25. Reverse Nodes in k-Group
这道题是一道典型的综合题，适合复习备考多刷。它的子function是reverseList的改良，因为需要保存头节点和尾节点，所以需要设置lastNode和nextNode，然后与之相对应的就是lastNode不断和后面的节点进行调换。可以看看对比

```python
/*
 * 0->1->2->3->4->5->6
 * |           |   
 * pre        next
 *
 * after calling pre = reverse(pre, next)
 * 
 * 0->3->2->1->4->5->6
 *          |  |
 *          pre next 
 */
 
def reverseNode(self, pre, nextNode):
    lastNode = pre.next
    curr = lastNode.next
    
    while curr != nextNode:
        lastNode.next = curr.next
        curr.next = pre.next
        pre.next = curr
        curr = lastNode.next
    return lastNode
    
def reverseList(self, head):
	if not head or not head.next:
	    return head
	prev = None
	curr = head
	while curr:
	    nextNode = curr.next
	    curr.next = prev
	    prev = curr
	    curr = nextNode
	return prev
```
