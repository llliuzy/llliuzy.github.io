# Assignment #8: 图论：概念、遍历，及 树算

Updated 1919 GMT+8 Apr 8, 2024

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

### 19943: 图的拉普拉斯矩阵

matrices, http://cs101.openjudge.cn/practice/19943/

请定义Vertex类，Graph类，然后实现



思路：



代码

```python
class Vertex:	
    def __init__(self, key):
        self.id = key
        self.connectedTo = {}

    def addNeighbor(self, nbr, weight=0):
        self.connectedTo[nbr] = weight

    def __str__(self):
        return str(self.id) + ' connectedTo: ' + str([x.id for x in self.connectedTo])

    def getConnections(self):
        return self.connectedTo.keys()

    def getId(self):
        return self.id

    def getWeight(self, nbr):
        return self.connectedTo[nbr]

class Graph:
    def __init__(self):
        self.vertList = {}
        self.numVertices = 0

    def addVertex(self, key):
        self.numVertices = self.numVertices + 1
        newVertex = Vertex(key)
        self.vertList[key] = newVertex
        return newVertex

    def getVertex(self, n):
        if n in self.vertList:
            return self.vertList[n]
        else:
            return None

    def __contains__(self, n):
        return n in self.vertList

    def addEdge(self, f, t, weight=0):
        if f not in self.vertList:
            self.addVertex(f)
        if t not in self.vertList:
            self.addVertex(t)
        self.vertList[f].addNeighbor(self.vertList[t], weight)

    def getVertices(self):
        return self.vertList.keys()

    def __iter__(self):
        return iter(self.vertList.values())
    
n,m=map(int,input().split())
g=Graph()
for i in range(n):
    g.addVertex(i)
for _ in range(m):
    u,v=map(int,input().split())
    g.addEdge(u,v)
    g.addEdge(v,u)

Laplace=[]
for vertex in g:
    line=[0]*n
    line[vertex.getId()]=(len(vertex.getConnections()))
    for connect in vertex.getConnections():  
        line[connect.getId()]=-1
    Laplace.append(line)

for l in Laplace:
    print(' '.join(map(str,l)))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1713252340582](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1713252340582.png)



### 18160: 最大连通域面积

matrix/dfs similar, http://cs101.openjudge.cn/practice/18160



思路：



代码

```python
ans=0
def dfs(l,i,j):
    ways=[(1,0),(-1,0),(0,1),(0,-1),(1,1),(1,-1),(-1,-1),(-1,1)]
    global ans
    l[i][j]="."
    ans+=1
    for _ in ways:
        p,q=_
        if 0<=i+p<=n-1 and 0<=j+q<=m-1:
            p+=i
            q+=j
            if l[p][q]=="W":
                dfs(l,p,q)
               
t=int(input()) 
for _ in range(t):
    l_ans=[0]
    n,m=map(int,input().split())
    l=[]
    for _ in range(n):
        t=input()
        t=list(t)
        l.append(t)
    for i in range(n):
       for j in range(m):
           if l[i][j]=="W":
               dfs(l,i,j)
               l_ans.append(ans)
               ans=0
           else:
               continue

    print(max(l_ans))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1713252839596](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1713252839596.png)

### sy383: 最大权值连通块

https://sunnywhy.com/sfbj/10/3/383



思路：



代码

```python

n, m = map(int, input().split())
values = list(map(int, input().split()))
graph = [[] for _ in range(n)]

for _ in range(m):
    a, b = map(int, input().split())
    graph[a].append(b)
    graph[b].append(a)

visited = [False] * n
def dfs(v):
    visited[v] = True
    total = values[v]
    for w in graph[v]:
        if not visited[w]:
            total += dfs(w)
    return total

max_value = max(dfs(v) for v in range(n) if not visited[v])
print(max_value)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1713252868233](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1713252868233.png)

### 03441: 4 Values whose Sum is 0

data structure/binary search, http://cs101.openjudge.cn/practice/03441



思路：感觉不是图？大概就是分别组合，减少了一定的计算量



代码

```python
n = int(input().strip())
A, B, C, D = [], [], [], []
for _ in range(n):
    a, b, c, d = map(int, input().strip().split())
    A.append(a)
    B.append(b)
    C.append(c)
    D.append(d)

AB = {}
for a in A:
    for b in B:
        if a + b in AB:
            AB[a + b] += 1
        else:
            AB[a + b] = 1

count = 0
for c in C:
    for d in D:
        if -c - d in AB:
            count += AB[-c - d]

print(count)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1713252890095](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1713252890095.png)

### 04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

Trie 数据结构可能需要自学下。



思路：



代码

```python
t=int(input())
          
def detect(l):
    l=sorted(l)
    for i in range(len(l)-1):
        if len(l[i])>len(l[i+1]):
            if l[i].startswith(l[i+1]):
                print("NO")
                return 
        else:
            if l[i+1].startswith(l[i]):
                print("NO")
                return 
    print("YES")
    return
            
while True:
    try:
        n=int(input())
        l=[]
        for _ in range(n):
            l.append(str(input()))
        detect(l)
        
    except EOFError:
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1713258505216](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1713258505216.png)



### 04082: 树的镜面映射

http://cs101.openjudge.cn/practice/04082/



思路：



代码

```python
from collections import deque

class TreeNode:
    def __init__(self, x):
        self.x = x
        self.children = []

def create_node():
    return TreeNode('')

def build_tree(tempList, index):
    node = create_node()
    node.x = tempList[index][0]
    if tempList[index][1] == '0':
        index += 1
        child, index = build_tree(tempList, index)
        node.children.append(child)
        index += 1
        child, index = build_tree(tempList, index)
        node.children.append(child)
    return node, index

def print_tree(p):
    Q = deque()
    s = deque()

    # 遍历右子节点并将非虚节点加入栈s
    while p is not None:
        if p.x != '$':
            s.append(p)
        p = p.children[1] if len(p.children) > 1 else None

    # 将栈s中的节点逆序放入队列Q
    while s:
        Q.append(s.pop())

    # 宽度优先遍历队列Q并打印节点值
    while Q:
        p = Q.popleft()
        print(p.x, end=' ')

        # 如果节点有左子节点，将左子节点及其右子节点加入栈s
        if p.children:
            p = p.children[0]
            while p is not None:
                if p.x != '$':
                    s.append(p)
                p = p.children[1] if len(p.children) > 1 else None

            # 将栈s中的节点逆序放入队列Q
            while s:
                Q.append(s.pop())


n = int(input())
tempList = input().split()

# 构建多叉树
root, _ = build_tree(tempList, 0)

# 执行宽度优先遍历并打印镜像映射序列
print_tree(root)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1713262638755](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1713262638755.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

最近期中季在数理化中挣扎，数算投入的时间确实有点少了。特别是上周还是月考的作业，所以已经快十几天没有打代码了。这次写的时候明显感觉到手生了，看来还是要多练习啊。

电话号码那道题以外的可以用内置的排序来做，python内置方法真是好啊。不过意识到考点是Trie以后还是尝试手搓了，虽然自己写的很麻烦，最后还是看答案了。

镜面映射有点没时间写了，自己写了把bintree转化成tree的函数，其他部分就看答案了。

不过现在期中考完了应该在数算上多投入一点时间了，特别是每日选做还有笔试部分。



