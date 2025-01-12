# Assign #3: Oct Mock Exam暨选做题目满百

Updated 1537 GMT+8 Oct 10, 2024

2024 fall, Complied by Hongfei Yan==（请改为同学的姓名、院系）==朱玺谕 工学院



**说明：**

1）Oct⽉考： AC6==（请改为同学的通过数）==4 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++/C（已经在Codeforces/Openjudge上AC），截图（包含Accepted, 学号），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、作业评论有md或者doc。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E28674:《黑神话：悟空》之加密

http://cs101.openjudge.cn/practice/28674/



思路：由于输入的字符串中既有大写字母又有小写字母，遍历字符串时分为两类进行操作，分别设出含有所有小写和大写字母的两个列表，通过对新字母下标-k再模26得出原字母下标，再将得到的原字母插入空列表中，最后用join方法合成原密码字符串

用时约15min

代码

```python
k=int(input())
list_letter_s=['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z']
list_letter_b=['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z']
message=input()
answer=[]
for i in message:
    if i in list_letter_s:
        j=(list_letter_s.index(i)-k)%26
        answer.append(list_letter_s[j])
    if i in list_letter_b:
        j=(list_letter_b.index(i)-k)%26
        answer.append(list_letter_b[j])

answer_str=''.join(answer)
print(answer_str)

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241011131336640](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241011131336640.png)



### E28691: 字符串中的整数求和

http://cs101.openjudge.cn/practice/28691/



思路：先用map()函数将同行输入的两个内容用两个变量转化为字符串后分别接收，再分别读取前两个字符（即数字部分）类型转换为整数后相加得到结果

用时约4min

代码

```python
a,b=map(str,input().split())
sum_=int(a[0:2])+int(b[0:2])
print(sum_)

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241011131429943](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241011131429943.png)



### M28664: 验证身份证号

http://cs101.openjudge.cn/practice/28664/



思路：预先将各位的系数和最终得到的余数对应的第18位数字设成两个列表便于后续计算，将输入的字符串遍历，并将各位数字存储到字典中便于在循环中索引出来进行运算，注意若第十八位数为'X'则无法用int()函数，所以在最后进行分类处理

由于反复试错次数较多，用时较长，约35分钟

代码

```python
cases=int(input())
dict_={}

list_=[7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2]
list_last=[1,0,'X',9,8,7,6,5,4,3,2]
for i in range(cases):
    m=1
    sum_ = 0
    str_=input()
    for j in str_:
        dict_[m]=j
        if m<=17:
            sum_+=list_[m-1]*int(dict_[m])
        m+=1
    left_=sum_%11
    last_num=list_last[left_]
    if dict_[18]=='X':
        if last_num==dict_[18]:
            print('YES')
        else:
            print('NO')
    else:
        if last_num==int(dict_[18]):
            print('YES')
        else:
            print('NO')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241011131643158](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241011131643158.png)



### M28678: 角谷猜想

http://cs101.openjudge.cn/practice/28678/



思路：代码主体为一个while循环嵌套if else语句，每次循环结束前将m赋值给n

用时约5min

代码

```python
n=int(input())
while n!=1:
    if n%2==1:
        m=3*n+1
        print(f'{n}*3+1={m}')
    else:
        m=int(n/2)
        print(f'{n}/2={m}')
    n=m
print('End')

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241011131544589](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241011131544589.png)



### M28700: 罗马数字与整数的转换

http://cs101.openjudge.cn/practice/28700/



思路：此题可以分为”罗马数字转为整数“和”整数转为罗马数字“两个部分，用try except ValueError实现对输入内容的分类，对两个部分分别定义一个函数，前者主要用逐个翻译字母求和实现转换，后者则是按由大到小的顺序向列表中存储若干个字符，最后用join()方法连接成字符串输出

考场上未能完成，下考后仍是反复试错，用时大概1h

##### 代码

```python
# 
def f(n):
    n=int(n)
    list_=[]

    num_M=n//1000

    num_C=(n%500)//100

    num_X=(n%50)//10

    num_I=n%5

    for i in range(num_M):
        list_.append('M')

    if (n%1000)//100==9:
        list_.append('CM')
    elif (n%1000)//100==4:
        list_.append('CD')
    else:
        if (n%1000)//100>=5:
            list_.append('D')
        for i in range(num_C):
            list_.append('C')

    if (n%100)//10==9:
        list_.append('XC')
    elif (n%100)//10==4:
        list_.append('XL')
    else:
        if (n%100)//10>=5:
            list_.append('L')
        for i in range(num_X):
            list_.append('X')

    if n%10==9:
        list_.append('IX')
    elif n%10==4:
        list_.append('IV')
    else:
        if n%10>=5:
            list_.append('V')
        for i in range(num_I):
            list_.append('I')

    str_n=''.join(list_)
    print(str_n)

def g(n):
    sum_=0
    num_M=n.count('M')
    sum_+=num_M*1000
    num_D=n.count('D')
    sum_+= num_D*500
    num_C=n.count('C')
    sum_+= num_C*100
    num_L=n.count('L')
    sum_+=num_L*50
    num_X=n.count('X')
    sum_+= num_X*10
    num_V=n.count('V')
    sum_+=num_V*5
    num_I=n.count('I')
    sum_+=num_I*1
    num_CM=n.count('CM')
    sum_-=200*num_CM
    num_CD=n.count('CD')
    sum_-=200*num_CD
    num_XC=n.count('XC')
    sum_-=20*num_XC
    num_XL=n.count('XL')
    sum_-=20*num_XL
    num_IX=n.count('IX')
    sum_-=2*num_IX
    num_IV=n.count('IV')
    sum_-=2*num_IV

    print(sum_)
n=input()
try:
    f(n)
except ValueError:
    g(n)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241011131222144](C:\Users\32786\AppData\Roaming\Typora\typora-user-images\image-20241011131222144.png)



### *T25353: 排队 （选做）





思路：



代码

```python


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

从9.30才完成基本语法的学习开始做题，国庆期间做了较多的每日选做的训练，目前做了大概有一半的题，有时候一道题会卡很久做不出来（比如装箱问题到现在还没解决），有时候能一天做五六道，因此赶进度速度不稳定，收假后受到其他课程任务的影响，做题时间变少了。不过在近期做题的过程中感觉收获很多，进步很快，月考中也是AC了4道，个人比较满意了，希望能保持这样的劲头，加快进步脚步！









