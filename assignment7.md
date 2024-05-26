# Assignment #7: April 月考

Updated 1557 GMT+8 Apr 3, 2024

2024 spring, Complied by ==刘致远 工学院==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/



思路：



代码

```python
l=list(input().split())
l=l[::-1]
print(" ".join(l))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1712135736385](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712135736385.png)



### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/



思路：



代码

```python
m,n=map(int,input().split())
l=list(map(int,input().split()))
word=list()
ans=0
for i in l:
    if i in word:
        continue
    else:
        ans+=1
        word.append(i)
        if len(word)>m:
            word=word[1:]
print(ans)

```



代码运行截图 ==（至少包含有"Accepted"）==

![1712135763103](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712135763103.png)



### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/



思路：



代码

```python
import heapq
n,k=map(int,input().split())
l=list(map(int,input().split()))
heapq.heapify(l)
for _ in range(k):
    ans=heapq.heappop(l)
    
if n==k: 
    print(ans)
elif k==0:
    m=heapq.heappop(l)
    if m==1:
        print(-1)
    else:
        print(m-1)
elif heapq.heappop(l)==ans:
    print(-1)
else:
    print(ans)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1712135790318](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712135790318.png)



### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/



思路：



代码

```python
class node:
    def __init__(self,value):
        self.value=value
        self.right=None
        self.left=None
        
def det(s):
    if len(s)==sum(s):
        return "I"
    elif sum(s)==0:
        return "B"
    else:
        return "F"

def fbi(s):
    if len(s)==1:
        return node(det(s))
    else:
        R=node(det(s))
        R.left=fbi(s[: len(s)//2])
        R.right=fbi(s[len(s)//2 :])
        return R

def postorder(node):
    output = []
    if node.left is not None:
        output.extend(postorder(node.left))
    if node.right is not None:
        output.extend(postorder(node.right))
    output.append(node.value)
    return ''.join(output)

n=int(input())
lst=list(input())
l=[int(i) for i in lst]
print(postorder(fbi(l)))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1712135815886](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712135815886.png)



### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/



思路：



代码

```python
from collections import deque as dq
t=int(input())
team=[];guy=dict();i=-1
nowTeam=dq()
now=[dq() for i in range(t)]
for _ in range(t):
    tem=list(map(int,input().split()))
    team.append(tem)
    i+=1
    for j in tem:
        guy[str(j)]=i
         
while True:
    l=input().split()
    if l[0]=="STOP":
        break
    elif l[0]=="ENQUEUE":
        theTeam=guy[l[1]]
        now[theTeam].append(l[1])
        if theTeam in nowTeam:
            continue
        else:
            nowTeam.append(theTeam)
    else:
        head=nowTeam[0]
        print(now[head].popleft())
        if len(now[head])==0:
            nowTeam.popleft()

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240403171719997](C:\Users\31861\AppData\Roaming\Typora\typora-user-images\image-20240403171719997.png)



### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/



思路：



代码

```python
class node:
    def __init__(self,value):
        self.value=value
        self.neib=[value]
        
def theorder(node):
    if len(node.neib)==1: 
        output = [str(node.value)]
    else:
        output= []
        child=sorted(node.neib)
        for i in child:
            if i!=node.value:
                output.extend(theorder(tree[i]))
            else:
                output.append(str(i))
    
    return output

n=int(input())
tree=dict()
hasP=set()
for _ in range(n):
    l=list(map(int,input().split()))
    for q in range(len(l)-1):
        hasP.add(l[q+1])
    tree[l[0]]=node(l[0])
    for q in range(len(l)-1):  
        tree[l[0]].neib.append(l[q+1])
        
P=set(tree.keys())
NODE=tree[list(P-hasP)[0]]

ans=theorder(NODE)
for i in ans:
    if i!=" ": 
        print(i)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1712135930539](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712135930539.png)



## 2. 学习总结和收获

在M2上因为边界条件WA和RE了几次，下次一定要小心参数的取值范围，注意边界条件

总之考的还是不错的，但是发现有一些小地方用的不太熟练，比如deque和heapq的一些方法





