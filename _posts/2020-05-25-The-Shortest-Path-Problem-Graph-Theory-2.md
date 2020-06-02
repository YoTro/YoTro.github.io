---
title: How to Solve The Shortest Path Problem in Grapgh 1.2
date: 2020-05-25 17:44:00
tags: [Graph,python]
music-id: 1424343020
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

#### BFS

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

#### [Astar](https://en.wikipedia.org/wiki/A*_search_algorithm) + [Best-first](https://en.wikipedia.org/wiki/Best-first_search)

1. Firstly people use best-first algorithm to find the box and reach the box side, then push the box to the target using Astar algorithm
2. It is easy to caculate that use a complex number to represent the coordinates
3. The best-first algorithm uses the priority queue ([min heap](https://en.wikipedia.org/wiki/Binary_heap)) (the evaluation function is [Euclidean distance](https://en.wikipedia.org/wiki/Euclidean_distance)) to traverse all paths, and the traversed nodes are removed from the queue until they reach the end point (beside the box)
4. Push the box along the direction of the box. If the box is not on the floor assembly, it means that it has crossed the boundary
5. In Astar algorithm, the estimation function is [Manhattan distance](https://en.wikipedia.org/wiki/Manhattan_distance), and the positions of people and boxes cannot be repeatedf

##### Astar steps

1. Create two min-heaps, openlist and closedlist,
2. Openlist is used to store nodes to be traversed
3. Closedlist is used to store the traversed nodes
4. Estimate function f to guide the direction of traversal 
5. The end traversal condition is: openlist is empty, or the end point is traversed in openlist

```
Step

1. Build a min-Heap open_list and add the starting point to the open_list.
2. Repeat the following process:
a. Traverse the open list, find the node with the lowest F value, and take it as the current node to be processed.
b. Move this node to the close list.
c. For each of the eight adjacent childnodes of the current node
◆ if it is not reachable or it is in the close list, ignore it. Otherwise, do the following.
◆ if it is not in the open list, add it to the open list, and set the current node as its father, and calculate the F, G and H values of the node.
◆ if it is already in the open list, check whether this path (i.e. through the current node to get to it) is better, and use the g value as a reference. A smaller g value means that this is a better path from start node to this adjacent node. If so, set its parent to the current node, recalculate its G and F values, and then push to open list. If your open list is sorted by F value, it will be reordered after being put into the heap.
d. Stop condition,
Add the end point to the open list, and the path has been found or failed to find the end point, and the open list is empty, there is no path at this time.
3. Save the path. From the end, each node moves along the parent node to the start, which is your path.
```

```python
import heapq
import copy
import collections
class Solution():
    def minPushBox(self, grid):
        n, m = len(grid), len(grid[0])     #Maze size
        g = collections.defaultdict(list)  #Store coordinates for each important position
        for i in range(n):
            for j in range(m):
                g[grid[i][j]] += [complex(i,j)]
        sp = g["S"][0]  #Player
        tp = g["T"][0]  #Target
        bp = g["B"][0]  #Box
        path = g["."]   #Path
        wall = g["#"]   #wall
        directions = (1, -1, 1j, -1j)#Up, down, left and right
        visited = set() #Store visited nodes
        step = 1        #Moving steps of the box
        Path_set = path + [sp] + [tp] + [bp]   #List of all path coordinates


        #A* Valuation function
        def F(a, s):
            '''
			a: Current position coordinates
			s: Steps taken
			Returns a tuple, one of which is Manhattan distance and the other is European distance
            '''
            euclidean_dist = abs(a - tp)
            manhattan_dist = abs((a - tp).real) + abs((a - tp).imag) + s 
            return (manhattan_dist, euclidean_dist)
        #Move for Player
        def bestfirst(from_position, to_position, path_set):
            '''
			The best-first algorithm is also called A algorithm, similar to A *
			from_ Position:          current position
			To_ Position:            last position
			path_ Set:               determine whether the location is on the path set
			Return value rtype:      bool
            '''
            p = 0                                #Prevent the nodes in the min-heap from being repeatedly calculated, similar to the unique key value prime_ Key
            f = abs(from_position - to_position) #The evaluation function is the distance between the current node and the target node
            heapplayer = [(f, p, from_position)]
            #Priority queue for player traversal 
            while heapplayer:
                #Pop the node with the shortest path absolute value, where the F and P parameters are useless
                f, _, curr_position = heapq.heappop(heapplayer)
                #Returns true if the target position is reached
                if curr_position == to_position:
                    return True
                #Traverse the four directions of the current position
                for direction in directions:
                    next_position = curr_position + direction
                    #If the new location does not cross the boundary or on the wall, add the priority queue (min-heap) for entering player
                    if next_position in path_set:
                        #print(abs(next_position - to_position), p, next_position)
                        heapq.heappush(heapplayer, (abs(next_position - to_position), p, next_position))
                        p += 1
                        path_set.remove(next_position) #remove the position if has visited
            return False #if can't reach, return false
        t = 1        #The first several parameters of the node are the same after preventing the node from entering the heap,occurrence of complex number comparison       
        node = (F(bp, step), step, t, sp, bp)
        heapbox = [node]                #min-heap of box path
        while heapbox:
            #Pop the node from heap, where the evaluation function and t are not used in the program
            f, step, _, sp, bp = heapq.heappop(heapbox)
            #traverse up,down,left and right
            for direction in directions:
                #Player's next position needs to be next to the box
                next_position = bp - direction
                #The next position of the box is the position after one step along the direction of the box        
                nextbox_position = bp + direction
                #The box must be on the path
                if nextbox_position in Path_set:
                	#There is no repeated visit to the location of player and boxes
                    if (next_position, bp) not in visited:
                    	#The position of the box must be removed because player cannot on or cross it
                        if bp in Path_set:
                            copypathset = copy.deepcopy(Path_set)
                            copypathset.remove(bp)
                        #Player go to the box current position and next to the box
                        if bestfirst(sp, next_position, copypathset):
                            #Box arrives target, return steps
                            if nextbox_position == tp:
                                return step
                            heapq.heappush(heapbox, (F(nextbox_position, step + 1), step + 1, t, bp, nextbox_position))
                            t += 1
                            #Add traversed node
                            visited.add((next_position, bp))
        return -1

```

#### [Tarjan](https://baike.baidu.com/item/tarjan%E7%AE%97%E6%B3%95) + Astar算法

[tarjan](https://en.wikipedia.org/wiki/Tarjan%27s_strongly_connected_components_algorithm)

Tarjan's algorithm is an algorithm in graph theory for finding the strongly connected components of a directed graph. It runs in linear time, matching the time bound for alternative methods including Kosaraju's algorithm and the path-based strong component algorithm. Tarjan's algorithm is named for its inventor, Robert Tarjan.

The algorithm takes a directed graph as input, and produces a partition of the graph's vertices into the graph's strongly connected components. Each vertex of the graph appears in exactly one of the strongly connected components. Any vertex that is not on a directed cycle forms a strongly connected component all by itself: for example, a vertex whose in-degree or out-degree is 0, or any vertex of an acyclic graph.

The basic idea of the algorithm is this: a depth-first search begins from an arbitrary start node (and subsequent depth-first searches are conducted on any nodes that have not yet been found). As usual with depth-first search, the search visits every node of the graph exactly once, declining to revisit any node that has already been visited. Thus, the collection of search trees is a spanning forest of the graph. The strongly connected components will be recovered as certain subtrees of this forest. The roots of these subtrees are called the "roots" of the strongly connected components. Any node of a strongly connected component might serve as a root, if it happens to be the first node of a component that is discovered by search.

`Stack invariant`

Nodes are placed on a stack in the order in which they are visited. When the depth-first search recursively visits a node v and its descendants, those nodes are not all necessarily popped from the stack when this recursive call returns. The crucial invariant property is that a node remains on the stack after it has been visited if and only if there exists a path in the input graph from it to some node earlier on the stack. In other words it means that in the DFS a node would be only removed from the stack after all its connected paths have been traversed. When the DFS will backtrack it would remove the nodes on a single path and return back to the root in order to start a new path.

At the end of the call that visits v and its descendants, we know whether v itself has a path to any node earlier on the stack. If so, the call returns, leaving v on the stack to preserve the invariant. If not, then v must be the root of its strongly connected component, which consists of v together with any nodes later on the stack than v (such nodes all have paths back to v but not to any earlier node, because if they had paths to earlier nodes then v would also have paths to earlier nodes which is false). The connected component rooted at v is then popped from the stack and returned, again preserving the invariant.

`Bookkeeping`

Each node v is assigned a unique integer v.index, which numbers the nodes consecutively in the order in which they are discovered. It also maintains a value v.lowlink that represents the smallest index of any node known to be reachable from v through v's DFS subtree, including v itself. Therefore v must be left on the stack if v.lowlink < v.index, whereas v must be removed as the root of a strongly connected component if v.lowlink == v.index. The value v.lowlink is computed during the depth-first search from v, as this finds the nodes that are reachable from v.

###### Source of Trajan Algorithm
```python
#coding:utf-8
def tarjan_scc(graph):
    """
    Tarjan's Algorithm (named for its discoverer, Robert Tarjan) is a graph theory algorithm
    for finding the strongly connected components of a graph.
    
    Based on: http://en.wikipedia.org/wiki/Tarjan%27s_strongly_connected_components_algorithm
    """

    count = [0]        #PS: why not use number type? Because the scope of array is larger than number, it can act on the whole recursive function. Of course, we can also upload an initial value of int type in the function parameter
    stack = []         #Used to store the strong connected nodes that have been traversed 
    low = {}           #Record the timestamp of the node when it visited and update it every time it traverses the child node to look back its parent node, so as to find the root node of the entire search subtree
    dfn = {}           #depth-first-number:Add the time stamp of the first access to the node. Once the node is time stamped, the timestamp will not be modified
    result = []        #include all strongly connected components
    
    def strongconnect(node):

        dfn[node] = count[0]# Give each node a depth first search label index
        low[node] = count[0]
        count[0] += 1
        stack.append(node)
    
        # If the node is a single node and no other nodes are connected, it is empty
        try:
            E = graph[node]
        except:
            E = []
        #Depth traverse the child nodes E of a node (which can be called the root of a strongly connected component because it is the first node to be visited)
        for v in E:
            if v not in low:
                # If the subsequent node v is not traversed, recursively call the strongconnect function to add v to low and number it into the stack
                strongconnect(v)
                low[node] = min(low[node],low[v])

            #if node has been traversed, find its father node
            elif v in stack:
                # return the mini number 
                low[node] = min(low[node],dfn[v])
        
        # If the node is the root,  the node is at the bottom of the stack, pop the strong connected component from stack.
        if low[node] == dfn[node]:
            connected_component = []
            #Add all the child nodes of root(node) in the stack to result
            while True:
                v = stack.pop()
                connected_component.append(v)
                if v == node:
                    break
            component = tuple(connected_component)
            result.append(component)
    #Push into the stack in turn and traverse all nodes of the graph to prevent the existence of nodes not traversed due to run tarjan one times
    for node in graph:
        if node not in low:
            strongconnect(node)
    return result
if __name__ == '__main__':
    
    grid = {
    "A": {"B":5,"C":1},
    "B": {"A":5,"C":2,"D":1},
    "C": {"A":1,"B":2,"D":4,"E":8},
    "D": {"B":1,"C":4,"E":3,"F":6},
    "E": {"C":8,"D":3},
    "F": {"D":6},
    "G": {"F":3, "H":5,"S":19},
    "H": {"G":5,"I":8,"J":4},
    "I": {"H":8,"K":3}
    }
    ret = tarjan_scc(grid)
    print("The strongly connected componets is \n{}\n".format(ret))

```

In this problem, we only need to use tarjan to make the current player's coordinates form strong connection with the next position next to the box, then we can reach and push the box without calculating the steps, and the box is OK as long as we get the shortest path according to the astar algorithm

```python
class Solution():
    def minPushBox_tarjan(self, graph):
        n, m = len(graph), len(graph[0])  #graph's size
        p = collections.defaultdict(list) #default dict, the item type is list
        sp = "S"                          #player
        bp = "B"                          #box
        tp = "T"                          #target
        path = "."                        #path
        uid = [0]                         #add uid to avoid collision
        step = 1                          #the steps of box
        directions = (1, -1, 1j, -1j)     #directions of box and player
        visited = set()                   #judge the box and player have stayed, avoid loop
        for i in range(n):                #find all important positions
            for j in range(m):
                p[graph[i][j]] += [complex(i,j)] 
        Sp, Bp, Tp, Path = p[sp][0], p[bp][0], p[tp][0], p[path]
        Path_set = [Sp] + [Bp] + [Tp] + Path
        def F(bp, step):
            '''Valuation function return Manhattan distance and European distance'''
            #The local variable like nonlocal keyword that acts on the nested function. The scope is the entire minPushBox_tarjan() after using the function f (BP, step)
            uid[0] += 1
            manhattan_dist = abs(Bp - Tp).real + abs(Bp - Tp).imag + step
            euclidean_dist = abs(Bp - Tp)
            return (manhattan_dist, euclidean_dist, uid[0])
        node = (F(Bp, 1), step, Sp, Bp)
        heapbox = [node]                  #the heap of box's move path
        count = [0]                       #index the node with number when depth traversed
        low = dict.fromkeys(Path_set, 0)  #The parameter to help the tarjan algorithm find the root node in depth traversal. The default value is 0
        dfn = low.copy()                  #depth first number
        index = {}                        #The mapping between coordinates and timestamps is established, which is convenient to calculate the strongly connected components of each coordinate
        def tarjan(curr_node, parent_node):
            '''Tarjan algorithm based on depth first search for standard undirected graph with curr_node is the current expansion point, recording the parent_node prevent repeated expansion'''
            dfn[curr_node] = count[0]
            low[curr_node] = count[0]
            index[count[0]] = curr_node
            count[0] += 1
            #Traverse four directions
            for direction in directions:
                neighbor_node = curr_node + direction
                #If the neighbor node is on the path collection and is not the parent node
                if neighbor_node in Path_set:
                    #if not visited
                    if not low[neighbor_node]:
                        tarjan(neighbor_node, curr_node)
                    #If the timestamp of the visited node is smaller than that of the current node, it means that it belongs to the same strong connected component, and the smaller one is taken
                    low[curr_node] = min(low[neighbor_node], low[curr_node])
        #Traverse from the current box position, parent node is - 1
        tarjan(Bp, -1)
        #Traverse all path points to find all strongly connected components
        for curr_node in Path_set:
            connect_node = [curr_node]
            #Find all nodes belonging to the same strongly connected component
            while (low[connect_node[-1]] != dfn[connect_node[-1]]):
                connect_node.append(index[low[connect_node[-1]]])
            #Make all timestamps belonging to a strongly connected component equal to the root‘s
            for v in connect_node[:-2]:
                    low[v] = low[connect_node[-1]]
        #Traverse heapbox
        while heapbox:
            f, step, Sp, Bp = heapq.heappop(heapbox)
            for direction in directions:
                #The box moves one step in a certain direction
                nextbox_position = Bp + direction
                # the player reaches a coordinate next to the current box position 
                next_sp = Bp - direction
                #If the next position of the box is on the path, it means that the player can push it
                if nextbox_position in Path_set:
                    #If the player can reach beside the box
                    if next_sp in Path_set:
                        #If neither the box nor the person's location has been traversed (to prevent circular traversal)
                        if (next_sp, Bp) not in visited:
                            #If timestamps are equal, they are interconnected
                            if low[next_sp] == low[Sp]:
                                #If arrive at the destination
                                if nextbox_position == Tp:
                                    return step
                                #The player's next position is BP, which means that the player is always next to the box and pushes the box along the direction of the box by one step
                                heapq.heappush(heapbox, (F(nextbox_position, step+1), step+1, Bp, nextbox_position))
                                #Record visited nodes
                                visited.add((next_sp, Bp))
        return -1
 ```

## Reference

1. [leetcode typingMonkey](https://leetcode-cn.com/u/tuotuoli/)
2. wikipedia
3. http://www.logarithmic.net/pfh-files/blog/01208083168/tarjan.py
4. [1972 Depth-First Search and Linear Graph Algorithms
](https://epubs.siam.org/doi/abs/10.1137/0201010)
5. [Algorithm 447 Efficient Algorithms for Graph Manipulation [H]](https://dl.acm.org/doi/pdf/10.1145/362248.362272)


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

#### BFS
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

#### [Astar](https://baike.baidu.com/item/A%2A%E6%90%9C%E7%B4%A2%E7%AE%97%E6%B3%95) + [Best-first](https://en.wikipedia.org/wiki/Best-first_search)

1. 先用Best-first算法使人找到箱子并到达箱子旁然后推动箱子到目标使用Astar算法
2. 使用负数来表示坐标,方便运算
3. 最好优先算法使用优先队列(小根堆)(估值函数为欧式距离)来遍历所有路径, 遍历过的点都从队列中去除,直到到达终点(箱子旁边)
4. 沿箱子方向推动箱子, 如果箱子不在路集合上表示过界
5. Astar算法中估值函数为曼哈顿距离, 人和箱子的位置不能重复 

##### Astar算法步骤

```
创建两个列表,openlist和closedlist,
openlist用来储存需要遍历的节点,并把openlist堆化
closedlist用来储存已经遍历过的节点
估值函数F来引导遍历的方向
结束遍历条件为:openlist为空,或者终点在openlist中被遍历到

	1.         把open list堆化, 起点加入 open list 。

	2.         重复如下过程：

	a.         遍历 open list ，查找 F 值最小的节点，把它作为当前要处理的节点。

	b.         把这个节点移到 close list 。

	c.         对当前方格的 8 个相邻方格的每一个方格

	◆     如果它是不可抵达的或者它在 close list 中，忽略它。否则，做如下操作。

	◆     如果它不在 open list 中，把它加入 open list ，并且把当前方格设置为它的父亲，计算该方格的 F ， G 和 H 值。

	◆     如果它已经在 open list 中，检查这条路径 ( 即经由当前方格到达它那里 ) 是否更好，用 G 值作参考。更小的 G 值表示这是更好的路径。如果是这样，把它的父亲设置为当前方格，并重新计算它的 G 和 F 值, 然后入堆。如果你的 open list 是按 F 值排序的话，入堆后会重新排序。

	d.         停止条件，

	◆     把终点加入到了 open list 中，此时路径已经找到

	◆     或者查找终点失败，并且 open list 是空的，此时没有路径。

	3.         保存路径。从终点开始，每个方格沿着父节点移动直至起点，这就是你的路径。
```

```python
import heapq
import copy
import collections
class Solution():
    def minPushBox(self, grid):
        n, m = len(grid), len(grid[0])     #迷宫长,宽
        g = collections.defaultdict(list)  #存储每个重要物品的坐标
        for i in range(n):
            for j in range(m):
                g[grid[i][j]] += [complex(i,j)]
        sp = g["S"][0]  #玩家位置
        tp = g["T"][0]  #目标位置
        bp = g["B"][0]  #箱子位置
        path = g["."]   #路位置list
        wall = g["#"]   #墙位置list
        directions = (1, -1, 1j, -1j)#上下左右
        visited = set() #存储已访问过的节点
        step = 1        #箱子的移动步数
        Path_set = path + [sp] + [tp] + [bp]   #所有路坐标的list


        #A*估值函数
        def F(a, s):
            '''
            a: (箱子)当前位置坐标
            s: (箱子)已走步数
            返回一个元组,其中一个是曼哈顿距离, 一个是欧式距离
            '''
            euclidean_dist = abs(a - tp)
            manhattan_dist = abs((a - tp).real) + abs((a - tp).imag) + s 
            return (manhattan_dist, euclidean_dist)
        #对人的移动使用
        def bestfirst(from_position, to_position, path_set):
            '''
            最好优先算法也叫做A算法,和A*相似
            from_position: 当前位置
            to_position:   最后位置
            path_set:      判断所在位置是否在路位置集合上
            返回值rtype:    bool
            '''
            p = 0                                #防止重复计算小根堆里的节点,类似于唯一键值prime_key
            f = abs(from_position - to_position) #估价函数为当前点和目标点的距离
            heapplayer = [(f, p, from_position)]
            #遍历人的优先队列
            while heapplayer:
                #取出堆中估值函数最小, 即路径绝对值最短节点, 其中, f和p参数没有用处
                f, _, curr_position = heapq.heappop(heapplayer)
                #如果到达最后位置,返回True
                if curr_position == to_position:
                    return True
                #遍历现在位置的上下左右四个方位
                for direction in directions:
                    next_position = curr_position + direction
                    #如果新的位置没有越界和碰墙,则添加进入人的优先队列(小根堆)
                    if next_position in path_set:
                        #print(abs(next_position - to_position), p, next_position)
                        heapq.heappush(heapplayer, (abs(next_position - to_position), p, next_position))
                        p += 1
                        path_set.remove(next_position) #把进入堆的坐标都去点
            return False #如果不能到达指定位置,则返回false
        time = 1        #作用于防止节点进堆后存在节点前几个参数相同,发生比较复数情况的发生        
        node = (F(bp, step), step, time, sp, bp)
        heapbox = [node]                #箱子移动小根堆
        #遍历箱子移动堆
        while heapbox:
            #取出节点, 其中, 估值函数, step在程序中没有使用到
            f, step, _, sp, bp = heapq.heappop(heapbox)
            #向四个方向开始遍历
            for direction in directions:
                #玩家下一个位置需要处于箱子旁边
                next_position = bp - direction
                #箱子下一个位置是沿着箱子方向移动一步之后的位置         
                nextbox_position = bp + direction
                #箱子必须在路上
                if nextbox_position in Path_set:
                	#人和箱子的位置没有重复访问
                    if (next_position, bp) not in visited:
                    	#必须去除箱子的位置,因为人不能触碰或跨过箱子
                        if bp in Path_set:
                            copypathset = copy.deepcopy(Path_set)
                            copypathset.remove(bp)
                        #人去找箱子,并到达箱子旁
                        if bestfirst(sp, next_position, copypathset):
                            #箱子到达终点,返回步数
                            if nextbox_position == tp:
                                return step
                            heapq.heappush(heapbox, (F(nextbox_position, step + 1), step + 1, time, bp, nextbox_position))
                            time += 1
                            #添加遍历过的节点
                            visited.add((next_position, bp))
        return -1

```

#### [tarjan](https://baike.baidu.com/item/tarjan%E7%AE%97%E6%B3%95) + Astar算法

[tarjan](https://en.wikipedia.org/wiki/Tarjan%27s_strongly_connected_components_algorithm)

Tarjan算法 （以发现者Robert Tarjan[1]命名）是一个基于深度优先搜索的,在图中查找[强连通分量](https://baike.baidu.com/item/%E5%BC%BA%E8%BF%9E%E9%80%9A%E5%88%86%E9%87%8F/7448759)的算法。虽然发表时间更早，它仍可以被视为Kosaraju算法的一个改进。它的效率跟Gabow算法差不多。

算法的基本思想如下：
任选一节点开始进行深度优先搜索（若深度优先搜索结束后仍有未访问的节点，则再从中任选一点再次进行）。
搜索过程中已访问的节点不再访问。
搜索树的若干子树构成了图的强连通分量。
节点按照被访问的顺序存入堆栈中。
从搜索树的子树返回至一个节点时，检查该节点是否是某一强连通分量的根节点并将其从堆栈中删除。
	如果某节点是强连通分量的根，则在它之前出堆栈且还不属于其他强连通分量的节点构成了该节点所在的强连通分量。

[源码]
```python
#coding:utf-8
def strongly_connected_components(graph):
    """
    Tarjan's Algorithm (named for its discoverer, Robert Tarjan) is a graph theory algorithm
    for finding the strongly connected components of a graph.
    
    Based on: http://en.wikipedia.org/wiki/Tarjan%27s_strongly_connected_components_algorithm
    """

    count = [0]        #编号器(ps: 为什么不用int?因为数组的作用域大于int,可以作用于整个递归函数.当然我们也可以在函数参数上传入一个int类型的初始值)
    stack = []         #用来存储被访问过的强连通节点
    low = {}           #记录节点访问时的编号并在每次遍历子节点时更新它来找到它的父节点,以此来找到整个搜索子树的根节点
    dfn = {}           #depth-first-number:为节点添加首次访问的时间戳,节点一旦被访问打上时间戳,就不在被修改
    result = []        #最终需要返回的结果(所有强连通分量,和单个节点)
    
    def strongconnect(node):
        # 给每个节点node一个深度优先搜索标号index

        dfn[node] = count[0]
        low[node] = count[0]
        count[0] += 1
        stack.append(node)
    
        # 如果该节点为单节点, 没有其它节点相连,则为空
        try:
            E = graph[node]
        except:
            E = []
        #深度遍历node节点(可以称其为强连通分量的根,因为它是第一个被访问的节点)的子节点
        for v in E:
            if v not in low:
                # 后继节点v未访问，递归调用strongconnect函数把v加入low并编号入栈
                strongconnect(v)
                low[node] = min(low[node],low[v])

            #如果节点是访问过的
            elif v in stack:
                # 返回编号是最小的节点
                low[node] = min(low[node],dfn[v])
        
        # 若node是根则出栈，并得到一个强连通分量,此时的node在栈底
        if low[node] == dfn[node]:
            connected_component = []
            #把栈内的子节点全部加入result
            while True:
                v = stack.pop()
                connected_component.append(v)
                if v == node:
                    break
            component = tuple(connected_component)
            result.append(component)
    #依次push入栈,遍历图所有节点,防止存在因一次tarjan而没有遍历到的节点
    for node in graph:
        if node not in low:
            strongconnect(node)
    return result
if __name__ == '__main__':
    
    grid = {
    "A": {"B":5,"C":1},
    "B": {"A":5,"C":2,"D":1},
    "C": {"A":1,"B":2,"D":4,"E":8},
    "D": {"B":1,"C":4,"E":3,"F":6},
    "E": {"C":8,"D":3},
    "F": {"D":6},
    "G": {"F":3, "H":5,"S":19},
    "H": {"G":5,"I":8,"J":4},
    "I": {"H":8,"K":3}
    }
    ret = strongly_connected_components(grid)
    print("The strongly connected componets is \n{}\n".format(ret))

```

在此题,我们只需要通过tarjan使得现在玩家的坐标能和箱子下一个位置的旁边位置形成强连通, 则无论如何我们都可以在不计算步数的情况下到达并推动箱子, 箱子只要按照Astar算法得出最短路径就OK了

```python
class Solution():
    def minPushBox_tarjan(self, graph):
        n, m = len(graph), len(graph[0])  #图的尺寸
        p = collections.defaultdict(list) #默认字典
        sp = "S"                          #玩家
        bp = "B"                          #箱子
        tp = "T"                          #目标位置
        path = "."                        #路
        uid = [0]                         #防止重复节点冲突,增加uid
        step = 1                          #箱子移动步数
        directions = (1, -1, 1j, -1j)     #方向
        visited = set()                   #是否被访问过节点集合
        for i in range(n):                #遍历查找相关节点位置
            for j in range(m):
                p[graph[i][j]] += [complex(i,j)] 
        Sp, Bp, Tp, Path = p[sp][0], p[bp][0], p[tp][0], p[path]
        Path_set = [Sp] + [Bp] + [Tp] + Path
        def F(bp, step):
            '''估值函数曼哈顿距离和欧式距离'''
            #作用于嵌套函数的局部变量.作用域为使用本函数F(bp, step)之后的整个minPushBox_tarjan函数
            uid[0] += 1
            manhattan_dist = abs(Bp - Tp).real + abs(Bp - Tp).imag + step
            euclidean_dist = abs(Bp - Tp)
            return (manhattan_dist, euclidean_dist, uid[0])
        node = (F(Bp, 1), step, Sp, Bp)
        heapbox = [node]                  #箱子路径堆
        count = [0]                       #tarjan算法中的时间戳
        low = dict.fromkeys(Path_set, 0)  #帮助算法深度遍历时回溯找到根节点的参数,默认为0
        dfn = low.copy()                  #首次添加的时间戳, 一经添加无法修改
        index = {}                        #建立坐标与时间戳的映射，方便计算各个坐标所归属的强连通分量
        def tarjan(curr_node, parent_node):
            '''标准无向图基于深度优先搜索的tarjan算法，参数curr_node为当前拓展点，记录拓展的父节点parent_node防止重复拓展'''
            dfn[curr_node] = count[0]
            low[curr_node] = count[0]
            index[count[0]] = curr_node
            count[0] += 1
            #遍历四个方向
            for direction in directions:
                neighbor_node = curr_node + direction
                #如果邻居节点在路径集合上并且不是父节点
                if neighbor_node in Path_set:
                    #如果没有被访问过
                    if not low[neighbor_node]:
                        tarjan(neighbor_node, curr_node)
                    #如果访问过的节点时间戳比当前节点的时间戳要小，则表示属于同一个强连通分量，取较小那个
                    low[curr_node] = min(low[neighbor_node], low[curr_node])
        #从现在箱子位置出发遍历,父节点为-1
        tarjan(Bp, -1)
        #遍历所有路径点,查找所有强连通分量
        for curr_node in Path_set:
            connect_node = [curr_node]
            #查找所有属于同一个强连通分量的点
            while (low[connect_node[-1]] != dfn[connect_node[-1]]):
                connect_node.append(index[low[connect_node[-1]]])
            #时所有归属于一个强连通分量的时间戳相等于根
            for v in connect_node[:-2]:
                    low[v] = low[connect_node[-1]]
        #遍历整个堆
        while heapbox:
            f, step, Sp, Bp = heapq.heappop(heapbox)
            for direction in directions:
                #箱子沿着某个方向移动一步
                nextbox_position = Bp + direction
                #表示玩家到达现在箱子位置坐标的旁边一个坐标
                next_sp = Bp - direction
                #如果箱子下个位置在路径上,表示玩家能够推动它
                if nextbox_position in Path_set:
                    #如果玩家能够到达箱子推动方向的反方向一步距离
                    if next_sp in Path_set:
                        #如果箱子和人的位置都没有访问过(防止循环遍历)
                        if (next_sp, Bp) not in visited:
                            #如果时间戳相等,表示它们是相互连通的
                            if low[next_sp] == low[Sp]:
                                #如果到达目的地
                                if nextbox_position == Tp:
                                    return step
                                #玩家位置为下一个位置为Bp表示玩家始终处于箱子的旁边一个位置并沿箱子方向推动箱子移动了一个步数
                                heapq.heappush(heapbox, (F(nextbox_position, step+1), step+1, Bp, nextbox_position))
                                #记录访问过的节点
                                visited.add((next_sp, Bp))
        return -1
 ```

## 参考

1. [leetcode typingMonkey](https://leetcode-cn.com/u/tuotuoli/)
2. wikipedia
3. http://www.logarithmic.net/pfh-files/blog/01208083168/tarjan.py
4. [1972 Depth-First Search and Linear Graph Algorithms
](https://epubs.siam.org/doi/abs/10.1137/0201010)
5. [Algorithm 447 Efficient Algorithms for Graph Manipulation [H]](https://dl.acm.org/doi/pdf/10.1145/362248.362272)
