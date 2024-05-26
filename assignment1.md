# Assignment #1: 拉齐大家Python水平

Updated 0940 GMT+8 Feb 19, 2024

2024 spring, Complied by == ==



**说明：**

1）数算课程的先修课是计概，由于计概学习中可能使用了不同的编程语言，而数算课程要求Python语言，因此第一周作业练习Python编程。如果有同学坚持使用C/C++，也可以，但是建议也要会Python语言。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Win11

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/



思路：



##### 代码

```python
dp=[0 for i in range(32)]
dp[1]=1;dp[2]=1
for i in range(3,31):
    dp[i]=dp[i-1]+dp[i-2]+dp[i-3]

n=int(input())
print(dp[n])

```



代码运行截图 ==（至少包含有"Accepted"）==

![1709345946907](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709345946907.png)



### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A



思路：



##### 代码

```python
# -*- coding: utf-8 -*-
"""
Created on Tue Sep 26 15:56:50 2023

@author: Liuzhiyuan PKUCoE 2300011042

"""

s=list(input())
ans="NO"
for i in range(len(s)):
    if s[i]=="h":
        t=i
        for i in range(t,len(s)):
            if s[i]=="e":
                t=i
                for i in range(t,len(s)):
                    if s[i]=="l":
                        t=i
                        for i in range(t+1,len(s)):
                            if s[i]=="l":
                                t=i
                                for i in range(t,len(s)):
                                    if s[i]=="o":
                                        ans="YES"







print(ans)
    
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240302102050078](C:\Users\31861\AppData\Roaming\Typora\typora-user-images\image-20240302102050078.png)



### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A



思路：



##### 代码

```python
# -*- coding: utf-8 -*-
"""
Created on Thu Sep 28 15:55:09 2023

@author: CCLAB
"""

words=list(input())
word3=[]
word2=[]
for _ in words:
    word3.append(_.lower())

for _ in word3:
    if _ =='a'or  _ =='e'or  _ =='i'or  _ =='o'or  _ =='u' or _=="y":
        continue
    else:
        word2.append(".")
        word2.append(_)
print("".join(word2))        

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709346066954](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709346066954.png)



### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/



思路：



##### 代码

```python
prime=[]
def Euler_sieve(n):                                       
    primes = [True for _ in range(n+1)]
    p = 2
    while p*p <= n:
        if primes[p]:
            for i in range(p*p, n+1, p):
                primes[i] = False
        p+=1
    return primes
lst=(Euler_sieve(5005))
for i in range(5004):
    if lst[i]:
        prime.append(i)
        
ab=int(input())
for i in prime:
    if ab-i in prime:
        print(i,ab-i)
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709346101748](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709346101748.png)



### 23563: 多项式时间复杂度

http://cs101.openjudge.cn/practice/23563/



思路：



##### 代码

```python
s=input().split("+")
maxn=0
for i in s:
    if i[0]!="0":
        x,y=i.split("^")
        if int(y)>maxn:
            maxn=int(y)
ans="n^"+str(maxn)
print(ans)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709346118068](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709346118068.png)



### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/



思路：



##### 代码

```python
l=list(map(int,input().split()))
ll={1:0}
for i in l:
    if i in ll.keys():
        ll[i]+=1
    else:
        ll[i]=1

ans=sorted(ll,key=ll.get)
anss=[ans[-1]]
for i in range(len(ans)-1,1,-1):
    if ll[ans[i]]==ll[ans[i-1]]:
        anss.append(ans[i-1])
    else:
        break
print(" ".join(str(num) for num in sorted(anss)))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709346132637](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709346132637.png)



## 2. 学习总结和收获

好多题目上学期都写过了，但是再练练手也是避免生疏的好方式





