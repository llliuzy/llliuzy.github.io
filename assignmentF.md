# Assignment #F: All-Killed 满分

Updated 1844 GMT+8 May 20, 2024

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

### 22485: 升空的焰火，从侧面看

http://cs101.openjudge.cn/practice/22485/



思路：树的基础题目。



代码

```python
class node:
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None


n=int(input())
nodesl=[node(i) for i in range(n+1)]
for _ in range(n):
    l,r=map(int,input().split())
    if r!=-1:
        nodesl[_+1].right=nodesl[r]
    if l!=-1:
        nodesl[_+1].left=nodesl[l]


def bfs():
    stack=[(nodesl[1],0)]
    ans=[]
    dl = [-1] * (n + 3)
    while stack:
        now,depth=stack.pop(0)
        if dl[depth - 1] != -1:
            ans.append(dl[depth - 1])
            dl[depth - 1] = -1
        dl[depth] = now.value

        if now.left is not None and now.left.value!=-1:
            stack.append((now.left, depth + 1))
        if now.right is not None and now.right.value!=-1:
            stack.append((now.right, depth + 1))
    ans.append(dl[depth])
    return ans

print(" ".join([str(i) for i in bfs()]))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1716465694818](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716465694818.png)



### 28203:【模板】单调栈

http://cs101.openjudge.cn/practice/28203/



思路：模版题目。



代码

```python
n = int(input())
heights = list(map(int,input().split()))
rightb = [str(0) for i in range(n)]

stack = []
for i in range(n - 1, -1, -1):
    while stack and heights[stack[-1]] <= heights[i]:
        stack.pop()
    if stack:
        rightb[i] = str(stack[-1]+1)
    stack.append(i)

print(" ".join(rightb))


```



代码运行截图 ==（至少包含有"Accepted"）==

![1716465715055](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716465715055.png)



### 09202: 舰队、海域出击！

http://cs101.openjudge.cn/practice/09202/



思路：dfs判环的算法，要用颜色来实现



代码

```python
t=int(input())

def cir(l):
    color = [0] * len(l)
    def dfs(node):
        if color[node] == 1:
            return True
        if color[node] == 2:
            return False

        color[node] = 1
        for neighbor in l[node]:
            if dfs(neighbor):
                return True
        color[node] = 2
        return False

    for i in range(1,len(l)):
        if dfs(i):
            return "Yes"
    return "No"

for _ in range(t):
    n,m=map(int,input().split())
    lst=[[]for _ in range(n+1)]
    for i in range(m):
        x,y=map(int,input().split())
        lst[x].append(y)
    print(cir(lst))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1716465731270](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716465731270.png)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135/



思路：做了挺久但是还是tle，无奈看题解。发现自己的思维还是固化在了数算中，最初想到的算法就是：不断合并和最小的相邻数，直到总数达到n  。于是想尝试利用heapq创造出一个类似于哈夫曼树的算法，但是总是失败。

看了题解才发现是计概题目，居然是二分查找😓

再仔细想想，为什么二分查找这种看似反复次数很多的方法能节省时间呢？因为这题的数据结构为了体现相邻性，很可能要用列表来表示。而我想的方法无疑每一步都要删改列表，时间复杂度都是O（n）。同时heapq的优势难以得到发挥，每次找最小相邻和的过程时间复杂度都很高。而答案的做法在check的时候，其实只是遍历了一遍列表。



代码

```python
n,m=map(int,input().split())
lst=[]
for i in range(n):
    lst.append(int(input()))

def check(x):
    now=0;num=1
    for i in lst:
        now+=i
        if now>x:
            num+=1
            now=i
    if num>m:
        return False
    else:
        return True

up=sum(lst)
down=max(lst)

while up>down:
    mid=(up+down)//2
    if check(mid):
        up=mid
    else:
        down=mid+1

print(int(up))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1716465752273](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716465752273.png)



### 07735: 道路

http://cs101.openjudge.cn/practice/07735/



思路：基本的Dijkstra



代码

```python
from heapq import *

k=int(input())
n=int(input())
r=int(input())
mat=dict()
for i in range(n):
    mat[i+1]=[]
for i in range(r):
    s,d,l,t=map(int,input().split())
    mat[s].append([d,l,t])

def dijkstra(frm,to,money):
    q=[]
    tim=0
    heappush(q,(tim,frm,money))
    if to==1:
        return 0
    while q:
        tim,x,mon = heappop(q)
        if x==to:
            return tim

        for way in mat[x]:
            nx=way[0];cst=way[1];nm=mon-way[2]
            if  nm>=0:
                heappush(q,(tim+cst,nx,nm))
    return -1

print(dijkstra(1,n,k))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1716465773441](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716465773441.png)



### 01182: 食物链

http://cs101.openjudge.cn/practice/01182/



思路：一开始意识到并查集，但是想了很久都没能有效的解决“吃与被吃”怎么构建。看了答案，将三种动物分区储存的方法确实很巧妙。



代码

```python
class DisjointSet:
    def __init__(self, n):
        #设[1,n] 区间表示同类，[n+1,2*n]表示x吃的动物，[2*n+1,3*n]表示吃x的动物。
        self.parent = [i for i in range(3 * n + 1)] # 每个动物有三种可能的类型，用 3 * n 来表示每种类型的并查集
        self.rank = [0] * (3 * n + 1)

    def find(self, u):
        if self.parent[u] != u:
            self.parent[u] = self.find(self.parent[u])
        return self.parent[u]

    def union(self, u, v):
        pu, pv = self.find(u), self.find(v)
        if pu == pv:
            return False
        if self.rank[pu] > self.rank[pv]:
            self.parent[pv] = pu
        elif self.rank[pu] < self.rank[pv]:
            self.parent[pu] = pv
        else:
            self.parent[pv] = pu
            self.rank[pu] += 1
        return True


def is_valid(n, k, statements):
    dsu = DisjointSet(n)

    def find_disjoint_set(x):
        if x > n:
            return False
        return True

    false_count = 0
    for d, x, y in statements:
        if not find_disjoint_set(x) or not find_disjoint_set(y):
            false_count += 1
            continue
        if d == 1:  # X and Y are of the same type
            if dsu.find(x) == dsu.find(y + n) or dsu.find(x) == dsu.find(y + 2 * n):
                false_count += 1
            else:
                dsu.union(x, y)
                dsu.union(x + n, y + n)
                dsu.union(x + 2 * n, y + 2 * n)
        else:  # X eats Y
            if dsu.find(x) == dsu.find(y) or dsu.find(x + 2*n) == dsu.find(y):
                false_count += 1
            else: #[1,n] 区间表示同类，[n+1,2*n]表示x吃的动物，[2*n+1,3*n]表示吃x的动物
                dsu.union(x + n, y)
                dsu.union(x, y + 2 * n)
                dsu.union(x + 2 * n, y + n)

    return false_count


if __name__ == "__main__":
    N, K = map(int, input().split())
    statements = []
    for _ in range(K):
        D, X, Y = map(int, input().split())
        statements.append((D, X, Y))
    result = is_valid(N, K, statements)
    print(result)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1716465790591](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1716465790591.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

本次作业感觉选题比较丰富，有点期末模拟的感觉。1235大概都是M的难度，4本身不太难但是没想到二分查找就很难了。6可以说是模版的升级，应该可以作为T，目前对这类题还是很难下手。

正在整理模版题目，并且顺便搭建一下github。也在看群里同学发的整理，叹服之余也暗自勉励自己o(╥﹏╥)o。



