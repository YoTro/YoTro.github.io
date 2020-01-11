---
title: The Shortest Path Problem in Grapgh
layout: post
date: 2019-11-11 16:48:00
tags: [Graph,Dijkstra,Floyd]
music-id: 1401924960

---

[EN](#EN)|[CN](#CN)
<span id="EN"></span>
# Preface
Why I want to study and solve this problem?

# definition
## [Classification of graphs](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics))
Undirected/directed graph, Weighted graph

## [The shortest path problem](https://en.wikipedia.org/wiki/Shortest_path_problem)
### Algorithms
The most important algorithms for solving this problem are:

- [Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) solves the single-source shortest path problem with non-negative edge weight.  
- [Bellman–Ford algorithm](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm) solves the single-source problem if edge weights may be negative.
- [SPFA algorithm](https://en.wikipedia.org/wiki/Shortest_Path_Faster_Algorithm) Improved version of the Bellman-Ford algorithm
- [A* search algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm) solves for single pair shortest path using heuristics to try to speed up the search.
- [Floyd–Warshall algorithm](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm) solves all pairs shortest paths.  
- [Johnson's algorithm](https://en.wikipedia.org/wiki/Johnson%27s_algorithm) solves all pairs shortest paths, and may be faster than Floyd–Warshall on sparse graphs.  
- [Viterbi ](https://en.wikipedia.org/wiki/Viterbi_algorithm) solves the shortest stochastic path problem with an additional probabilistic weight on each node.
---

### Algorithm Thought
For the undirected graphs with nonnegative weights G, two sets Q and S, Q stores all nodes, S stores the shortest path node, and initially there is only one start node, and push the node to S after calculates the shortest path every time , updating the shortest path until all nodes are placed in the set S

## The code

{% highlight ruby linenos %}  
# dists: the distance of shortest path 
# parents: You can use the parent node to make the shortest path tree
def dij(start, graph):
    n = len(graph)
    if n == 0:
	print “graph is none”
	return 0
    # Initialize individual data，dists[start] = 0, other dists are infinity
    # Set the parent node of each vertex to -1
    inf = float(“inf”)
    dists = [inf for _ in range(n)]
    print dists
    dists[start] = 0
    parents = [-1 for _ in range(n)]
    visited = [False for _ in range(n)] # mark up the node which has been caculated temporary shortest path
    t = []  # A list of nodes that have been determined for the shortest path
    while len(t) < n:
        minCost = float(“inf”)
        minNode = None
        for i in range(n):
            if not visited[i] and dists[i] < minCost:
                minCost = dists[i]
                minNode = i
        t.append(minNode)
        visited[minNode] = True
        # From this vertex traverse the edges of the neighboring vertices, calculate the shortest path, update dists and parents
        for edge in graph[minNode]:
            if not visited[edge[0]] and minCost + edge[1] < dists[edge[0]]:
                dists[edge[0]] = minCost + edge[1]
                parents[edge[0]] = minNode
    return dists, parents
#Data
data = [
    [1, 0, 8],
    [1, 2, 5],
    [1, 3, 10],
    [1, 6, 9],
    [2, 0, 1],
    [0, 6, 2],
    [3, 6, 5],
    [3, 4, 8],
    [0, 5, 4],
    [5, 6, 7],
    [5, 3, 8],
    [5, 4, 5]
]
n = 7  # the number of nodes

#Translate the list date to graph
graph = [[] for _ in range(n)]
for edge in data:
    graph[edge[0]].append([edge[1], edge[2]])
    graph[edge[1]].append([edge[0], edge[2]])

#Finding the shortest path from the one node

dists, parents = dij(2, graph)
print('dists')
print(dists)
print('parents')
print(parents)

{% endhighlight %}

<span id="CN"></span>
# 图论之最短路径问题
# 前言
为什么我要研究最短路径问题？
图论是神经网络学习中必修科目，我的项目机器人自动回复涉及到深度学习，所以必不可少的需要学习这方面的知识
## [图的分类](https://zh.wikipedia.org/wiki/%E5%9B%BE_(%E6%95%B0%E5%AD%A6))
有向图，无向图
## [最短路径算法](https://zh.wikipedia.org/wiki/%E6%9C%80%E7%9F%AD%E8%B7%AF%E9%97%AE%E9%A2%98)
- Dijkstra算法
- [A\*搜索算法](https://zh.wikipedia.org/wiki/A*%E6%90%9C%E5%B0%8B%E6%BC%94%E7%AE%97%E6%B3%95)
- [Bellman-Ford算法](https://zh.wikipedia.org/wiki/%E8%B4%9D%E5%B0%94%E6%9B%BC-%E7%A6%8F%E7%89%B9%E7%AE%97%E6%B3%95) 
- SPFA算法（Bellman-Ford算法的改进版本）
- [Floyd-Warshall算法](https://zh.wikipedia.org/wiki/Floyd-Warshall%E7%AE%97%E6%B3%95) 弗洛伊德算法 原理是动态规划。
- Johnson算法
- Bi-Direction 双向BFS算法（核心是广度优先搜索算法，从起点和终点开始搜索效率更高） 

## Dijkstra算法
用于计算起始点到图中所有节点的最短路径，限制条件：不能有负权边
### 算法思想
对于带权无向图G，两个集合Q和S,Q存放所有节点，S存放最短路径节点，一开始S只有一个源点，以后每求出一条最短路径，就将它放入集合S中，每次更新集合S中的到每个节点的最短路径直到全部节点放入集合S中，
### 算法步骤
如图![directed graphs with unbounded non-negative weights](\images\python_struction\Dijkstra.jpg)
![步骤图](\images\python_struction\20191006112455385.jpg)

### 代码实现

{% highlight ruby linenos %}   
def dij(start, graph):
    n = len(graph)
    if n == 0:
	print “graph is none”
	return 0
    # 初始化各項資料，把dists[start]初始化為0，其他為無窮大
    # 把各個頂點的父結點設定成-1
    inf = float(“inf”)
    dists = [inf for _ in range(n)]
    print dists
    dists[start] = 0
    parents = [-1 for _ in range(n)]
    visited = [False for _ in range(n)] # 標記已確定好最短花銷的點
    t = []  # 已經確定好最短花銷的點列表
    while len(t) < n:
        # 從dists裡面找最短路径(找還沒確定的點的路徑)，標記這個最短邊的頂點，把頂點加入t中
        minCost = float(“inf”)
        minNode = None
        for i in range(n):
            if not visited[i] and dists[i] < minCost:
                minCost = dists[i]
                minNode = i
        t.append(minNode)
        visited[minNode] = True

        # 從這個頂點出發，遍歷與它相鄰的頂點的邊，計算最短路徑，更新dists和parents
        for edge in graph[minNode]:
            if not visited[edge[0]] and minCost + edge[1] < dists[edge[0]]:
                dists[edge[0]] = minCost + edge[1]
                parents[edge[0]] = minNode
    return dists, parents
data = [
    [1, 0, 8],
    [1, 2, 5],
    [1, 3, 10],
    [1, 6, 9],
    [2, 0, 1],
    [0, 6, 2],
    [3, 6, 5],
    [3, 4, 8],
    [0, 5, 4],
    [5, 6, 7],
    [5, 3, 8],
    [5, 4, 5]
]
n = 7  # 結點數  
# 用data資料構建鄰接表
graph = [[] for _ in range(n)]
for edge in data:
    graph[edge[0]].append([edge[1], edge[2]])
    graph[edge[1]].append([edge[0], edge[2]])
# for edges in graph:
#     print(edges)

# 從1開始找各點到1的最短路徑（單源最短路徑）
# dists: 各點到店1的最短路徑
# parents: 各點連結的父結點，可以用parents建立最短路徑生成樹
dists, parents = dij(2, graph)
print('dists')
print(dists)
print('parents')
print(parents)

{% endhighlight %}

其中graph=
[  
	[  
		[1, 8], [2, 1], [6, 2], [5, 4]  
	],  
 	[	
		[0, 8], [2, 5], [3, 10], [6, 9]  
	],  
	[  
		[1, 5], [0, 1]  
	],  
	[  
		[1, 10], [6, 5], [4, 8], [5, 8]  
	],  
	[  
		[3, 8], [5, 5]  
	],  
	[  
		[0, 4], [6, 7], [3, 8], [4, 5]  
	],  
 	[	  
		[1, 9], [0, 2], [3, 5], [5, 7]  
	]  
]  
graph[0] 代表节点1，graph[1]代表结点2，以此类推
其中[1,8]代表结点1可以到达结点2，距离值为8，不得不说这种表达方式非常棒

