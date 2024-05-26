# Assignment #A: 图论：遍历，树算及栈

Updated 2018 GMT+8 Apr 21, 2024

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

### 20743: 整人的提词本

http://cs101.openjudge.cn/practice/20743/



思路：本身不难，之前练过几遍。但是，我看时间限制比较死，就选择了pypy提交，但是一直tle。结果发现用py3提交就过了，时间是pypy的15%？？



代码

```python
def turn(word):
    stack=[]
    for i in word:
        if i=="(":
            stack.append(i)
        elif i==")":
            now=""
            while stack[-1]!="(":
                now=stack.pop()+now
            now=now[::-1]
            stack[len(stack)-1]=now
        else:
            stack.append(i)
    if len(stack)==0: 
        return stack[0]
    else:
        return "".join(stack)
                

print(turn(input()))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1714746263958](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1714746263958.png)



### 02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/



思路：最快解决的一集



代码

```python
def rebuild(pre,mid):
    if len(pre)==0:
        return ""
    root=pre[0]
    left_mid=mid[:mid.index(root)]
    right_mid=mid[mid.index(root)+1:]
    left_pre=pre[1:mid.index(root)+1]
    right_pre=pre[mid.index(root)+1:]
    return rebuild(left_pre,left_mid)+rebuild(right_pre,right_mid)+root

while True:
    try:
        Pre,Mid=input().split()
        print(rebuild(Pre,Mid))
    except EOFError:
        break

```



代码运行截图 ==（至少包含有"Accepted"）==

![1714746284041](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1714746284041.png)



### 01426: Find The Multiple

http://cs101.openjudge.cn/practice/01426/

要求用bfs实现



思路：很重要的思路就是建立结果，判断是否满足条件



代码

```python
from collections import deque
def bfs(n):
    queue = deque(['1'])
    while queue:
        num = queue.popleft()
        mod = int(num) % n
        if mod == 0:
            return num
        queue.append(num + '0')
        queue.append(num + '1')
while True:
    n=int(input())
    if n==0:
        break
    else:
        print(bfs(n))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1714746299941](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1714746299941.png)



### 04115: 鸣人和佐助

bfs, http://cs101.openjudge.cn/practice/04115/



思路：

要使用更多元参数的dfs搜索，记录查克拉

代码

```python
from collections import deque
m,n,T=map(int,input().split())
MAP=[]
mr=None
for i in range(m):
    q=list(input())
    MAP.append(q)
    for j in q:
        if j=="@":
            mr=(i,q.index(j))

def persue(now,ckl,t):
    ny,nx=now
    visited=set((nx,ny,ckl))
    quene=deque([(nx,ny,ckl,t)])
    way=[(0, 1), (0, -1), (1, 0), (-1, 0)]
    while quene: 
        nx,ny,ckl,t=quene.popleft()
        if MAP[ny][nx]=="+":
            return t
        elif MAP[ny][nx]=="#":
            ckl-=1
            if ckl<0:
                continue
        for dx,dy in way:
            x=nx+dx;y=ny+dy
            if 0<=x<n and 0<=y<m and (x,y,ckl) not in visited:
                visited.add((x,y,ckl))
                quene.append((x,y,ckl,t+1))
    return -1

print(persue(mr,T,0))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1714746322340](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1714746322340.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/



思路：dijkstra的基本实现，使用堆代替双向列表



代码

```python
from heapq import *
m,n,p=map(int,input().split())
ma=[list(input().split()) for _ in range(m)]

def dijkstra(frmx,frmy,tox,toy):
    q=[]
    t=0
    heappush(q,(t,frmx,frmy))
    visited=set((t,frmx,frmy))
    if ma[frmy][frmx]=="#" or ma[toy][tox]=="#":
        return "NO" 
    while q:
        t,x,y=heappop(q)
        if x==tox and y==toy:
            return t
        visited.add((x,y))
        for dx,dy in [(0,1),(0,-1),(1,0),(-1,0)]:
            nx,ny=x+dx,y+dy
            if 0<=nx<n and 0<=ny<m and ma[ny][nx]!="#" and (nx,ny) not in visited:
                cost=abs(int(ma[ny][nx])-int(ma[y][x]))
                heappush(q,(t+cost,nx,ny))
    return "NO"

for _ in range(p):
    frmy,frmx,toy,tox=map(int,input().split())
    print(dijkstra(frmx,frmy,tox,toy))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1714746346505](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1714746346505.png)



### 05442: 兔子与星空

Prim, http://cs101.openjudge.cn/practice/05442/



思路：想了比较久没有结果，于是看答案了



代码

```python
import heapq

def prim(graph, start):
    mst = []
    used = set([start])
    edges = [
        (cost, start, to)
        for to, cost in graph[start].items()
    ]
    heapq.heapify(edges)

    while edges:
        cost, frm, to = heapq.heappop(edges)
        if to not in used:
            used.add(to)
            mst.append((frm, to, cost))
            for to_next, cost2 in graph[to].items():
                if to_next not in used:
                    heapq.heappush(edges, (cost2, to, to_next))

    return mst

def solve():
    n = int(input())
    graph = {chr(i+65): {} for i in range(n)}
    for i in range(n-1):
        data = input().split()
        star = data[0]
        m = int(data[1])
        for j in range(m):
            to_star = data[2+j*2]
            cost = int(data[3+j*2])
            graph[star][to_star] = cost
            graph[to_star][star] = cost
    mst = prim(graph, 'A')
    print(sum(x[2] for x in mst))

solve()

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1714746510538](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1714746510538.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==



关键还是巩固了一下dfs和dijkstra的模版

