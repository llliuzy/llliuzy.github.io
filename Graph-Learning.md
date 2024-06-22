---
Graph-Learning
by Apocalypse from PKU
---

**1.图的建立：**

邻接表：

​	数算往往使用邻接表来表示图，因为方便后续的遍历等操作，这种方法适用于不在坐标系之中的图，相互之间路径已经给出。



**2.图的遍历：**

bfs:

```py
def bfs(graph, initial):
    visited = set()
    queue = [(initial,tim)]
 
    while queue:
        node = queue.pop(0)
        if node not in visited:
            visited.add(node)
            neighbours = graph[node]
            nt=tim
 
            for neighbour in neighbours:
                cst=costs[neighbour]
                queue.append((neighbour,cst+tim)
```

---

dfs：

```py
def dfs(v):
    visited.add(v)
    total = values[v]  #以最大权值联通块为例
    for w in graph[v]:
        if w not in visited:
            total += dfs(w)
    return total
```

---



**3.经典算法：**

**1.Dijkstra：**

​	用于确定给定两点间的最短距离（有边权且均为正）。

​	实现：

```py
#用字典储存路径
ways=dict()
for _ in range(p):
    ways[input()]=[]
q=int(input())
for i in range(q):
    FRM,TO,CST=input().split()
    ways[FRM].append((TO,int(CST)))
    ways[TO].append((FRM,int(CST)))

#函数主体(带路径的实现)
from heapq import *
def dijkstra(frm,to):
    q=[]
    tim=0
    heappush(q,(tim,frm,[frm]))
    visited=set([frm])
    if frm==to:
        return frm,0
    while q:
        tim,x,how=heappop(q)
        if x==to:
            return "->".join(how),tim
        visited.add(x)
        for way in ways[x]:
            nx=way[0];cst=way[1]
            if nx not in visited:
                nhow=how.copy()
                nhow.append(f"({cst})")
                nhow.append(nx)
                heappush(q,(tim+cst,nx,nhow))
    return "NO"
```

2.Bellman-Ford*：

​	用于确定给定两点间的最短距离（有边权且可以为负）。

​	实现：略



**3.Prim：**

用途：在N**2时间内实现最小生成树。

```py
import heapq

def prim(graph, n):
    visited = [False] * n
    min_heap = [(0, 0)]  # (weight, vertex)
    min_spanning_tree_cost = 0

    while min_heap:
        weight, vertex = heapq.heappop(min_heap)

        if visited[vertex]:
            continue

        visited[vertex] = True
        min_spanning_tree_cost += weight

        for neighbor, neighbor_weight in graph[vertex]:
            if not visited[neighbor]:
                heapq.heappush(min_heap, (neighbor_weight, neighbor))

    return min_spanning_tree_cost if all(visited) else -1

def main():
    n, m = map(int, input().split())
    graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        graph[u].append((v, w))
        graph[v].append((u, w))

    min_spanning_tree_cost = prim(graph, n)
    print(min_spanning_tree_cost)

if __name__ == "__main__":
    main()

```



**4.Kruskal：**

​	用途：在 *NlogN* 时间内实现最小生成树。



