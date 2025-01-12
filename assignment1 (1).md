# Assignment #1: 自主学习

Updated 0110 GMT+8 Sep 10, 2024

2024 fall, Complied by ==同学的姓名、院系==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02733: 判断闰年

http://cs101.openjudge.cn/practice/02733/



思路：运用嵌套的if语句，逐层加强对年份的能否被整除的判断，再逐层输出相应的判断结果，最终达到题目要求



##### 代码

```python
# 
year=input()
if int(year)%4==0:
    if int(year)%100==0:
        if int(year)%400==0:
            print('Y')
        else:
            print('N')
    else:
        print('Y')
else:
    print('N')

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240930210344033](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20240930210344033.png)



### 02750: 鸡兔同笼

http://cs101.openjudge.cn/practice/02750/



思路：主体思路是一个简单的if语句，对输入的“脚数”进行分类讨论，遵循“尽可能为兔则只数最少，全为鸡则只数最多”的原则，借助求被四除得到的余数分为三类，最后得到满足题意的结果



##### 代码

```python
# 
sum=int(input())
if sum%4==0:
    min=sum//4
    max=min*2
elif sum%4==2:
    min=sum//4+1
    max=sum//2
else:
    min=0
    max=0
print(min,max)

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240930210204909](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20240930210204909.png)



### 50A. Domino piling

greedy, math, 800, http://codeforces.com/problemset/problem/50/A



思路：等效为用1*2的砖去铺地，分析后发现最多能铺的砖数就是地的面积除以2的余数，依次设计程序

借助split方法将输入的字符串中的信息存储在列表中，按下标索引后使用



##### 代码

```python
# 
size=input()
size_list=size.split(" ")
lenth=int(size_list[0])
width=int(size_list[1])
max_num=(lenth*width)//2
print(max_num)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240930223416151](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20240930223416151.png)



### 1A. Theatre Square

math, 1000, https://codeforces.com/problemset/problem/1/A



思路：与上一道题同样的方法将输入的字符串形式的信息提取转化为三个独立的参数，学习使用了math模块中的ceil方法对除法运算结果进行向上取整，最后长宽相乘，得到结果



##### 代码

```python
# 
list=input().split()
n=int(list[0])
m=int(list[1])
a=int(list[2])
import math
num_l=math.ceil(n/a)
num_w=math.ceil(m/a)
num=num_l*num_w
print(num)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240930235340976](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20240930235340976.png)



### 112A. Petya and Strings

implementation, strings, 1000, http://codeforces.com/problemset/problem/112/A



思路：使用lower方法将输入的字符串中的所有大写字母转化为小写，由于字符串比较大小本身就是按照字典从头到尾比较的，所以直接进行大小比较得到结果

##### 代码

```python
# 
str1=input().lower()
str2=input().lower()
if str1<str2:
    print(-1)
elif str1>str2:
    print(1)
else:
    print(0)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240930233043092](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20240930233043092.png)



### 231A. Team

bruteforce, greedy, 800, http://codeforces.com/problemset/problem/231/A



思路：由于即将输入的行数取决于输入的第一个数据，先用变量存储输入数据的行数，再使用for循环实现满足要求的多次数据输入，同时在循环中完成对该行数据的提取转换，并嵌套一个if语句完成“是否回答”的判断，累计次数，最终解决问题



##### 代码

```python
# 
num_problem=int(input())
num=0
for i in range(num_problem):
    list=input().split()
    sure_num=list.count('1')
    if sure_num>=2:
        num+=1
print(num)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240930235717592](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20240930235717592.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

我是纯编程小白，而且到九月初才加到群里（之前忘了看邮箱了），这一个月跟着B站上的视频才初步学习了基本的Python语法，一直担心基础不够没有跟进每日选做的练习，连作业也是临近ddl基础语法学习的差不多了才赶紧完成的，不过在完成作业的过程中有了很不同的学习体验，一是通过问AI学习了一些新的函数方法知识，二是通过看答案了解了更多优越的解法，感觉这种边练边学的方法很高效而且很有趣，刷新了我前一段时间对于编程学习的理解，接下来准备抓紧补上之前的每日选做，用这种高效的方法加速提高自己的水平



