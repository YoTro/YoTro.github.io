---
title: How to Solve The Shortest Path Problem in Grapgh 1.2
date: 2020-05-25 17:44:00
tags: [Graph,python]
music-id: 
youtube-video-ID: 
bilibili-video-ID: 

---

# Breath Fisrt Search

[EN](#EN)|[CN](#CN)

<span id="EN">

Breadth-first search (BFS) is an algorithm for traversing or searching tree or graph data structures. It starts at the tree root (or some arbitrary node of a graph, sometimes referred to as a 'search key'), and explores all of the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level.

It uses the opposite strategy as depth-first search, which instead explores the node branch as far as possible before being forced to backtrack and expand other nodes.

BFS and its application in finding connected components of graphs were invented in 1945 by Konrad Zuse, in his (rejected) Ph.D. thesis on the Plankalkül programming language, but this was not published until 1972. It was reinvented in 1959 by Edward F. Moore, who used it to find the shortest path out of a maze, and later developed by C. Y. Lee into a wire routing algorithm (published 1961). 

![Animated_BFS](https://upload.wikimedia.org/wikipedia/commons/4/46/Animated_BFS.gif)
## [Leetcode 1263th minipushbox](https://leetcode.com/problems/minimum-moves-to-move-a-box-to-their-target-location/)

```
Storekeeper is a game in which the player pushes boxes around in a warehouse trying to get them to target locations.

The game is represented by a grid of size m x n, where each element is a wall, floor, or a box.

Your task is move the box 'B' to the target position 'T' under the following rules:

Player is represented by character 'S' and can move up, down, left, right in the grid if it is a floor (empy cell).
Floor is represented by character '.' that means free cell to walk.
Wall is represented by character '#' that means obstacle  (impossible to walk there). 
There is only one box 'B' and one target cell 'T' in the grid.
The box can be moved to an adjacent free cell by standing next to the box and then moving in the direction of the box. This is a push.
The player cannot walk through the box.
Return the minimum number of pushes to move the box to the target. If there is no way to reach the target, return -1.
```
### Solution

1. Locate important positions which are `S`,`#`,`T`
2. Determine the direction of movement (judge whether it crosses the boundary or against the wall)
3. Two-dimensional array to store the movement track
4. BFS traversal every node,return minipush

![BFS_minipushbox](/images/Algorithm\ formula/minipushbox.gif)

```python
import random
import time
import numpy as np
class Maze():
	'''generate random maze by DFS'''
	def __init__(self):
		L = random.randint(5,21) #size of maze
		#definte the wall and path
		Wall = 0
		Path = 1
		wall = "#"
		path = "."
		#complexity of maze
		Rank = 0
		RandMAX = 32767
		#initialize the maze
		self.maze = np.zeros((L,L))
		
	def __createMaze(self, x, y):
		'''
		(x, y)
		'''
		self.maze[x][y] = Path
		#chonse a random direction
		#build a random array
		direction = [[-1,0], [0,1], [0,-1], [1,0]]
		random.shuffle(direction)
		for i in range(4):
			#set the complexity of maze
			if Rank == 0:
				rank = 1
			else:
				rank = 1 + random.randint(0, RandMAX) % Rank
			while rank > 0:
				dx = x + direction[i][0]
				dy = y + direction[i][1]
				#if back then break
				if self.maze[dx][dy] == Path:
					break

				#judge whether mining out
				count = 0
				for j in range(dx - 1, dx + 2, 1):
					for k in range(dy - 1, dy + 2, 1):
						#judge whether this position (dx, dy) only one wall beside its
						if abs(j-dx) + abs(k-dy) == 1 and self.maze[j][k] == Path:
							count += 1
				#if dig out
				if count > 1:
					break
				rank -= 1
				self.maze[dx][dy] = Path
			if rank <= 0:
				self.__createMaze(dx, dy)
	def CreateMaze(self):
		#change shell of maze to path avoid dig out 
		for i in range(len(self.maze)):
			for j in range(len(self.maze[0])):
				self.maze[i][0] = Path
				self.maze[i][L-1] = Path
				self.maze[L-1][j] = Path
				self.maze[0][j] = Path
		self.__createMaze(2, 2)
		self.showMaze(self.maze)
		#copy every rows and columns twice
		maze1 = np.repeat(self.maze, 2, axis=1)
		maze1 = np.repeat(maze1, 2, axis=0)
		print("New maze")
		self.showMaze(maze1)
	def showMaze(self, maze):
		for i in range(len(maze)):
			for j in range(len(maze[0])):
				if maze[i][j]:
					print(path),
				else:
					print(wall),
			print("\n")

class Solution():
	'''get the minipushbox'''
	def minipushbox(self, grid):
		'''
		1. 确定各个重要位置
		2. 判断运动方向(是否越界或者碰墙)
		3. 储存运动轨迹
		4. BFS遍历返回值
		'''
		ret = -1 #initialize the value
		n = len(grid)
		m = len(grid[0])
		mx = [0, 0, -1, 1] #move 
		my = [-1, 1, 0, 0]
		def get_pos(p):
			for i in range(n):
				for j in range(m):
					if grid[i][j] == p:
						return (i,j)
		#player position
		Sp = get_pos("S")
		#box position
		Bp = get_pos("B")
		#Target position
		Tp = get_pos("T")
		def judge(sx, sy)z:
			'''judge whether it crosses the boundary or against the wall'''
			if sx < 0 or sx > n and sy <0 or sy > m:
				return False
			if grid[sx][sy] == "#":
				return False
			return True
		#if the node has traversal
		visit = [[-1]*2021 for _ in range(2021)]
		def path_visit(Sp, Bp, step):
			#x100 avoid conflict between 2 coordinates
			s1 = 100*Sp[0] + Sp[1]
			s2 = 100*Bp[0] + Bp[1]
			#has not traversed
			if visit[s1][s2] == -1:
				visit[s1][s2] = step
				return True
			if step >= visit[s1][s2]:
				return False
			visit[s1][s2] = step
			return True
		#generate node
		s = (Sp, Bp, 0)
		#queue helps BFS
		q = [s]
		#begin to BFS
		while len(q) > 0:
			Sp, Bp, step = q.pop(0)
			#if the box arrives Target
			if Bp[0] == Tp[0] and Bp[1] == Tp[1]:
				if ret == -1:
					ret = step
				ret = min(ret, step)
			#Player try to up,down,right or left
			for i in range(4):
				sx = Sp[0] + mx[i]
				sy = Sp[1] + my[i]
				if not judge(sx, sy):
					continue
				#update the position of player
				new_sp = (sx, sy)
				#When players find the box, they move along the direction of the box +(mx[i],my[i])
				if sx == Bp[0] and sy == Bp[1]:
					bx = Bp[0] + mx[i]
					by = Bp[1] + my[i]
					if not judge(bx, by):
						continue
					#Update box position
					new_bp = (bx, by)
					f = 1
				else:
					#If the player does not find the box, the original position of the box doesn't change, and the player continues to traverse
					new_bp = (Bp[0], Bp[1])
					f = 0
				#add new node to queue
				if path_visit(new_sp, new_bp, step + f):
					q.append((new_sp, new_bp, step + f))
		return ret 


```
<span id="CN">

# 广度优先遍历

使用队列,遍历一个节点的所有儿子节点, 然后在进入下一层遍历下一层级的所有子节点

像水波一样扩散出去

今天我们来看看Leetcode 第1263题 推箱子使用BFS来解决此问题

## [Leetcode 第1263题 推箱子](https://leetcode-cn.com/problems/minimum-moves-to-move-a-box-to-their-target-location/)

```
「推箱子」是一款风靡全球的益智小游戏，玩家需要将箱子推到仓库中的目标位置。

游戏地图用大小为 n * m 的网格 grid 表示，其中每个元素可以是墙、地板或者是箱子。

现在你将作为玩家参与游戏，按规则将箱子 'B' 移动到目标位置 'T' ：

玩家用字符 'S' 表示，只要他在地板上，就可以在网格中向上、下、左、右四个方向移动。
地板用字符 '.' 表示，意味着可以自由行走。
墙用字符 '#' 表示，意味着障碍物，不能通行。 
箱子仅有一个，用字符 'B' 表示。相应地，网格上有一个目标位置 'T'。
玩家需要站在箱子旁边，然后沿着箱子的方向进行移动，此时箱子会被移动到相邻的地板单元格。记作一次「推动」。
玩家无法越过箱子。
返回将箱子推到目标位置的最小 推动 次数，如果无法做到，请返回 -1。

```
### 方法

1. 确定各个重要位置
2. 判断运动方向(是否越界或者碰墙)
3. 二维数组储存运动轨迹
4. BFS遍历返回值

![BFS_minipushbox](/images/Algorithm\ formula/minipushbox.gif)

```python
import random
import time
import numpy as np
class Maze():
	'''随机生成一个游戏沙盒地图此处用了DFS'''
	def __init__(self):
		L = random.randint(5,21) #迷宫大小
		#墙和路定义
		Wall = 0
		Path = 1
		wall = "#"
		path = "."
		#迷宫复杂度, 数值越大,迷宫越简单
		Rank = 0
		RandMAX = 32767
		#初始化迷宫
		self.maze = np.zeros((L,L))
		
	def __createMaze(self, x, y):
		'''
		(x, y)
		'''
		self.maze[x][y] = Path
		#选择方向开挖
		#所以需要生成一个随机方向数组
		direction = [[-1,0], [0,1], [0,-1], [1,0]]
		random.shuffle(direction)
		for i in range(4):
			#设置迷宫复杂度
			if Rank == 0:
				rank = 1
			else:
				rank = 1 + random.randint(0, RandMAX) % Rank
			while rank > 0:
				dx = x + direction[i][0]
				dy = y + direction[i][1]
				#如果回头则break
				if self.maze[dx][dy] == Path:
					break

				#判断是否挖空
				count = 0
				for j in range(dx - 1, dx + 2, 1):
					for k in range(dy - 1, dy + 2, 1):
						#判断(dx,dy)新位置的上下左右位置是否只有一个坐标为通路
						if abs(j-dx) + abs(k-dy) == 1 and self.maze[j][k] == Path:
							count += 1
				#挖穿退出
				if count > 1:
					break
				rank -= 1
				self.maze[dx][dy] = Path
			if rank <= 0:
				self.__createMaze(dx, dy)
	def CreateMaze(self):
		#把最外层修改成路防止挖穿
		for i in range(len(self.maze)):
			for j in range(len(self.maze[0])):
				self.maze[i][0] = Path
				self.maze[i][L-1] = Path
				self.maze[L-1][j] = Path
				self.maze[0][j] = Path
		self.__createMaze(2, 2)
		self.showMaze(self.maze)
		#行列各复制一次扩展
		maze1 = np.repeat(self.maze, 2, axis=1)
		maze1 = np.repeat(maze1, 2, axis=0)
		print("New maze")
		self.showMaze(maze1)
	def showMaze(self, maze):
		'''
		打印迷宫
		'''
		for i in range(len(maze)):
			for j in range(len(maze[0])):
				if maze[i][j]:
					print(path),
				else:
					print(wall),
			print("\n")

class Solution():
	'''获取箱子移动最短步数'''
	def minipushbox(self, grid):
		'''
		1. 确定各个重要位置
		2. 判断运动方向(是否越界或者碰墙)
		3. 储存运动轨迹
		4. BFS遍历返回值
		'''
		ret = -1 #初始化最终返回值
		n = len(grid)
		m = len(grid[0])
		mx = [0, 0, -1, 1] #上下左右移动
		my = [-1, 1, 0, 0]
		def get_pos(p):
			'''获取物体位置'''
			for i in range(n):
				for j in range(m):
					if grid[i][j] == p:
						return (i,j)
		#玩家位置
		Sp = get_pos("S")
		#箱子位置
		Bp = get_pos("B")
		#目标位置
		Tp = get_pos("T")
		def judge(sx, sy)z:
			'''判断是否越界或者碰墙'''
			if sx < 0 or sx > n and sy <0 or sy > m:
				return False
			if grid[sx][sy] == "#":
				return False
			return True
		#判断是否走过
		visit = [[-1]*2021 for _ in range(2021)]
		def path_visit(Sp, Bp, step):
			#(4,4)和(5,3)的和都等于8, 这样会造成坐标冲突,x100可以避免坐标冲突
			s1 = 100*Sp[0] + Sp[1]
			s2 = 100*Bp[0] + Bp[1]
			#未遍历过更新步数
			if visit[s1][s2] == -1:
				visit[s1][s2] = step
				return True
			#如果步长大于已知步长, 则返回False
			if step >= visit[s1][s2]:
				return False
			#如果小于,则更新步长
			visit[s1][s2] = step
			return True
		#生成节点s(玩家位置, 箱子位置, 箱子移动步数)
		s = (Sp, Bp, 0)
		#队列帮助BFS
		q = [s]
		#开始BFS
		while len(q) > 0:
			Sp, Bp, step = q.pop(0)
			#判断箱子是否到达目标位置
			if Bp[0] == Tp[0] and Bp[1] == Tp[1]:
				if ret == -1:
					ret = step
				ret = min(ret, step)
			#玩家向左右上下开始遍历
			for i in range(4):
				sx = Sp[0] + mx[i]
				sy = Sp[1] + my[i]
				#判断玩家是否越界或者碰墙
				if not judge(sx, sy):
					continue
				#更新玩家位置
				new_sp = (sx, sy)
				#玩家找到箱子,则沿着箱子的方向进行位移(mx[i],my[i])
				if sx == Bp[0] and sy == Bp[1]:
					bx = Bp[0] + mx[i]
					by = Bp[1] + my[i]
					if not judge(bx, by):
						continue
					#更新箱子位置
					new_bp = (bx, by)
					#更新步长
					f = 1
				else:
					#如果玩家未找到箱子,箱子原位置不动, 玩家继续遍历
					new_bp = (Bp[0], Bp[1])
					f = 0
				if path_visit(new_sp, new_bp, step + f):
					q.append((new_sp, new_bp, step + f))
		return ret 


```


