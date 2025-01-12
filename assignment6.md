# Assignment #6: Recursion and DP

Updated 2201 GMT+8 Oct 29, 2024

2024 fall, Complied by <mark>同学的姓名、院系</mark>朱玺谕 工学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### sy119: 汉诺塔

recursion, https://sunnywhy.com/sfbj/4/3/119  

思路：这道题自己在本子上写出了一个递归的思路，但是一直不知道怎么用代码表达出来，后来问了AI，学习到了用函数表达递归的方法



代码：

```python
def move(n,start,assistant,end):
    if n==0:
        return
    move(n-1,start,end,assistant)
    print(f'{start}->{end}')
    move(n-1,assistant,start,end)



n=int(input())
print(2**n-1)
move(n,'A','B','C')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241030103107970](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241030103107970.png)

### sy132: 全排列I

recursion, https://sunnywhy.com/sfbj/4/3/132

思路：

受到上一道题的启发，采用递归的思路，每次从待排列的数字列表中取出一个数字放在首位，对含有剩下数字的列表进行上一级运算

代码：

```python
def f(list_):
  if len(list_)==1:#list_=[1,2,3,4]
      return list_
  else:
      list_f=[]
      for i in list_:
          list__ = list_.copy()
          list__.remove(i)
          for j in f(list__):
              list_f.append(f'{i} {j}')
      return list_f

n=int(input())
list1=list(range(1,n+1))
for i in f(list1):
    print(i)

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241030153813072](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241030153813072.png)



### 02945: 拦截导弹 

dp, http://cs101.openjudge.cn/2024fallroutine/02945

思路：这道题每日选做里做过一遍，这次第二遍做沿用了第一次向AI学习的方法，求最大降序子列表，关键是用一个lenths列表储存各个数前的最长子序列长度，最后取最大值输出



代码：

```python
n=int(input())
list_=[int(x) for x in input().split()]


lenths=[1]*n

for i in range(1,n):
    for j in range(i):
        if list_[i]<=list_[j] and lenths[i]<=lenths[j]:
            lenths[i]=lenths[j]+1

print(max(lenths)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241030164850664](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241030164850664.png)



### 23421: 小偷背包 

dp, http://cs101.openjudge.cn/practice/23421

思路：

这道题自己想了好几个小时没做出来，感觉自己有思路但是没办法用代码表达出来，或者就是过于复杂，最后看了答案觉得非常精妙，感觉完全是我想不出来的办法，目前的水平还是拿不下这个难度的题（叹气

代码：

```python
n,b=map(int, input().split())
price=[0]+[int(i) for i in input().split()]
weight=[0]+[int(i) for i in input().split()]
bag=[[0]*(b+1) for _ in range(n+1)]
for i in range(1,n+1):
    for j in range(1,b+1):
        if weight[i]<=j:
            bag[i][j]=max(price[i]+bag[i-1][j-weight[i]], bag[i-1][j])
        else:
            bag[i][j]=bag[i-1][j]
print(bag[-1][-1])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241030231652686](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241030231652686.png)



### 02754: 八皇后

dfs and similar, http://cs101.openjudge.cn/practice/02754

思路：

自己在网上找了一些讲解看了一下，但是还是花了好久没能将思路转化为代码，写的又长又繁还好多问题，最后看了看答案觉得别人的想法貌似和我也差不多，代码却简单清晰得多，晕~

代码：

```python


answer = []

def Queen(s):
    for col in range(1, 9):
        for j in range(len(s)):
            if (str(col) == s[j] or # 两个皇后不能在同一列
                    abs(col - int(s[j])) == abs(len(s) - j)): # 两个皇后不能在同一斜线
                break
        else:
            if len(s) == 7:
                answer.append(s + str(col))
            else:
                Queen(s + str(col))

Queen('')

n = int(input())
for _ in range(n):
    a = int(input())
    print(answer[a - 1])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241103145656661](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241103145656661.png)



### 189A. Cut Ribbon 

brute force, dp 1300 https://codeforces.com/problemset/problem/189/A

思路：

实在是心力交瘁了，其中结束后再恶补一下这块的内容吧

代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

被这次的作业题干麻了，只有一道是自己AC的，其他都是花了好久才搞明白，最后一道题还没做已经累趴了。。。感觉一到dp和递归难度就激增，变成了我完全拿不下的类型，期中结束后再努力补一补吧



