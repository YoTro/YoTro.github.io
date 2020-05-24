---
title: Dynamic Programming 1.3
date: 2020-05-22 22:24:00
tags: [DP,python]
music-id: 
youtube-video-ID: 
bilibili-video-ID: BV1tC4y1H7yz
---

# Knapsack problem

> 鱼跃此时海,花开彼岸天

The knapsack problem is a problem in combinatorial optimization: Given a set of items, each with a weight and a value, determine the number of each item to include in a collection so that the total weight is less than or equal to a given limit and the total value is as large as possible. It derives its name from the problem faced by someone who is constrained by a fixed-size knapsack and must fill it with the most valuable items. The problem often arises in resource allocation where the decision makers have to choose from a set of non-divisible projects or tasks under a fixed budget or time constraint, respectively.

The knapsack problem has been studied for more than a century, with early works dating as far back as 1897. The name "knapsack problem" dates back to the early works of mathematician Tobias Dantzig (1884–1956), and refers to the commonplace problem of packing the most valuable or useful items without overloading the luggage.


### [Leetcode 322th coin Change](https://leetcode-cn.com/problems/coin-change/)

```
You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

Example 1:

Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1
Example 2:

Input: coins = [2], amount = 3
Output: -1
Note:
You may assume that you have an infinite number of each kind of coin.

```

### Solution

#### DP

Input: Coins = [1, 2, 5], amount = 11

The minimum number of coins with a par value of 11 can be obtained from the minimum value of 33:



1. The minimum number of coins with a coin value of 10 + the coin with a coin value of 1;



2. The minimum number of coins with a coin value of 9 + the coin with a coin value of 2;



3. The minimum number of coins with a coin value of 6 + the coin with a coin value of 5;


That is DP [11] = min (DP [10] + 1, DP [9] + 1, DP [6] + 1).


You can directly design the questions into a state.

1. You need to know what is state in this problem, the subproblem is if now I have amount - coin, which coin can help me to get mininumber

2. definite the function and table：you only need to consider how to get miniNumber with coins, 
```python
 dp[amount] = min(1 + dp[amount - coins[i]]) for i in range(len(coins)) if coins[i] <= amount
```
3. dp base:

if n == 0, `dp[0] = 0` is mean I don't need any coins to change

[Dynamic_python](http://pythontutor.com/visualize.html#code=%0Aclass%20Solution%28%29%3A%0A%20%20%20%20def%20coinChange_dp%28self,%20coins,%20amount%29%3A%0A%20%20%20%20%20%20%20%20'''%0A%20%20%20%20%20%20%20%20coins%3A%20list%5Bint%5D%0A%20%20%20%20%20%20%20%20amount%3A%20int%0A%20%20%20%20%20%20%20%20rtype%3A%20int%0A%20%20%20%20%20%20%20%20'''%0A%20%20%20%20%20%20%20%20%23special%20case%0A%20%20%20%20%20%20%20%20if%20not%20coins%20or%20not%20amount%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20return%200%0A%20%20%20%20%20%20%20%20%23init%20dp%0A%20%20%20%20%20%20%20%20dp%20%3D%20%5Bfloat%28%22inf%22%29%5D*%28amount%20%2B%201%29%0A%20%20%20%20%20%20%20%20%23dp%20base%0A%20%20%20%20%20%20%20%20dp%5B0%5D%20%3D%200%0A%20%20%20%20%20%20%20%20for%20i%20in%20range%28len%28coins%29%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20for%20j%20in%20range%28coins%5Bi%5D,%20amount%2B1%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%23compare%20to%20every%20coins,%20if%20have%20one%20coin%20could%20make%20the%20mininumber%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20dp%5Bj%5D%20%3D%20min%281%20%2B%20dp%5Bj-coins%5Bi%5D%5D,%20dp%5Bj%5D%29%0A%20%20%20%20%20%20%20%20return%20dp%5Bamount%5D%20if%20dp%5Bamount%5D%20!%3D%20float%28%22inf%22%29%20else%20-1%0A%0Aif%20__name__%20%3D%3D%20'__main__'%3A%0A%20%20%20%20solution%20%3D%20Solution%28%29%0A%20%20%20%20coins%20%3D%20%5B1,2,5%5D%0A%20%20%20%20amount%20%3D%2011%0A%20%20%20%20ret%20%3D%20solution.coinChange_dp%28coins,%20amount%29%0A%20%20%20%20print%28ret%29&cumulative=true&curInstr=76&heapPrimitives=true&mode=display&origin=opt-frontend.js&py=2&rawInputLstJSON=%5B%5D&textReferences=false)
```python
class Solution():
	def coinChange_dp(self, coins, amount):
		'''
		coins: list[int]
		amount: int
		rtype: int
		'''
		#special case
		if not coins or not amount:
			return 0
		#init dp
		dp = [float("inf")]*(amount + 1)
		#dp base
		dp[0] = 0
		for i in range(len(coins)):
			for j in range(coins[i], amount+1):
				#compare to every coins, if have one coin could make the mininumber
				dp[j] = min(1 + dp[j-coins[i]], dp[j])
		return dp[amount] if dp[amount] != float("inf") else -1

```

#### DFS+Alpha-beta

##### C program version

You could check here to see the recursive content.
[Dynamic_recursive_c](http://pythontutor.com/c.html#code=%23include%20%3Cstdio.h%3E%0A%23include%20%3Cstdlib.h%3E%0A%23define%20min%28a,b%29%20%28%20%28a%29%3E%28b%29%20%3F%20%28b%29%3A%28a%29%20%29/*%E8%BF%94%E5%9B%9E%E6%9C%80%E5%B0%8F%E5%80%BC*/%0A%23define%20random%28x%29%20%28rand%28%29%25x%29/*%E9%9A%8F%E6%9C%BA%E5%87%BD%E6%95%B0*/%0A%23define%20INT_MAX%202147483647%20/*%E6%9C%80%E5%A4%A7%E5%80%BC%28%E9%9A%8F%E4%BE%BF%E5%AE%9A%E4%B9%89%E7%9A%84%29*/%0Aint%20retMin%20%3D%20INT_MAX%3B/*%E5%88%9D%E5%A7%8B%E5%8C%96retMin%20%3A%20%E6%9C%80%E5%B0%91%E7%A1%AC%E5%B8%81%E6%95%B0*/%0A%0A/*%E9%80%86%E5%BA%8F%E6%95%B0%E7%BB%84%E5%B9%B6%E4%BB%8E%E5%A4%A7%E5%88%B0%E5%B0%8F%E6%8E%92%E5%88%97*/%0Aint%20cmpCoin%28const%20void%20*a,%20const%20void%20*b%29%20%7B%0A%20%20int%20*ap%20%3D%20%28int%20*%29a%3B%0A%20%20int%20*bp%20%3D%20%28int%20*%29b%3B%0A%0A%20%20return%20*bp%20-%20*ap%3B%0A%7D%0A/*DFS%E6%B7%B1%E5%BA%A6%E9%81%8D%E5%8E%86%2B%E5%89%AA%E6%9E%9D*/%0Avoid%20coinChange_dfs%28int*%20coins,%20int%20coinsSize,%20int%20amount,%20int%20index,%20int%20c%29%20%7B%0A%20%20%0A%20%20/*%E5%A6%82%E6%9E%9C%E5%88%9A%E5%A5%BD%E5%87%91%E5%A4%9Famount*/%0A%20%20if%20%28amount%20%3D%3D%200%29%20%7B%0A%20%20%20%20retMin%20%3D%20min%28c,%20retMin%29%3B%0A%20%20%20%20return%3B%0A%20%20%7D%0A%20%20/*%E5%A6%82%E6%9E%9C%E8%B6%8A%E7%95%8C%E5%88%99%E8%BF%94%E5%9B%9E*/%0A%20%20if%20%28index%20%3D%3D%20coinsSize%29%20return%3B%0A%20%20%0A%20%20for%20%28int%20i%20%3D%20amount%20/%20coins%5Bindex%5D%3B%20i%20%3E%3D%200%20%26%26%20i%2Bc%20%3C%20retMin%3B%20i%20--%29%20%7B%0A%20%20%20%20coinChange_dfs%28coins,%20coinsSize,%20amount%20-%20i%20*%20coins%5Bindex%5D,%20index%2B1,%20c%2Bi%29%3B%0A%20%20%7D%0A%0A%7D%0A/*%E4%B8%BB%E5%87%BD%E6%95%B0%0A%E6%8D%A2%E9%9B%B6%E9%92%B1%3A%E7%94%A8%E6%9C%80%E5%B0%91%E7%9A%84%E7%A1%AC%E5%B8%81%E6%95%B0%E6%8D%A2%E5%88%9A%E5%A5%BD%E7%9A%84%E9%9B%B6%E9%92%B1%0Acoins%3A%E9%9D%A2%E9%A2%9D%E4%B8%8D%E7%AD%89%E7%9A%84%E6%95%B0%E7%BB%84%0AcoinsSize%3A%20coins%E6%95%B0%E7%BB%84%E7%9A%84%E5%A4%A7%E5%B0%8F%0Aamount%3A%20%E6%89%80%E8%A6%81%E4%BA%A4%E6%8D%A2%E7%9A%84%E7%9B%AE%E6%A0%87%E5%80%BC%E6%80%BB%E9%87%91%E9%A2%9D%0A*/%0Aint%20coinChange%28int*%20coins,%20int%20coinsSize,%20int%20amount%29%7B%0A%20%20/*%0A%20%20retMin%20%3A%20%E6%9C%80%E5%B0%91%E7%A1%AC%E5%B8%81%E6%95%B0%0A%20%20*/%0A%20%20retMin%20%3D%20INT_MAX%3B%0A%20%20/*%E5%A6%82%E6%9E%9C%E6%95%B0%E7%BB%84%E4%B8%BA%E7%A9%BA,%E5%88%99%E7%AB%8B%E9%A9%AC%E8%BF%94%E5%9B%9E-1*/%0A%20%20if%20%28%28coins%20%3D%3D%20NULL%29%20%7C%7C%20%28coinsSize%20%3D%3D%200%29%29%20%7B%0A%20%20%20%20return%20-1%3B%0A%20%20%7D%0A%20%20/*%E5%A6%82%E6%9E%9Camount%E4%B8%BA%E7%A9%BA,%E5%88%99%E7%AB%8B%E9%A9%AC%E8%BF%94%E5%9B%9E0,%E8%A1%A8%E7%A4%BA%E4%B8%8D%E9%9C%80%E8%A6%81%E4%BA%A4%E6%8D%A2*/%0A%20%20if%20%28amount%20%3D%3D%200%29%20%7B%0A%20%20%20%20return%200%3B%0A%20%20%7D%0A%20%20/*sorts%20an%20array%E5%AF%B9%E6%95%B0%E7%BB%84%E6%8E%92%E5%BA%8F*/%0A%20%20qsort%28coins,%20coinsSize,%20sizeof%28int%29,%20cmpCoin%29%3B%0A%20%20/*%E6%89%93%E5%8D%B0%E6%95%B0%E7%BB%84*/%0A%20%20for%28int%20x%20%3D%200%3B%20x%20%3C%20coinsSize%3B%20x%2B%2B%29%7B%0A%20%20%20%20printf%28%22coins%20%3D%20%25d%5Cn%22,%20coins%5Bx%5D%29%3B%0A%20%20%7D%0A%20%20%0A%20%20coinChange_dfs%28coins,%20coinsSize,%20amount,%200,%200%29%3B%0A%20%20/*%E5%A6%82%E6%9E%9C%E6%9C%80%E5%B0%8F%E7%A1%AC%E5%B8%81%E6%95%B0%E4%BE%9D%E7%84%B6%E7%AD%89%E4%BA%8E%E6%9C%80%E5%A4%A7%E5%80%BC%E8%A1%A8%E7%A4%BA%E5%8C%B9%E9%85%8D%E4%B8%8D%E5%88%B0%E9%9B%B6%E9%92%B1,%E8%BF%94%E5%9B%9E-1,%E5%90%A6%E5%88%99%E8%BF%94%E5%9B%9E%E6%9C%80%E5%B0%8F%E5%80%BC*/%0A%20%20return%20%28retMin%20%3D%3D%20INT_MAX%29%20%3F%20-1%20%3A%20retMin%3B%0A%7D%0A%0Aint%20main%28%29%0A%7B%0A%20%20int%20coins%5B%5D%3D%7B1,2,5%7D%3B%0A%20%20int%20amount%20%3D%208%3B%0A%20%20printf%28%22amount%20%3D%20%25d%5Cn%22,%20amount%29%3B%0A%20%20int%20coinsSize%20%3D%20sizeof%28coins%29%20/%20sizeof%28coins%5B0%5D%29%3B%0A%20%20int%20res%3B%0A%20%20res%3DcoinChange%28coins,%20coinsSize,%20amount%29%3B%0A%20%20printf%28%22res%20%3D%20%25d%22,res%29%3B%0A%20%20return%200%3B%0A%7D&curInstr=0&mode=display&origin=opt-frontend.js&py=c&rawInputLstJSON=%5B%5D)

```c
#include <stdio.h>
#include <stdlib.h>
#define min(a,b) ( (a)>(b) ? (b):(a) )/*返回最小值*/
#define random(x) (rand()%x)/*随机函数*/
#define INT_MAX 2147483647 /*最大值(随便定义的)*/
int retMin = INT_MAX;/*初始化retMin : 最少硬币数*/

/*逆序数组并从大到小排列*/
int cmpCoin(const void *a, const void *b) {
  int *ap = (int *)a;
  int *bp = (int *)b;

  return *bp - *ap;
}
/*DFS深度遍历+剪枝*/
void coinChange_dfs(int* coins, int coinsSize, int amount, int index, int c) {
  
  /*如果刚好凑够amount*/
  if (amount == 0) {
    retMin = min(c, retMin);
    return;
  }
  /*如果越界则返回*/
  if (index == coinsSize) return;
  
  for (int i = amount / coins[index]; i >= 0 && i+c < retMin; i --) {
    coinChange_dfs(coins, coinsSize, amount - i * coins[index], index+1, c+i);
  }

}
/*主函数
换零钱:用最少的硬币数换刚好的零钱
coins:面额不等的数组
coinsSize: coins数组的大小
amount: 所要交换的目标值总金额
*/
int coinChange(int* coins, int coinsSize, int amount){
  /*
  retMin : 最少硬币数
  */
  retMin = INT_MAX;
  /*如果数组为空,则立马返回-1*/
  if ((coins == NULL) || (coinsSize == 0)) {
    return -1;
  }
  /*如果amount为空,则立马返回0,表示不需要交换*/
  if (amount == 0) {
    return 0;
  }
  /*sorts an array对数组排序*/
  qsort(coins, coinsSize, sizeof(int), cmpCoin);
  /*打印数组*/
  for(int x = 0; x < coinsSize; x++){
    printf("coins = %d\n", coins[x]);
  }
  
  coinChange_dfs(coins, coinsSize, amount, 0, 0);
  /*如果最小硬币数依然等于最大值表示匹配不到零钱,返回-1,否则返回最小值*/
  return (retMin == INT_MAX) ? -1 : retMin;
}

int main()
{
  int coins[]={1,2,5};
  int amount = 8;
  printf("amount = %d\n", amount);
  int coinsSize = sizeof(coins) / sizeof(coins[0]);
  int res;
  res=coinChange(coins, coinsSize, amount);
  printf("res = %d",res);
  return 0;
}

```
## Source code

[My Github Code](https://github.com/YoTro/Python_repository/blob/master/Leetcode/DP/leetcode-322-coin-change.py)

# 背包问题

给定一组多个物品比如多面额的硬币或者产物，每种物品都有自己的重量和价值，在限定的总重量/总容量内，选择其中若干个（也即每种物品可以选0个或1个），设计选择方案使得物品的总价值最高。

### [Leetcode 322 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

```
给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

示例 1:

输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
示例 2:

输入: coins = [2], amount = 3
输出: -1
 

说明:
你可以认为每种硬币的数量是无限的。

```

### 方法

#### 动态规划

输入: coins = [1, 2, 5], amount = 11
凑成面值为 11 的最小硬币数可以由以下 33 者的最小值得到：

1、凑成面值为 10 的最小硬币数 + 面值为 1 的这一枚硬币；

2、凑成面值为 9 的最小硬币数 + 面值为 2 的这一枚硬币；

3、凑成面值为 6 的最小硬币数 + 面值为 5 的这一枚硬币；

即 dp[11] = min (dp[10] + 1, dp[9] + 1, dp[6] + 1)。

可以直接把题目的问法设计成状态。

1. 明确状态：这里的状态是指原问题和子问题中变化的变量，从问题出发,我们发现要凑成刚好的硬币数, 只有amount是可变的, 它可以当作状态, 因为硬币的个数是无限的，所以唯一的状态就是目标的金额amount。

2. 定义dp数据/函数的含义：这里和定义递归函数是一样的，递归的一个非常重要的点就是：不去管函数的内部细节是如何处理的，我们只看其函数作用以及输入与输出。这里的含义就是指函数作用：我们定义一个dp数组，dp[n]表示当前目标金额是n，至少需要dp[n]个硬币凑出该金额。

3. 明确选择：通过我们当前的选择来改变我们的状态,也就是我们的填表方式

[Dynamic_python](http://pythontutor.com/visualize.html#code=%0Aclass%20Solution%28%29%3A%0A%20%20%20%20def%20coinChange_dp%28self,%20coins,%20amount%29%3A%0A%20%20%20%20%20%20%20%20'''%0A%20%20%20%20%20%20%20%20coins%3A%20list%5Bint%5D%0A%20%20%20%20%20%20%20%20amount%3A%20int%0A%20%20%20%20%20%20%20%20rtype%3A%20int%0A%20%20%20%20%20%20%20%20'''%0A%20%20%20%20%20%20%20%20%23special%20case%0A%20%20%20%20%20%20%20%20if%20not%20coins%20or%20not%20amount%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20return%200%0A%20%20%20%20%20%20%20%20%23init%20dp%0A%20%20%20%20%20%20%20%20dp%20%3D%20%5Bfloat%28%22inf%22%29%5D*%28amount%20%2B%201%29%0A%20%20%20%20%20%20%20%20%23dp%20base%0A%20%20%20%20%20%20%20%20dp%5B0%5D%20%3D%200%0A%20%20%20%20%20%20%20%20for%20i%20in%20range%28len%28coins%29%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20for%20j%20in%20range%28coins%5Bi%5D,%20amount%2B1%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%23compare%20to%20every%20coins,%20if%20have%20one%20coin%20could%20make%20the%20mininumber%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20dp%5Bj%5D%20%3D%20min%281%20%2B%20dp%5Bj-coins%5Bi%5D%5D,%20dp%5Bj%5D%29%0A%20%20%20%20%20%20%20%20return%20dp%5Bamount%5D%20if%20dp%5Bamount%5D%20!%3D%20float%28%22inf%22%29%20else%20-1%0A%0Aif%20__name__%20%3D%3D%20'__main__'%3A%0A%20%20%20%20solution%20%3D%20Solution%28%29%0A%20%20%20%20coins%20%3D%20%5B1,2,5%5D%0A%20%20%20%20amount%20%3D%2011%0A%20%20%20%20ret%20%3D%20solution.coinChange_dp%28coins,%20amount%29%0A%20%20%20%20print%28ret%29&cumulative=true&curInstr=76&heapPrimitives=true&mode=display&origin=opt-frontend.js&py=2&rawInputLstJSON=%5B%5D&textReferences=false)

```python
class Solution():
	def coinChange_dp(self, coins, amount):
		'''
		coins: list[int]
		amount: int
		rtype: int
		'''
		#special case
		if not coins or not amount:
			return 0
		#init dp
		dp = [float("inf")]*(amount + 1)
		#dp base
		dp[0] = 0
		for i in range(len(coins)):
			for j in range(coins[i], amount+1):
				#compare to every coins, if have one coin could make the mininumber
				dp[j] = min(1 + dp[j-coins[i]], dp[j])
		return dp[amount] if dp[amount] != float("inf") else -1

```

#### DFS+剪枝

##### Python版

[Dynamic_dfs_python](http://pythontutor.com/live.html#code=class%20Solution%28%29%3A%0A%20%20%20%20def%20coinChange_dfs%28self,%20coins,%20amount%29%3A%0A%20%20%20%20%20%20%20%20%22%22%22%0A%20%20%20%20%20%20%20%20dfs%2B%E5%89%AA%E6%9E%9D%0A%20%20%20%20%20%20%20%20%E5%8A%A8%E6%80%81%E6%98%BE%E7%A4%BA%3Ahttp%3A//pythontutor.com/live.html%23code%3Dclass%2520Solution%2528object%2529%253A%250A%2520%2520%2520%2520def%2520coinChange%2528self,%2520coins,%2520amount%2529%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520'''%250A%2520%2520%2520%2520%2520%2520%2520%2520dfs%252B%25E5%2589%25AA%25E6%259E%259D%250A%2520%2520%2520%2520%2520%2520%2520%2520'''%250A%2520%2520%2520%2520%2520%2520%2520%2520if%2520amount%2520%253D%253D%25200%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520return%25200%250A%2520%2520%2520%2520%2520%2520%2520%2520coins.sort%2528reverse%253DTrue%2529%250A%2520%2520%2520%2520%2520%2520%2520%2520self.res%2520%253D%2520amount%2520%252B%25201%250A%2520%2520%2520%2520%2520%2520%2520%2520def%2520helper%2528remiander,%2520index,%2520c%2529%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520'''dfs%25E9%2581%258D%25E5%258E%2586%25E6%2589%2580%25E6%259C%2589%25E5%25AD%2590%25E6%2595%25B0%25E7%25BB%2584'''%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520if%2520remiander%2520%253D%253D%25200%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520self.res%2520%253D%2520min%2528self.res,%2520c%2529%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520return%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520if%2520index%2520%253D%253D%2520len%2528coins%2529%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520return%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520for%2520x%2520in%2520range%2528remiander/coins%255Bindex%255D,%2520-1,%2520-1%2529%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520if%2520x%2520%252B%2520c%2520%253C%2520self.res%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520helper%2528remiander%2520-%2520x*coins%255Bindex%255D,%2520index%252B1,%2520x%252Bc%2529%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520else%253A%250A%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520break%250A%2520%2520%2520%2520%2520%2520%2520%2520helper%2528amount,%25200,%25200%2529%250A%2520%2520%2520%2520%2520%2520%2520%2520return%2520-1%2520if%2520self.res%2520%253D%253D%2520amount%2520%252B%25201%2520else%2520self.res%250A%250Aif%2520__name__%2520%253D%253D%2520'__main__'%253A%250A%2520%2520%2520%2520amount%2520%253D%25208%250A%2520%2520%2520%2520coins%2520%253D%2520%255B1,2,5%255D%250A%2520%2520%2520%2520solution%2520%253D%2520Solution%2528%2529%250A%2520%2520%2520%2520res%2520%253D%2520solution.coinChange%2528coins,%2520amount%2529%250A%2520%2520%2520%2520print%2520res%26cumulative%3Dtrue%26curInstr%3D72%26heapPrimitives%3Dtrue%26mode%3Ddisplay%26origin%3Dopt-live.js%26py%3D2%26rawInputLstJSON%3D%255B%255D%26textReferences%3Dfalse%0A%20%20%20%20%20%20%20%20%22%22%22%0A%20%20%20%20%20%20%20%20if%20amount%20%3D%3D%200%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20return%200%0A%20%20%20%20%20%20%20%20coins.sort%28reverse%3DTrue%29%0A%20%20%20%20%20%20%20%20self.res%20%3D%20amount%20%2B%201%0A%20%20%20%20%20%20%20%20def%20helper%28remiander,%20index,%20c%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20'''dfs%E9%81%8D%E5%8E%86%E6%89%80%E6%9C%89%E5%AD%90%E6%95%B0%E7%BB%84'''%0A%20%20%20%20%20%20%20%20%20%20%20%20%23%E5%A6%82%E6%9E%9C%E4%BD%99%E6%95%B0%E5%88%9A%E5%A5%BD%E4%B8%BA0,%20%E8%A1%A8%E7%A4%BA%E5%87%91%E5%88%B0%E4%BA%86%E5%8F%AF%E4%BB%A5%E5%90%88%E6%88%90amount%E7%9A%84%E5%B8%81%E6%95%B0%0A%20%20%20%20%20%20%20%20%20%20%20%20if%20remiander%20%3D%3D%200%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20self.res%20%3D%20min%28self.res,%20c%29%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20return%0A%20%20%20%20%20%20%20%20%20%20%20%20%23%E5%A6%82%E6%9E%9Ccoins%E7%9A%84%E6%8C%87%E9%92%88index%E8%B6%8A%E7%95%8C,%E5%88%99%E8%BF%94%E5%9B%9E%E4%B8%8A%E4%B8%80%E5%B1%82%E9%80%92%E5%BD%92%0A%20%20%20%20%20%20%20%20%20%20%20%20if%20index%20%3D%3D%20len%28coins%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20return%0A%20%20%20%20%20%20%20%20%20%20%20%20%23%E4%BB%8E%E5%A4%A7%E5%88%B0%E5%B0%8F%E5%9C%B0%E9%81%8D%E5%8E%86%E8%83%BD%E5%A4%9F%E5%BE%97%E5%88%B0amount%E7%9A%84%E9%9D%A2%E5%80%BC%E5%92%8C%E6%95%B0%E9%87%8F%0A%20%20%20%20%20%20%20%20%20%20%20%20for%20ci%20in%20range%28remiander/coins%5Bindex%5D,%20-1,%20-1%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%23%E5%A6%82%E6%9E%9C%E8%AF%A5%E9%9D%A2%E5%80%BC%E7%9A%84%E6%95%B0%E9%87%8F%E5%8A%A0%E4%B8%8A%E5%B7%B2%E6%9C%89%E7%9A%84%E7%A1%AC%E5%B8%81%E6%95%B0%E9%87%8F%E5%B0%8F%E4%BA%8E%E5%B7%B2%E7%9F%A5%E7%9A%84res,%E5%88%99%E7%BB%A7%E7%BB%AD%E5%BE%80%E4%B8%8B%E9%80%92%E5%BD%92,%E7%9B%B4%E5%88%B0%E5%87%91%E5%A4%9F%E6%88%96%E7%9D%80%E8%B6%85%E8%BF%87amount%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20if%20ci%20%2B%20c%20%3C%20self.res%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20helper%28remiander%20-%20ci*coins%5Bindex%5D,%20index%2B1,%20ci%2Bc%29%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%23%E5%90%A6%E5%88%99%E8%BF%94%E5%9B%9E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20break%0A%20%20%20%20%20%20%20%20%23%E4%BB%8E%E6%9C%80%E5%A4%A7%E7%9A%84%E9%9D%A2%E5%80%BC%E5%BC%80%E5%A7%8B%E9%80%92%E5%BD%92%E6%B7%B1%E5%BA%A6%E9%81%8D%E5%8E%86%0A%20%20%20%20%20%20%20%20helper%28amount,%200,%200%29%0A%20%20%20%20%20%20%20%20return%20-1%20if%20self.res%20%3D%3D%20amount%20%2B%201%20else%20self.res%0Aif%20__name__%20%3D%3D%20'__main__'%3A%0A%20%20%20%20coins%20%3D%20%5B1,2,5%5D%0A%20%20%20%20amount%20%3D%2011%0A%20%20%20%20solution%20%3D%20Solution%28%29%0A%20%20%20%20print%20solution.coinChange_dfs%28coins,%20amount%29&cumulative=true&curInstr=71&heapPrimitives=true&mode=display&origin=opt-live.js&py=2&rawInputLstJSON=%5B%5D&textReferences=false)

```python
class Solution():
	def coinChange_dfs(self, coins, amount):
		if not amount or not coins:
			return 0
		l = len(coins)
		#从大到小排列
		coins.sort(reverse = True)
		#初始化最少硬币数为最大值
		self.res = amount + 1
		def dfs(balance, index, c):
			'''
			balance: int 剩余金额
			coins:   int 数组长度
			index:   int coins下标
			c:       int 此时的硬币数
			'''
			#如果余额为0,说明刚好凑成总金额,返回最小值
			if balance == 0:
				self.res = min(c, self.res)
				return
			#如果指针index从底部到顶部又回到底部,则返回
			if index == len(coins):
				return
			for i in range(balance/coins[index], -1, -1):
				#如果此时硬币数小于则继续遍历
				if i + c < self.res:
					dfs(balance - i*coins[index], index + 1, c+i)
				#如果是大于,则直接返回
				else:
					break
		#从最大面值开始递归
		dfs(amount, 0, 0)
		return self.res if self.res != amount + 1 else -1
```

##### C语言版
[Dynamic_recursive_c](http://pythontutor.com/c.html#code=%23include%20%3Cstdio.h%3E%0A%23include%20%3Cstdlib.h%3E%0A%23define%20min%28a,b%29%20%28%20%28a%29%3E%28b%29%20%3F%20%28b%29%3A%28a%29%20%29/*%E8%BF%94%E5%9B%9E%E6%9C%80%E5%B0%8F%E5%80%BC*/%0A%23define%20random%28x%29%20%28rand%28%29%25x%29/*%E9%9A%8F%E6%9C%BA%E5%87%BD%E6%95%B0*/%0A%23define%20INT_MAX%202147483647%20/*%E6%9C%80%E5%A4%A7%E5%80%BC%28%E9%9A%8F%E4%BE%BF%E5%AE%9A%E4%B9%89%E7%9A%84%29*/%0Aint%20retMin%20%3D%20INT_MAX%3B/*%E5%88%9D%E5%A7%8B%E5%8C%96retMin%20%3A%20%E6%9C%80%E5%B0%91%E7%A1%AC%E5%B8%81%E6%95%B0*/%0A%0A/*%E9%80%86%E5%BA%8F%E6%95%B0%E7%BB%84%E5%B9%B6%E4%BB%8E%E5%A4%A7%E5%88%B0%E5%B0%8F%E6%8E%92%E5%88%97*/%0Aint%20cmpCoin%28const%20void%20*a,%20const%20void%20*b%29%20%7B%0A%20%20int%20*ap%20%3D%20%28int%20*%29a%3B%0A%20%20int%20*bp%20%3D%20%28int%20*%29b%3B%0A%0A%20%20return%20*bp%20-%20*ap%3B%0A%7D%0A/*DFS%E6%B7%B1%E5%BA%A6%E9%81%8D%E5%8E%86%2B%E5%89%AA%E6%9E%9D*/%0Avoid%20coinChange_dfs%28int*%20coins,%20int%20coinsSize,%20int%20amount,%20int%20index,%20int%20c%29%20%7B%0A%20%20%0A%20%20/*%E5%A6%82%E6%9E%9C%E5%88%9A%E5%A5%BD%E5%87%91%E5%A4%9Famount*/%0A%20%20if%20%28amount%20%3D%3D%200%29%20%7B%0A%20%20%20%20retMin%20%3D%20min%28c,%20retMin%29%3B%0A%20%20%20%20return%3B%0A%20%20%7D%0A%20%20/*%E5%A6%82%E6%9E%9C%E8%B6%8A%E7%95%8C%E5%88%99%E8%BF%94%E5%9B%9E*/%0A%20%20if%20%28index%20%3D%3D%20coinsSize%29%20return%3B%0A%20%20%0A%20%20for%20%28int%20i%20%3D%20amount%20/%20coins%5Bindex%5D%3B%20i%20%3E%3D%200%20%26%26%20i%2Bc%20%3C%20retMin%3B%20i%20--%29%20%7B%0A%20%20%20%20coinChange_dfs%28coins,%20coinsSize,%20amount%20-%20i%20*%20coins%5Bindex%5D,%20index%2B1,%20c%2Bi%29%3B%0A%20%20%7D%0A%0A%7D%0A/*%E4%B8%BB%E5%87%BD%E6%95%B0%0A%E6%8D%A2%E9%9B%B6%E9%92%B1%3A%E7%94%A8%E6%9C%80%E5%B0%91%E7%9A%84%E7%A1%AC%E5%B8%81%E6%95%B0%E6%8D%A2%E5%88%9A%E5%A5%BD%E7%9A%84%E9%9B%B6%E9%92%B1%0Acoins%3A%E9%9D%A2%E9%A2%9D%E4%B8%8D%E7%AD%89%E7%9A%84%E6%95%B0%E7%BB%84%0AcoinsSize%3A%20coins%E6%95%B0%E7%BB%84%E7%9A%84%E5%A4%A7%E5%B0%8F%0Aamount%3A%20%E6%89%80%E8%A6%81%E4%BA%A4%E6%8D%A2%E7%9A%84%E7%9B%AE%E6%A0%87%E5%80%BC%E6%80%BB%E9%87%91%E9%A2%9D%0A*/%0Aint%20coinChange%28int*%20coins,%20int%20coinsSize,%20int%20amount%29%7B%0A%20%20/*%0A%20%20retMin%20%3A%20%E6%9C%80%E5%B0%91%E7%A1%AC%E5%B8%81%E6%95%B0%0A%20%20*/%0A%20%20retMin%20%3D%20INT_MAX%3B%0A%20%20/*%E5%A6%82%E6%9E%9C%E6%95%B0%E7%BB%84%E4%B8%BA%E7%A9%BA,%E5%88%99%E7%AB%8B%E9%A9%AC%E8%BF%94%E5%9B%9E-1*/%0A%20%20if%20%28%28coins%20%3D%3D%20NULL%29%20%7C%7C%20%28coinsSize%20%3D%3D%200%29%29%20%7B%0A%20%20%20%20return%20-1%3B%0A%20%20%7D%0A%20%20/*%E5%A6%82%E6%9E%9Camount%E4%B8%BA%E7%A9%BA,%E5%88%99%E7%AB%8B%E9%A9%AC%E8%BF%94%E5%9B%9E0,%E8%A1%A8%E7%A4%BA%E4%B8%8D%E9%9C%80%E8%A6%81%E4%BA%A4%E6%8D%A2*/%0A%20%20if%20%28amount%20%3D%3D%200%29%20%7B%0A%20%20%20%20return%200%3B%0A%20%20%7D%0A%20%20/*sorts%20an%20array%E5%AF%B9%E6%95%B0%E7%BB%84%E6%8E%92%E5%BA%8F*/%0A%20%20qsort%28coins,%20coinsSize,%20sizeof%28int%29,%20cmpCoin%29%3B%0A%20%20/*%E6%89%93%E5%8D%B0%E6%95%B0%E7%BB%84*/%0A%20%20for%28int%20x%20%3D%200%3B%20x%20%3C%20coinsSize%3B%20x%2B%2B%29%7B%0A%20%20%20%20printf%28%22coins%20%3D%20%25d%5Cn%22,%20coins%5Bx%5D%29%3B%0A%20%20%7D%0A%20%20%0A%20%20coinChange_dfs%28coins,%20coinsSize,%20amount,%200,%200%29%3B%0A%20%20/*%E5%A6%82%E6%9E%9C%E6%9C%80%E5%B0%8F%E7%A1%AC%E5%B8%81%E6%95%B0%E4%BE%9D%E7%84%B6%E7%AD%89%E4%BA%8E%E6%9C%80%E5%A4%A7%E5%80%BC%E8%A1%A8%E7%A4%BA%E5%8C%B9%E9%85%8D%E4%B8%8D%E5%88%B0%E9%9B%B6%E9%92%B1,%E8%BF%94%E5%9B%9E-1,%E5%90%A6%E5%88%99%E8%BF%94%E5%9B%9E%E6%9C%80%E5%B0%8F%E5%80%BC*/%0A%20%20return%20%28retMin%20%3D%3D%20INT_MAX%29%20%3F%20-1%20%3A%20retMin%3B%0A%7D%0A%0Aint%20main%28%29%0A%7B%0A%20%20int%20coins%5B%5D%3D%7B1,2,5%7D%3B%0A%20%20int%20amount%20%3D%208%3B%0A%20%20printf%28%22amount%20%3D%20%25d%5Cn%22,%20amount%29%3B%0A%20%20int%20coinsSize%20%3D%20sizeof%28coins%29%20/%20sizeof%28coins%5B0%5D%29%3B%0A%20%20int%20res%3B%0A%20%20res%3DcoinChange%28coins,%20coinsSize,%20amount%29%3B%0A%20%20printf%28%22res%20%3D%20%25d%22,res%29%3B%0A%20%20return%200%3B%0A%7D&curInstr=0&mode=display&origin=opt-frontend.js&py=c&rawInputLstJSON=%5B%5D)

```c
#include <stdio.h>
#include <stdlib.h>
#define min(a,b) ( (a)>(b) ? (b):(a) )/*返回最小值*/
#define random(x) (rand()%x)/*随机函数*/
#define INT_MAX 2147483647 /*最大值(随便定义的)*/
int retMin = INT_MAX;/*初始化retMin : 最少硬币数*/

/*逆序数组并从大到小排列*/
int cmpCoin(const void *a, const void *b) {
  int *ap = (int *)a;
  int *bp = (int *)b;

  return *bp - *ap;
}
/*DFS深度遍历+剪枝*/
void coinChange_dfs(int* coins, int coinsSize, int amount, int index, int c) {
  
  /*如果刚好凑够amount*/
  if (amount == 0) {
    retMin = min(c, retMin);
    return;
  }
  /*如果越界则返回*/
  if (index == coinsSize) return;
  
  for (int i = amount / coins[index]; i >= 0 && i+c < retMin; i --) {
    coinChange_dfs(coins, coinsSize, amount - i * coins[index], index+1, c+i);
  }

}
/*主函数
换零钱:用最少的硬币数换刚好的零钱
coins:面额不等的数组
coinsSize: coins数组的大小
amount: 所要交换的目标值总金额
*/
int coinChange(int* coins, int coinsSize, int amount){
  /*
  retMin : 最少硬币数
  */
  retMin = INT_MAX;
  /*如果数组为空,则立马返回-1*/
  if ((coins == NULL) || (coinsSize == 0)) {
    return -1;
  }
  /*如果amount为空,则立马返回0,表示不需要交换*/
  if (amount == 0) {
    return 0;
  }
  /*sorts an array对数组排序*/
  qsort(coins, coinsSize, sizeof(int), cmpCoin);
  /*打印数组*/
  for(int x = 0; x < coinsSize; x++){
    printf("coins = %d\n", coins[x]);
  }
  
  coinChange_dfs(coins, coinsSize, amount, 0, 0);
  /*如果最小硬币数依然等于最大值表示匹配不到零钱,返回-1,否则返回最小值*/
  return (retMin == INT_MAX) ? -1 : retMin;
}

int main()
{
  int coins[]={1,2,5};
  int amount = 8;
  printf("amount = %d\n", amount);
  int coinsSize = sizeof(coins) / sizeof(coins[0]);
  int res;
  res=coinChange(coins, coinsSize, amount);
  printf("res = %d",res);
  return 0;
}
```

## 参考

1. [知乎 0-1背包问题的动态规划算法](https://zhuanlan.zhihu.com/p/30959069)
2. [leetcode 题解](https://leetcode-cn.com/problems/coin-change/solution/dong-tai-gui-hua-shi-yong-wan-quan-bei-bao-wen-ti-/)

## 源码

[My Github Code](https://github.com/YoTro/Python_repository/blob/master/Leetcode/DP/leetcode-322-coin-change.py)