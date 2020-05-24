---
title: Linkedlist
date: 2020-05-07 16:51:00
tags: [Graph,Linkedlist,python]
music-id: 247529
youtube-video-ID: 
bilibili-video-ID: BV1wt411r7Zx
mathjax: true

---
# [Linked list](https://en.wikipedia.org/wiki/Linked_list)

[EN](#EN)|[CN](#CN)


<span id="EN">

In computer science, a linked list is a linear collection of data elements, whose order is not given by their physical placement in memory. Instead, each element points to the next. It is a data structure consisting of a collection of nodes which together represent a sequence. In its most basic form, each node contains: data, and a reference (in other words, a link) to the next node in the sequence. This structure allows for efficient insertion or removal of elements from any position in the sequence during iteration. More complex variants add additional links, allowing more efficient insertion or removal of nodes at arbitrary positions. A drawback of linked lists is that access time is linear (and difficult to pipeline). Faster access, such as random access, is not feasible. Arrays have better cache locality compared to linked lists.

Linked lists are among the simplest and most common data structures. They can be used to implement several other common abstract data types, including lists, stacks, queues, associative arrays, and S-expressions, though it is not uncommon to implement those data structures directly without using a linked list as the basis.

The principal benefit of a linked list over a conventional array is that the list elements can be easily inserted or removed without reallocation or reorganization of the entire structure because the data items need not be stored contiguously in memory or on disk, while restructuring an array at run-time is a much more expensive operation. Linked lists allow insertion and removal of nodes at any point in the list, and allow doing so with a constant number of operations by keeping the link previous to the link being added or removed in memory during list traversal.

On the other hand, since simple linked lists by themselves do not allow random access to the data or any form of efficient indexing, many basic operations—such as obtaining the last node of the list, finding a node that contains a given datum, or locating the place where a new node should be inserted—may require iterating through most or all of the list elements. The advantages and disadvantages of using linked lists are given below. Linked list are dynamic, so the length of list can increase or decrease as necessary. Each node does not necessarily follow the previous one physically in the memory.

## Create Linked list 

```python
class ListNode(object):
 def __init__(self, x):
     self.val = x
     self.next = None
res = []

```

## Add and Traversal

```python
class Solution:
    
    def __init__(self):
        self.head = None
        
    def add(self, head, x):
        if not head:
            head = ListNode(x)
        else:
            head.next = self.add(head.next, x)
        return head
    def add2(self, l):
        if len(l) == 0:
            return None
        for i in range(len(l)):
            self.head = self.add(self.head, l[i])
        return self.head
    def dfs(self, head):
        if head:
            res.append(head.val)
            self.dfs(head.next)
        
        return res
```   



## Delete

```python
    def delete(self, head, x):
        if not head or not head.next:
            return None
        a = head
        b = head.next
        if x != head.val:
            head.next = self.delete( head.next, x)
        if b.val == x:
            a.next = b.next
        return head
```

## Search

```python
    def search(self, x):
        node = self.head
        found = False
        while not found and node:
            if node.val == x:
                print("We found this node in the linked list")
                found = True
                return node
            else:
                node = node.next
        print("We can't found this node")
```

## Insert

```python
    def insert(self, index, x):
        '''
        index->int: 索引要插入的地方
        x->int:     要插入的值
        '''
        if not self.head:
            return None
        node = self.search(index)
        if node:
            tmp = ListNode(x)
            node.next = tmp
            tmp.next = node.next.next
        return self.head
```

## Update

```python
   def update(self, index, x):
        '''
        index->int: original node
        x->int:     new node
        '''
        if not self.head:
            return None
        node = self.search(index)
        if node:
            node.val = x
            print("Update success!")
        else:
            return None
        return node
```

### [Reverse Linked list](https://leetcode-cn.com/problems/reverse-linked-list/)

[loading...](https://pic.leetcode-cn.com/c573939fa872edd4da5ac39e703bccdc65c32849a95ba7637dea5cfdfb6eced6-%E5%B9%BB%E7%81%AF%E7%89%871.jpg)
[loading...](https://pic.leetcode-cn.com/bfa449ed16ecfc905f8ef8e9b049ed397b608f30dad84d37b2005e19797b1d49-%E5%B9%BB%E7%81%AF%E7%89%872.jpg)
[loading...](https://pic.leetcode-cn.com/c790acd5bd2ebf285ef2f687d76c9a48f2e32418acdb32909cc76cc069501ac7-%E5%B9%BB%E7%81%AF%E7%89%873.jpg)
[loading...](https://pic.leetcode-cn.com/d078fc5c224964b468210b205838ecac26efde1a0bd6fade6f3a758a87b82068-%E5%B9%BB%E7%81%AF%E7%89%874.jpg)
[loading...](https://pic.leetcode-cn.com/cd9c77c8c7873c4aa39bbb7a185b729151516021cee7009575c258fb82f77383-%E5%B9%BB%E7%81%AF%E7%89%875.jpg)
[loading...](https://pic.leetcode-cn.com/f89c9e095d414b8366dd7d490508ba9c99d6c93953b4429168af262169f18e83-%E5%B9%BB%E7%81%AF%E7%89%876.jpg)
[loading...](https://pic.leetcode-cn.com/485dbe9ad44ab7b05e01c46a1bc1718187a01f8a9fe8331f497e7011f9508b57-%E5%B9%BB%E7%81%AF%E7%89%877.jpg)
[loading...](https://pic.leetcode-cn.com/04550182527e6570d9d04f2eeae330848b83d3ff13b23ea4153410210586dc85-%E5%B9%BB%E7%81%AF%E7%89%878.jpg)
[loading...](https://pic.leetcode-cn.com/b167f5ee08eafab82d680cf782898b649dd2a4453cd122ad690e83e35c387a11-%E5%B9%BB%E7%81%AF%E7%89%879.jpg)
[loading...](https://pic.leetcode-cn.com/950f6edfb553cbeec65ce3c9679dd8d3401fd1837ad9eb16989217bf83f30e58-%E5%B9%BB%E7%81%AF%E7%89%8710.jpg)
[loading...](https://pic.leetcode-cn.com/dcd5bef9b8ff3de98e9533da7da3a8411643bbef9bc5e1a576085b4403197649-%E5%B9%BB%E7%81%AF%E7%89%8711.jpg)
[loading...](https://pic.leetcode-cn.com/1d6aaab3d9a42c20420fb6087e520ea05bd6d0789213f228481e55891b847b1e-%E5%B9%BB%E7%81%AF%E7%89%8712.jpg)
[loading...](https://pic.leetcode-cn.com/74bb9de62d3bb36a9c174d7702c7116974838b46eb96933e2921c054a34fa5d9-%E5%B9%BB%E7%81%AF%E7%89%8713.jpg)
[loading...](https://pic.leetcode-cn.com/e2bea51573eb5a763b5b9c0873887ced8932db95bc6bf4ff5befb363c8960984-%E5%B9%BB%E7%81%AF%E7%89%8714.jpg)
[loading...](https://pic.leetcode-cn.com/95db06ce86448f3265b0f3178baf7372915601e4b367fcde54ff5cf509532bc8-%E5%B9%BB%E7%81%AF%E7%89%8715.jpg)
[loading...](https://pic.leetcode-cn.com/fc96f0786c05a3a2ae0987757b4f569b3b18e59da0b5cb3afb063b9fc0ace069-%E5%B9%BB%E7%81%AF%E7%89%8716.jpg)
[loading...](https://pic.leetcode-cn.com/6a1a4b01a7dd20e3a5a560c482b514c7bd3d9462d8f297d373232229995e6681-%E5%B9%BB%E7%81%AF%E7%89%8717.jpg)
[loading...](https://pic.leetcode-cn.com/1318eda47ab9b0d505b98b5c11503012053a1bbcf754ce0c0145af14de5d41cd-%E5%B9%BB%E7%81%AF%E7%89%8718.jpg)[loading...](https://pic.leetcode-cn.com/a0669e29700f80938c95faf9a5cc839d316a31b6f6613da88bcdd31636897d96-%E5%B9%BB%E7%81%AF%E7%89%8719.jpg)[loading...](https://pic.leetcode-cn.com/30db7c04d56193840e53c4dc6c6f095ebe80b2b7a71107ca5ed19da499467914-%E5%B9%BB%E7%81%AF%E7%89%8720.jpg)[loading...](https://pic.leetcode-cn.com/052984cbdec432f46e771e0c7f214b7303941e4a11a7f59ee53a519e6b400adc-%E5%B9%BB%E7%81%AF%E7%89%8721.jpg)[loading...](https://pic.leetcode-cn.com/e096fac734909bc4b1ff55d8446332b5d7e67ec238266b0427ad0d69bc50174b-%E5%B9%BB%E7%81%AF%E7%89%8722.jpg)[loading...](https://pic.leetcode-cn.com/4e0975fd8d898364d24177684967fdf72d9476e64341c375ac2b5df7130b2f0a-%E5%B9%BB%E7%81%AF%E7%89%8723.jpg)
[loading...](https://pic.leetcode-cn.com/5625ed08dbaa84719f0b4b631fa6c4c7f08c1c5cf18646dbab2f65d410e99e8f-%E5%B9%BB%E7%81%AF%E7%89%8724.jpg)

```python
class Solution(object):
	def reverseList(self, head):
		"""
		:type head: ListNode
		:rtype: ListNode
		"""
		# The recursive termination condition is that the current is empty, or the next node is empty

		if(head==None or head.next==None):
			return head
		# the cur is last node
		cur = self.reverseList(head.next)
		
		# if the linked list is  1->2->3->4->5，cur is 5
		# head.val = 4，head.next.val = 5
		# head.next.next  => 5->4
		head.next.next = head
		head.next = None
		return cur

```

## Source code

[My Github Code](https://github.com/YoTro/Python_repository/blob/master/Linkedlist/Linkedlist.py)

<span id="CN"> 

# 链表

链表（Linked list）是一种常见的基础数据结构，是一种线性表，但是并不会按线性的顺序存储数据，而是在每一个节点里存到下一个节点的指针(Pointer)。由于不必须按顺序存储，链表在插入的时候可以达到O(1)的复杂度，比另一种线性表顺序表快得多，但是查找一个节点或者访问特定编号的节点则需要O(n)的时间，而顺序表相应的时间复杂度分别是O(logn)和O(1)。

使用链表结构可以克服数组链表需要预先知道数据大小的缺点，链表结构可以充分利用计算机内存空间，实现灵活的内存动态管理。但是链表失去了数组随机读取的优点，同时链表由于增加了结点的指针域，空间开销比较大。

在计算机科学中，链表作为一种基础的数据结构可以用来生成其它类型的数据结构。链表通常由一连串节点组成，每个节点包含任意的实例数据（data fields）和一或两个用来指向上一个/或下一个节点的位置的链接（"links"）。链表最明显的好处就是，常规数组排列关联项目的方式可能不同于这些数据项目在记忆体或磁盘上顺序，数据的访问往往要在不同的排列顺序中转换。而链表是一种自我指示数据类型，因为它包含指向另一个相同类型的数据的指针（链接）。链表允许插入和移除表上任意位置上的节点，但是不允许随机存取。链表有很多种不同的类型：单向链表，双向链表以及循环链表。


## 链表的创建

{% highlight ruby linenos %}
class ListNode(object):
 def __init__(self, x):
     self.val = x
     self.next = None
res = []

{% endhighlight %}

## 添加和遍历

```python
class Solution:
    
    def __init__(self):
        self.head = None
        
    def add(self, head, x):
        if not head:
            head = ListNode(x)
        else:
            head.next = self.add(head.next, x)
        return head
    def add2(self, l):
        if len(l) == 0:
            return None
        for i in range(len(l)):
            self.head = self.add(self.head, l[i])
        return self.head
    def dfs(self, head):
        if head:
            res.append(head.val)
            self.dfs(head.next)
        
        return res
    
```

## 删除

```python
    def delete(self, head, x):
        if not head or not head.next:
            return None
        a = head
        b = head.next
        if x != head.val:
            head.next = self.delete( head.next, x)
        if b.val == x:
            a.next = b.next
        return head
```

## 查询

```python
    def search(self, x):
        node = self.head
        found = False
        while not found and node:
            if node.val == x:
                print("We found this node in the linked list")
                found = True
                return node
            else:
                node = node.next
        print("We can't found this node")
```

## 插入

```python
    def insert(self, index, x):
        '''
        index->int: 索引要插入的地方
        x->int:     要插入的值
        '''
        if not self.head:
            return None
        node = self.search(index)
        if node:
            tmp = ListNode(x)
            node.next = tmp
            tmp.next = node.next.next
        return self.head
```

## 更新

```python
   def update(self, index, x):
        '''
        index->int: original node
        x->int:     new node
        '''
        if not self.head:
            return None
        node = self.search(index)
        if node:
            node.val = x
            print("Update success!")
        else:
            return None
        return node
```

## [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

[loading...](https://pic.leetcode-cn.com/c573939fa872edd4da5ac39e703bccdc65c32849a95ba7637dea5cfdfb6eced6-%E5%B9%BB%E7%81%AF%E7%89%871.jpg)
[loading...](https://pic.leetcode-cn.com/bfa449ed16ecfc905f8ef8e9b049ed397b608f30dad84d37b2005e19797b1d49-%E5%B9%BB%E7%81%AF%E7%89%872.jpg)
[loading...](https://pic.leetcode-cn.com/c790acd5bd2ebf285ef2f687d76c9a48f2e32418acdb32909cc76cc069501ac7-%E5%B9%BB%E7%81%AF%E7%89%873.jpg)
[loading...](https://pic.leetcode-cn.com/d078fc5c224964b468210b205838ecac26efde1a0bd6fade6f3a758a87b82068-%E5%B9%BB%E7%81%AF%E7%89%874.jpg)
[loading...](https://pic.leetcode-cn.com/cd9c77c8c7873c4aa39bbb7a185b729151516021cee7009575c258fb82f77383-%E5%B9%BB%E7%81%AF%E7%89%875.jpg)
[loading...](https://pic.leetcode-cn.com/f89c9e095d414b8366dd7d490508ba9c99d6c93953b4429168af262169f18e83-%E5%B9%BB%E7%81%AF%E7%89%876.jpg)
[loading...](https://pic.leetcode-cn.com/485dbe9ad44ab7b05e01c46a1bc1718187a01f8a9fe8331f497e7011f9508b57-%E5%B9%BB%E7%81%AF%E7%89%877.jpg)
[loading...](https://pic.leetcode-cn.com/04550182527e6570d9d04f2eeae330848b83d3ff13b23ea4153410210586dc85-%E5%B9%BB%E7%81%AF%E7%89%878.jpg)
[loading...](https://pic.leetcode-cn.com/b167f5ee08eafab82d680cf782898b649dd2a4453cd122ad690e83e35c387a11-%E5%B9%BB%E7%81%AF%E7%89%879.jpg)
[loading...](https://pic.leetcode-cn.com/950f6edfb553cbeec65ce3c9679dd8d3401fd1837ad9eb16989217bf83f30e58-%E5%B9%BB%E7%81%AF%E7%89%8710.jpg)
[loading...](https://pic.leetcode-cn.com/dcd5bef9b8ff3de98e9533da7da3a8411643bbef9bc5e1a576085b4403197649-%E5%B9%BB%E7%81%AF%E7%89%8711.jpg)
[loading...](https://pic.leetcode-cn.com/1d6aaab3d9a42c20420fb6087e520ea05bd6d0789213f228481e55891b847b1e-%E5%B9%BB%E7%81%AF%E7%89%8712.jpg)
[loading...](https://pic.leetcode-cn.com/74bb9de62d3bb36a9c174d7702c7116974838b46eb96933e2921c054a34fa5d9-%E5%B9%BB%E7%81%AF%E7%89%8713.jpg)
[loading...](https://pic.leetcode-cn.com/e2bea51573eb5a763b5b9c0873887ced8932db95bc6bf4ff5befb363c8960984-%E5%B9%BB%E7%81%AF%E7%89%8714.jpg)
[loading...](https://pic.leetcode-cn.com/95db06ce86448f3265b0f3178baf7372915601e4b367fcde54ff5cf509532bc8-%E5%B9%BB%E7%81%AF%E7%89%8715.jpg)
[loading...](https://pic.leetcode-cn.com/fc96f0786c05a3a2ae0987757b4f569b3b18e59da0b5cb3afb063b9fc0ace069-%E5%B9%BB%E7%81%AF%E7%89%8716.jpg)
[loading...](https://pic.leetcode-cn.com/6a1a4b01a7dd20e3a5a560c482b514c7bd3d9462d8f297d373232229995e6681-%E5%B9%BB%E7%81%AF%E7%89%8717.jpg)
[loading...](https://pic.leetcode-cn.com/1318eda47ab9b0d505b98b5c11503012053a1bbcf754ce0c0145af14de5d41cd-%E5%B9%BB%E7%81%AF%E7%89%8718.jpg)[loading...](https://pic.leetcode-cn.com/a0669e29700f80938c95faf9a5cc839d316a31b6f6613da88bcdd31636897d96-%E5%B9%BB%E7%81%AF%E7%89%8719.jpg)[loading...](https://pic.leetcode-cn.com/30db7c04d56193840e53c4dc6c6f095ebe80b2b7a71107ca5ed19da499467914-%E5%B9%BB%E7%81%AF%E7%89%8720.jpg)[loading...](https://pic.leetcode-cn.com/052984cbdec432f46e771e0c7f214b7303941e4a11a7f59ee53a519e6b400adc-%E5%B9%BB%E7%81%AF%E7%89%8721.jpg)[loading...](https://pic.leetcode-cn.com/e096fac734909bc4b1ff55d8446332b5d7e67ec238266b0427ad0d69bc50174b-%E5%B9%BB%E7%81%AF%E7%89%8722.jpg)[loading...](https://pic.leetcode-cn.com/4e0975fd8d898364d24177684967fdf72d9476e64341c375ac2b5df7130b2f0a-%E5%B9%BB%E7%81%AF%E7%89%8723.jpg)
[loading...](https://pic.leetcode-cn.com/5625ed08dbaa84719f0b4b631fa6c4c7f08c1c5cf18646dbab2f65d410e99e8f-%E5%B9%BB%E7%81%AF%E7%89%8724.jpg)


```python
class Solution(object):
	def reverseList(self, head):
		"""
		:type head: ListNode
		:rtype: ListNode
		"""
		# 递归终止条件是当前为空，或者下一个节点为空
		if(head==None or head.next==None):
			return head
		# 这里的cur就是最后一个节点
		cur = self.reverseList(head.next)
		# 这里请配合动画演示理解
		# 如果链表是 1->2->3->4->5，那么此时的cur就是5
		# 而head是4，head的下一个是5，下下一个是空
		# 所以head.next.next 就是5->4
		head.next.next = head
		# 防止链表循环，需要将head.next设置为空
		head.next = None
		# 每层递归函数都返回cur，也就是最后一个节点
		return cur

```

## 源码

[My Github Code](https://github.com/YoTro/Python_repository/blob/master/Linkedlist/Linkedlist.py)