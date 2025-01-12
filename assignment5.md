# Assignment #5: Greedy穷举Implementation

Updated 1939 GMT+8 Oct 21, 2024

2024 fall, Complied by <mark>同学的姓名、院系</mark>朱玺谕 工学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 04148: 生理周期

brute force, http://cs101.openjudge.cn/practice/04148

思路：

将21252+d内模33余i的数放进一个列表中，遍历列表，找出同时满足与p模23同余和与e模28同余的数，注意m要大于d

代码：

```python
def f(x,i):
    return 33*x+i
case=1

while True:
    p,e,i,d=map(int,input().split())
    if p==e==i==d==-1:
        break


    list_i=[f(x,i) for x in range((21252+d-i)//33+1)]
    for m in list_i:
        if (m-p)%23==0 and (m-e)%28==0 and m>d and m>=max(p,e,i):
            print(f'Case {case}: the next triple peak occurs in {m-d} days.')
            break
    case+=1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241022130450124](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241022130450124.png)



### 18211: 军备竞赛

greedy, two pointers, http://cs101.openjudge.cn/practice/18211

思路：

将武器库按钱数排序，在钱够的情况下尽量多生产，因此挑便宜的生产，卖的时候挑贵的卖

代码：

```python
p=int(input())
list_weapons=[int(x) for x in input().split()]
list_weapons.sort()
num_=0
list_=[0]
while len(list_weapons)>0 and num_>=0:
    while list_weapons!=[] and p>=list_weapons[0]:
        num_+=1
        p-=list_weapons[0]
        list_weapons.pop(0)
        list_.append(num_)
    if list_weapons==[]:
        break
    p+=list_weapons[-1]
    num_-=1
    list_weapons.pop(-1)
print(max(list_))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241022140244212](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241022140244212.png)



### 21554: 排队做实验

greedy, http://cs101.openjudge.cn/practice/21554

思路：

将每个学生编号与所需时间放进一个列表中便于后续输出，让用时短的学生先做，最后计算总时间

代码：

```python
n=int(input())
i=0
list_1=[]
for j in input().split():
    i+=1
    list_1.append([i,int(j)])
list_1.sort(key=lambda x:x[1])
print(' '.join([str(x[0]) for x in list_1]))
sum_=0
x=0
for m in list_1:
    sum_+=m[1]*(n-x-1)
    x+=1
print(f'{sum_/n:.2f}')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241022153111635](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241022153111635.png)



### 01008: Maya Calendar

implementation, http://cs101.openjudge.cn/practice/01008/

思路：

先将各种玛雅符放进列表中便于后续索引，将H纪年法的年月日全转换为天数，再转换为T纪年，比较特殊的记日法对天数除以260的余数分别求两个周期的余数即可

代码：

```python
n=int(input())
print(n)
Haab_months_list=['pop','no','zip','zotz','tzec','xul','yoxkin','mol','chen','yax','zac','ceh','mac','kankin','muan','pax','koyab','cumhu','uayet']
Tzolkin_day_list=['ahau','imix','ik','akbal','kan','chicchan','cimi','manik','lamat','muluk','ok','chuen','eb','ben','ix','mem','cib','caban','eznab','canac']
T_list=[13,1,2,3,4,5,6,7,8,9,10,11,12]
for i in range(n):
    day,month_year=map(str,input().split('.'))
    month,year=map(str,month_year.split())
    days=int(year)*365+Haab_months_list.index(month)*20+int(day)+1
    T_year=days//260
    T_day=days%260
    if T_day%260==0:
        T_year-=1
    T_date=' '.join([str(T_list[T_day%13]),Tzolkin_day_list[T_day%20],str(T_year)])
    print(T_date)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241022162340596](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241022162340596.png)



### 545C. Woodcutters

dp, greedy, 1500, https://codeforces.com/problemset/problem/545/C

思路：

发现向两边都能倒的树倒的方向会影响结果是否为最优，所以向左向右各计算一次，得出最大值

代码：

```python
n=int(input())
tree_list=[list(map(int,input().split()))for i in range(n)]

num_=2

for x in range(1,n-1):
    if tree_list[x][0]-tree_list[x-1][0]-1<tree_list[x][1] and tree_list[x+1][0]-tree_list[x][0]-1<tree_list[x][1]:
        continue
    elif tree_list[x][0]-tree_list[x-1][0]-1>=tree_list[x][1] and tree_list[x+1][0]-tree_list[x][0]-1<tree_list[x][1]:
        num_+=1
    elif tree_list[x][0]-tree_list[x-1][0]-1<tree_list[x][1] and tree_list[x+1][0]-tree_list[x][0]-1>=tree_list[x][1]:
        num_+=1
        tree_list[x][0]=tree_list[x][0]+tree_list[x][1]
    else:
        num_+=1
tree_list.sort(reverse=True)
num__=2
for x in range(1,n-1):
    if tree_list[x][0]-tree_list[x-1][0]-1<tree_list[x][1] and tree_list[x+1][0]-tree_list[x][0]-1<tree_list[x][1]:
        continue
    elif tree_list[x][0]-tree_list[x-1][0]-1>=tree_list[x][1] and tree_list[x+1][0]-tree_list[x][0]-1<tree_list[x][1]:
        num__+=1
    elif tree_list[x][0]-tree_list[x-1][0]-1<tree_list[x][1] and tree_list[x+1][0]-tree_list[x][0]-1>=tree_list[x][1]:
        num__+=1
        tree_list[x][0]=tree_list[x][0]+tree_list[x][1]
    else:
        num__+=1
if n==1:
    num_=num__=1
print(max(num_,num__))


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241022174335832](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241022174335832.png)



### 01328: Radar Installation

greedy, http://cs101.openjudge.cn/practice/01328/

思路：

感觉自己的思路太复杂了，来回改了3个小时还是RuntimeError，看不出问题，最后看了看答案

代码：

```python
import math

def solve(n, d, islands):
    if d < 0:
        return -1

    ranges = []
    for x, y in islands:
        if y > d:
            return -1
        delta = math.sqrt(d * d - y * y)
        ranges.append((x - delta, x + delta))

    if not ranges:
        return -1

    ranges.sort(key=lambda x:x[1])

    number = 1
    r = ranges[0][1]
    for start, end in ranges[1:]:
        if r < start:
            r = end
            number += 1

    return number

case_number = 0
while True:
    n, d = map(int, input().split())
    if n == 0 and d == 0:
        break

    case_number += 1
    islands = []
    for _ in range(n):
        islands.append(tuple(map(int, input().split())))

    result = solve(n, d, islands)
    print(f"Case {case_number}: {result}")
    input()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241027224301908](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241027224301908.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

感觉最近作业、每日选做题目的难度明显有提升，每做一道题都很花时间，每日选做在努力追（在做新题，之前的落下的有些还没补），但由于其他科的压力，感觉练习量还是有限。这次的作业前五道虽然用时很长但最后总算是做出来的，第六道一直RE卡了三个小时，下来再看看答案研究研究。



