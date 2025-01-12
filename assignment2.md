# Assignment #2: 语法练习

Updated 0126 GMT+8 Sep 24, 2024

2024 fall, Complied by ==同学的姓名、院系==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 263A. Beautiful Matrix

https://codeforces.com/problemset/problem/263/A



思路：借助while循环完成五行数字的输入，通过嵌套if语句再套while循环判断成功定位非零数的位置，最后运用取绝对值的函数 abs()计算得出对换次数



##### 代码

```python
# 
from unittest.mock import right

l=0
r=0
for i in range(5):
    list=input().split()
    num0=list.count('0')
    if num0!=5:
        l+=1
        while r<5:
            if list[r]=='0':
                r+=1
            else:
                r+=1
                break
        right=False
    elif num0==5 and right:
        l+=1
l_step=abs(l-3)
r_step=abs(r-3)
print(l_step+r_step)

```



代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-10-01 112344](C:\Users\32786\OneDrive\Pictures\Screenshots\Screenshot 2024-10-01 112344.png)



### 1328A. Divisibility Problem

https://codeforces.com/problemset/problem/1328/A



思路：

运用 for循环实现逐行输入逐行计算，将每行运算的结果存储到列表中，最后遍历列表一次性输出

##### 代码

```python
# 
num_problems=int(input())
answer_list=[]
for i in range(num_problems):
    list=input().split()
    a=int(list[0])
    b=int(list[1])
    if a%b==0:
        answer_list.append(0)
    else:
        answer_list.append(b-a%b)
for i in answer_list:
    print(i)
```



代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-10-01 114952](C:\Users\32786\OneDrive\Pictures\Screenshots\Screenshot 2024-10-01 114952.png)



### 427A. Police Recruits

https://codeforces.com/problemset/problem/427/A



思路：

用for循环实现对输入内容的逐个处理，再用嵌套if语句进行分类判断

##### 代码

```python
# 
input()
list=input().split()
untreated_crime=0
police_force=0
for i in list:
    if i=='-1':
        if police_force==0:
            untreated_crime+=1
        else:
            police_force-=1
    else:
        police_force+=int(i)
print(untreated_crime)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-10-01 120626](C:\Users\32786\OneDrive\Pictures\Screenshots\Screenshot 2024-10-01 120626.png)



### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：

考虑到修地铁有去重要求，联想到集合取并集可以去除重复元素，于是将数据输入到集合中取并集，得到结果

##### 代码

```python
# 
list0=input().split()
all_tree_num=int(list0[0])
rail_num=int(list0[1])
set_=set()
for i in range(rail_num):
    list=input().split()
    set_new=set(range(int(list[0]),int(list[1])+1))
    set_=set_.union(set_new)
left_tree_num=all_tree_num-len(set_)+1
print(left_tree_num)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-10-01 125116](C:\Users\32786\OneDrive\Pictures\Screenshots\Screenshot 2024-10-01 125116.png)



### sy60: 水仙花数II

https://sunnywhy.com/sfbj/3/1/60



思路：

用for循环将区间里的每个数先后取出来检验，因为字符串可以遍历，所以将数转化为字符串取出各位的数字求立方和，并进行验证，由于输出要求为同行输出且尾部无空格，联想到字符串可合并而且其strip方法可去首位空格，并通过对字符串长度的判断实现分类讨论，得到答案

##### 代码

```python
# 
list_=input().split()
answer_str=' '
for num in range(int(list_[0]),int(list_[1])+1):
    sum=0
    for i in str(num):
        sum+=int(i)**3
    if sum==num:
        answer_str+=' '+str(num)
    else:
        continue
if len(answer_str)<2:
    print('NO')
else:
    answer_str_ultra=answer_str.strip()
    print(answer_str_ultra)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-10-01 162822](C:\Users\32786\OneDrive\Pictures\Screenshots\Screenshot 2024-10-01 162822.png)



### 01922: Ride to School

http://cs101.openjudge.cn/practice/01922/



思路：

先建立一个循环实现多组cases输入的要求，当骑手数等于0时用break退出循环，再使用一个for循环反复计算抵达时间，取最小值得到答案，简化了题设中的数学情景，在循环中将答案存储在列表中最后遍历列表一并输出

##### 代码

```python
# 
answer_list=[]
while True:
    num_riders=int(input())
    if num_riders==0:
        break
    max_time=float('inf')
    for i in range(num_riders):
        speed,time=map(int,input().split())
        if time<0:
            continue
        import math
        finish_time=math.ceil(4.5/speed*3600+time)
        max_time=min(max_time,finish_time)
    answer_list.append(max_time)


for i in answer_list:
    print(i)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241002171327455](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241002171327455.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

我是纯编程小白，而且到九月初才加到群里（之前忘了看邮箱了），这一个月跟着B站上的视频才初步学习了基本的Python语法，一直担心基础不够没有跟进每日选做的练习，连作业也是临近ddl基础语法学习的差不多了才赶紧完成的，不过在完成作业的过程中有了很不同的学习体验，一是通过问AI学习了一些新的函数方法知识，二是通过看答案了解了更多优越的解法，感觉这种边练边学的方法很高效而且很有趣，刷新了我前一段时间对于编程学习的理解，接下来准备抓紧补上之前的每日选做，用这种高效的方法加速提高自己的水平

作业2难度较作业1明显有增加，不过我最开始还是坚持自己做，直到最后一题做了一天半还是Runtime error，才去看了答案，发现原来代码编译过程中的思路就存在漏洞，没有考虑Ti<0时无法相遇而是强行代入公式计算，答案中的算法非常简单而且用了map()函数（之前一直没有学习过，这次顺便学了），最后按照答案的思路自己敲了一遍，感觉很有收获。







