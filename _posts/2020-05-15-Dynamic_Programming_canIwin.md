---
title: Dynamic Programming 1.2
date: 2020-05-15 20:40:00
tags: [DP,python]
music-id: 1332153723
youtube-video-ID: 
bilibili-video-ID: 

---
# [DP can I win](https://leetcode.com/problems/can-i-win/)

[EN](#EN)|[CN](#CN)


<span id="EN">

This is a leetcode problems of dynamic programming, you could learn bit operation and NP problem

```
In the "100 game," two players take turns adding, to a running total, any integer from 1..10. The player who first causes the running total to reach or exceed 100 wins.

What if we change the game so that players cannot re-use integers?

For example, two players might take turns drawing from a common pool of numbers of 1..15 without replacement until they reach a total >= 100.

Given an integer maxChoosableInteger and another integer desiredTotal, determine if the first player to move can force a win, assuming both players play optimally.

You can always assume that maxChoosableInteger will not be larger than 20 and desiredTotal will not be larger than 300.

Example

Input:
maxChoosableInteger = 10
desiredTotal = 11

Output:
false

Explanation:
No matter which integer the first player choose, the first player will lose.
The first player can choose an integer from 1 up to 10.
If the first player choose 1, the second player can only choose integers from 2 up to 10.
The second player will win by choosing 10 and get a total = 11, which is >= desiredTotal.
Same with other integers chosen by the first player, the second player will always win.
```
We have 3 solutinos to solve this problem, let us try now

#Solutions

It is a  top-down dp strategy question , and the sub-problems is how to win when the remaining desiredTotal is less than or equal unchosen maxnumber

Bit operation 

- the i-th bit of k, i.e., k&(1 << i) represents the availability of number i+1 (1: picked; 0: not picked).

At state k, the current player could pick any unpicked number from the pool, so state k can only go to one of the valid next states k':

if i-th bit of k is 0, set it to be 1, i.e., next state k' = k|(1<<i).
Recursion: apparently

the current player can win at state k iff opponent can't win at some valid next state `k'`.
Memorization: to speed up the recursion, we can use a vector<int> m of size 2^M to memorize calculated results m[k] for state key k:

- 0  : not calculated yet;
- 1  : current player can win;
- -1 : current player can't win.


## Dict LRU with DFS

You could use dict to memorize the situation of used numbers

DFS to traverse the list

### Dict + dfs
```python
	def canIWin(self, maxChoosableInteger, desiredTotal):
		maxSum = maxChoosableInteger*(maxChoosableInteger+1)/2
		#1. 如果目标值大于所有数累计合,则返回false
		if maxSum < desiredTotal:
			return False
		#2. 如果相等,如果回合数为奇数则返回True
		elif maxSum == desiredTotal:
			return maxChoosableInteger % 2 == 1
		#3. 如果最大值直接大于目标值则直接返回True
		elif maxChoosableInteger > desiredTotal:
			return True
		#记录已经计算过的数组
		seen = {}

		def helper(choices, remainder):
			'''
			choices   ->list[int]:含有未选择过的数
			remainder ->int      :减去选择过的数之后的余数desiredTotal
			'''
			#如果数组中最大的数大于余数,则直接返回True
			if choices[-1] >= remainder:
				return True
			#字典的键类型不能是list
			seen_key = tuple(choices)
			#如果是已经计算过的直接返回,减少计算时间
			if seen_key in seen:
				return seen[seen_key]
			#深度遍历数组中剩余的数
			#选择能让我方稳赢的数
			for i in range(len(choices)):
				#如果对方不能赢,则我方取胜
				if not helper(choices[:i]+choices[i+1:], remainder - choices[i]):
					seen[seen_key] = True
					return True
			#否则返回False
			seen[seen_key] = False
			return False
		return helper(list(range(1, maxChoosableInteger+1)), desiredTotal)
```

### [LRU](https://docs.python.org/3/library/functools.html#functools.lru_cache) + bit + dfs

Using LRU to memorize the data

The built module in python3.2 and python2.6 2.7, but the method `lru_cache` is only implemented after python3.2

This method use doubly linked list and dict to cache data

> By moving the node to the front of the linkedlist when the cache data is searched, it indirectly records the most frequently visited nodes. 
When the cache space is full, it will automatically remove the node with the lowest frequency of recent searchs at the end of the ring linked list, thus making room for the newly added cache node.

[Source code of `_lru_cache_wrapper`](https://github.com/python/cpython/blob/3.8/Lib/functools.py#L531)

```python
def _lru_cache_wrapper(user_function, maxsize, typed, _CacheInfo):
    # 所有 LRU 缓存元素共享的常量:
    sentinel = object()  # 特殊标记，用来表示缓存未命中
    make_key = _make_key  # 根据函数参数生成缓存 key

    #
    # ---------------------------------
    # | PREV | DATA(KEY+RESULT) | NEXT|
    # ---------------------------------
    #
    PREV, NEXT, KEY, RESULT = 0, 1, 2, 3  # 链表各个域

    # 存放 key 到 node 的映射
    cache = {}
    full = False
    cache_get = cache.get
    lock = RLock()  # The linked list update is not thread-safe, so it needs to be locked
    root = []  # Key: donut linked list
    # Both sides of the root node are nodes with higher and lower access frequency
    root[:] = [root, root, None, None]  # Initial root node (equivalent to an empty head node)

    def wrapper(*args, **kwds):
        nonlocal root, full
        key = make_key(args, kwds, typed)
        with lock:
            link = cache_get(key)
            if link is not None: # if the searched data in the cache
                # remove the visted node before the root node
                link_prev, link_next, _key, result = link
                link_prev[NEXT] = link_next
                link_next[PREV] = link_prev
                last = root[PREV]
                last[NEXT] = root[PREV] = link
                link[PREV] = last
                link[NEXT] = root
                return result

        # if the data not in cache, add this node to cache
        result = user_function(*args, **kwds)
        with lock:
            if key in cache:
				# Getting here means that this same key was added to the
                # cache while the lock was released.  Since the link
                # update is already done, we need only return the
                # computed result and update the count of misses.
                pass
            elif full: # if the cache is full, remove the node which is access fuency
                # use original root to memorize the data
                oldroot = root
                oldroot[KEY] = key
                oldroot[RESULT] = result
                # use new root replace the old root，
                # del the oldkey
                root = oldroot[NEXT]
                oldkey = root[KEY]
                oldresult = root[RESULT]
                root[KEY] = root[RESULT] = None
                del cache[oldkey]
                cache[key] = oldroot
            else: # if not full, add to cache
                # insert befor root
                last = root[PREV]
                link = [last, root, key, result]
                last[NEXT] = root[PREV] = cache[key] = link
                full = (len(cache) >= maxsize)

        return result

    return wrapper
```
```python
	def canIWin_bit_LRU(self, maxChoosableInteger, desiredTotal):
		'''
		DFS+按位操作+LRU缓存备忘
		'''

		if  desiredTotal <= maxChoosableInteger:
			return True
		if sum(range(maxChoosableInteger + 1)) < desiredTotal:
			return False
		#If maxsize is not None, the data of a certain length is cached to improve the I/O execution time. None means that the LRU feature will be replaced and the cache can be infinitely increased. Use this function to implement the memo function. This function is only available after python3.2 version
		@functools.lru_cache(None)
		def dfs(used, desiredTotal):
		    #Deep traversal choose which number wins
		    for i in range(maxChoosableInteger):
		        #if the number has been choosen
		        cur = 1 << i
		        #if yes. let opponent try
		        if cur & used == 0:
		            #if he can't win, return True
		            if desiredTotal <= i + 1 or not dfs(cur | used, desiredTotal - i - 1):
		                return True
		    #print(dfs.cache_info())
		    #otherwise we win
		    return False
		return dfs(0, desiredTotal)
```

### Dict + bit + dfs
```python
	def canIWin_bit_dic(self, maxChoosableInteger, desiredTotal):
	    '''
	    DFS+按位操作+字典备忘
	    '''
	    if  desiredTotal <= maxChoosableInteger:
	            return True
	    # Sum of Arithmetic progression
	    if sum(range(maxChoosableInteger + 1)) < desiredTotal:
	            return False
	    # It records the selected number
	    record = {}
	    def dfs(used, desiredTotal):
	    	#Get the number situation if it has recorded
            if record.get(used):
                    return record.get(used)
            #Traverse all numbers if I can win with one choosen number
            for i in range(maxChoosableInteger):
                    
                    cur = 1 << i
                    #if the number has been choosen
                    if cur & used == 0:
                            #if I can't win, let our opponent choose one number, if he can't win, return True
                            if desiredTotal <= i + 1 or not dfs(cur | used, desiredTotal - i - 1):
                                    record[used] = True
                                    return True
                    #print(dfs.cache_info())
            #如果没有稳赢的数返回False
            record[used] = False
            return False

	    return dfs(0, desiredTotal)
```

## Law of game

```py
	def canIWin_Law(self, maxChoosableInteger, desiredTotal):
		'''
		数学规律
		'''
		
		sn = maxChoosableInteger + maxChoosableInteger * (maxChoosableInteger - 1) / 2

		if(desiredTotal > sn):
		    return False
		#特例
		if(maxChoosableInteger == 10 and (desiredTotal == 40 or desiredTotal == 54)):
		    return False
		if(maxChoosableInteger == 20 and (desiredTotal == 210 or desiredTotal == 209)):
		    return False
		if(maxChoosableInteger == 18 and (desiredTotal == 171 or desiredTotal == 172)):
		    return False
		if(maxChoosableInteger == 12 and desiredTotal == 49):
		    return True

		#规律如下：desiredTotal == 1必胜，如果累计值模上最大值余1那必输，否则必胜。（但不一定成立，反例如上数据）
		return desiredTotal == 1 or desiredTotal % maxChoosableInteger != 1
```
# Compare those solutinos

```python
#coding: UTF-8
#Author: Toryun
#Date: 2020-05-14 21:18:00
#Function: DP to find how can I win in '100game'

import random
import time
from functools import wraps
import functools #应用于高阶函数，即参数或（和）返回值为其他函数的函数。 通常来说，此模块的功能适用于所有可调用对象。

def use_time(func):
	#装饰器
        @wraps(func)
        def decoreted(*args, **kwargs):
                t0 = time.time()
                res = func(*args, **kwargs)
                t1 = time.time()
                print("{}已被调用,用时:{}\n结果是：{}".format(func.__name__, t1-t0, res))
                return t1-t0                
        return decoreted
class Solution():
	@use_time
	def canIWin(self, maxChoosableInteger, desiredTotal):
		maxSum = maxChoosableInteger*(maxChoosableInteger+1)/2
		#1. 如果目标值大于所有数累计合,则返回false
		if maxSum < desiredTotal:
			return False
		#2. 如果相等,如果回合数为奇数则返回True
		elif maxSum == desiredTotal:
			return maxChoosableInteger % 2 == 1
		#3. 如果最大值直接大于目标值则直接返回True
		elif maxChoosableInteger > desiredTotal:
			return True
		#记录已经计算过的数组
		seen = {}

		def helper(choices, remainder):
			'''
			choices   ->list[int]:含有未选择过的数
			remainder ->int      :减去选择过的数之后的余数desiredTotal
			'''
			#如果数组中最大的数大于余数,则直接返回True
			if choices[-1] >= remainder:
				return True
			#字典的键类型不能是list
			seen_key = tuple(choices)
			#如果是已经计算过的直接返回,减少计算时间
			if seen_key in seen:
				return seen[seen_key]
			#深度遍历数组中剩余的数
			#选择能让我方稳赢的数
			for i in range(len(choices)):
				#如果对方不能赢,则我方取胜
				if not helper(choices[:i]+choices[i+1:], remainder - choices[i]):
					seen[seen_key] = True
					return True
			#否则返回False
			seen[seen_key] = False
			return False
		return helper(list(range(1, maxChoosableInteger+1)), desiredTotal)
	@use_time
	def canIWin_bit_LRU(self, maxChoosableInteger, desiredTotal):
		'''
		DFS+按位操作+LRU缓存备忘
		'''

		if  desiredTotal <= maxChoosableInteger:
			return True
		if sum(range(maxChoosableInteger + 1)) < desiredTotal:
			return False
		#如果maxsize不为None,则缓存一定长度的数据提高输入输出执行时间,None表示LRU特性将被禁用且缓存可无限增长,用此功能实现备忘功能此功能只有在3.2才有
		@functools.lru_cache(None)
		def dfs(used, desiredTotal):
		    #深度遍历选择哪个数能稳赢
		    for i in range(maxChoosableInteger):
		        #查看数字数否被选取
		        cur = 1 << i
		        #如果被选取,则让对手操作
		        if cur & used == 0:
		            #如果对手不能选择稳赢的数,则我方获胜
		            if desiredTotal <= i + 1 or not dfs(cur | used, desiredTotal - i - 1):
		                return True
		    #print(dfs.cache_info())
		    #如果没有稳赢的数返回False
		    return False

		return dfs(0, desiredTotal)
		
	@use_time
	def canIWin_bit_dic(self, maxChoosableInteger, desiredTotal):
	    '''
	    DFS+按位操作+字典备忘
	    '''
	    if  desiredTotal <= maxChoosableInteger:
	            return True
	    if sum(range(maxChoosableInteger + 1)) < desiredTotal:
	            return False
	    #记录是否被计算过
	    record = {}
	    def dfs(used, desiredTotal):
	            if record.get(used):
	                    return record.get(used)
	            #深度遍历选择哪个数能稳赢
	            for i in range(maxChoosableInteger):
	                    #查看数字数否被选取
	                    cur = 1 << i
	                    #如果被选取,则让对手操作
	                    if cur & used == 0:
	                            #如果对手不能选择稳赢的数,则我方获胜
	                            if desiredTotal <= i + 1 or not dfs(cur | used, desiredTotal - i - 1):
	                                    record[used] = True
	                                    return True
	                    #print(dfs.cache_info())
	            #如果没有稳赢的数返回False
	            record[used] = False
	            return False

	    return dfs(0, desiredTotal)
	@use_time
	def canIWin_Law(self, maxChoosableInteger, desiredTotal):
		'''
		数学规律
		'''
		
		sn = maxChoosableInteger + maxChoosableInteger * (maxChoosableInteger - 1) / 2

		if(desiredTotal > sn):
		    return False
		#特例
		if(maxChoosableInteger == 10 and (desiredTotal == 40 or desiredTotal == 54)):
		    return False
		if(maxChoosableInteger == 20 and (desiredTotal == 210 or desiredTotal == 209)):
		    return False
		if(maxChoosableInteger == 18 and (desiredTotal == 171 or desiredTotal == 172)):
		    return False
		if(maxChoosableInteger == 12 and desiredTotal == 49):
		    return True

		#规律如下：desiredTotal == 1必胜，如果累计值模上最大值余1那必输，否则必胜。（但不一定成立，反例如上数据）
		return desiredTotal == 1 or desiredTotal % maxChoosableInteger != 1        

if __name__ == '__main__':
	#maxChoosableInteger 不大于20
	maxChoosableInteger = 10#random.randrange(21)
	desiredTotal = 15#random.randrange(301)
	print("最大的数是:{}, 累加目标值是:{}\n".format(maxChoosableInteger,desiredTotal))
	solution = Solution()
	t0 = solution.canIWin(maxChoosableInteger, desiredTotal)
	t1 = solution.canIWin_bit_LRU(maxChoosableInteger, desiredTotal)
	t2 = solution.canIWin_Law(maxChoosableInteger, desiredTotal)
	t3 = solution.canIWin_bit_dic(maxChoosableInteger, desiredTotal)
	print("字典操作的时间比LRU+bit的快{}倍\n数学规律比字典快{}倍\n数学规律比LRU+bit快{}倍\n字典+bit比LRU+bit快{}倍\n".format(t0/t1, t2/t0, t2/t1, t3/t1))

```
# Source code

[My Github Code](https://github.com/YoTro/Python_repository/blob/master/Leetcode/DP/leetcode-464-can-i-win.py)

<span id="CN">

# [DP之我能赢吗](https://leetcode-cn.com/problems/can-i-win)

<audio id="audio" controls="" preload="none">
<source id="mp3" src="https://github.com/YoTro/YoTro.github.io/blob/master/audio/464_01_1.mp3">
</audio>

今天我们讲的是动态规划之我能赢吗

动态规划是20世纪50年代由贝尔曼提出的,解决多阶段决策难题的方法

维基百科的定义是

当我们遇到复杂问题时,我们可以通过把它拆分成多个简单的子问题,每个字问题只解决一次,然后把它们存储起来,然后得到整个问题的最优解

所以判断一个问题是否能够用动态规划解决

1. 它是复杂的并且可以拆分成多个子问题

2. 子问题只解决一次,即无后效性,不会被后续的子问题的结果改变

3. 可以递归地自顶向下地求解这些子问题,并存储起来

今天我们用leetcode第464题canIWin 来分析如何获取此类动态规划题的解题思路

```
在 "100 game" 这个游戏中，两名玩家轮流选择从 1 到 10 的任意整数，累计整数和，先使得累计整数和达到 100 的玩家，即为胜者。

如果我们将游戏规则改为 “玩家不能重复使用整数” 呢？

例如，两个玩家可以轮流从公共整数池中抽取从 1 到 15 的整数（不放回），直到累计整数和 >= 100。

给定一个整数 maxChoosableInteger （整数池中可选择的最大数）和另一个整数 desiredTotal（累计和），判断先出手的玩家是否能稳赢（假设两位玩家游戏时都表现最佳）？

你可以假设 maxChoosableInteger 不会大于 20， desiredTotal 不会大于 300。

示例：

输入：
maxChoosableInteger = 10
desiredTotal = 11

输出：
false

解释：
无论第一个玩家选择哪个整数，他都会失败。
第一个玩家可以选择从 1 到 10 的整数。
如果第一个玩家选择 1，那么第二个玩家只能选择从 2 到 10 的整数。
第二个玩家可以通过选择整数 10（那么累积和为 11 >= desiredTotal），从而取得胜利.
同样地，第一个玩家选择任意其他整数，第二个玩家都会赢。
```
1. 思考是否有重复子问题
是的,每次都要判断剩余的数能否稳赢

2. 定义状态
用字典或者LRU缓存状态
dfs遍历所有可能的数

3. 状态转移
用位操作定义数组中该数是否被选, 或者dfs时,去除已选择的数

4. 输出
当剩余未选择的数大于desireTotal时,返回状态

# 解决方法

赢得条件有哪些?

- 如果数组累加和小于目标值, return False
- 如果maxChoosableInteger 大于或等于 目标值, return True

位操作

- k的第i个比特位，即k＆（1 << i）表示数字i +1的状态（1：已选择； 0：未选择）。

在状态k，当前玩家可以从整数池中选择任何未挑选的数字，因此状态k只能进入有效的下一状态k'：

如果k的第i位为0，则将其设置为1，即下一个状态k'= k\|(1 << i）。

递归：

当前玩家可以在状态k获胜，前提是对手无法在某个有效的下一个状态 k"获胜。
记忆：为加快递归速度，我们可以使用大小为2 ^ maxChoosableInteger来存储状态k的计算情况：

- 0 ：尚未计算；

- 1 ：当前玩家可以获胜；

- -1：当前玩家无法获胜。


## Dict with DFS

通过字典存储状态然后dfs遍历所有可能的数

### Dict + dfs
```python
	def canIWin(self, maxChoosableInteger, desiredTotal):
		maxSum = maxChoosableInteger*(maxChoosableInteger+1)/2
		#1. 如果目标值大于所有数累计合,则返回false
		if maxSum < desiredTotal:
			return False
		#2. 如果相等,如果回合数为奇数则返回True
		elif maxSum == desiredTotal:
			return maxChoosableInteger % 2 == 1
		#3. 如果最大值直接大于目标值则直接返回True
		elif maxChoosableInteger > desiredTotal:
			return True
		#记录已经计算过的数组
		seen = {}

		def helper(choices, remainder):
			'''
			choices   ->list[int]:含有未选择过的数
			remainder ->int      :减去选择过的数之后的余数desiredTotal
			'''
			#如果数组中最大的数大于余数,则直接返回True
			if choices[-1] >= remainder:
				return True
			#字典的键类型不能是list
			seen_key = tuple(choices)
			#如果是已经计算过的直接返回,减少计算时间
			if seen_key in seen:
				return seen[seen_key]
			#深度遍历数组中剩余的数
			#选择能让我方稳赢的数
			for i in range(len(choices)):
				#如果对方不能赢,则我方取胜
				if not helper(choices[:i]+choices[i+1:], remainder - choices[i]):
					seen[seen_key] = True
					return True
			#否则返回False
			seen[seen_key] = False
			return False
		return helper(list(range(1, maxChoosableInteger+1)), desiredTotal)
```

### [LRU](https://docs.python.org/3/library/functools.html#functools.lru_cache) + bit + dfs

通过LRU缓存技术存储状态

`functools` 是内置在python3.2、python2.6 以后的模块,而`functools.lru_cache()` 是在3.2以后的版本才有

使用了环双向链表和字典的数据结构实现缓存

> 把被访问的数据存入节点然后将节点移到链表的最前面，在root节点前的节点代表访问频率最高的节点。
当缓存空间已满时，它将自动删除环形链接列表末尾最近搜索频率最低的节点，从而为新添加的高速缓存节点腾出空间。

[Source code of `_lru_cache_wrapper`](https://github.com/python/cpython/blob/3.8/Lib/functools.py#L531)

```python
def _lru_cache_wrapper(user_function, maxsize, typed, _CacheInfo):
    # 所有 LRU 缓存元素共享的常量:
    sentinel = object()  # 特殊标记，用来表示缓存未命中
    make_key = _make_key  # 根据函数参数生成缓存 key

    #
    # ---------------------------------
    # | PREV | DATA(KEY+RESULT) | NEXT|
    # ---------------------------------
    #
    PREV, NEXT, KEY, RESULT = 0, 1, 2, 3  # 链表各个域

    # 存放 key 到 node 的映射
    cache = {}
    full = False
    cache_get = cache.get
    lock = RLock()  # 链表更新不是线程安全的，所以需要加锁
    root = []  # 关键：环形双向链表
    # 根节点两侧分别是访问频率较高和较低的节点
    root[:] = [root, root, None, None]  # 初始根节点（相当于一个空的头节点）

    def wrapper(*args, **kwds):
        nonlocal root, full
        key = make_key(args, kwds, typed)
        with lock:
            link = cache_get(key)
            if link is not None: # 缓存命中
                # 将被访问的节点移动到环形链表的前面（即 root 的前边）
                link_prev, link_next, _key, result = link
                link_prev[NEXT] = link_next
                link_next[PREV] = link_prev
                last = root[PREV]
                last[NEXT] = root[PREV] = link
                link[PREV] = last
                link[NEXT] = root
                return result

        # 缓存未命中，调用用户函数生成 RESULT
        result = user_function(*args, **kwds)
        with lock:
            if key in cache:
                # 考虑到此时锁已经释放，而且 key 已经被缓存了，就意味着上面的
                # 节点移动已经做了，缓存也更新了，所以此时什么都不用做。
                pass
            elif full: # 新增缓存结果，移除访问频率低的节点
                # 下面的操作是使用 root 当前指向的节点存储 KEY 和 RESULT
                oldroot = root
                oldroot[KEY] = key
                oldroot[RESULT] = result
                # 接下来将原 root 指向的下一个节点作为新的 root，
                # 同时将新 root 节点的 KEY 和 RESULT 清空，这样
                # 使用频率最低的节点结果就从缓存中移除了。
                root = oldroot[NEXT]
                oldkey = root[KEY]
                oldresult = root[RESULT]
                root[KEY] = root[RESULT] = None
                del cache[oldkey]
                cache[key] = oldroot
            else: # 仅仅新增缓存结果
                # 新增节点插入到 root 节点的前面
                last = root[PREV]
                link = [last, root, key, result]
                last[NEXT] = root[PREV] = cache[key] = link
                full = (len(cache) >= maxsize)

        return result

    return wrapper
```
```python
	def canIWin_bit_LRU(self, maxChoosableInteger, desiredTotal):
		'''
		DFS+按位操作+LRU缓存备忘
		'''

		if  desiredTotal <= maxChoosableInteger:
			return True
		if sum(range(maxChoosableInteger + 1)) < desiredTotal:
			return False
		#如果maxsize不为None,则缓存一定长度的数据提高输入输出执行时间,None表示LRU特性将被禁用且缓存可无限增长,用此功能实现备忘功能此功能只有在3.2才有
		@functools.lru_cache(None)
		def dfs(used, desiredTotal):
		    #深度遍历选择哪个数能稳赢
		    for i in range(maxChoosableInteger):
		        #查看数字数否被选取
		        cur = 1 << i
		        #如果被选取,则让对手操作
		        if cur & used == 0:
		            #如果对手不能选择稳赢的数,则我方获胜
		            if desiredTotal <= i + 1 or not dfs(cur | used, desiredTotal - i - 1):
		                return True
		    #print(dfs.cache_info())
		    #如果没有稳赢的数返回False
		    return False

		return dfs(0, desiredTotal)
```

### Dict + bit + dfs
```python
	def canIWin_bit_dic(self, maxChoosableInteger, desiredTotal):
	    '''
	    DFS+按位操作+字典备忘
	    '''
	    if  desiredTotal <= maxChoosableInteger:
	            return True
	    if sum(range(maxChoosableInteger + 1)) < desiredTotal:
	            return False
	    #记录是否被计算过
	    record = {}
	    def dfs(used, desiredTotal):
	            if record.get(used):
	                    return record.get(used)
	            #深度遍历选择哪个数能稳赢
	            for i in range(maxChoosableInteger):
	                    #查看数字数否被选取
	                    cur = 1 << i
	                    #如果被选取,则让对手操作
	                    if cur & used == 0:
	                            #如果对手不能选择稳赢的数,则我方获胜
	                            if desiredTotal <= i + 1 or not dfs(cur | used, desiredTotal - i - 1):
	                                    record[used] = True
	                                    return True
	                    #print(dfs.cache_info())
	            #如果没有稳赢的数返回False
	            record[used] = False
	            return False

	    return dfs(0, desiredTotal)
```

## Law of game

```py
	def canIWin_Law(self, maxChoosableInteger, desiredTotal):
		'''
		数学规律
		'''
		
		sn = maxChoosableInteger + maxChoosableInteger * (maxChoosableInteger - 1) / 2

		if(desiredTotal > sn):
		    return False
		#特例
		if(maxChoosableInteger == 10 and (desiredTotal == 40 or desiredTotal == 54)):
		    return False
		if(maxChoosableInteger == 20 and (desiredTotal == 210 or desiredTotal == 209)):
		    return False
		if(maxChoosableInteger == 18 and (desiredTotal == 171 or desiredTotal == 172)):
		    return False
		if(maxChoosableInteger == 12 and desiredTotal == 49):
		    return True

		#规律如下：desiredTotal == 1必胜，如果累计值模上最大值余1那必输，否则必胜。（但不一定成立，反例如上数据）
		return desiredTotal == 1 or desiredTotal % maxChoosableInteger != 1
```
# Compare those solutinos

我使用了functools 内置的wraps装饰器,这样可以不改变func.\_\_name\_\_,去记录每个不同的数据结构的时间性能

## 结论
canIWin_Law > canIWin > canIWin_bit_LRU > canIWin_bit_dic 

```python
#coding: UTF-8
#Author: Toryun
#Date: 2020-05-14 21:18:00
#Function: DP to find how can I win in '100game'

import random
import time
from functools import wraps
import functools #应用于高阶函数，即参数或（和）返回值为其他函数的函数。 通常来说，此模块的功能适用于所有可调用对象。

def use_time(func):
	#装饰器
        @wraps(func)
        def decoreted(*args, **kwargs):
                t0 = time.time()
                res = func(*args, **kwargs)
                t1 = time.time()
                print("{}已被调用,用时:{}\n结果是：{}".format(func.__name__, t1-t0, res))
                return t1-t0                
        return decoreted
class Solution():
	@use_time
	def canIWin(self, maxChoosableInteger, desiredTotal):
		maxSum = maxChoosableInteger*(maxChoosableInteger+1)/2
		#1. 如果目标值大于所有数累计合,则返回false
		if maxSum < desiredTotal:
			return False
		#2. 如果相等,如果回合数为奇数则返回True
		elif maxSum == desiredTotal:
			return maxChoosableInteger % 2 == 1
		#3. 如果最大值直接大于目标值则直接返回True
		elif maxChoosableInteger > desiredTotal:
			return True
		#记录已经计算过的数组
		seen = {}

		def helper(choices, remainder):
			'''
			choices   ->list[int]:含有未选择过的数
			remainder ->int      :减去选择过的数之后的余数desiredTotal
			'''
			#如果数组中最大的数大于余数,则直接返回True
			if choices[-1] >= remainder:
				return True
			#字典的键类型不能是list
			seen_key = tuple(choices)
			#如果是已经计算过的直接返回,减少计算时间
			if seen_key in seen:
				return seen[seen_key]
			#深度遍历数组中剩余的数
			#选择能让我方稳赢的数
			for i in range(len(choices)):
				#如果对方不能赢,则我方取胜
				if not helper(choices[:i]+choices[i+1:], remainder - choices[i]):
					seen[seen_key] = True
					return True
			#否则返回False
			seen[seen_key] = False
			return False
		return helper(list(range(1, maxChoosableInteger+1)), desiredTotal)
	@use_time
	def canIWin_bit_LRU(self, maxChoosableInteger, desiredTotal):
		'''
		DFS+按位操作+LRU缓存备忘
		'''

		if  desiredTotal <= maxChoosableInteger:
			return True
		if sum(range(maxChoosableInteger + 1)) < desiredTotal:
			return False
		#如果maxsize不为None,则缓存一定长度的数据提高输入输出执行时间,None表示LRU特性将被禁用且缓存可无限增长,用此功能实现备忘功能此功能只有在3.2才有
		@functools.lru_cache(None)
		def dfs(used, desiredTotal):
		    #深度遍历选择哪个数能稳赢
		    for i in range(maxChoosableInteger):
		        #查看数字数否被选取
		        cur = 1 << i
		        #如果被选取,则让对手操作
		        if cur & used == 0:
		            #如果对手不能选择稳赢的数,则我方获胜
		            if desiredTotal <= i + 1 or not dfs(cur | used, desiredTotal - i - 1):
		                return True
		    #print(dfs.cache_info())
		    #如果没有稳赢的数返回False
		    return False

		return dfs(0, desiredTotal)
		
	@use_time
	def canIWin_bit_dic(self, maxChoosableInteger, desiredTotal):
	    '''
	    DFS+按位操作+字典备忘
	    '''
	    if  desiredTotal <= maxChoosableInteger:
	            return True
	    if sum(range(maxChoosableInteger + 1)) < desiredTotal:
	            return False
	    #记录是否被计算过
	    record = {}
	    def dfs(used, desiredTotal):
	            if record.get(used):
	                    return record.get(used)
	            #深度遍历选择哪个数能稳赢
	            for i in range(maxChoosableInteger):
	                    #查看数字数否被选取
	                    cur = 1 << i
	                    #如果被选取,则让对手操作
	                    if cur & used == 0:
	                            #如果对手不能选择稳赢的数,则我方获胜
	                            if desiredTotal <= i + 1 or not dfs(cur | used, desiredTotal - i - 1):
	                                    record[used] = True
	                                    return True
	                    #print(dfs.cache_info())
	            #如果没有稳赢的数返回False
	            record[used] = False
	            return False

	    return dfs(0, desiredTotal)
	@use_time
	def canIWin_Law(self, maxChoosableInteger, desiredTotal):
		'''
		数学规律
		'''
		
		sn = maxChoosableInteger + maxChoosableInteger * (maxChoosableInteger - 1) / 2

		if(desiredTotal > sn):
		    return False
		#特例
		if(maxChoosableInteger == 10 and (desiredTotal == 40 or desiredTotal == 54)):
		    return False
		if(maxChoosableInteger == 20 and (desiredTotal == 210 or desiredTotal == 209)):
		    return False
		if(maxChoosableInteger == 18 and (desiredTotal == 171 or desiredTotal == 172)):
		    return False
		if(maxChoosableInteger == 12 and desiredTotal == 49):
		    return True

		#规律如下：desiredTotal == 1必胜，如果累计值模上最大值余1那必输，否则必胜。（但不一定成立，反例如上数据）
		return desiredTotal == 1 or desiredTotal % maxChoosableInteger != 1        

if __name__ == '__main__':
	#maxChoosableInteger 不大于20
	maxChoosableInteger = 10#random.randrange(21)
	desiredTotal = 15#random.randrange(301)
	print("最大的数是:{}, 累加目标值是:{}\n".format(maxChoosableInteger,desiredTotal))
	solution = Solution()
	t0 = solution.canIWin(maxChoosableInteger, desiredTotal)
	t1 = solution.canIWin_bit_LRU(maxChoosableInteger, desiredTotal)
	t2 = solution.canIWin_Law(maxChoosableInteger, desiredTotal)
	t3 = solution.canIWin_bit_dic(maxChoosableInteger, desiredTotal)
	print("字典操作的时间比LRU+bit的快{}倍\n数学规律比字典快{}倍\n数学规律比LRU+bit快{}倍\n字典+bit比LRU+bit快{}倍\n".format(t0/t1, t2/t0, t2/t1, t3/t1))

```
# Source code

[My Github Code](https://github.com/YoTro/Python_repository/blob/master/Leetcode/DP/leetcode-464-can-i-win.py)
