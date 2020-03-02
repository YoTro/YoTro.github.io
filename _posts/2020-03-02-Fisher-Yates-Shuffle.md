---
title: Fisher-Yates Shuffle 
date: 2020-03-02 11:39:00 -0700
tags: [python,random]
music-id: 1318582717
mathjax: true

---

We usually use random.shuffle() to get a random list in python, now we search which algorithm use in this function

[EN](#EN)|[CN](#CN)

<span id="EN"> 

# [Source Code](file:///Library/Frameworks/Python.framework/Versions/3.8/lib/python3.8/random.py)

```python

   def shuffle(self, x, random=None):
        """Shuffle list x in place, and return None.

        Optional argument random is a 0-argument function returning a
        random float in [0.0, 1.0); if it is the default None, the
        standard random.random will be used.

        """

        if random is None:
            randbelow = self._randbelow
            for i in reversed(range(1, len(x))):
                # pick an element in x[:i+1] with which to exchange x[i]
                j = randbelow(i+1)
                x[i], x[j] = x[j], x[i]
        else:
            _int = int
            for i in reversed(range(1, len(x))):
                # pick an element in x[:i+1] with which to exchange x[i]
                j = _int(random() * (i+1))
                x[i], x[j] = x[j], x[i]

```

# [Fisher-Yates Shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)

Time complexity: O(n)
Space complexity: O(n)

# Proof of randomness
Prove that the i-th position where each element is placed in the new array is 1 / n.

## Proof: The probability that an element m is placed in the i-th position P = the probability that m is not selected when the first i-1 positions select elements * the probability that m is selected in the i-th position,

$ p = \frac{n-1}{n}\times \frac{n-1}{n} ... \frac{n-i+1}{n-i+2} \times \frac{1}{n-i+1} 

1. Initialize the original array and the new array, the length of the original array is n (known);

2. From the unprocessed array (if there are i left), randomly generate a number p between [0, i) (assuming the array starts from 0);

3. Take out the p-th number from the remaining i-numbers;

4. Repeat steps 2 and 3 until all numbers have been taken;

5. The sequence of numbers taken from step 3 is a scrambled sequence.


-------

<span id="CN">

```python

   def shuffle(self, x, random=None):
        """Shuffle list x in place, and return None.
        将列表x随机排序，然后返回None。

        Optional argument random is a 0-argument function returning a
        random float in [0.0, 1.0); if it is the default None, the
        standard random.random will be used.
        可选参数random是一个0参数的函数，返回一个
         [0.0，1.0）中的随机浮点数； 如果是默认值None，则
         将使用标准random.random。

        """

        if random is None:
            randbelow = self._randbelow
            for i in reversed(range(1, len(x))):
                # pick an element in x[:i+1] with which to exchange x[i]
                # 选择数组x [：i + 1]中与x [i]交换的元素

                j = randbelow(i+1)
                x[i], x[j] = x[j], x[i]
        else:
            _int = int
            for i in reversed(range(1, len(x))):
                # pick an element in x[:i+1] with which to exchange x[i]
                # 选择数组x [：i + 1]中与x [i]交换的元素

                j = _int(random() * (i+1))
                x[i], x[j] = x[j], x[i]

```

# [Fisher-Yates Shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)

Time complexity: O(n)
Space complexity: O(n)

# 随机性证明

证明每个元素被放置在新数组中的第i个位置是1/n。

## 证明：一个元素m被放入第i个位置的概率 P = 前i-1个位置选择元素时没有选中m的概率 * 第i个位置选中m的概率，即：

$ p = \frac{n-1}{n}\times \frac{n-1}{n} ... \frac{n-i+1}{n-i+2} \times \frac{1}{n-i+1} 

# 原理

1. 初始化原始数组和新数组，原始数组长度为n(已知)；

2. 从还没处理的数组（假如还剩k个）中，随机产生一个[0, k)之间的数字p（假设数组从0开始）；

3. 从剩下的k个数中把第p个数取出；

4. 重复步骤2和3直到数字全部取完；

5. 从步骤3取出的数字序列便是一个打乱了的数列。


