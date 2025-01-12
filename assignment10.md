# Assignment #10: dp & bfs

Updated 2 GMT+8 Nov 25, 2024

2024 fall, Complied by <mark>同学的姓名、院系</mark>朱玺谕 工学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### LuoguP1255 数楼梯

dp, bfs, https://www.luogu.com.cn/problem/P1255

思路：

第一遍用了递归的方法，结果不出所料地超时了，然后就改用了dp，保持dp列表中的元素个数始终为三个，不断更新列表里的方法数。

代码：

```python
n=int(input())
dp=[1,2,0]
for i in range(2,n):
    dp[i%3]=dp[(i-1)%3]+dp[(i-2)%3]
print(dp[(n-1)%3])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241126105912853](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241126105912853.png)



### 27528: 跳台阶

dp, http://cs101.openjudge.cn/practice/27528/

思路：

没一个台阶可能的走法数都是之前的所有台阶的走法数之和，注意还要加上第0级台阶。

代码：

```python
n=int(input())
dp=[1,1,2]+[0]*(n-2)
for i in range(3,n+1):
    dp[i]=sum(dp[:i])
print(dp[n])
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241126110934889](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241126110934889.png)



### 474D. Flowers

dp, https://codeforces.com/problemset/problem/474/D

思路：这道题自己搞了好久，debug优化花了快一下午，一直是超时，气急败坏去看了答案，发现dp思路都一样，不过答案先将所有结果一次算好，最后直接调用，减少了运算次数，所以用时更短。

```python
def flower(k,n):
    if n<k:
        return [0]*(k-n)+[1]*n
    dp=[1]*(k-1)+[2]
    for i in range(k,n):
        dp.append(dp[-1]+dp[0])
        dp.pop(0)
    return dp
m=10**9+7
n,k=map(int,input().split())
for i in range(n):
    a,b=map(int,input().split())
    if k==1:
        print((2**a)*(2**(b-a+1)-1))
    else:
        count=0
        for j in range(b,a-1,-k):
            count+=(sum(flower(k,j)))%m
        count-=(sum(flower(k,a+(b-a)%k)[0:-(b-a)%k-1]))%m
        print(count%m)
```

代码：

```python
MOD = 1000000007
MAXN = 100001

t, k = map(int, input().split())

f = [0] * MAXN
f[0] = 1
s = [0] * MAXN
for i in range(1, 100001):
    if i >= k:
        f[i] = (f[i-1] + f[i - k]) % MOD
    else:       
        f[i] = 1

    s[i] = (s[i - 1] + f[i]) % MOD

for _ in range(t):
    a, b = map(int, input().split())
    print((s[b] - s[a - 1] + MOD) % MOD)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241126165947532](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241126165947532.png)



### LeetCode5.最长回文子串

dp, two pointers, string, https://leetcode.cn/problems/longest-palindromic-substring/

思路：

这道题自己没什么思路，直接看了题解，觉得人家的想法很巧妙，就默写了一下。果然 dp的题还是可以上限很高啊

代码：

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        if n < 2:
            return s
        
        max_len = 1
        begin = 0
        # dp[i][j] 表示 s[i..j] 是否是回文串
        dp = [[False] * n for _ in range(n)]
        for i in range(n):
            dp[i][i] = True
        
        # 递推开始
        # 先枚举子串长度
        for L in range(2, n + 1):
            # 枚举左边界，左边界的上限设置可以宽松一些
            for i in range(n):
                # 由 L 和 i 可以确定右边界，即 j - i + 1 = L 得
                j = L + i - 1
                # 如果右边界越界，就可以退出当前循环
                if j >= n:
                    break
                    
                if s[i] != s[j]:
                    dp[i][j] = False 
                else:
                    if j - i < 3:
                        dp[i][j] = True
                    else:
                        dp[i][j] = dp[i + 1][j - 1]
                
                # 只要 dp[i][L] == true 成立，就表示子串 s[i..L] 是回文，此时记录回文长度和起始位置
                if dp[i][j] and j - i + 1 > max_len:
                    max_len = j - i + 1
                    begin = i
        return s[begin:begin + max_len]
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241126173246748](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241126173246748.png)

### 12029: 水淹七军

bfs, dfs, http://cs101.openjudge.cn/practice/12029/

思路：

最开始不知道要一次性输入，一直都是RE，在群里看了说明后试着改成了sys.stdin，结果还是报RE，最后看了答案感觉也没啥区别，可恶的OJ

代码：

```python
import sys

sys.setrecursionlimit(300000)
input = sys.stdin.read


# 判断坐标是否有效
def is_valid(x, y, m, n):
    return 0 <= x < m and 0 <= y < n


# 深度优先搜索模拟水流
def dfs(x, y, water_height_value, m, n, h, water_height):
    dx = [-1, 1, 0, 0]
    dy = [0, 0, -1, 1]

    for i in range(4):
        nx, ny = x + dx[i], y + dy[i]
        if is_valid(nx, ny, m, n) and h[nx][ny] < water_height_value:
            if water_height[nx][ny] < water_height_value:
                water_height[x][y] = water_height_value
                dfs(nx, ny, water_height_value, m, n, h, water_height)


# 主函数
def main():
    data = input().split()  # 快速读取所有输入数据
    idx = 0
    k = int(data[idx])
    idx += 1
    results = []

    for _ in range(k):
        m, n = map(int, data[idx:idx + 2])
        idx += 2
        h = []
        for i in range(m):
            h.append(list(map(int, data[idx:idx + n])))
            idx += n
        water_height = [[0] * n for _ in range(m)]

        i, j = map(int, data[idx:idx + 2])
        idx += 2
        i, j = i - 1, j - 1

        p = int(data[idx])
        idx += 1

        for _ in range(p):
            x, y = map(int, data[idx:idx + 2])
            idx += 2
            x, y = x - 1, y - 1
            if h[x][y] <= h[i][j]:
                continue

            dfs(x, y, h[x][y], m, n, h, water_height)

        results.append("Yes" if water_height[i][j] > 0 else "No")

    sys.stdout.write("\n".join(results) + "\n")


if __name__ == "__main__":
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241126211400379](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241126211400379.png)



### 02802: 小游戏

bfs, http://cs101.openjudge.cn/practice/02802/

思路：

 这道题彻彻底底底底底底底做红温了（愤怒），做了整整一天，最开始用的 dfs一直不对，后来才发现应该用bfs，又一直不知道咋样才能让visited的地方能重新走还不会陷入死循环，一直爆肝，最后终于心态崩了，去看了答案，这下懂了。。。。。。。

代码：

```python
from collections import deque


def bfs(start, end, grid, h, w):
    queue = deque([start])
    visited = set()
    dirs = [(0, -1), (-1, 0), (0, 1), (1, 0)]

    ans = []
    while queue:
        x, y, d_i_r, seg = queue.popleft()
        # print(x,y,end)
        if (x, y) == end:
            # return seg
            ans.append(seg)
            break

        for i, (dx, dy) in enumerate(dirs):
            nx, ny = x + dx, y + dy

            if 0 <= nx < h + 2 and 0 <= ny < w + 2 and ((nx, ny, i) not in visited):
                new_dir = i
                new_seg = seg if new_dir == d_i_r else seg + 1
                if (nx, ny) == end:
                    # return new_seg
                    ans.append(new_seg)
                    continue

                if grid[nx][ny] != 'X':
                    visited.add((nx, ny, i))
                    queue.append((nx, ny, new_dir, new_seg))

    if len(ans) == 0:
        return -1
    else:
        return min(ans)


board_num = 1
while True:
    w, h = map(int, input().split())
    if w == h == 0:
        break

    # grid = [[' '] * (w + 2)] + \
    # [[' '] + list(input()) + [' '] for _ in range(h)] + \
    # [[' '] * (w + 2)]
    grid = [' ' * (w + 2)] + [' ' + input() + ' ' for _ in range(h)] + [' ' * (w + 2)]
    print(f"Board #{board_num}:")
    pair_num = 1
    while True:
        y1, x1, y2, x2 = map(int, input().split())
        if x1 == y1 == x2 == y2 == 0:
            break

        start = (x1, y1, -1, 0)
        end = (x2, y2)

        seg = bfs(start, end, grid, h, w)
        if seg == -1:
            print(f"Pair {pair_num}: impossible.")
        else:
            print(f"Pair {pair_num}: {seg} segments.")
        pair_num += 1

    print()
    board_num += 1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241127200838250](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241127200838250.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

上周做dfs和bfs还蛮顺的，这周就去世了。做作业最久的一次，最后两个题分别让我的周二和周三陷入持续红温状态。导致最近都没学别的，赶紧西安补一下别的课吧，周末写完各科作业再战！！！

最近做题不多，更多的是看课件自学，上周觉得这样蛮有效的，这周把各种论文肝完了，周末应该有时间做点题了。



