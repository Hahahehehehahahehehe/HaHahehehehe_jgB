# Assignment #4: T-primes + 贪心

Updated 0337 GMT+8 Oct 15, 2024

2024 fall, Complied by <mark>朱玺谕、工学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 34B. Sale

greedy, sorting, 900, https://codeforces.com/problemset/problem/34/B



思路：

将各个电视的出价存储到列表中，升次排序，用for循环累加前m项中的非正项，当出现正数项时用break终止循环

代码

```python
# 
n,m=map(int,input().split())
TV_list=[int(x) for x in input().split()]
TV_list.sort()
money_earned=0
for i in TV_list[:m]:
    if i>0:
        break
    money_earned+=i
print(-money_earned)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241015154913653](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241015154913653.png)



### 160A. Twins

greedy, sortings, 900, https://codeforces.com/problemset/problem/160/A

思路：

将硬币按面额递减顺序存储在一个列表中，用while循环附加钱总额不低于半数的条件，最后得到硬币数

代码

```python
n=int(input())
coin_list=[int(x) for x in input().split()]
coin_list.sort(reverse=True)
my_money=0
i=0
while my_money<=sum(coin_list)/2:
    my_money+=coin_list[i]
    i+=1
print(i)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241015155904979](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241015155904979.png)



### 1879B. Chips on the Board

constructive algorithms, greedy, 900, https://codeforces.com/problemset/problem/1879/B

思路：

最后的结果无非是两种情况：{an}中最小的一项与{bn}各项相加之和，或{bn}中最小的一项与{an}各项相加之和，两者取较小者即为答案

```python
cases=int(input())
for i in range(cases):
    n=int(input())
    list_a=[int(x) for x in input().split()]
    list_b=[int(x) for x in input().split()]
    print(min(min(list_b)*n+sum(list_a),min(list_a)*n+sum(list_b)))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241015162443078](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241015162443078.png)



### 158B. Taxi

*special problem, greedy, implementation, 1100, https://codeforces.com/problemset/problem/158/B

思路：

本题思路有点类似于之前的装箱问题，不过比装箱问题简单，一组中有4/3人的都需要一辆车，2人的两两一组坐一车，单人组队的填缝，最后得到车辆数

代码

```python
import math
groups=int(input())
group_list=[int(x) for x in input().split()]
cars=group_list.count(4)+group_list.count(3)+math.ceil(group_list.count(2)/2)
if group_list.count(1)>group_list.count(3)+group_list.count(2)%2*2:
    cars+=math.ceil((group_list.count(1)-(group_list.count(3)+group_list.count(2)%2*2))/4)
print(cars)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241015164218812](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241015164218812.png)



### *230B. T-primes（选做）

binary search, implementation, math, number theory, 1300, http://codeforces.com/problemset/problem/230/B

思路：

这道题主要就是考察质数的判断，用自己的方法依次取多个数做除数一直超时卡在test36，后来去看了答案，学了埃氏筛法和欧拉筛法，发现预先求出质数列表会省时好多，最后选用了欧拉筛法

代码

```python
def euler_sieve(n):
    is_prime = [True] * (n + 1)
    primes = []
    for i in range(2, n + 1):
        if is_prime[i]:
            primes.append(i)
        for p in primes:
            if i * p > n:
                break
            is_prime[i * p] = False
            if i % p == 0:
                break
    return primes
n=int(input())
num_list=[int(x) for x in input().split()]
list_prime=euler_sieve(10**6)
for i in num_list:
    if int(i**(1/2)) in list_prime and i**(1/2)%1==0:
        print('YES')
    else:
        print('NO')

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241015200122246](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241015200122246.png)



### *12559: 最大最小整数 （选做）

greedy, strings, sortings, http://cs101.openjudge.cn/practice/12559

思路：

自己做的过程思路是列出所有可能出现的排列，从中取最大值和最小值，结果一直超时，就学习了答案的几种做法

代码

```python
n = int(input())
nums = input().split()
for i in range(n - 1):
    for j in range(i+1, n):
        if nums[i] + nums[j] < nums[j] + nums[i]:
            nums[i], nums[j] = nums[j], nums[i]

ans = "".join(nums)
nums.reverse()
print(ans + " " + "".join(nums))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241016103754109](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241016103754109.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

感觉近期的题目和前段时间的题目相比难度明显有上升，很多题自己琢磨很久还是AC不了，往往存在超时的问题，对于时间复杂度如何计算感到很困惑，又是自己觉得明明非常简单的代码，运行时间却要几万毫秒，反而有些多层循环嵌套的代码，运行时间却只有几十毫秒，希望老师和助教能在时间复杂度的问题上能给予一些指导，不然有时候超时了都不知道该往什么方向改。同时也有不少收获，和之前有所不同的是收获比起来自AI更多来自答案了，可能是对于基本语言已经大致掌握，从更高级更精妙的答案优解中总能更快习得常用的技巧与思路，是提升水平的重要途径。随着题目难度的上升，之前“起飞”的“得意忘形”感差不多耗尽了，接下来会是更艰难的爬坡，加油！



