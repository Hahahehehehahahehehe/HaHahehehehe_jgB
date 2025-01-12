# Assignment #8: 田忌赛马来了

Updated 1021 GMT+8 Nov 12, 2024

2024 fall, Complied by <mark>同学的姓名、院系</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 12558: 岛屿周⻓

matices, http://cs101.openjudge.cn/practice/12558/ 

思路：

构造保护圈，像围棋一样扣掉每一块陆地旁边的气，最后加和计算周长

代码：

```python
def lenth(x,y):
    if map_list[x][y]==0:
        return False
    num_=0
    if map_list[x-1][y]==1:
        num_+=1
    if map_list[x+1][y]==1:
        num_+=1
    num_+=map_list[x][y-1:y+2:2].count(1)
    return 4-num_

n,m=map(int,input().split())
map_list=[[0 for _ in range(m+2)] for _ in range(n+2)]
for i in range(1,n+1):
    map_list[i]=[0]+list(map(int,input().split()))+[0]
lenths=0
for i in range(1,n+1):
    for j in range(1,m+1):
        if not lenth(i,j):
            continue
        else:
            lenths+=lenth(i,j)
print(lenths)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241113090326467](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241113090326467.png)



### LeetCode54.螺旋矩阵

matrice, https://leetcode.cn/problems/spiral-matrix/

与OJ这个题目一样的 18106: 螺旋矩阵，http://cs101.openjudge.cn/practice/18106

思路：自己的做法如下

```python
def read(a, b, i):
    if 2 * i == min(a, b):
        return
    if 2 * i + 1 == min(a, b):
        if a <= b:
            list_ = list2[i][i:-i]
        if a > b:
            list_ = []
            for j in range(i, a - i):
                list_.append(list2[j][i])
        return list_
    if i != 0:
        list_ = list2[i][i:-i]
    else:
        list_ = list2[i]
    for j in range(i + 1, a - 1 - i):
        list_.append(list2[j][-1 - i])
    if i != 0:
        list_ += list2[a - 1 - i][i:-i:-1]
    else:
        list_ += list2[a - 1 - i][::-1]
    for k in range(a - 1 - i - 1, i, -1):
        list_.append(list2[k][i])
    return list_ + read(a, b, i + 1)


class Solution:
    def spiralOrder(self, matrix):
        return list(map(int, read(len(matrix), len(matrix[0]), 0)))


matrix_str = input().strip(']')
matrix_str = matrix_str.strip('[')
list1 = matrix_str.split('],[')
list2 = []
for x in list1:
    list2.append(x.split(','))
solution = Solution()
print(solution.spiralOrder(list2))
```

但是在pycharm上测试正确在LeetCode上却不正确，老师说是函数递归浅拷贝的问题，（没太明白555），于是去看了答案，和我自己的做法不太一样，不过也收获了新的思路

代码：

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix or not matrix[0]:
            return list()
        
        rows, columns = len(matrix), len(matrix[0])
        visited = [[False] * columns for _ in range(rows)]
        total = rows * columns
        order = [0] * total

        directions = [[0, 1], [1, 0], [0, -1], [-1, 0]]
        row, column = 0, 0
        directionIndex = 0
        for i in range(total):
            order[i] = matrix[row][column]
            visited[row][column] = True
            nextRow, nextColumn = row + directions[directionIndex][0], column + directions[directionIndex][1]
            if not (0 <= nextRow < rows and 0 <= nextColumn < columns and not visited[nextRow][nextColumn]):
                directionIndex = (directionIndex + 1) % 4
            row += directions[directionIndex][0]
            column += directions[directionIndex][1]
        return order
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241113152541740](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241113152541740.png)



### 04133:垃圾炸弹

matrices, http://cs101.openjudge.cn/practice/04133/

思路：

将输入数据存在矩阵中，加保护圈，遍历更新最大值及最大值数量

代码：

```python
d=int(input())
n=int(input())
list1=[[0 for _ in range(1025+2*d)] for _ in range(1025+2*d)]
for _ in range(n):
    x,y,i=map(int,input().split())
    for l in range(-d,d+1):
        for k in range(-d,d+1):
            list1[x+d+l][y+d+k]+=i
num_=0
max_=0
for i in range(d,1025+d):
    a=max(list1[i])
    if a>max_:
        num_=list1[i][d:d+1025].count(a)
        max_=a
    elif a==max_:
        num_+=list1[i][d:d+1025].count(max_)
    else:
        continue
print(f'{num_} {max_}')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241113162533429](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241113162533429.png)



### LeetCode376.摆动序列

greedy, dp, https://leetcode.cn/problems/wiggle-subsequence/

与OJ这个题目一样的，26976:摆动序列, http://cs101.openjudge.cn/routine/26976/

思路：

感觉思路和之前那个拦截导弹差不多，都是dp的套路，区别在于要用到倒数第二项，所以用了一个小技巧，最开始构造列表的时候把第一项放两遍，避免了后面的分类讨论

代码：

```python
n=int(input())
list1=[int(x) for x in input().split()]
list2=[[x,x] for x in list1]

for i in range(1,n):
    for j in range(i):
        if (list2[j][-1]-list2[j][-2])*(list2[j][-1]-list1[i])>=0 and list2[j][-1]-list1[i]!=0 and len(list2[j])>=len(list2[i]):
            list2[i]=list2[j]+[list1[i]]
          
max_lenth=0
for x in list2:
    max_lenth=max(max_lenth,len(x))
print(max_lenth-1)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241113165835944](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241113165835944.png)



### CF455A: Boredom

dp, 1500, https://codeforces.com/contest/455/problem/A

思路：

这道题一开始自己做还是采用拦截导弹的模式，优化了好多遍还是超时，去看了答案，发现果然好奇妙，居然建立了好大的一个数列表，不过这样确实节约了时间，代码也更简洁。学到了另一种dp的办法。

```python
n=int(input())
list1=[int(x) for x in input().split()]
list2=list(set(list1))
list2.sort()
dict1={x:list1.count(x) for x in list2}
points=[x*dict1[x] for x in list2]
dp=points.copy()
for i in range(1,len(list2)):
    for j in range(i):
        if list2[i]-list2[j]>1 :
            dp[i]=max(dp[i],points[i]+dp[j])
print(max(dp))
```

代码：

```python
input()
c = [0] * 100001
for m in map(int, input().split()):
    c[m] += 1

dp = [0] * 100001
for i in range(1, 100001):
    dp[i] = max(dp[i - 1], dp[i - 2] + i * c[i])

print(max(dp))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241113193848146](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241113193848146.png)



### 02287: Tian Ji -- The Horse Racing

greedy, dfs http://cs101.openjudge.cn/practice/02287

思路：

这道题自己琢磨了好久一直有点问题，先看了一下答案，打算周末再好好研究一下

代码：

```python
while True:
    n = int(input())
    if n == 0:
        break
    a = sorted([int(x) for x in input().split()], reverse=True)
    b = sorted([int(x) for x in input().split()], reverse=True)
    c = [[0]*(n+1) for _ in range(n+1)]
    for i in range(1, n+1):
        for j in range(1, n+1):
            if a[i-1] > b[j-1]:
                c[i][j] = max(c[i-1][j], c[i][j-1], c[i-1][j-1]+2)
            elif a[i-1] == b[j-1]:
                c[i][j] = max(c[i-1][j], c[i][j-1], c[i-1][j-1]+1)
            else:
                c[i][j] = max(c[i-1][j], c[i][j-1], c[i-1][j-1])
    print((c[n][n]-n)*200)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241113225826816](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241113225826816.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

期中季终于终于终于结束啦！紧急补了一下 dp和递归的内容，感觉还蛮有收获的，再多加练习应该会更好，已经能较为熟练地运用一些套路做法了，就是最近没怎么做每日选做，打算这个周末再跟一下。作业中的最后一题自己半天搞不出来，但觉得蛮有研究价值的，周末再仔细研究研究/奋斗/奋斗



