# Assignment #9: dfs, bfs, & dp

Updated 2107 GMT+8 Nov 19, 2024

2024 fall, Complied by <mark>同学的姓名、院系</mark>  朱玺谕 工学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 18160: 最大连通域面积

dfs similar, http://cs101.openjudge.cn/practice/18160

思路：

这道题运用了栈模拟递归的办法，在dfs函数中设出8个向量，由于是八个方向所以不用加保护圈，直接计数就行

代码：

```python
def dfs(x,y):
    stark=[(x,y)]
    lake_map[x][y]='.'
    directions=[[-1,-1],[-1,0],[-1,1],[0,-1],[0,1],[1,-1],[1,0],[1,1]]
    size=0
    while stark:
        j,k=stark.pop()
        size+=1
        for dx,dy in directions:
            p,q=j+dx,k+dy
            if 0<=p<n and 0<=q<m and lake_map[p][q]=='W':
                stark.append((p,q))
                lake_map[p][q]='.'
    return size
cases=int(input())
for _ in range(cases):
    n,m=map(int,input().split())
    lake_map=[list(input()) for _ in range(n)]
    max_size=0
    for i in range(n):
        for j in range(m):
            if lake_map[i][j]=='W':
                max_size=max(max_size,dfs(i,j))
    print(max_size)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241120112740073](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241120112740073.png)



### 19930: 寻宝

bfs, http://cs101.openjudge.cn/practice/19930

思路：

运用广度优先搜索，借用队列deque每下一步开始时都取出上一步的第一个位置，采用字典记录步长

代码：

```python
from collections import deque
m,n=map(int,input().split())
treasure_map=[[int(x) for x in input().split()] for _ in range(m)]
result='NO'
result_list=[]
if treasure_map[0][0]==1:
    result_list.append(0)
stark=deque([(0,0)])
directions=[[0,-1],[-1,0],[1,0],[0,1]]
found=set()
step={(0,0):0}
while stark:
    x,y=stark.popleft()
    for dx,dy in directions:
        p,q=x+dx,y+dy
        if 0<=p<m and 0<=q<n and (p,q) not in found:
            found.add((p,q))
            step[(p,q)]=step[(x,y)]+1
            if treasure_map[p][q]==1:
                result_list.append(step[(p,q)])
            elif treasure_map[p][q]==0:
                stark.append((p,q))
if result_list==[]:
    print('NO')
else:
    print(min(result_list))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241120154459541](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241120154459541.png)



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123

思路：

采用dfs递归回溯的做法，踩过的地方标记为visited,但是自己写的报 compile error找AI优化了一下才发现原来好多地方输入不规范，修改后成功AC

代码：

```python
def reach(a,b,m,n,visited):
    directions=[[-2,-1],[-2,1],[-1,-2],[-1,2],[2,-1],[2,1],[1,-2],[1,2]]
    movement=[]
    for dx,dy in directions:
        p,q=a+dx,b+dy
        if 0<=p<n and 0<=q<m and not visited[p][q]:
            movement.append([p,q])

    return movement
def dfs(a,b,m,n,visited,count,target):
    visited[a][b]=True
    if count==target:
        return 1
    result=0
    for next_a,next_b in reach(a,b,m,n,visited):
        result+=dfs(next_a,next_b,m,n,visited,count+1,target)
        visited[next_a][next_b]=False
    return result
T=int(input())
for _ in range(T):
    n,m,x,y=map(int,input().split())
    visited=[[False]*m for _ in range(n)]
    times=dfs(x,y,m,n,visited,1,n*m)
    print(times)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241120165606978](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241120165606978.png)



### sy316: 矩阵最大权值路径

dfs, https://sunnywhy.com/sfbj/8/1/316

思路：

递归回溯得到所有路径，更新最大权值的路径并输出

代码：

```python
max_value,max_trace=-float('inf'),[]
def dfs(x,y,n,m,visited,matrix,value,trace):
    visited[x][y]=True
    trace.append(f'{x+1} {y+1}')
    if (x,y)==(n-1,m-1):
        global max_value,max_trace
        if max_value<value+matrix[x][y]:
            max_value=value+matrix[x][y]
            max_trace=trace.copy()
        return
    directions=[[-1,0],[1,0],[0,-1],[0,1]]
    for dx,dy in directions:
        p,q=x+dx,y+dy
        if 0<=p<n and 0<=q<m and not visited[p][q]:
            dfs(p,q,n,m,visited,matrix,value+matrix[x][y],trace)
            visited[p][q]=False
            trace.remove(f'{p+1} {q+1}')
n,m=map(int,input().split())
matrix=[[int(x) for x in input().split()] for _ in range(n)]
visited=[[False]*m for _ in range(n)]
dfs(0,0,n,m,visited,matrix,0,[])
for x in max_trace:
    print(x)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241120181027246](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241120181027246.png)





### LeetCode62.不同路径

dp, https://leetcode.cn/problems/unique-paths/

思路：

在左边和上边加保护圈，每一个位置的路径数等于其上边和左边的路径数之和。

这次发现LeetCode原来不用调用方法和输出，只需要把方法写完就行了啊。。。。。

难怪上次咋都过不了。。。。。

可恶。。。。。

代码：

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp=[[0]*(n+1) for _ in range(m+1)]
        dp[1][1]=1
        for i in range(1,m+1):
            for j in range(1,n+1):
                if i!=1 or j!=1:
                    dp[i][j]=dp[i-1][j]+dp[i][j-1]
        return dp[m][n]
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241120185631833](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241120185631833.png)



### sy358: 受到祝福的平方

dfs, dp, https://sunnywhy.com/sfbj/8/3/539

思路：

主要思路还是递归，如果产生要求的结果则每层都返回True，否则无返回值，据此得出结果

代码：

```python
num=input()

def dfs(start,num):
    if start==len(num):
        return True
    for i in range(start,len(num)):
        squart=int(num[start:i+1])**0.5
        if (int(num[start:i+1])**0.5)%1==0 and squart>0:
            if dfs(i+1,num)==None:
                continue
            elif  dfs(i+1,num):
                return True
if dfs(0,num)==None:
    print('No')
else:
    print('Yes')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![image-20241120202202476](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241120202202476.png)

## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

本周作业独立AC了五道半（马走日用AI优化了一下），感觉还是蛮开心的。和平时不一样的一点是，这次是先学习了课件中的dfs、bfs才开始做题的，感觉效率一下提高了，可能这块的题目都差不多是一个套路，把套路掌握了做起来就顺手了。这次的学习模式不错，真正有学到东西了，下次还这么做吧。额外练习的没有很多，做了最近发的几道每日选做，学了一下二分法查找，也蛮有收获的，状态还不错，继续加油！！！



