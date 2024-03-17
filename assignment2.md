# Assignment #2: 编程练习

Updated 0953 GMT+8 Feb 24, 2024

2024 spring, Complied by ==刘致远 工学院==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：WIn11

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/2024sp_routine/27653/



思路：



##### 代码

```python
import math
a,b,c,d=map(int,input().split())
e=b*c+a*d
f=b*d
k=100
while k>1:
    k=math.gcd(e,f)
    e=e//k
    f=f//k
   
    
print(str(e)+"/"+str(f))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1709601564874](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709601564874.png)



### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110



思路：



##### 代码

```python
l=[];ans=0
n,p=map(int,input().split())
for i in range(n):
    v,w=map(int,input().split())
    for j in range(w):
        l.append(v/w)
l=sorted(l,reverse=True)
for i in range(min(len(l),p)):
    ans+=l[i]
print("{:.1f}".format(ans))

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240302110654342](C:\Users\31861\AppData\Roaming\Typora\typora-user-images\image-20240302110654342.png)



### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/



思路：



##### 代码

```python
ncase=int(input())
for _ in range(ncase):
    n,m,b=map(int,input().split())
    jn=[];ll=dict();h=0
    for i in range(n):
        t,x=map(int,input().split())
        jn.append([t,x])
        if t in ll.keys(): 
            ll[t].append(x)
            if len(ll[t])>m:
                ll[t].remove(min(ll[t]))
        else:
            ll[t]=[x]
    for i in sorted(ll.keys()):
        b-=sum(ll[i])
        if b<=0:
           print(i)
           h=1
           break 
    if h==0:
        print("alive")

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709601592159](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709601592159.png)



### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B



思路：



##### 代码

```python
# -*- coding: utf-8 -*-
"""
Created on Mon Mar  4 18:11:10 2024

@author: 31861
"""

                                 
primes = [True for _ in range(10**6 + 5)]
primes[1]=False
p = 2
while p*p <= 10**6:
    if primes[p]:
        for i in range(p*p, 10**6 +1, p):
            primes[i] = False
    p += 1

ll=set()
for i in range(10**6):
    if primes[i]:
        ll.add(i*i)

n=int(input())
l=list(map(int,input().split()))
for i in range(n):
    x=l[i]
    if x in ll:
        print("YES")
    else:
        print("NO")

    
 
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709601624819](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709601624819.png)



### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A



思路：



##### 代码

```python
t=int(input())
for i in range(t):
    n,x=map(int,input().split())
    l=list(map(int,input().split()))
    ans=len(l)
    if sum(l)%x !=0:
        print(ans)
    else:
        p=-111
        for ii in range(len(l)):
            if l[ii]%x!=0:
                p=ii
                break
        l.reverse()
        for ii in range(len(l)):
            if l[ii]%x!=0:
                if ii<p:
                    p=ii
                    break
        if p!=-111 :
            print(ans-1-p)
        else:
            print(-1)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709601648224](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709601648224.png)



### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/



思路：



##### 代码

```python
dp=[True for i in range(10003)]
dp[0]=dp[1]=False

for i in range(2,10003):
    for j in range(2*i,10003,i):
        dp[j]=False
tp=[]
for i in range(10003):
    if dp[i]:
        tp.append(i*i)
tp=set(tp)
m,n=map(int,input().split())
for _ in range(m):
    l=list(map(int,input().split()))
    total=num=0
    for i in l:
        if i in tp:
            total+=i
            num+=1
    if total==0 :
        print(0)
    else:
        print("{:.2f}".format(total/len(l)))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709603048753](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709603048753.png)



## 2. 学习总结和收获

希望以后能跟上每日选做





