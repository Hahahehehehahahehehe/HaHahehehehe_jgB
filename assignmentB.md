# Assignment #B: Dec Mock Exam大雪前一天

Updated 1649 GMT+8 Dec 5, 2024

2024 fall, Complied by <mark>同学的姓名、院系</mark> 朱玺谕 工学院 



**说明：**

1）⽉考： AC6<mark>（请改为同学的通过数）</mark> 1。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E22548: 机智的股民老张

http://cs101.openjudge.cn/practice/22548/

思路：

将输入的数据先存在一个列表中，再把各个数据及其index存进另一个列表中，按获利排序，从两头开始作差，使用过的放进used列表中

代码：

```python
list1=list(map(int,input().split()))
list2=[]
earn=0
for index,value in enumerate(list1):
    list2.append([index,value])
list2.sort(key=lambda x:x[1],reverse=True)
used=[]
i=0
while len(used)<len(list1):
   for j in range(i+1,len(list1)):
       if list2[i][0]>list2[j][0]:
           earn=max(earn,list2[i][1]-list2[j][1])
           used+=[list2[i][0],list2[j][0]]
   i+=1
print(earn)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241208094810122](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241208094810122.png)



### M28701: 炸鸡排

greedy, http://cs101.openjudge.cn/practice/28701/

思路：

这道题考场上没找到清晰的思路，就先跳过了，下来看了看答案发现其实想法很简单，只要理解了题目背后的意思就能很快做出来。

代码：

```python
n, k = map(int, input().split())
time = list(map(int, input().split()))
time.sort()
s = sum(time)
while True:
    if time[-1] > s / k:
        s -= time.pop()
        k -= 1
    else:
        print(f'{s / k:.3f}')
        break
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241208115646428](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241208115646428.png)



### M20744: 土豪购物

dp, http://cs101.openjudge.cn/practice/20744/

思路：

第一遍想着把每个位置作为放回点求最大价值，但是没用dp交上去超时了，第二遍沿着第一次的思路，写出了dp公式，终于ac了

```python
def max_get(list):
    sum=0
    max_value=0
    for x in list:
        sum+=x
        max_value=max(max_value,sum)
    return max_value
prices=[int(x) for x in input().split(',')]
n=len(prices)
max_=max(prices)
if max_<=0:
    print(max_)
else:
    for i in range(n):
        if i==0:
            max_=max(max_,max_get(prices[1:]))

        elif i==n-1:
            max_=max(max_,max_get(prices[n-2::-1]))

        else:
            max_=max(max_get(prices[i-1::-1])+max_get(prices[i+1:]),max_)

    print(max(max_,sum(prices)))
```

代码：

```python
prices=[int(x) for x in input().split(',')]
n=len(prices)
max_=max(prices)
if max_<=0:
    print(max_)
else:
    pre=[0]*n
    suf=[0]*n
    for i in range(1,n):
        pre[i]=max(pre[i-1]+prices[i-1],prices[i-1])
    for i in range(n-2,-1,-1):
        suf[i]=max(suf[i+1]+prices[i+1],prices[i+1])
    for j in range(n):
        max_=max(pre[j]+suf[j],max_)
    print(max(max_,sum(prices)))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241208140635673](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241208140635673.png)



### T25561: 2022决战双十一

brute force, dfs, http://cs101.openjudge.cn/practice/25561/

思路：

主题代码花了半个小时完成，debug用了快2个小时。。。终于还是做出来了。思路就是把每种可能的组合用dfs考虑到，算出价格不断更新最小值。

代码：

```python
def dfs(x,y,n,m,price):
    global min_price
    if y==n-1:
        # print(price)
        prices=sum(price)
        for i in range(m):
            for j in range(len(cut[i])):
                if price[i]>=cut[i][j][0]:
                    prices-=cut[i][j][1]
                    # print(prices)
                    break
        prices-=(sum(price)//300)*50
        min_price=min(min_price,prices)
        return
    for nx in range(n):
        ny=y+1
        if 0<=nx<m and glories[ny][nx]!=0:
            price[nx]+=glories[ny][nx]
            dfs(nx,ny,n,m,price)
            price[nx]-=glories[ny][nx]
n,m=map(int,input().split())
glories=[[0]*m for _ in range(n)]
cut=[[] for _ in range(m)]
for i in range(n):
    for x in input().split():
        a,b=map(int,x.split(':'))
        glories[i][a-1]=b
for j in range(m):
    cut[j]=[(int(x.split('-')[0]),int(x.split('-')[1])) for x in input().split()]
    cut[j].sort(key=lambda x:x[1],reverse=True)
# print(cut)
min_price=float('inf')
for i in range(m):
    if glories[0][i]!=0:
        price=[0]*m
        price[i]=glories[0][i]
        dfs(i,0,n,m,price)
print(min_price)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241208155435676](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241208155435676.png)



### T20741: 两座孤岛最短距离

dfs, bfs, http://cs101.openjudge.cn/practice/20741/

思路：这道题自己做的一直超时，一直想着先把两个岛屿构造出来再bfs找路线，结果一直超时，最后看了答案，只构造了一个岛屿，再去找另一个岛屿，时间就缩短了不少。

```python
from  _collections import deque
def bfs_island(x,y):
    directions=[[0,1],[0,-1],[1,0],[-1,0]]
    stark=deque([(x,y)])
    island=[(x,y)]
    step={(x,y):0}
    while stark:
        x0,y0=stark.popleft()
        for dx,dy in directions:
            x1,y1=x0+dx,y0+dy
            if 0<=x1<m and 0<=y1<n and island_map[x1][y1]==1:
                island_map[x1][y1]=0
                step[(x1,y1)]=step[(x0,y0)]+1
                stark.append((x1,y1))
                island.append((x1,y1))
    width=max(step.values())
    border=[]
    for pos in step.keys():
        if step[pos]==width:
            border.append(pos)
    return island,border
def bfs_distance(x,y):
    directions = [[0, 1], [0, -1], [1, 0], [-1, 0]]
    stark = deque([(x, y)])
    step={(x,y):0}
    ways=float('inf')
    while stark:
        x0, y0 = stark.popleft()
        for dx, dy in directions:
            x1, y1 = x0 + dx, y0 + dy
            if 0 <= x1 < m and 0 <= y1 < n:
                if island_map[x1][y1] == 2:
                    ways=min(ways,step[(x0,y0)])
                if island_map[x1][y1] == 0 and (x1, y1) not in step:
                    stark.append((x1, y1))
                    step[(x1,y1)]=step[(x0,y0)]+1
    return ways
n=int(input())
island_map=[[int(x) for x in input()] for _ in range(n)]
m=len(island_map[0])
island_list=[]
border_list=[]
for i in range(n):
    for j in range(m):
        if island_map[i][j]==1:
            island,border=bfs_island(i,j)
            island_list.append(island)
            border_list.append(border)
for x,y in island_list[0]:
    island_map[x][y]=1
for x,y in island_list[1]:
    island_map[x][y]=2
print(island_map)
print(border_list)
min_distance=float('inf')
for x,y in border_list[0]:
    min_distance=min(min_distance,bfs_distance(x,y))
print(min_distance)
```

代码：

```python
from collections import deque

def dfs(x, y, grid, n, queue, directions):
    """ Mark the connected component starting from (x, y) as visited using DFS. """
    grid[x][y] = 2  # Mark as visited
    queue.append((x, y))
    for dx, dy in directions:
        nx, ny = x + dx, y + dy
        if 0 <= nx < n and 0 <= ny < n and grid[nx][ny] == 1:
            dfs(nx, ny, grid, n, queue, directions)

def bfs(grid, n, queue, directions):
    """ Perform BFS to find the shortest path to another component. """
    distance = 0
    while queue:
        for _ in range(len(queue)):
            x, y = queue.popleft()
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < n:
                    if grid[nx][ny] == 1:
                        return distance
                    elif grid[nx][ny] == 0:
                        grid[nx][ny] = 2  # Mark as visited
                        queue.append((nx, ny))
        distance += 1
    return distance

def main():
    n = int(input())
    grid = [list(map(int, input())) for _ in range(n)]
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    queue = deque()

    # Start DFS from the first '1' found and use BFS from there
    for i in range(n):
        for j in range(n):
            if grid[i][j] == 1:
                dfs(i, j, grid, n, queue, directions)
                return bfs(grid, n, queue, directions)

if __name__ == "__main__":
    print(main())

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241208113000969](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241208113000969.png)



### T28776: 国王游戏

greedy, http://cs101.openjudge.cn/practice/28776

思路：这道题真的是一点思路都没有，直接去看了答案，觉得好巧妙啊，果然贪心是真的很考验思维啊，想不到原理根本一点办法都没有。



代码：

```python
from typing import List
def Solution(n:int, a:int, b:int, lst:List[List]) -> int:
    lst.sort(key=lambda x: (x[0] * x[1]))
    ans = 0
    for i in range(n):
        ans = max(ans, a // lst[i][1])
        a *= lst[i][0]
    return ans
if __name__ == "__main__":
    n = int(input())
    a, b = map(int, input().split())
    lst = []
    for i in range(n):
        lst.append([int(_) for _ in input().split()])
    # 时间复杂度O(nlogn)，空间复杂度O(n)

    print(Solution(n, a, b, lst))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241208162019496](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241208162019496.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

这次月考只AC了一道（5555555），在考场上感觉好绝望啊，第一题都是卡了蛮久的超时才出的，感觉之后就好慌呀，后面的题都是读了一下题，没太写，只把第四题做了大概有一半。下考场后再去做这些题感觉还是很难，炸鸡排和国王游戏两个贪心题完全没有思路，都是直接看的题解，剩下的几道虽然做出来了，但平均也都要花两个小时左右，麻了。。。。。最近做的题也不多，每日选做挑了几道做了做又被卡了好久，心态爆炸。时间也没剩多久了，再努力努力期末冲一冲吧。



