---
title: Byebye 2019, Hello 2020
date: 2019-12-31 18:00:00
tags: [2019]
music-id: 1387679200

---

[EN](#EN)|[CN](#CN)

<span id="EN">  
> It passes on just like this, not ceasing day or night  
Bye bye 2019, hope you have a good luck in 2020.
I have experienced so many in the last year
- Changed the work in May
- Becane a regular worker in Sep, then got 3 Aliexpress accounts.
- Double 11  
![loading...](/images/SMT/1111.png) 
- Black Friday in 11/29
![loading...](/images/SMT/1129.png)  
- One of my Aliexpress account which is sold Home Improvement was closed due to the "fake" goods, who reported me sold fake xiaomi goods, but I sold real and nice xiaomi goods.  
![loading...](/images/SMT/accountunavailable.png)  
![loading...](/images/SMT/IPRInfringement.png)

## Work summary
![loading...](/images/SMT/main.png)  

### Publish goods on Aliexpress

### Protect store
[About the change of the deposit rules in the settlement] (https://sell.aliexpress.com/en/__pc/15iCAXj8Og.htm)
Change:
| 2019 | 2020 |
| ------ | ------ |
| An annual fee of ￥10,000 will be paid, and will be fully returned when the sales required by the platform are achieved. | Money (starting at 500) |
| The store's overseas warehouse delivery time does not over 15 days | 7 days |
| | Penalty Rule Increases Search Cheating |

### data analysis

Due to YKS Inc has 459 shops, they sold a large number of goods every day , the sales is over 1 million, the orders are over 60k, and the sales sku is about 10k. Hot Categories include **Household**, **Beauty, Health & Hair**, **Automobiles & Motorcycles** 
The average price of Beauty, Health & Hair is $ 20 and the profit margin is 19% the highest in our company. Massage sell the best, but the sales of home appliances are the largest. There are too few skus for Automobiles & Motorcycles (the style is still not rich enough so it can't go up, but the market should not be underestimated)  
We can build a data model to analyze and extract hidden potential goods which are best seller goods, attract goods,profitable goods from several following dimensions:  

1. Sale stores  
2. Shipment quantity  
3. Marginal interest rate  
4. Average customer price  
5. Development date  
6. The total number of accounts that sell this sku  
7. Motion account rate = number of billing accounts / total sales account  
8. Store burst rate = shipments / movements  
9. Risk of infringement  

Starting from the above several elements, after considering the risk of infringement, we can choose the products we need through the following ideas

1. **Best Seller Goods**, choose to develop new products that have been developed in the past three months. The number of mobile accounts is 1, the unit price is less than 2 US dollars, and the lowest price is sold.

2. **High Flow Goods**, choose 3 or more accounts with movable accounts, and the ratio of accounts with movable accounts is higher than the average (usually 11%). At lower prices than the average price, fine-tune the price according to the main country where the order is issued (usually Sku has stable orders, and high profit margins often indicate that the market is not full, and the probability of strong sales is high)

3. **High Profitable Goods**, select a new product that has been sold for nearly three months, with less than 3 accounts, covering a lower number of accounts, sell with high price which is set on hot store

In this way, the overall store structure is relatively stable. This is the strategy used when developing to 700-800 US dollars a day. At less than 400 US dollars a day, stores should use low prices to launch new products to increase exposure and visitors to increase sales.  
The zombie listing of the store should be cleaned up at the beginning of each month to sort out the space and increase exposure.

<span id="CN">
> 逝者如斯夫，不舍昼夜

2019年即将过去，总结一下这一年发生的事情。  
- 5月换工作  
- 9月转正接手了三个账号  
- 11月11日双十一  
![loading...](/images/SMT/1111.png)  
- 11月29日黑五  
![loading...](/images/SMT/1129.png)  
- 12月29日店铺永久  
![loading...](/images/SMT/accountunavailable.png)  
![loading...](/images/SMT/IPRInfringement.png)  

## 工作总结

![loading...](/images/SMT/main.png)  
关店的原因是因为被客户3次投诉卖假货，产品logo是小米，而listing上的品牌是别的。我以后就算卖我也不会再发货给俄罗斯了

### 日常上架  

### 账号维护  

[关于入驻中的保证金规则变更](https://sell.aliexpress.com/zh/__pc/15iCAXj8Og.htm)  
改变：  
|2019|2020|
| ------ | ------ |
|要交年费10000元，当达到平台要求的销售额时会全额返还|只要保证支付宝里有1万金额（类目不同有所不同），支付宝冻结这笔金额用于缴纳违规扣分后的钱（500起步）|
|店铺的海外仓配送时长不超过15天|7天|
| |惩罚规则增加搜索作弊|

### 数据分析
由于有颗树铺货模式459个店铺积累了大量的发货数据，每天光速卖通的销售额就有100多万，发货量有6、7万单，动销sku约1万个，其中热门类目有家居、美容健康、汽摩配
美容健康的客单价$20和利润19%是最高的，按摩仪卖的最好，而家居的销量却是最大的，汽摩配的sku太少（样式还是不够丰富所以不上不下，但市场不容小觑）
我们可以建立一个数据模型从几个维度分析并提取爆款，引流款、利润款

1.出单的账号数  
2.发货数量
3.边利率
4.平均客单价
5.开发时间
6.总销售此sku的账号数
7.动销账号率=出单账号数/总销售账号
8.店铺爆款率=发货数/动销数
9.侵权风险

从上述的几个元素出发，在考虑了侵权风险的情况下，我们可以通过以下思路选出我们需要的产品

1.**新品做爆款**，选择开发在近三个月开发的动销新品，动销账号数为1，客单价在2美金以内，以市场最低价出售

2.**做引流款**，选择动销账号数在3以上，动销账号率高于平均值（一般为11%），以低于平均价，根据出单主要国家进行价格微调出售（一般这样的sku出单稳定，利润率高的往往表示这个市场还没有饱，动销概率很大）

3.**做利润款**，选择近三个月的动销新品，动销账号数小于3，覆盖账号数较低，客单价和利润率都高于平均值的产品，以热销店铺的客单价去出售

这样整个店铺结构就比较稳定，这是在发展到7-800美金一天时使用的策略，在低于400美金一天的店铺应该用压价的方式去上架动销新品增加曝光和访客来提高销售额  
每个月初应该清理一遍店铺的僵尸listing整理出上架空间增加曝光

### 遗憾
没有抓取到批发客户

## 学习总结
![loading...](/images/mind/Processlearningplan.svg)
### 鬼畜调教
《你曾是少年葛平版》正在制作中
### python
成功使用Google sheet的api进行了读取操作

心态总结
经历了几次波折，没有了更多的进步和成长，我的心态又发生了变化，变得和以前一样，需要一次长假来调整。但是时间又如此无情不会因为你而停留半分。辞职风险巨大但是继续苟且又很痛苦，进退两难。我有很多时候都会怀疑自己是否选错了路，是不是不适合这一行。一想到自己离30“不惑”还有几年就一些压力
