# Assignment #7: Nov Mock Exam立冬

Updated 1646 GMT+8 Nov 7, 2024

2024 fall, Complied by <mark>同学的姓名、院系</mark>朱玺谕 工学院



**说明：**

1）⽉考： AC6<mark>（请改为同学的通过数）</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E07618: 病人排队

sorttings, http://cs101.openjudge.cn/practice/07618/

思路：

将病人的ID，年龄，排队顺序作为三个变量存入列表中，用自定义函数排列，是年龄大的优先，且排在前面的优先

代码：

```python
n=int(input())
list_=[0]*n
answer_list=[]
list_young=[]
for i in range(n):
    list_[i]=list(map(str,input().split()))
    list_[i].append(i)
list_.sort(key=lambda x:int(x[1])-0.001*int(x[2]),reverse=True)
for x in list_:
    if int(x[1])>=60:
        answer_list.append(x[0])
    else:
        list_young.append(x)
list_young.sort(key=lambda x:x[2])
for x in list_young:
    answer_list.append(x[0])
for x in answer_list:
    print(x)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241107220258846](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241107220258846.png)



### E23555: 节省存储的矩阵乘法

implementation, matrices, http://cs101.openjudge.cn/practice/23555/

思路：

先将输入的数据转化为矩阵中的信息，再进行矩阵乘法，最后在还原为原来的信息格式输出

代码：

```python
n,m1,m2=map(int,input().split())
list_x=[list(map(int,input().split())) for i in range(m1)]
list_y=[list(map(int,input().split())) for i in range(m2)]
matrix_x=[[0 for _ in range(n)] for _ in range(n)]
matrix_y=[[0 for _ in range(n)] for _ in range(n)]
for x in list_x:
    matrix_x[x[0]][x[1]]=x[2]
for x in list_y:
    matrix_y[x[0]][x[1]]=x[2]
matrix_answer=[[0 for _ in range(n)] for _ in range(n)]
for i in range(n):
    for k in range(n):
        a=0
        for j in range(n):
            a+=matrix_x[i][j]*matrix_y[j][k]
        matrix_answer[i][k]=a
answer_list=[]
for i in range(n):
    for j in range(n):
        if matrix_answer[i][j]!=0:
            answer_list.append([i,j,matrix_answer[i][j]])
for x in answer_list:
    x=list(map(str,x))
    print(' '.join(x))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241107220552715](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241107220552715.png)



### M18182: 打怪兽 

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/

思路：

主体思路就是先用扣血量大的技能，难点在于如何判断同一时间点技能使用次数，我用了一个字典，把使用次数作为value用时间点作为key索引，最后得到结果

代码：

```python
cases=int(input())
for i in range(cases):
    n,m,b=map(int,input().split())
    skills=[list(map(int,input().split())) for _ in range(n)]
    skills.sort(key=lambda x:x[0]-10**(-9)*x[1])
    used_times={a[0]:0 for a in skills}

    for i in range(n):
         if used_times[skills[i][0]]<m:
            if b<=skills[i][1]:
                print(skills[i][0])
                b-=skills[i][1]
                break
            else:
                b-=skills[i][1]
                used_times[skills[i][0]]+=1
    if b>0:
        print('alive')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241107220754179](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241107220754179.png)



### M28780: 零钱兑换3

dp, http://cs101.openjudge.cn/practice/28780/

思路：

这道题考试的时候一直卡着，当时用的是

```python
import math
n,m=map(int,input().split())
coins=[int(x) for x in input().split()]
coins.sort()
max_num=math.ceil(m/coins[-1])
list_=coins
num_=-1
if m in coins:
    num_=1
for i in range(1,max_num):
    list__=[]
    if num_!=-1:
        break
    for x in coins:
       for k in list_:
           if x+k==m:
               num_=i+1
           elif x+k<m:
               list__.append(x+k)

           else:
               break
    list_=list__.copy()
print(num_)
```

这种思路，模仿0-1背包，让硬币数逐渐增加，但是一直报超内存，下考后问了一下ai，截然相反地用了对钱数列表的方法，的确减小了内存，最后AC了。

代码：

```python
import math

def min_coins(coins, m):
    dp = [float('inf')] * (m + 1)
    dp[0] = 0

    for coin in coins:
        for x in range(coin, m + 1):
            dp[x] = min(dp[x], dp[x - coin] + 1)

    return dp[m] if dp[m] != float('inf') else -1


n, m = map(int, input().split())
coins = [int(x) for x in input().split()]

result = min_coins(coins, m)
print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241107214813166](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241107214813166.png)



### T12757: 阿尔法星人翻译官

implementation, http://cs101.openjudge.cn/practice/12757

思路：

这道题一开始感觉思路很顺，很快就把代码写出来了，但是提交上去不是WA就是RE，debug花了很长时间，最后打了很多补丁才AC的。思路就是把输入的字符从million和thousand处切开，一部分一部分地处理

代码：

```python
language_dict={'negative':-1, 'zero':0, 'one':1, 'two':2, 'three':3, 'four':4, 'five':5, 'six':6, 'seven':7, 'eight':8, 'nine':9, 'ten':10, 'eleven':11, 'twelve':12, 'thirteen':13, 'fourteen':14, 'fifteen':15, 'sixteen':16, 'seventeen':17, 'eighteen':18, 'nineteen':19, 'twenty':20, 'thirty':30, 'forty':40, 'fifty':50, 'sixty':60, 'seventy':70, 'eighty':80, 'ninety':90, 'hundred':100, 'thousand':1000, 'million':1000000}
def word_to_num(word):
    num=0
    ori_word=word.copy()
    if word==[]:
        return [0,0]
    else:
        if word[0]=='negative':
            word.remove('negative')
        if 'hundred' in word:
            hundred_index=word.index('hundred')
            for x in range(len(word)):
                if x<hundred_index:
                    num+=language_dict[word[x]]*100
                elif x>hundred_index:
                    num+=language_dict[word[x]]
        else:
            for x in range(len(word)):
                num+=language_dict[word[x]]
        return [num,ori_word[0]]
str_=input()
if 'thousand' in str_:
    list1=str_.split('thousand')
    if 'million' in str_:
         list2=list1[0].split('million')
         num=word_to_num(list2[0].split())[0]*1000000+word_to_num(list2[1].split())[0]*1000+word_to_num(list1[1].split())[0]
         if word_to_num(list2[0].split())[1]=='negative':
             num=-num
    else:
        num = word_to_num(list1[0].split())[0]*1000+word_to_num(list1[1].split())[0]
        if word_to_num(list1[0].split())[1]=='negative':
             num=-num
elif 'million' in str_ and 'thousand' not in str_:
    list0=str_.split('million')
    num=word_to_num(list0[0].split())[0]*1000000+word_to_num(list0[1].split())[0]
    if word_to_num(list0[0].split())[1]=='negative':
        num=-num
else:
    num=word_to_num(str_.split())[0]
    if word_to_num(str_.split())[1]=='negative':
        num=-num
print(num)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241109111404151](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241109111404151.png)



### T16528: 充实的寒假生活

greedy/dp, cs10117 Final Exam, http://cs101.openjudge.cn/practice/16528/

思路：

考场上没来得及看这道题，下来发现这道题反而很快的地做出来了，思路就是将活动按结束顺序升序排列，创建一个截至时间列表和活动数列表，分别在遍历过程中记录该起始活动条件下的不冲突活动链的结束时间和目前链中的活动个数。

代码：

```python
n=int(input())
end_list=[0]*n
num_list=[1]*n
act_list=[list(map(int,input().split())) for _ in range(n)]
act_list.sort(key=lambda x:x[1]+0.01*x[0])
for i in range(n):
    end_list[i]=act_list[i][1]
    for j in range(i,n):
        if act_list[j][0]>end_list[i]:
            end_list[i]=act_list[j][1]
            num_list[i]+=1
print(max(num_list))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241109095843039](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241109095843039.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

这次月考发挥的并不好，只AC了两道，第四道一直超内存，卡了很久，所以后两道题压根就没看，下考后才发现后两道题其实都不难，尤其是第六道很快就做出来了，下一次在考试的过程中还是要注意统筹安排，尽可能多AC，不要死咬不放。

由于近期是期中考试，没有做每日选做，感觉对dp和递归的掌握不是很好，下周二数分考完后计划好好补一下这块的内容，每日选做也继续努力跟上。



