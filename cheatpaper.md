```python
#eular sieve to find all primes up to n
def euler_sieve(n):
    is_prime = [True] * (n + 1)
    primes = []
    for i in range(2, n + 1):
        if is_prime[i]:
            primes.append(i)
        for p in primes:
            if i * p > n:
                break
            is_prime[i * p] = False
            if i % p == 0:
                break
    return primes


# http://codeforces.com/problemset/problem/230/B

# https://www.geeksforgeeks.org/python-program-for-sieve-of-eratosthenes/
# Python program to print all primes smaller than or equal to
# n using Sieve of Eratosthenes

def SieveOfEratosthenes(n, prime):
    p = 2
    while (p * p <= n):

        # If prime[p] is not changed, then it is a prime
        if (prime[p] == True):

            # Update all multiples of p
            for i in range(p * 2, n + 1, p):
                prime[i] = False
        p += 1


n = int(input())
x = [int(i) for i in input().split()]

s = [True] * (10 ** 6 + 1)

SieveOfEratosthenes(10 ** 6, s)

for i in x:
    if i < 4:
        print('NO')
        continue
    elif int(i ** 0.5) ** 2 != i:
        print('NO')
        continue
    print(['NO', 'YES'][s[int(i ** 0.5)]])
    # if s[int(i**0.5)]:
    #    print('YES')
    # else:
    #    print('NO')



#堆
import heapq

# 创建一个空的堆
heap = []

# 向堆中添加元素
heapq.heappush(heap, 10)
heapq.heappush(heap, 1)
heapq.heappush(heap, 5)

# 输出堆中的最小元素
print("The smallest element is:", heap[0])

# 弹出最小元素
min_element = heapq.heappop(heap)
print("Removed the smallest element:", min_element)

# 获取前两个最小元素
two_smallest = heapq.nsmallest(2, heap)
print("The two smallest elements are:", two_smallest)

# 将列表转换为堆
lst = [9, 8, 7, 6, 5, 4, 3, 2, 1]
heapq.heapify(lst)
print("Heapified list:", lst)



#deque 队列
from collections import deque

# 创建一个空的 deque
d = deque()

# 创建一个包含初始元素的 deque
d = deque([1, 2, 3, 4])

# 创建一个固定大小的 deque
d = deque(maxlen=5)

from collections import deque

# 创建一个 deque
d = deque([1, 2, 3])

# 添加元素
d.append(4)
d.appendleft(0)
print(d)  # 输出: deque([0, 1, 2, 3, 4])

# 移除元素
d.pop()
d.popleft()
print(d)  # 输出: deque([1, 2, 3])

# 扩展 deque
d.extend([4, 5])
d.extendleft([-1, 0])
print(d)  # 输出: deque([0, -1, 1, 2, 3, 4, 5])

# 旋转
d.rotate(2)
print(d)  # 输出: deque([4, 5, 0, -1, 1, 2, 3])

# 固定大小的 deque
d = deque(maxlen=3)
d.append(1)
d.append(2)
d.append(3)
d.append(4)  # 1 被移除
print(d)  # 输出: deque([2, 3, 4], maxlen=3)







#归并排序法求最大逆序数
mininum=0
def mergesort(arr):
    global mininum
    if len(arr) > 1:
        mid = len(arr) // 2
        left = arr[:mid]
        right = arr[mid:]

        mergesort(left)
        mergesort(right)

        Lptr = Rptr = ptr = 0
        while len(left) > Lptr and len(right) > Rptr:
            if left[Lptr] <= right[Rptr]:
                arr[ptr] = left[Lptr]
                Lptr += 1
            else:
                arr[ptr] = right[Rptr]
                Rptr += 1
                mininum += len(left) - Lptr
            ptr += 1

        while len(left) > Lptr:
            arr[ptr] = left[Lptr]
            ptr += 1
            Lptr += 1
        while len(right) > Rptr:
            arr[ptr] = right[Rptr]
            ptr += 1
            Rptr += 1


n = int(input())
arr = list(map(int, input().split()))
mergesort(arr)
print(mininum)

#defaultdictdefaultdict 是 Python 标准库 collections 模块中的一个类，它继承自内置的字典类 dict。与普通的字典不同，defaultdict 在访问不存在的键时不会引发 KeyError 异常，而是自动创建一个新的默认值。这个默认值的类型是在创建 defaultdict 实例时指定的。

# 主要特点
# 自动创建默认值:当你试图访问或修改一个不存在的键时defaultdict会自动调用一个工厂函数来生成一个默认值
# 而不是抛出异常
# 工厂函数:在创建defaultdict时需要提供一个工厂函数
# 用于生成默认值.常见的工厂函数包括list,int,set等

from collections import defaultdict

# 使用 list 作为工厂函数
dd_list = defaultdict(list)

# 使用 int 作为工厂函数
dd_int = defaultdict(int)

# 使用 set 作为工厂函数
dd_set = defaultdict(set)

# 使用 list 作为工厂函数
dd_list['a'].append(1)
dd_list['a'].append(2)
dd_list['b'].append(3)
print(dd_list)  # 输出: defaultdict(<class 'list'>, {'a': [1, 2], 'b': [3]})

# 使用 int 作为工厂函数
dd_int['a'] += 1
dd_int['b'] += 2
print(dd_int)  # 输出: defaultdict(<class 'int'>, {'a': 1, 'b': 2})

# 使用 set 作为工厂函数
dd_set['a'].add(1)
dd_set['a'].add(2)
dd_set['b'].add(3)
print(dd_set)  # 输出: defaultdict(<class 'set'>, {'a': {1, 2}, 'b': {3}})

from collections import defaultdict

data = [('apple', 1), ('banana', 2), ('apple', 3), ('orange', 4), ('banana', 5)]
grouped_data = defaultdict(list)

for category, value in data:
    grouped_data[category].append(value)

print(grouped_data)  # 输出: defaultdict(<class 'list'>, {'apple': [1, 3], 'banana': [2, 5], 'orange': [4]})


#回溯 递归 dfs
def dfs(mx, visited, x, y):
# 如果到达右下⻆，返回True
    if x == n - 1 and y == n - 1:
        return True
    # 定义向右和向下的移动方向
    directions = [(0, 1), (1, 0)]
    for dx, dy in directions:
        nx = x + dx
        ny = y + dy
    # 检查新坐标是否在矩阵范围内，是否已经访问过，以及是否可以通过
        if 0 <= nx < n and 0 <= ny < n and not visited[nx][ny] and mx[nx][ny] == 0:
            visited[nx][ny] = True
            if dfs(mx, visited, nx, ny):
                return True
            visited[nx][ny] = False
    return False
# 读取输入
n = int(input())
mx = [list(map(int, input().split())) for _ in range(n)]
# 初始化访问标记数组
visited = [[False] * n for _ in range(n)]
# 起始点 (0, 0) 必须是可以通过的
if mx[0][0] == 1:

    print('No')
else:
   visited[0][0] = True
   if dfs(mx, visited, 0, 0):
       print('Yes')
   else:
       print('No')

#通过使用栈来模拟递归，可以避免因递归过深导致的栈溢出问题。

#bfs 广度优先搜索
from collections import deque
def bfs(s, e):
    inq = set()
    inq.add(s)
    q = deque()
    q.append((0, s))
    while q:
        now, top = q.popleft() # 取出队首元素
        if top == e:
            return now # 返回需要的结果，如：步⻓、路径等信息
# 将 top 的下一层结点中未曾入队的结点全部入队q，并加入集合inq设置为已入队
#示例：

from collections import deque
# Constants
MAXD = 4
dx = [0, 0, 1, -1]
dy = [1, -1, 0, 0]
def bfs(x, y):
    q = deque([(x, y)])
    inq_set.add((x,y))
    while q:
        front = q.popleft()
        for i in range(MAXD):
            next_x = front[0] + dx[i]
            next_y = front[1] + dy[i]
            if matrix[next_x][next_y] == 1 and (next_x,next_y) not in inq_set:
                inq_set.add((next_x, next_y))
                q.append((next_x, next_y))
# Input

#inq 数组，结点是否已入过队
n, m = map(int, input().split())
matrix=[[-1]*(m+2)]+[[-1]+list(map(int,input().split()))+[-1] for i in range(n)]+[[-1]*(m+2)]
inq_set = set()
# Main process
counter = 0
for i in range(1,n+1):
    for j in range(1,m+1):
        if matrix[i][j] == 1 and (i,j) not in inq_set:
            bfs(i, j)
            counter += 1
# Output
print(counter)

#二分法
L, n, m = map(int, input().split())
rock = [0]
for i in range(n):
    rock.append(int(input()))
rock.append(L)


def check(x):
    num = 0
    now = 0
    for i in range(1, n + 2):
        if rock[i] - now < x:
            num += 1
        else:
            now = rock[i]

    if num > m:
        return True
    else:
        return False


# https://github.com/python/cpython/blob/main/Lib/bisect.py
'''
2022fall-cs101，刘子鹏，元培。
源码的二分查找逻辑是给定一个可行的下界和不可行的上界，通过二分查找，将范围缩小同时保持下界可行而区间内上界不符合，
但这种最后print(lo-1)的写法的基础是最后夹出来一个不可行的上界，但其实L在这种情况下有可能是可行的
（考虑所有可以移除所有岩石的情况），所以我觉得应该将上界修改为不可能的 L+1 的逻辑才是正确。
例如：
25 5 5
1
2
3
4
5

应该输出 25
'''
# lo, hi = 0, L
lo, hi = 0, L + 1
ans = -1
while lo < hi:
    mid = (lo + hi) // 2

    if check(mid):
        hi = mid
    else:  # 返回False，有可能是num==m
        ans = mid  # 如果num==m, mid可能是答案
        lo = mid + 1

# print(lo-1)
print(ans)
```





在 Python 中，`bisect` 模块提供了一种保持列表有序的方式。这个模块的名字来源于“二分查找”（binary search）算法，它是一种在有序列表中查找特定元素的高效方法。`bisect` 模块中的函数可以帮助你在已经排序的列表中插入新元素，并保证列表仍然保持有序状态。

`bisect` 模块中最常用的几个函数包括：

- `bisect.bisect_left(a, x, lo=0, hi=len(a))`: 查找 `x` 应该插入的位置以保持列表 `a` 的顺序。如果 `x` 已经存在于列表中，那么该函数会返回最左边 `x` 的位置。`lo` 和 `hi` 参数用于指定搜索范围，默认为整个列表。
- `bisect.bisect_right(a, x, lo=0, hi=len(a))` 或者 `bisect.bisect(a, x, lo=0, hi=len(a))`: 类似于 `bisect_left`，但是当 `x` 存在于列表中时，该函数返回最右边 `x` 的下一个位置。
- `bisect.insort_left(a, x, lo=0, hi=len(a))`: 将 `x` 插入到列表 `a` 中，以保持 `a` 的顺序。如果 `x` 已经存在，则新的 `x` 会被插入到已存在的 `x` 之前。
- `bisect.insort_right(a, x, lo=0, hi=len(a))` 或者 `bisect.insort(a, x, lo=0, hi=len(a))`: 类似于 `insort_left`，但是当 `x` 存在时，新的 `x` 会被插入到已存在的 `x` 之后。

这些函数特别适用于需要维护一个动态有序列表的情况，例如优先队列等场景。使用 `bisect` 可以避免每次插入后都需要重新对整个列表进行排序，从而提高效率。





在 Python 中，深拷贝（deep copy）是指创建一个新的对象，并且递归地将原始对象中的所有对象都复制一份新的副本到新对象中。这意味着原始对象和拷贝对象完全独立，对一个对象的修改不会影响另一个对象。

要执行深拷贝，你可以使用 `copy` 模块中的 `deepcopy` 函数。下面是如何使用深拷贝的例子：

```python
import copy

# 示例1：列表的深拷贝
original_list = [[1, 2, 3], [4, 5, 6]]
copied_list = copy.deepcopy(original_list)

# 修改原始列表中的元素
original_list[0][0] = 'changed'

print("Original list:", original_list)  # 输出: Original list: [['changed', 2, 3], [4, 5, 6]]
print("Copied list:", copied_list)      # 输出: Copied list: [[1, 2, 3], [4, 5, 6]]

# 示例2：字典的深拷贝
original_dict = {'key1': [1, 2, 3], 'key2': [4, 5, 6]}
copied_dict = copy.deepcopy(original_dict)

# 修改原始字典中的元素
original_dict['key1'][0] = 'changed'

print("Original dict:", original_dict)  # 输出: Original dict: {'key1': ['changed', 2, 3], 'key2': [4, 5, 6]}
print("Copied dict:", copied_dict)      # 输出: Copied dict: {'key1': [1, 2, 3], 'key2': [4, 5, 6]}
```

在上面的例子中，`original_list` 和 `original_dict` 是原始的对象，而 `copied_list` 和 `copied_dict` 则是通过 `copy.deepcopy()` 创建的它们的深拷贝。当你修改原始对象时，你会发现深拷贝的对象保持不变，这证明了深拷贝确实创建了一个完全独立的新对象。

需要注意的是，对于包含大量数据结构的复杂对象，进行深拷贝可能会消耗较多的内存和时间。因此，在实际应用中，应根据具体需求权衡是否使用深拷贝。









`from functools import lru_cache` 和 `@lru_cache(maxsize=None)` 是Python中用于实现函数结果缓存的工具。它们特别适用于那些计算成本高且输入参数相同的情况下会重复调用的函数。下面是对这两个部分的具体解释：

### `functools.lru_cache`

- **LRU Cache**: LRU是"Least Recently Used"（最近最少使用）的缩写，它是一种缓存淘汰策略。当缓存满时，最久未被访问的数据会被移除。
- **作用**: `lru_cache` 装饰器可以用来自动缓存一个函数的结果。当带有相同参数的函数再次被调用时，将直接返回之前计算过的值，而不是重新执行函数体，从而提高性能。

### `@lru_cache(maxsize=None)`

- **装饰器**: `@lru_cache` 是一个装饰器，放在函数定义前，用于包裹该函数。
- **参数**:
  - `maxsize`: 指定缓存的最大条目数。如果设置为 `None`，则缓存没有大小限制。
  - 其他可选参数包括 `typed`，当设置为 `True` 时，不同类型的参数会被视为不同的键，例如 `f(3)` 和 `f(3.0)` 会被区分对待。

### 使用场景

- **递归函数**: 在处理递归问题时，比如斐波那契数列、动态规划等，`lru_cache` 可以显著减少重复计算，提高效率。
- **耗时计算**: 对于那些计算非常耗时但结果不会改变的函数，使用 `lru_cache` 可以存储结果，避免不必要的重复计算。
- **频繁调用**: 如果某个函数被频繁调用且输入参数经常重复，使用 `lru_cache` 可以大大加速程序运行速度。

### 示例

考虑一个简单的递归实现的斐波那契数列生成函数，不使用缓存和使用缓存的对比：

```python
# 不使用缓存
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# 使用缓存
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci_cached(n):
    if n < 2:
        return n
    return fibonacci_cached(n-1) + fibonacci_cached(n-2)

# 测试
print(fibonacci(30))  # 不使用缓存
print(fibonacci_cached(30))  # 使用缓存
```

在这个例子中，`fibonacci_cached` 函数通过 `@lru_cache` 装饰后，对于相同的 `n` 值，只会计算一次，并且后续的调用将直接从缓存中获取结果，极大地提高了效率。







### Dijkstra算法（迪杰斯特拉算法）是图论中的一种贪心算法，用于计算从一个起点到其他所有顶点的最短路径。它是由荷兰计算机科学家艾兹赫尔·迪杰斯特拉在1956年提出的，并于1959年发表。该算法主要解决的是带权重的有向图或无向图中的单源最短路径问题，这里的权重代表边的成本、距离或者时间等非负数值。

### 算法特点

- **适用范围**：适用于所有边权为非负数的图。
- **效率**：对于稀疏图来说，使用二叉堆优化后的Dijkstra算法的时间复杂度为 \(O((V + E) \log V)\)，其中 \(V\) 是顶点的数量，\(E\) 是边的数量。
- **贪心策略**：每次选择当前离起始节点最近且未访问过的节点进行扩展，保证了当一个节点被确定时，已经找到了到达它的最短路径。

### 算法步骤

1. **初始化**：设定起点的距离为0，其余所有节点的距离设为无穷大；创建一个集合来跟踪已处理过的节点。
2. **选择节点**：从未处理的节点中选出距离最小的一个作为当前节点。
3. **更新邻居**：检查当前节点的所有邻居，如果通过当前节点到达邻居的距离比之前记录的距离更短，则更新邻居的距离。
4. **标记节点**：将当前节点标记为已处理。
5. **重复**：重复步骤2至4，直到所有节点都被处理过或没有可更新的节点为止。

### 注意事项

- 如果图中含有负权重边，Dijkstra算法可能无法正确工作，此时可以考虑使用Bellman-Ford算法或者其他能够处理负权重边的算法。
- 在实际应用中，通常会用优先队列（如最小堆）来优化选择下一个节点的过程，从而提高算法的效率。

Dijkstra算法广泛应用于路由协议、网络设计、交通规划等领域，是一个非常重要的基础算法。







### `sys.stdin.read()` 是 Python 中用于从标准输入读取所有内容的方法。下面是如何使用它的基本示例以及一些注意事项：

### 基本用法

```python
import sys

# 从标准输入读取全部数据
input_data = sys.stdin.read()

# 打印读取的数据
print(input_data)
```

当你运行这段代码时，程序会等待你输入数据。在命令行中，你可以输入任何文本，然后通过发送 EOF（文件结束符）来结束输入。在 Unix/Linux 系统上，通常可以通过按下 `Ctrl+D` 来发送 EOF；在 Windows 上，则是通过 `Ctrl+Z` 后跟回车。

### 实际应用例子

假设你需要读取一个包含多行的文本，并且每行代表一个整数，你可以这样处理：

```python
import sys

# 从标准输入读取所有数据
input_data = sys.stdin.read()

# 将输入数据按行分割成列表
lines = input_data.strip().split('\n')

# 转换每一行为整数
numbers = [int(line) for line in lines]

# 打印转换后的数字列表
print(numbers)
```

在这个例子中，`strip()` 方法用于去除可能存在的末尾空白字符，包括换行符。`split('\n')` 方法将整个字符串按照换行符分割成多个子字符串，每个子字符串代表一行输入。最后，我们使用列表推导式将每一行转换为整数。

### 注意事项

- **内存限制**：如果输入非常大，可能会占用大量内存。在这种情况下，考虑使用 `sys.stdin.readline()` 或其他方法逐行处理。
- **错误处理**：如果你期望特定格式的输入，应该加入适当的错误处理逻辑来检查输入是否符合预期。
- **编码问题**：确保你的环境支持正确的字符编码，特别是当输入包含非ASCII字符时。可以使用 `sys.stdin.read().encode('utf-8')` 来指定编码。

### 示例：逐行读取并处理

如果你需要逐行处理输入，而不是一次性读取所有数据，可以这样做：

```python
import sys

for line in sys.stdin:
    # 处理每一行
    print(line.strip())  # 假设我们要打印每一行的内容
```

这种方法更加灵活，适合处理大型输入或流式输入。







#### `enumerate` 是 Python 中的一个内置函数，它允许你在遍历一个序列（如列表、元组或字符串等）时同时获取元素及其对应的索引位置。这对于需要基于索引进行操作的情况非常有用。

`enumerate` 的基本语法是：
```python
enumerate(iterable, start=0)
```
- `iterable` 是你想要遍历的序列。
- `start` 参数是可选的，用来指定索引开始的位置，默认值为 0。

`enumerate` 返回的是一个枚举对象，它是一个迭代器，每次迭代返回一个包含两个元素的元组：第一个元素是当前项的索引，第二个元素是该项本身。

这里有一个简单的例子来说明 `enumerate` 的用法：

```python
fruits = ['apple', 'banana', 'cherry']

# 使用 enumerate 遍历列表
for index, fruit in enumerate(fruits):
    print(f"Index: {index}, Fruit: {fruit}")

# 输出:
# Index: 0, Fruit: apple
# Index: 1, Fruit: banana
# Index: 2, Fruit: cherry

# 如果你想从索引 1 开始计数
for index, fruit in enumerate(fruits, start=1):
    print(f"Index: {index}, Fruit: {fruit}")

# 输出:
# Index: 1, Fruit: apple
# Index: 2, Fruit: banana
# Index: 3, Fruit: cherry
```

在这个例子中，`enumerate` 让我们可以很方便地访问到每个水果的名字以及它们在列表中的位置。这样就不需要手动维护一个单独的计数器变量了。









![img](https://broadscope-dialogue-new.oss-cn-beijing.aliyuncs.com/upload/20241119/25a27e822b0aef1d9254906ea7e8c9f5/f0fd4e3c05274a44a59f23e25b5169fd/image.png?Expires=1795103392&OSSAccessKeyId=LTAI5tL97mBYzVcjkG1cUyin&Signature=AJuqLlR%2FDSN9qgyqVAEGtZpZScw%3D)

这是一道经典的二分查找问题。我们需要找到最大的最小距离，使得C头牛可以被分配到N个摊位中。

### 解决方案

1. **输入处理**：
   - 读取第一行的两个整数 N 和 C。
   - 读取接下来 N 行，每行一个整数 xi，表示每个摊位的位置。

2. **排序**：
   - 将所有摊位位置按照从小到大的顺序进行排序。

3. **二分查找**：
   - 初始化左边界 left = 0，右边界 right = 最大摊位位置。
   - 在每次循环中，计算中间值 mid = (left + right) / 2。
   - 检查是否可以在当前最小距离为 mid 的情况下将 C 头牛分配到摊位上。
     - 如果可以，则更新答案，并尝试更大的距离（right = mid）。
     - 否则，尝试更小的距离（left = mid + 1）。

4. **检查函数**：
   - 定义一个 check 函数，用于判断在给定的最小距离 d 下，是否可以将 C 头牛分配到摊位上。
     - 遍历摊位位置，记录最后一个放置牛的位置 last_pos。
     - 对于每个摊位位置 x，如果 x - last_pos >= d，则可以在此处放置一头牛，并更新 last_pos。
     - 如果最后放置的牛的数量大于等于 C，则返回 true；否则返回 false。

5. **输出结果**：
   - 输出最大最小距离。

### 示例代码（Python）

```python
def check(d, stalls, c):
    last_pos = stalls[0]
    count = 1
    for i in range(1, len(stalls)):
        if stalls[i] - last_pos >= d:
            count += 1
            last_pos = stalls[i]
    return count >= c


def aggressive_cows(n, c, stalls):
    stalls.sort()
    left, right = 0, max(stalls)
    while left < right:
        mid = (left + right) // 2
        if check(mid, stalls, c):
            left = mid + 1
        else:
            right = mid
    return left - 1


# 输入处理
n, c = map(int, input().split())
stalls = [int(input()) for _ in range(n)]

# 计算并输出结果
print(aggressive_cows(n, c, stalls))
```

这个解决方案使用了二分查找来优化搜索过程，确保在 O(N log M) 时间内找到解，其中 N 是摊位数量，M 是最大可能的最小距离。









###  使用 `filter()` 函数过滤一次性过滤所有某个元素

`filter()` 函数可以用来过滤掉不需要的元素，它返回一个迭代器，可以将其转换为列表。



```python
# 原始列表
my_list = [1, 2, ¼, 4, 5, ¼, 7, ¼]

# 要删除的元素
element_to_remove = ¼

# 使用 filter() 函数
new_list = list(filter(lambda x: x != element_to_remove, my_list))

print(new_list)  # 输出: [1, 2, 4, 5, 7]
```





### `sorted()` 是 Python 内置的一个函数，用于对可迭代对象（如列表、元组、字符串等）进行排序，并返回一个新的已排序列表。原始的可迭代对象不会被改变。`sorted()` 函数可以接受三个参数：

1. `iterable`：需要排序的可迭代对象。
2. `key` (可选)：一个函数，它用来从每个元素中提取出用于比较的关键值。默认情况下，这个函数是 `None`，意味着直接比较元素本身。
3. `reverse` (可选)：一个布尔值。如果设置为 `True`，则列表将按降序排列；如果设置为 `False` 或者省略，则按升序排列。

### 基本用法
```python
# 对列表进行排序
numbers = [5, 2, 9, 1, 5, 6]
sorted_numbers = sorted(numbers)
print(sorted_numbers)  # 输出: [1, 2, 5, 5, 6, 9]

# 对字符串列表进行排序
words = ["banana", "apple", "cherry"]
sorted_words = sorted(words)
print(sorted_words)  # 输出: ['apple', 'banana', 'cherry']
```

### 使用 key 参数
`key` 参数允许你指定一个函数来确定排序规则。例如，你可以根据字符串长度或特定属性进行排序。

```python
# 根据字符串长度排序
words = ["banana", "apple", "cherry"]
sorted_by_length = sorted(words, key=len)
print(sorted_by_length)  # 输出: ['apple', 'banana', 'cherry']

# 自定义排序键
def second_letter(word):
    return word[1] if len(word) > 1 else ''

sorted_by_second_letter = sorted(words, key=second_letter)
print(sorted_by_second_letter)  # 输出: ['banana', 'apple', 'cherry']
```

### 反向排序
使用 `reverse=True` 可以让排序结果反转。

```python
# 降序排序
numbers = [5, 2, 9, 1, 5, 6]
sorted_descending = sorted(numbers, reverse=True)
print(sorted_descending)  # 输出: [9, 6, 5, 5, 2, 1]
```

### 复杂数据结构的排序
对于包含复杂数据结构（如字典）的列表，也可以通过 `key` 参数来进行排序。

```python
# 按字典中的某个键值排序
people = [{'name': 'Alice', 'age': 25}, {'name': 'Bob', 'age': 19}, {'name': 'Charlie', 'age': 30}]
sorted_people = sorted(people, key=lambda person: person['age'])
print(sorted_people)
# 输出: [{'name': 'Bob', 'age': 19}, {'name': 'Alice', 'age': 25}, {'name': 'Charlie', 'age': 30}]
```

`sorted()` 函数非常强大且灵活，能够满足多种不同的排序需求。







### Python中的`find()`函数用于查找指定子字符串在父字符串中第一次出现的位置。如果未找到子字符串，则返回-1。`find()`方法不会改变原字符串，而是返回一个新的字符串。

语法格式如下：
```python
string.find(sub[, start[, end]])
```

其中，
- `string`：要查找的原始字符串。
- `sub`：要在原始字符串中查找的子字符串。
- `start`（可选）：表示开始查找位置的索引，默认为0。
- `end`（可选）：表示结束查找位置的索引，默认为字符串长度。

例如：
```python
s = "Hello, World!"
print(s.find("World"))  # 输出：6
print(s.find("Hello"))  # 输出：0
print(s.find("Not found"))  # 输出：-1
```

如果你需要查找多个子字符串，你可以使用`in`关键字或者多次调用`find()`函数。例如：
```python
s = "Hello, World!"
print("World" in s)  # 输出：True
print("Hello" in s)  # 输出：True
print("Not found" in s)  # 输出：False
```

或者：
```python
s = "Hello, World!"
print(s.find("World"))
print(s.find("Hello"))
print(s.find("Not found"))
```

注意，`find()`函数只返回第一个匹配项的位置。如果你想找到所有的匹配项，你需要自己编写循环来实现。









### 在Python中实现二分查找，通常假定你有一个已经排序好的列表。二分查找的基本思想是每次将查找范围缩小一半，从而快速定位目标值。下面是一个简单的递归和非递归（迭代）版本的二分查找实现。

### 非递归（迭代）版本

```python
def binary_search_iterative(arr, target):
    low, high = 0, len(arr) - 1
    
    while low <= high:
        mid = (low + high) // 2
        guess = arr[mid]
        
        if guess == target:
            return mid  # 返回索引位置
        if guess > target:
            high = mid - 1
        else:
            low = mid + 1
            
    return None  # 如果没有找到目标值，则返回None
```

### 递归版本

```python
def binary_search_recursive(arr, target, low, high):
    if low > high:
        return None  # 没有找到目标值

    mid = (low + high) // 2
    guess = arr[mid]

    if guess == target:
        return mid  # 找到目标值，返回索引位置
    elif guess > target:
        return binary_search_recursive(arr, target, low, mid - 1)
    else:
        return binary_search_recursive(arr, target, mid + 1, high)
```

使用递归版本时，你需要提供初始的`low`和`high`参数，它们分别是数组的第一个和最后一个索引：

```python
# 假设我们有一个排序好的数组arr和要查找的目标值target
result = binary_search_recursive(arr, target, 0, len(arr) - 1)
```

请记住，二分查找只能应用于已经排序的数据。如果数据未排序，则需要先进行排序操作，或者使用其他类型的搜索算法。此外，上述代码假设列表中的元素都是唯一的；如果有重复元素，并且想要找到所有匹配项，那么可能需要调整逻辑来处理这种情况。





### 在Python中保留小数位数，你可以使用几种不同的方法。以下是几种常用的方法：

### 1. 使用`round()`函数

`round()`函数可以用来四舍五入到指定的小数位数。

```python
number = 123.456789
rounded_number = round(number, 2)  # 保留两位小数
print(rounded_number)  # 输出: 123.46
```

### 2. 使用格式化字符串（`format`函数或f-string）

- **`format`函数**：
  
  ```python
  number = 123.456789
  formatted_number = "{:.2f}".format(number)  # 保留两位小数
  print(formatted_number)  # 输出: 123.46
  ```

- **f-string (从 Python 3.6 开始支持)**：

  ```python
  number = 123.456789
  formatted_number = f"{number:.2f}"  # 保留两位小数
  print(formatted_number)  # 输出: 123.46
  ```

### 3. 使用`decimal`模块

如果你需要更精确的控制，例如进行金融计算时，可以使用`decimal`模块，它允许你设置全局或局部的精度，并且提供了多种舍入选项。

```python
from decimal import Decimal, getcontext

getcontext().prec = 5  # 设置全局精度为5位有效数字
decimal_number = Decimal('123.456789')
formatted_decimal = round(decimal_number, 2)  # 保留两位小数
print(formatted_decimal)  # 输出: 123.46
```

请注意，在使用`decimal`模块时，直接设置的是有效数字的数量，但通过`round()`函数，你可以明确地指定要保留的小数位数。

输出：
法一：f"{value:.nf}" #其中n是希望保留的小数位数
百分数
print(f"Percentage: {percentage:.2%}") # 输出：85.00%
科学计数法：
print(f"Scientific notation: {large_number:.2e}") # 输出：1.23e+06





![image-20241224152211670](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241224152211670.png)

## 获取字符的 ASCII 码——ord()
ascii_value = ord('A')
print(ascii_value) # 输出：65
## 获取 ASCII 码对应的字符——chr()
char = chr(65)
print(char) # 输出：A



## 使用Python内置库itertools

Python的`itertools`模块提供了一个名为`permutations`的函数，可以非常方便地生成全排列。

```python
from itertools import permutations

def generate_permutations(elements):
    return list(permutations(elements))

# 示例使用
elements = [1, 2, 3]
print(generate_permutations(elements))
```