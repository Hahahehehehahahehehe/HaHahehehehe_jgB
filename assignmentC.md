# Assignment #C: 五味杂陈 

Updated 1148 GMT+8 Dec 10, 2024

2024 fall, Complied by <mark>同学的姓名、院系</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 1115. 取石子游戏

dfs, https://www.acwing.com/problem/content/description/1117/

思路：

核心就是当多的一堆比少的一堆的两倍还多时，选手可以计算好后面的情况，从而选择一定能获胜的一种放法，设一个循环就可以了

代码：

```python
while True:
    i=1
    a,b=map(int,input().split())
    if a==0 and b==0:
        break
    a,b=max(a,b),min(a,b)
    while a//b<2 and a!=b:
        a,b=b,a-b
        i+=1
    if i%2==1:
        print('win')
    else:
        print('lose')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210160057460](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241210160057460.png)



### 25570: 洋葱

Matrices, http://cs101.openjudge.cn/practice/25570

思路：

设一个函数求每层的和，不断更新最大值即可

代码：

```python
def count(n,i):
    count=sum(matrix[i][i:n-i])+sum(matrix[n-i-1][i:n-i])
    for j in range(i+1,n-i-1):
        count+=matrix[j][i]+matrix[j][n-i-1]
    return count

n=int(input())
matrix=[[int(x) for x in input().split()] for _ in range(n)]
count_max=-float('inf')
for i in range(n//2):
    count_max=max(count_max,count(n,i))
if n%2==1:
    count_max=max(count_max,matrix[n//2][n//2])
print(count_max)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241210161437835](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241210161437835.png)



### 1526C1. Potions(Easy Version)

greedy, dp, data structures, brute force, *1500, https://codeforces.com/problemset/problem/1526/C1

思路：

比较新颖的想法就是创建一个negetives列表记录走到每一个位置所喝过的所有负数饮料，当再喝新的一瓶时就要死掉的时候，用这一瓶和上一步负列表中的最小值比较，若更小则复制上一步的数据，若更大则弹出原先的最小值，将新饮料放进去。

代码：

```python
n=int(input())
potions=[int(x) for x in input().split()]
dp=[0]*n
num=[0]*n
negetives=[[] for _ in range(n)]
if potions[0]>=0:
    dp[0]=potions[0]
    num[0]=1
for i in range(1,n):
    if dp[i-1]+potions[i]>=0:
        dp[i]=dp[i-1]+potions[i]
        num[i]=num[i-1]+1
        if potions[i]<0:
            negetives[i]=negetives[i-1]+[potions[i]]
        else:
            negetives[i]=negetives[i-1]
    else:
        if negetives[i-1]==[]:
            dp[i]=dp[i-1]
            num[i]=num[i-1]
            negetives[i]=negetives[i-1]
        else:
            min_negetive=min(negetives[i-1])
            if potions[i]>min_negetive:
                dp[i]=dp[i-1]+potions[i]-min_negetive
                num[i]=num[i-1]
                negetives[i-1].remove(min_negetive)
                negetives[i]=negetives[i-1]+[potions[i]]
            else:
                dp[i]=dp[i-1]
                num[i]=num[i-1]
                negetives[i]=negetives[i-1]
print(num[-1])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210165811450](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241210165811450.png)



### 22067: 快速堆猪

辅助栈，http://cs101.openjudge.cn/practice/22067/

思路：

创建列表的同时创建一个堆，根据指令进行操作

代码：

```python
import heapq
pigs=[]
pig_heap=[]
while True:
    try:
        order=input()
        if order=='pop':
            if pigs:
                pig_heap.remove(pigs.pop())
        elif order=='min':
            if pig_heap:
                print(pig_heap[0])
        else:
            pig=int(order.split()[1])
            pigs.append(pig)
            heapq.heappush(pig_heap,pig)
    except EOFError:
        break
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210171911209](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241210171911209.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/

思路：

这道题最开始用dfs递归一直报超时，缓存递归后又超内存，又改成栈模拟，结果一直不太对，最后红温了去看了答案，原来是用堆做的，之前没有见过，也算学到了一点新知识

代码：

```python
import heapq
m, n, p = map(int, input().split())
info = []
for _ in range(m):
    info.append(list(input().split()))
directions = [(-1, 0), (1, 0), (0, 1), (0, -1)]


def dijkstra(start_r, start_c, end_r, end_c):
    pos = []
    dist = [[float('inf')] * n for _ in range(m)]
    if info[start_r][start_c] == '#':
        return 'NO'
    dist[start_r][start_c] = 0
    heapq.heappush(pos, (0, start_r, start_c))
    while pos:
        d, r, c = heapq.heappop(pos)
        if r == end_r and c == end_c:
            return d
        h = int(info[r][c])
        for dr, dc in directions:
            nr = r + dr
            nc = c + dc
            if 0 <= nr < m and 0 <= nc < n and info[nr][nc] != '#':
                if dist[nr][nc] > d + abs(int(info[nr][nc]) - h):
                    dist[nr][nc] = d + abs(int(info[nr][nc]) - h)
                    heapq.heappush(pos, (dist[nr][nc], nr, nc))
    return 'NO'


for _ in range(p):
    x, y, z, w = map(int, input().split())
    print(dijkstra(x, y,z,w))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241211093849426](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241211093849426.png)



### 04129: 变换的迷宫

bfs, http://cs101.openjudge.cn/practice/04129/

思路：

这道题自己做了很久一直WA，去看了答案，发现人家用了一个取模k的办法，确实是我没想到的。

代码：

```python
from collections import deque

def bfs(x, y):
    visited = {(0, x, y)}
    dx = [0, 0, 1, -1]
    dy = [1, -1, 0, 0]
    queue = deque([(0, x, y)])
    while queue:
        time, x, y = queue.popleft()
        for i in range(4):
            nx, ny = x + dx[i], y + dy[i]
            temp = (time + 1) % k
            if 0 <= nx < r and 0 <= ny < c and (temp, nx, ny) not in visited:
                cur = maze[nx][ny]
                if cur == 'E':
                    return time + 1
                elif cur != '#' or temp == 0:
                    queue.append((time + 1, nx, ny))
                    visited.add((temp, nx, ny))
    return 'Oop!'


t = int(input())
for _ in range(t):
    r, c, k = map(int, input().split())
    maze = [list(input()) for _ in range(r)]
    for i in range(r):
        for j in range(c):
            if maze[i][j] == 'S':
                print(bfs(i, j))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241211110252231](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241211110252231.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

这次作业的前四道都顺利AC了，第五道不知道可以用堆没做出来，第六道想沿用第五道的套路还是不对。感觉现在对dfs,bfs, 堆的运用容易混淆，而且经常是只有一个地方搞不懂然后不断换方法还是没办法解决。期末要来了，希望不要寄掉哇。。。



