---
title: BinaryTree's building and searching
date: 2020-04-23 15:57:00
tags: [Tree]
music-id: 
youtube-video-ID: 
bilibili-video-ID: 
mathjax: true

---

# [Binary Tree](https://en.wikipedia.org/wiki/Binary_tree)

[EN](#EN)|[CN](#CN)

<span id="EN"> 

## Definition

In computer science, a binary tree is a tree data structure in which each node has at most two children, which are referred to as the left child and the right child. A recursive definition using just set theory notions is that a (non-empty) binary tree is a tuple (L, S, R), where L and R are binary trees or the empty set and S is a singleton set. Some authors allow the binary tree to be the empty set as well.

From a graph theory perspective, binary (and K-ary) trees as defined here are actually arborescences. A binary tree may thus be also called a bifurcating arborescence—a term which appears in some very old programming books, before the modern computer science terminology prevailed. It is also possible to interpret a binary tree as an undirected, rather than a directed graph, in which case a binary tree is an ordered, rooted tree. Some authors use rooted binary tree instead of binary tree to emphasize the fact that the tree is rooted, but as defined above, a binary tree is always rooted. A binary tree is a special case of an ordered K-ary tree, where k is 2.

In mathematics, what is termed binary tree can vary significantly from author to author. Some use the definition commonly used in computer science,[7] but others define it as every non-leaf having exactly two children and don't necessarily order (as left/right) the children either.

In computing, binary trees are used in two very different ways:

First, as a means of accessing nodes based on some value or label associated with each node. Binary trees labelled this way are used to implement binary search trees and binary heaps, and are used for efficient searching and sorting. The designation of non-root nodes as left or right child even when there is only one child present matters in some of these applications, in particular it is significant in binary search trees. However, the arrangement of particular nodes into the tree is not part of the conceptual information. For example, in a normal binary search tree the placement of nodes depends almost entirely on the order in which they were added, and can be re-arranged (for example by balancing) without changing the meaning.
Second, as a representation of data with a relevant bifurcating structure. In such cases the particular arrangement of nodes under and/or to the left or right of other nodes is part of the information (that is, changing it would change the meaning). Common examples occur with Huffman coding and cladograms. The everyday division of documents into chapters, sections, paragraphs, and so on is an analogous example with n-ary rather than binary trees.

## Classify

1. Perfect Tree:

       x
     /   \
    /     \
   x       x
  / \     / \
 x   x   x   x
/ \ / \ / \ / \
x x x x x x x x

2. Complete Tree:  
A Binary Tree is complete Binary Tree if all levels are completely filled except possibly the last level ad then last level has all keys as left as possible.

       x
     /   \
    /     \
   x       x
  / \     / \
 x   x   x   x
/ \ /
x x x

3. Strict/Full Tree:  
Each node has exactly 0 or 2 children.

       x
     /   \
    /     \
   x       x
  / \ 
 x   x 
    / \
    x x 


## Create and [Traveral](https://en.wikipedia.org/wiki/Tree_traversal)

### Definite Treenode
```python
class TreeNode():
	def __init__(self,x):
		self.value = x #Node's value
		self.left = None #left node
		self.right = None #right node
```

### BFS() build Binarytree

```python 
class binarytree():
	def __init__(self):
		'''初始化根节点'''
		self.root = None
	def add(self,x):
		'''插入值为x'''
		if self.root is None:
			self.root = x
		else:
			#BFS to find the null node
			queue = []
			queue.append(self.root)
			while len(queue)>0:
				node = queue.pop(0)
				if node:
					if not node.left:
						#if node.left is None,add x to it
						node.left = TreeNode(x)
						return
					else:
						queue.append(node.left)#add this node's left node then search its child node if empty
					if not node.right:
						node.right = TreeNode(x)
						return
					else:
						queue.append(node.right)
```

### Delete a node

**There are three situations when deleting:**

1. The deleted node has no child nodes
2. The deleted node has only one child node
3. The deleted node has 2 child nodes


### Depth-first search of binary tree

These searches are referred to as depth-first search (DFS), since the search tree is deepened as much as possible on each child before going to the next sibling. For a binary tree, they are defined as access operations at each node, starting with the current node, whose algorithm is as follows:

The general recursive pattern for traversing a binary tree is this:

Go down one level to the recursive argument N. If N exists (is non-empty) execute the following three operations in a certain order:
(L)	Recursively traverse N's left subtree.
(R)	Recursively traverse N's right subtree.
(N)	Process the current node N itself.
Return by going up one level and arriving at the parent node of N.
In the examples (L) is mostly performed before (R). But (R) before (L) is also possible, see (RNL).

### Pre-order (NLR)

Access the data part of the current node.
Traverse the left subtree by recursively calling the pre-order function.
Traverse the right subtree by recursively calling the pre-order function.
The pre-order traversal is a topologically sorted one, because a parent node is processed before any of its child nodes is done.

### In-order (LNR)

Traverse the left subtree by recursively calling the in-order function.
Access the data part of the current node.
Traverse the right subtree by recursively calling the in-order function.
In a binary search tree ordered such that in each node the key is greater than all keys in its left subtree and less than all keys in its right subtree, in-order traversal retrieves the keys in ascending sorted order.

### Post-order (LRN)

Traverse the left subtree by recursively calling the post-order function.
Traverse the right subtree by recursively calling the post-order function.
Access the data part of the current node.

## Source code

[My Github Code](https://github.com/YoTro/Python_repository/blob/master/Trees/BST.py)

<span id="CN"> 

# [二叉树](https://baike.baidu.com/item/%E4%BA%8C%E5%8F%89%E6%A0%91/1602879?fr=aladdin)

## 定义与性质

在计算机科学中，二叉树（英语：Binary tree）是每个节点最多只有两个分支（即不存在分支度大于2的节点）的树结构。通常分支被称作“左子树”或“右子树”。二叉树的分支具有左右次序，不能随意颠倒。

二叉树的第i层至多拥有2^{i-1}个节点；深度为k的二叉树至多总共有2^{\begin{aligned}k\end{aligned}}-1}个节点（定义根节点所在深度  k_{0}=0，而总计拥有节点数符合的，称为“满二叉树”；深度为k有{n的节点一对一对应时，称为完全二叉树。对任何一棵非空的二叉树{ T}T，如果其叶片（终端节点）数为n_{0}，分支度为2的节点数为n_{2}，则 n_{0}=n_{2}+1。

与普通树不同，普通树的节点个数至少为1，而二叉树的节点个数可以为0；普通树节点的最大分支度没有限制，而二叉树节点的最大分支度为2；普通树的节点无左、右次序之分，而二叉树的节点有左、右次序之分。

二叉树通常作为数据结构应用，典型用法是对节点定义一个标记函数，将一些值与每个节点相关系。这样标记的二叉树就可以实现二叉搜索树和二叉堆，并应用于高效率的搜索和排序。

## [分类](https://stackoverflow.com/questions/12359660/difference-between-complete-binary-tree-strict-binary-tree-full-binary-tre)

1. 满二叉树(Strict/Full Tree)
       x
     /   \
    /     \
   x       x
  / \ 
 x   x 
    / \
    x x 
2. 完全二叉树(Complete Tree)

       x
     /   \
    /     \
   x       x
  / \     / \
 x   x   x   x
/ \ /
x x x

3. 平衡二叉树

4. 完美二叉树(Perfect tree)
       x
     /   \
    /     \
   x       x
  / \     / \
 x   x   x   x
/ \ / \ / \ / \
x x x x x x x x

## 二叉树的创建与遍历

### 定义一个节点

```python
class TreeNode():
	def __init__(self,x):
		self.value = x #该节点的值为x
		self.left = None #左节点
		self.right = None #右节点
```

### add函数来创建树

```python 
class binarytree():
	def __init__(self):
		'''初始化根节点'''
		self.root = None
	def add(self,x):
		'''插入值为x'''
		if self.root is None:
			self.root = x#如果根节点不存在则添加为根节点
		else:
			#广度优先遍历添加子节点
			queue = []
			queue.append(self.root)
			while len(queue)>0:
				node = queue.pop(0)
				if node:
					if not node.left:
						#如果左节点为空
						node.left = TreeNode(x)#把x添加到左节点
						return
					else:
						queue.append(node.left)#把左节点放入队列,后面在pop出来查询它的子节点是否为空
					if not node.right:
						node.right = TreeNode(x)#把x添加到右节点
						return
					else:
						queue.append(node.right)
```				
### 删除一个节点

**删除的时候有三种情况**

1. 被删除的节点没有子节点
2. 被删除的节点有一个子节点
3. 被删除的节点有2个子节点

### 深度优先遍历

在深度优先级中，我们希望借助**栈**从根结点访问最远的结点。和图的深度优先搜索不同的是，不需记住访问过的每一个结点，因为树中不会有环。前序，中序和后序遍历都是深度优先遍历的特例。

### 广度优先遍历

和深度优先遍历不同，广度优先遍历会先访问离根节点最近的节点。二叉树的广度优先遍历又称按层次遍历。算法借助**队列**实现。


## 源码

[My Github Code](https://github.com/YoTro/Python_repository/blob/master/Trees/BST.py)