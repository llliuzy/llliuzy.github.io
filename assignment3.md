# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024

2024 spring, Complied by ====



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/



思路：



##### 代码

```python
k=int(input())
l=list(map(int,input().split()))
dp=[0]*k
dp[0]=1
for i in range(k):
    for j in range(i):
        if l[i]<=l[j]:
            dp[i]=max(dp[i],dp[j]+1)
    if dp[i]==0:
        dp[i]=1

print(max(dp))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1709827647515](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709827647515.png)



**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147



思路：



##### 代码

```python
n,a,b,c=input().split()
n=int(n)
p=n

def tomove(x,now,to):
    print(f"{x}:{now}->{to}")

def move(n,now,by,to):
    for i in [a,b,c]:
        if i!=now and i!=to:
            by=i   
    if n==1:
        tomove(1,now,to)
    else:
        move(n-1,now,to,by)
        tomove(n,now,to)
        move(n-1,by,now,to)
       
move(n,a,b,c)

```



代码运行截图 ==（至少包含有"Accepted"）==

![1709827885114](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709827885114.png)



**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253



思路：



##### 代码

```python
while True : 
    n,p,m=map(int,input().split())
    if n==m==p==0:
        break 
    else:
        l=[];ll=[]
        for i in range(n):
            l.append(i+1)
        num=p-1
        x=0
        while len(ll)<n:
            if l[num]!=0:
                x+=1              
                if x%m==0:
                    ll.append(str(l[num]))
                    l[num]=0                            
            num+=1
            if num==n:
                num-=n
            
                
        print(",".join(ll))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709827694318](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709827694318.png)



**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554



思路：



##### 代码

```python
n=int(input())
l=list(map(int,input().split()))
ll=sorted(l)
lll=[]
for i in ll:  
    for j in range(n):
        if i==l[j] and str(j+1) not in lll:
            lll.append(str(j+1))
            break
print(" ".join(lll))

k=n-1;ans=0
for i in ll:
    ans+=k*i
    k-=1
print("{:.2f}".format(ans/n))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709827746024](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709827746024.png)



**19963:买学区房**

http://cs101.openjudge.cn/practice/19963



思路：



##### 代码

```python
n=int(input())
if n%2==1:
    zws=n//2 
else:
    zws=0

num=0
l2=[]

l1=list(input().split())
for i in l1:
   p,q=i.split(",")
   p=p.replace("(","")
   q=q.replace(")","")
   j=int(p)+int(q)
   l2.append(j)


l3=list(map(int,input().split()))
jg=sorted(l3)
if zws:
    jgzws=jg[zws] 
else:
    jgzws=(jg[n//2]+jg[n//2 -1])/2
xjb=[]
for i in range(n):
    xjb.append(l2[i]/l3[i])
xjb1=sorted(xjb)
if zws:
    xjbzws=xjb1[zws]
else:
    xjbzws=(xjb1[n//2]+xjb1[n//2 -1])/2

for i in range(n): 
    if xjb[i] >xjbzws and l3[i]<jgzws:
        num+=1
print(num)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709827783838](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709827783838.png)



**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300



思路：



##### 代码

```python
l=dict()
n=int(input())
for i in range(n):
    name,num=input().split("-")
    if name in l.keys():
        l[name].append(num)
    else:
        l[name]=[num]

def mat(x):
    x=list(x)
    xx=x[0:len(x)-1]
    xx=float("".join(xx))
    if x[-1]=="B":
        return xx*1000
    else:
        return xx



for j in sorted(l.keys()):
    l[j]=sorted(l[j],key=lambda x :mat(x))
    ans=l[j][0]
    if len(l[j])>1:
        for i in l[j][1:]:
            ans+=", "
            ans+=i
    print(j+": "+ans)xxxxxxxxxx l=dict()n=int(input())for i in range(n):    name,num=input().split("-")    if name in l.keys():        l[name].append(num)    else:        l[name]=[num]def mat(x):    x=list(x)    xx=x[0:len(x)-1]    xx=float("".join(xx))    if x[-1]=="B":        return xx*1000    else:        return xxfor j in sorted(l.keys()):    l[j]=sorted(l[j],key=lambda x :mat(x))    ans=l[j][0]    if len(l[j])>1:        for i in l[j][1:]:            ans+=", "            ans+=i    print(j+": "+ans)# 
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1709827809244](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1709827809244.png)



## 2. 学习总结和收获

本次月考题目以前好像都做过，汉诺塔以前就觉得有点复杂于是跳过，大概80minAC5，因为还有点别的作业要补，汉诺塔直接复制的以前的代码交了。通过考试的过程，感觉到一些基本的implement还是生疏了，比如约瑟夫尝试了很多次才改过，需要加强对语法知识和基本实现的理解与应用。比较惊喜的就是放空导弹那道题ac的比较顺利，看来对dp还是稍有进步。

今天回过头把汉诺塔写了，发现确实在实现上有点不熟练，主要是移动的函数应该设置n，now，by，to四个变量，就非常便利。

每日选做也还在努力赶上进度，目前ac了一半。





