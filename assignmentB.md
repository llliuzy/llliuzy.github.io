# Assignment #B: 图论和树算

Updated 1709 GMT+8 Apr 28, 2024

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

### 28170: 算鹰

dfs, http://cs101.openjudge.cn/practice/28170/



思路：题目意思不太清楚



代码

```python
m=[]

def dfs(x,y):
    if m[y][x]==".":
        for way in [(1,0),(-1,0),(0,1),(0,-1)]:
            dx,dy=way
            nx,ny=dx+x,dy+y
            if 0<=nx<10 and 0<=ny<10:
                m[y][x]="-"
                dfs(nx,ny)  


for y in range(10):
    line=list(input())
    m.append(line)
                
ans=0
for p in range(10):
    for q in range(10):
        if m[q][p]==".":
            ans+=1
            dfs(p,q)
        
            
print(ans)

```



代码运行截图 ==（至少包含有"Accepted"）==

![1715062848010](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1715062848010.png)



### 02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/



思路：dfs查找即可，注意判断不相互吃的方式



代码

```python
ans = []
def queen_dfs(A, cur=0):   
    if cur == 8:            
        ans.append(''.join([str(x+1) for x in A])) 
        return 
    
    for col in range(len(A)):     
        for row in range(cur):    
           
            if A[row] == col or abs(col - A[row]) == cur - row:
                break
        else:                    
            A[cur] = col     
            queen_dfs(A, cur+1)	  
            
queen_dfs([None]*8)   
for _ in range(int(input())):
    print(ans[int(input()) - 1])

```



代码运行截图 ==（至少包含有"Accepted"）==

![1715062808726](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1715062808726.png)



### 03151: Pots

bfs, http://cs101.openjudge.cn/practice/03151/



思路：卡了比较久，关键是要用visited，防止重复试

还要，列表必须用copy复制，不然会出大问题



代码

```python
from collections import deque
A,B,c=map(int,input().split())
m=[A,B]
l=[0,0]

def fill(i,ll):
    l=ll.copy()
    l[i]=m[i]
    return l

def drop(i,ll):
    l=ll.copy()
    l[i]=0
    return l

def pour(i,j,ll):
    l=ll.copy()
    if l[i]+l[j]<=m[j]:
        l[j]=l[i]+l[j]
        l[i]=0
    else:
        l[i]=l[i]-(m[j]-l[j])
        l[j]=m[j]
    return l

def bfs(L,N,C):
    q=deque([(L,N,[])])
    visited=set((0,0))
    while q:
        l,n,way=q.popleft()
        if l[0]==C or l[1]==C:
            return [n,way]
        if n==100:
            return "impossible"
        n+=1
        
        nl=fill(0,l)
        nway=way.copy()
        nway.append('FILL(1)')
        if (nl[0],nl[1]) not in visited:
            q.append((nl,n,nway))
            visited.add((nl[0],nl[1]))
        
        nl=fill(1,l)
        nway=way.copy()
        nway.append('FILL(2)')
        if (nl[0],nl[1]) not in visited:
            q.append((nl,n,nway))
            visited.add((nl[0],nl[1]))
        
        nl=drop(0,l)
        nway=way.copy()
        nway.append('DROP(1)')
        if (nl[0],nl[1]) not in visited:
            q.append((nl,n,nway))
            visited.add((nl[0],nl[1]))
        
        nl=drop(1,l)
        nway=way.copy()
        nway.append('DROP(2)')
        if (nl[0],nl[1]) not in visited:
            q.append((nl,n,nway))
            visited.add((nl[0],nl[1]))
        
        nl=pour(0,1,l)
        nway=way.copy()
        nway.append('POUR(1,2)')
        if (nl[0],nl[1]) not in visited:
            q.append((nl,n,nway))
            visited.add((nl[0],nl[1]))
        
        nl=pour(1,0,l)
        nway=way.copy()
        nway.append('POUR(2,1)')
        if (nl[0],nl[1]) not in visited:
            q.append((nl,n,nway))
            visited.add((nl[0],nl[1]))

    return "impossible"

ans=bfs(l,0,c)
if ans=="impossible":
    print("impossible")
else:
    n,way=ans[0],ans[1]
    print(n)
    for i in way:
        print(i)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1715061826159](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1715061826159.png)



### 05907: 二叉树的操作

http://cs101.openjudge.cn/practice/05907/



思路：关键就是Switch函数怎么写，要综合考虑多种情况



代码

```python
class node:
    def __init__(self,data):
        self.data=data
        self.left=None
        self.right=None
        self.father=None

def switch(tree, nd1, nd2):
    if nd1.father == nd2 or nd2.father == nd1:
        # Handle the case where nd1 and nd2 are parent and child
        parent, child = (nd1, nd2) if nd1.father == nd2 else (nd2, nd1)
        grandpa, child_sibling = parent.father, parent.right if parent.left == child else parent.left
        if grandpa.left == parent:
            grandpa.left = child
        else:
            grandpa.right = child
        child.father = grandpa
        child.left, child.right = parent, child_sibling
        parent.father, parent.left, parent.right = child, None, None
        if child_sibling is not None:
            child_sibling.father = child
    elif nd1.father == nd2.father:
        # Handle the case where nd1 and nd2 have the same parent
        if nd1.father.left == nd1:
            nd1.father.left, nd2.father.right = nd2, nd1
        else:
            nd1.father.right, nd2.father.left = nd2, nd1
        nd1.father, nd2.father = nd2.father, nd1.father
    else:
        # Handle the case where nd1 and nd2 are not parent and child
        nd1.father, nd2.father = nd2.father, nd1.father
        if nd1.father.left == nd2:
            nd1.father.left = nd1
        else:
            nd1.father.right = nd1
        if nd2.father.left == nd1:
            nd2.father.left = nd2
        else:
            nd2.father.right = nd2

def query(tree,nd):
    if nd.left is None:
        return nd.data
    else:
        return query(tree,nd.left)

t=int(input())
for _ in range(t):
    n,m=map(int,input().split())
    TREE=[node(i) for i in range(n+1)]
    for i in range(n):
        x,y,z=map(int,input().split())
        if y!=-1:
            TREE[x].left=TREE[y]
            TREE[y].father=TREE[x]
        if z!=-1: 
            TREE[x].right=TREE[z]
            TREE[z].father=TREE[x]

    for j in range(m):
        inp=list(map(int,input().split()))
        type=inp[0]
        if type==1:
            x1,x2=inp[1],inp[2]
            switch(TREE,TREE[x1],TREE[x2])
        else:
            x=inp[1]
            print(query(TREE,TREE[x]))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



![1715073964992](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1715073964992.png)



### 18250: 冰阔落 I

Disjoint set, http://cs101.openjudge.cn/practice/18250/



思路：之前采用树的方式，，但是好像递归过长导致re了

于是学习了并查集的方法



代码

```python
#并查集
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    root_x = find(x)
    root_y = find(y)
    if root_x != root_y:
        parent[root_y] = root_x

while True:
    try:
        n, m = map(int, input().split())
        parent = list(range(n + 1))
        for _ in range(m):
            a, b = map(int, input().split())
            if find(a) == find(b):
                print('Yes')
            else:
                print('No')
                union(a, b)

        unique_parents = set(find(x) for x in range(1, n + 1))  # 获取不同集合的根节点
        ans = sorted(unique_parents)  # 输出有冰阔落的杯子编号
        print(len(ans))
        print(*ans)

    except EOFError:
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



![1715086160129](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1715086160129.png)

### 05443: 兔子与樱花

http://cs101.openjudge.cn/practice/05443/



思路：基本的dijkstra



代码

```python
from heapq import *

def dijkstra(frm,to):
    q=[]
    t=0
    heappush(q,(t,frm,[frm]))
    visited=set(frm)
    if frm==to:
        return frm
    while q:
        t,x,how=heappop(q)
        if x==to:
            return "->".join(how)
        visited.add(x)
        for way in ways[x]:
            nx=way[0];cst=way[1]
            if nx not in visited:
                nhow=how.copy()
                nhow.append(f"({cst})")
                nhow.append(nx)
                heappush(q,(t+cst,nx,nhow))
    return "NO"

p=int(input())
ways=dict()
for _ in range(p):
    ways[input()]=[]
q=int(input())
for  i in range(q):
    FRM,TO,CST=input().split()
    ways[FRM].append((TO,int(CST)))
    ways[TO].append((FRM,int(CST)))
r=int(input())
for i in range(r):
    FRM,TO=input().split()
    print(dijkstra(FRM,TO))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



![1715085890918](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1715085890918.png)

## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

今天笔试发现填代码方面还是有漏洞，需要结合题目多整理消化



