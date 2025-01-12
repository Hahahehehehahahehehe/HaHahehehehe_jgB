# Assignment #D: 十全十美 

Updated 1254 GMT+8 Dec 17, 2024

2024 fall, Complied by <mark>同学的姓名、院系</mark>朱玺谕 工学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02692: 假币问题

brute force, http://cs101.openjudge.cn/practice/02692

思路：

先将平衡状态的数据处理，把该状态下的所有硬币标记为正常，再处理非平衡状态，把各组中的轻端和重端分别存入一个列表中，如果每个重端或轻端中都有未被标记的同一个硬币，则可以判断该硬币为非正常，同时得出偏重或偏轻。

代码：

```python
cases=int(input())
for _ in range(cases):
    coin_condition=[2]*12
    dict_coin={'A':0,'B':1,'C':2,'D':3,'E':4,'F':5,'G':6,'H':7,'I':8,'J':9,'K':10,'L':11}
    tests=[input().split() for i in range(3)]
    heavier=[]
    lighter=[]
    ans='unclear'
    for example in tests:
        if example[2]=='even':
            for x in example[0]+example[1]:
                coin_condition[dict_coin[x]]=0
        elif example[2]=='up':
            heavier.append(example[0])
            lighter.append(example[1])
        else:
            heavier.append(example[1])
            lighter.append(example[0])
    for i in range(len(heavier)):
        for j in range(len(heavier[i])):
            if heavier[i][j] in heavier[(i+1)%len(heavier)] and heavier[i][j] in heavier[(i+2)%len(heavier)] and coin_condition[dict_coin[heavier[i][j]]]!=0:
                ans='heavy'
                letter=heavier[i][j]
                break
        if ans!='unclear':
            break

    for i in range(len(lighter)):
        for j in range(len(lighter[i])):
            if lighter[i][j] in lighter[(i+1)%len(heavier)] and lighter[i][j] in lighter[(i+2)%len(lighter)] and coin_condition[dict_coin[lighter[i][j]]]!=0:
                ans='light'
                letter=lighter[i][j]
                break
        if ans!='unclear':
            break

    print(f'{letter} is the counterfeit coin and it is {ans}.')

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241217143207341](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241217143207341.png)



### 01088: 滑雪

dp, dfs similar, http://cs101.openjudge.cn/practice/01088

思路：

按从低到高的顺序探寻各个位置出发的最大滑雪距离，dp递推公式就是此处的最大滑雪距离等于四周的更低的某点的最大滑雪距离+1

代码：

```python
def surround(i,j):
    sur=0
    directions=[(1,0),(0,1),(-1,0),(0,-1)]
    for dy,dx in directions:
        y,x=i+dy,j+dx
        if 0<=y<r and 0<=x<c and field[y][x]<field[i][j]:
            sur=max(sur,dp[y][x])
    return sur

r,c=map(int,input().split())
field=[[int(x) for x in input().split()] for _ in range(r)]
height_dict={}
for i in range(r):
    for j in range(c):
        if field[i][j] in height_dict:
            height_dict[field[i][j]].append((i,j))
        else:
            height_dict[field[i][j]]=[(i,j)]
key_list=list(height_dict.keys())
key_list.sort()
dp=[[1]*c for _ in range(r)]
for key in key_list:
    for i,j in height_dict[key]:
        dp[i][j]+=surround(i,j)
ans=1
for row in dp:
    ans=max(ans,max(row))
print(ans)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241217153228993](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241217153228993.png)

### 25572: 螃蟹采蘑菇

bfs, dfs, http://cs101.openjudge.cn/practice/25572/

思路：

实际上就是一个加强版的dfs，用乌龟的一头去做dfs，在判断能否通过的时候加一个对旁边位置的判断即可。

代码：

```python
def dfs(y,x,end,visited,matrix,directions):
    global flag
    if (y,x)==end or (y+derta_y,x+derta_x)==end:
        flag='yes'
        return
    for dy,dx in directions:
        ny,nx=y+dy,x+dx
        if 0<=ny<n and 0<=nx<n and 0<=ny+derta_y<n and 0<=nx+derta_x<n and not visited[ny][nx] and matrix[ny][nx]!=1 and matrix[ny+derta_y][nx+derta_x]!=1:
            visited[ny][nx]=True
            dfs(ny,nx,end,visited,matrix,directions)
            visited[ny][nx]=False
    return
n=int(input())
matrix=[]
start=[]
for i in range(n):
    matrix.append([int(x) for x in input().split()])
    if 9 in matrix[i]:
        end=(i,matrix[i].index(9))
    if matrix[i].count(5)==1:
        start.append((i,matrix[i].index(5)))
    elif matrix[i].count(5)==2:
        start=[(i,matrix[i].index(5)),(i,matrix[i].index(5)+1)]
derta_y,derta_x=start[0][0]-start[1][0],start[0][1]-start[1][1]
visited=[[False]*n for i in range(n)]
directions=[(0,1),(0,-1),(1,0),(-1,0)]
flag='no'
dfs(start[1][0],start[1][1],end,visited,matrix,directions)
print(flag)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241217160441586](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241217160441586.png)



### 27373: 最大整数

dp, http://cs101.openjudge.cn/practice/27373/

思路：

去年的期末考试题，昨天刚好刷到了，思路就是建立一个不同位数下能组成的最大整数的dp列表，类似于小偷背包

代码：

```python
m=int(input())
n=int(input())
nums=[int(x) for x in input().split()]
nums.sort()
dp=[0]*(1+m)
not_used=[nums.copy() for _ in range(1+m)]
for i in range(1,m+1):
    dp[i]=dp[i-1]
    not_used[i]=not_used[i-1]
    for x in nums:
        lenth=len(str(x))
        if lenth<=i and x in not_used[i-lenth]:
            if dp[i]<dp[i-lenth]*(10**(lenth))+x:
                dp[i]=dp[i-lenth]*(10**(lenth))+x
                not_used[i]=not_used[i-lenth].copy()
                not_used[i].remove(x)
print(dp[m])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241217160735051](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241217160735051.png)



### 02811: 熄灯问题

brute force, http://cs101.openjudge.cn/practice/02811

思路：这道题自己做的时候没有一个很确定的思路，就想着反复把各行熄完，如果熄不完就掉头，结果时间超级长，就去看了答案，原来是要把第一行可能的64种做法全列出来。之前没见过这么搞的，也算积累了一种新思路

```python
def turn(y,x):
    for dy,dx in directions:
        ny,nx=y+dy,x+dx
        if 0<=ny<5 and 0<=nx<5:
            lights[ny][nx]=(lights[ny][nx]+1)%2
    times[y][x]=(times[y][x]+1)%2
lights=[[int(x) for x in input().split()] for _ in range(5)]
times=[[0]*6 for _ in range(5)]
directions=[(1,0),(0,1),(-1,0),(0,-1)]
m=0
while True:
    dy=(-1)**m
    for y in range(m,4+m):
        if 1 not in lights[y]:
            continue
        for x in range(6):
            if lights[y][x]==1:
                turn(y+dy,x)
    if  (m%2==0 and 1 not in lights[4]) or (m%2==1 and 1 not in lights[0]):
        break
    m=(m+1)%2
for row in times:
    print(' '.join([str(x) for x in row]))
```

代码：

```python
X = [[0,0,0,0,0,0,0,0]]
Y = [[0,0,0,0,0,0,0,0]]
for _ in range(5):
    X.append([0] + [int(x) for x in input().split()] + [0])
    Y.append([0 for x in range(8)])    
X.append([0,0,0,0,0,0,0,0])
Y.append([0,0,0,0,0,0,0,0])

import copy
for a in range(2):
    Y[1][1] = a
    for b in range(2):
        Y[1][2] = b
        for c in range(2):
            Y[1][3] = c
            for d in range(2):
                Y[1][4] = d
                for e in range(2):
                    Y[1][5] = e
                    for f in range(2):
                        Y[1][6] = f
                        
                        A = copy.deepcopy(X)
                        B = copy.deepcopy(Y)
                        for i in range(1, 7):
                            if B[1][i] == 1:
                                A[1][i] = abs(A[1][i] - 1)
                                A[1][i-1] = abs(A[1][i-1] - 1)
                                A[1][i+1] = abs(A[1][i+1] - 1)
                                A[2][i] = abs(A[2][i] - 1)
                        for i in range(2, 6):
                            for j in range(1, 7):
                                if A[i-1][j] == 1:
                                    B[i][j] = 1
                                    A[i][j] = abs(A[i][j] - 1)
                                    A[i-1][j] = abs(A[i-1][j] - 1)
                                    A[i+1][j] = abs(A[i+1][j] - 1)
                                    A[i][j-1] = abs(A[i][j-1] - 1)
                                    A[i][j+1] = abs(A[i][j+1] - 1)
                        if A[5][1]==0 and A[5][2]==0 and A[5][3]==0 and A[5][4]==0 and A[5][5]==0 and A[5][6]==0:
                            for i in range(1, 6):
                                print(" ".join(repr(y) for y in [B[i][1],B[i][2],B[i][3],B[i][4],B[i][5],B[i][6] ]))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241217195707115](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241217195707115.png)



### 08210: 河中跳房子

binary search, greedy, http://cs101.openjudge.cn/practice/08210/

思路：

这道题自己做的时候用的栈bfs，一直超内存，去看了答案，感觉思路非常奇妙，居然从去掉几块石头出发，用二分法查找，只能感慨人家的伟大。

代码：

```python
L,n,m = map(int,input().split())
rock = [0]
for i in range(n):
    rock.append(int(input()))
rock.append(L)

def check(x):
    num = 0
    now = 0
    for i in range(1, n+2):
        if rock[i] - now < x:
            num += 1
        else:
            now = rock[i]
            
    if num > m:
        return True
    else:
        return False
lo, hi = 0, L+1
ans = -1
while lo < hi:
    mid = (lo + hi) // 2
    
    if check(mid):
        hi = mid
    else:               # 返回False，有可能是num==m
        ans = mid       # 如果num==m, mid可能是答案
        lo = mid + 1
        
#print(lo-1)
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241217230531016](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241217230531016.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

这周周一做了去年的期末题，竟然全AC了（没有整体计时，是利用碎片时间做的），这次的作业的前三道都是一次提交就AC，超级开心。第四题昨天刚做完，就直接交了。但是后两个题却成功把我撅爆乐，尤其是最后一个，用栈怎么改都是超内存，一看答案原来是二分法。。。但是一直对二分法掌握的不太好，期末要来力，赶紧补一下这块的东西。



