# Assignment #6: "树"算：Huffman,BinHeap,BST,AVL,DisjointSet

Updated 2214 GMT+8 March 24, 2024

2024 spring, Complied by ==刘致远 工学院==



**说明：**

1）这次作业内容不简单，耗时长的话直接参考题解。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 22275: 二叉搜索树的遍历

http://cs101.openjudge.cn/practice/22275/



思路：



代码

```python
def posto(preo):
    if len(preo)==0:
        return []
    root=preo[0]
    if len(preo)==1:
        return [root]
    left=[i for i in preo if int(i)<int(root)]
    right=[i for i in preo if int(i)>int(root)]
    return posto(left)+posto(right)+[root]

n=int(input())
preo=list(input().split())
print(" ".join(posto(preo)))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1712034616605](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712034616605.png)



### 05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/



思路：



代码

```python
class node():
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None
        
def binary(nod,val):
    if int(nod.value)>int(val):
        if nod.left is None:
            nod.left=node(val)
        else:
            binary(nod.left,val)
    else:
        if nod.right is None:
            nod.right=node(val)
        else:
            binary(nod.right,val)

def add(node):
    global stack
    if node.left is not None:
        stack.append(node.left)
    if node.right is not None:
        stack.append(node.right)

def levelo(node):
    if node is None:
        return 
    else:
        add(node)
        return node.value
    

Lst=list((input().split()))
lst=[]
for i in Lst:
    if i not in lst:
        lst.append(i)
Tree=node(lst[0])
for i in lst:
    if i!=lst[0]:
        binary(Tree,i)
  
ans=[];stack=[Tree]
while stack:
    now=stack[0]
    stack=stack[1:]
    ans.append(levelo(now))

    
print(" ".join(ans))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1712034640462](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712034640462.png)



### 04078: 实现堆结构

http://cs101.openjudge.cn/practice/04078/

练习自己写个BinHeap。当然机考时候，如果遇到这样题目，直接import heapq。手搓栈、队列、堆、AVL等，考试前需要搓个遍。



思路：



代码

```python
class Binheap:
    def __init__(self):
        self.hpLst=[0]
        self.CrtSz=0
        
    def percUp(self,i):
        while i//2>0:
            if self.hpLst[i//2]>self.hpLst[i]:
                now=self.hpLst[i//2]
                self.hpLst[i//2]=self.hpLst[i]
                self.hpLst[i]=now
            i=i//2
   
    def insert(self,k):
        self.hpLst.append(k)
        self.CrtSz+=1
        self.percUp(self.CrtSz)
        
    def percDown(self,i):
        while i*2<=self.CrtSz:
            now=self.minChild(i)
            if self.hpLst[now]<self.hpLst[i]:
                tmp=self.hpLst[now]
                self.hpLst[now]=self.hpLst[i]
                self.hpLst[i]=tmp
            i=now
            
    def minChild(self, i):
        if i * 2 + 1 > self.CrtSz:
            return i * 2
        else:
            if self.hpLst[i * 2] < self.hpLst[i * 2 + 1]:
                return i * 2
            else:
                return i * 2 + 1
            
    def delMin(self):
        target=self.hpLst[1]
        self.hpLst[1]=self.hpLst[self.CrtSz]
        self.hpLst.pop()
        self.CrtSz-=1
        self.percDown(1)
        return target

Binheap=Binheap()
n=int(input())
for _ in range(n):
    l=list(map(int,input().split()))
    if l[0]==1:
        Binheap.insert(l[1])
    else:
        print(Binheap.delMin())

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1712034698966](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712034698966.png)



### 22161: 哈夫曼编码树

http://cs101.openjudge.cn/practice/22161/



思路：想了比较久的一个点就是，在解码的时候，怎么判断一个字符对应的原码结束。解决方法是，不断往下判断，且判断node的left，right是否都为None。如果是，则输出，并且返回到treeroot



代码

```python
import heapq

class Node:
    def __init__(self, weight, char=None):
        self.weight = weight
        self.char = char
        self.left = None
        self.right = None

    def __lt__(self, other):
        if self.weight == other.weight:
            return self.char < other.char
        return self.weight < other.weight

def build_huffman_tree(characters):
    heap = []
    for char, weight in characters.items():
        heapq.heappush(heap, Node(weight, char))

    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        #merged = Node(left.weight + right.weight) #note: 合并后，char 字段默认值是空
        merged = Node(left.weight + right.weight, min(left.char, right.char))
        merged.left = left
        merged.right = right
        heapq.heappush(heap, merged)

    return heap[0]

def encode_huffman_tree(root):
    codes = {}
    def traverse(node, code):
        #if node.char:
        if node.left is None and node.right is None:
            codes[node.char] = code
        else:
            traverse(node.left, code + '0')
            traverse(node.right, code + '1')

    traverse(root, '')
    return codes

def huffman_encoding(codes, string):
    encoded = ''
    for char in string:
        encoded += codes[char]
    return encoded

def huffman_decoding(root, encoded_string):
    decoded = ''
    node = root
    for bit in encoded_string:
        if bit == '0':
            node = node.left
        else:
            node = node.right

        #if node.char:
        if node.left is None and node.right is None:
            decoded += node.char
            node = root
    return decoded

# 读取输入
n = int(input())
characters = {}
for _ in range(n):
    char, weight = input().split()
    characters[char] = int(weight)

#string = input().strip()
#encoded_string = input().strip()

# 构建哈夫曼编码树
huffman_tree = build_huffman_tree(characters)

# 编码和解码
codes = encode_huffman_tree(huffman_tree)

strings = []
while True:
    try:
        line = input()
        strings.append(line)

    except EOFError:
        break

results = []
#print(strings)
for string in strings:
    if string[0] in ('0','1'):
        results.append(huffman_decoding(huffman_tree, string))
    else:
        results.append(huffman_encoding(codes, string))

for result in results:
    print(result)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1712041695286](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712041695286.png)



### 晴问9.5: 平衡二叉树的建立

https://sunnywhy.com/sfbj/9/5/359



思路：



代码

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
        self.height = 1

class AVL:
    def __init__(self):
        self.root = None

    def insert(self, value):
        if not self.root:
            self.root = Node(value)
        else:
            self.root = self._insert(value, self.root)

    def _insert(self, value, node):
        if not node:
            return Node(value)
        elif value < node.value:
            node.left = self._insert(value, node.left)
        else:
            node.right = self._insert(value, node.right)

        node.height = 1 + max(self._get_height(node.left), self._get_height(node.right))

        balance = self._get_balance(node)

        if balance > 1:
            if value < node.left.value:	# 树形是 LL
                return self._rotate_right(node)
            else:	# 树形是 LR
                node.left = self._rotate_left(node.left)
                return self._rotate_right(node)

        if balance < -1:
            if value > node.right.value:	# 树形是 RR
                return self._rotate_left(node)
            else:	# 树形是 RL
                node.right = self._rotate_right(node.right)
                return self._rotate_left(node)

        return node

    def _get_height(self, node):
        if not node:
            return 0
        return node.height

    def _get_balance(self, node):
        if not node:
            return 0
        return self._get_height(node.left) - self._get_height(node.right)

    def _rotate_left(self, z):
        y = z.right
        T2 = y.left
        y.left = z
        z.right = T2
        z.height = 1 + max(self._get_height(z.left), self._get_height(z.right))
        y.height = 1 + max(self._get_height(y.left), self._get_height(y.right))
        return y

    def _rotate_right(self, y):
        x = y.left
        T2 = x.right
        x.right = y
        y.left = T2
        y.height = 1 + max(self._get_height(y.left), self._get_height(y.right))
        x.height = 1 + max(self._get_height(x.left), self._get_height(x.right))
        return x

    def preorder(self):
        return self._preorder(self.root)

    def _preorder(self, node):
        if not node:
            return []
        return [node.value] + self._preorder(node.left) + self._preorder(node.right)

n = int(input().strip())
sequence = list(map(int, input().strip().split()))

avl = AVL()
for value in sequence:
    avl.insert(value)

print(' '.join(map(str, avl.preorder())))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1712058904631](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712058904631.png)



### 02524: 宗教信仰

http://cs101.openjudge.cn/practice/02524/



思路：



代码

```python
def init_set(n):
    return list(range(n))

def get_father(x, father):
    if father[x] != x:
        father[x] = get_father(father[x], father)
    return father[x]

def join(x, y, father):
    fx = get_father(x, father)
    fy = get_father(y, father)
    if fx == fy:
        return
    father[fx] = fy

def is_same(x, y, father):
    return get_father(x, father) == get_father(y, father)

def main():
    case_num = 0
    while True:
        n, m = map(int, input().split())
        if n == 0 and m == 0:
            break
        count = 0
        father = init_set(n)
        for _ in range(m):
            s1, s2 = map(int, input().split())
            join(s1 - 1, s2 - 1, father)
        for i in range(n):
            if father[i] == i:
                count += 1
        case_num += 1
        print(f"Case {case_num}: {count}")

if __name__ == "__main__":
    main()

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1712065492229](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1712065492229.png)



## 2. 学习总结和收获

宗教信仰到最后还是没自己写出来，感觉对于函数的局部与全局变量还是掌握的不太熟练，还有一些基础的内容（比如if x  跟if x is True 有什么区别？）还是有问题

一些基本数据结构的实现大概掌握了



