# 0Assignment #D: May月考

Updated 1654 GMT+8 May 8, 2024

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

### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：



代码

```python
# l,m=map(int,input().split())
tree=[1]*(l+1)
for _ in range(m):
    x,y=map(int,input().split())
    for i in range(y-x+1):
        tree[x+i]=0
print(sum(tree))
```



代码运行截图 ==（至少包含有"Accepted"）==

![1716002193653](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716002193653.png)



### 20449: 是否被5整除

http://cs101.openjudge.cn/practice/20449/



思路：



代码

```python
l=list(input())
ans=[]
for i in range(len(l)):
    x=l[0:i+1]
    n="".join(x)
    
    y=int(n,2)
    if y%5==0:
        ans.append("1")
    else:
        ans.append("0")
print("".join(ans))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1716002256501](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716002256501.png)



### 01258: Agri-Net

http://cs101.openjudge.cn/practice/01258/



思路：运用kruskal算法。个人理解：首先定义并查集的类。然后核心算法就是，把所有边进行排序，从小到大加入，然后一直使用并查集进行合并，这样来确保把所有的点都连在一起。如果两个集合没有连在一起，那么就在总cst中加上它的cst，最后检查是否已经连在一起。



代码

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if self.rank[px] > self.rank[py]:
            self.parent[py] = px
        else:
            self.parent[px] = py
            if self.rank[px] == self.rank[py]:
                self.rank[py] += 1

def kruskal(n, edges):
    uf = UnionFind(n)
    edges.sort(key=lambda x: x[2])
    res = 0
    for u, v, w in edges:
        if uf.find(u) != uf.find(v):
            uf.union(u, v)
            res += w
    if len(set(uf.find(i) for i in range(n))) > 1:
        return -1
    return res

while True:
    try:
        n=int(input())
        edges = []
        for i in range(n):
            line=list(map(int,input().split()))
            for j in range(n):               
                u, v, w = i,j,line[j]
                edges.append((u, v, w))
        print(kruskal(n, edges))
    except EOFError:
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1716002294317](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716002294317.png)



### 27635: 判断无向图是否连通有无回路(同23163)

http://cs101.openjudge.cn/practice/27635/



思路：这一题分两个部分，一个是判断无向图是否联通，一个是判断是否有回路。先用邻接表存图。其实都是dfs，但是判断环路的时候要注意，需要加一个变量防止回头走。



代码

```python
n,m=map(int,input().split())
gra=[[]for i in range(n)]
GRA=[[]for i in range(n)]
for j in range(m):
    p,q=map(int,input().split())
    gra[p].append(q)
    gra[q].append(p)

def dfs(g,x,v):
    v[x]=True
    for nex in g[x]:
        if not v[nex]:
            dfs(gra,nex,visited)

visited=[False]*n
dfs(gra,0,visited)
if all(visited):
    ans1="yes"
else:
    ans1="no"

color=[0]*n
def Dfs(node,past):
        if color[node] == 1:
            return True
        if color[node] == 2:
            return False

        color[node] = 1
        for neighbor in gra[node]:
            if neighbor!=past: 
                if Dfs(neighbor,node):
                    return True
        color[node] = 2
        return False

ans2="no"
for i in range(n):
    if Dfs(i,-1):
        ans2="yes"
        break

print(f"connected:{ans1}")
print(f"loop:{ans2}")

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1716002327632](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716002327632.png)





### 27947: 动态中位数

http://cs101.openjudge.cn/practice/27947/



思路：第一次做这个题。看到heapq的提示，想到了用两个堆，但是一直想不到怎么建立一个大堆和一个小堆。看到题解豁然开朗。



代码

```python
from heapq import *
class MedianFinder:
    def __init__(self):
        self.heaps = [], []

    def addNum(self, num):
        small, large = self.heaps
        heappush(small, -heappushpop(large, num))
        if len(large) < len(small):
            heappush(large, -heappop(small))

    def findMedian(self):
        small, large = self.heaps
        if len(large) > len(small):
            return str(int(large[0]))
        return (large[0] - small[0]) / 2.0

t=int(input())
for _ in range(t):
    l=list(map(int,input().split()))
    m=len(l)
    h=MedianFinder()
    ans=[]
    for i in range(m):
        h.addNum(l[i])
        if i%2==0:
            ans.append(h.findMedian())
    print(len(ans))
    print(" ".join(ans))  

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1716002377911](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716002377911.png)



### 28190: 奶牛排队

http://cs101.openjudge.cn/practice/28190/



思路：非常艰难的一道题。首先学习了单调栈的写法与基本用法，然后发现必须一直优化建栈和输出的过程。



代码

```python
n=int(input())
heights=[int(input()) for i in range(n)]
leftb=[-1]*n
rightb=[n for i in range(n)]
stack=[]
for i in range(n):
    while stack and heights[stack[-1]]<heights[i]:
        stack.pop()
    if stack:
        leftb[i]=stack[-1]
    stack.append(i)

stack=[]
for i in range(n-1,-1,-1):
    while stack and  heights[stack[-1]]>heights[i]:
        stack.pop()
    if stack:
        rightb[i]=stack[-1]
    stack.append(i)

ans=0
            
for i in range(n):
    for j in range(leftb[i] + 1,i):
        if rightb[j]>i:
            ans=max(ans,i-j+1)
            break
    
print(ans)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1716003670035](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716003670035.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

近期全力准备机考吧，准备把讲义里面的算法和题目都过一遍，至少把两道M做出来，保4争5.同时也顺便更新一下github。



