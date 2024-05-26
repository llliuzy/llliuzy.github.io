# Assignment #5: "树"算：概念、表示、解析、遍历

Updated 2124 GMT+8 March 17, 2024

2024 spring, Complied by ==刘致远 工学院==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：Spyder IDE 5.2.2, PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：Mac terminal vi (version 9.0.1424), g++/gcc (Apple clang version 14.0.3, clang-1403.0.22.14.1)



## 1. 题目

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/



思路：这里第一次没有ac是因为找树根的代码有bug，要注意。另外，在数叶子的时候，要加两个if，一个处理叶子，一个处理none。对none的判定，尽量用  x is None。



代码

```python
class node:
    def __init__(self):
        self.left=None
        self.right=None

def height(node):
    if node is None:
        return 0
    else:
        return 1+max(height(node.left),height(node.right))

def countLeaf(node):
    
    if node is None:
        return 0
    elif node.left is None and node.right is None:
        return 1
    else:
        return (countLeaf(node.left)+countLeaf(node.right))

n=int(input())
tree=[node() for i in range(n)]
has_p=[1 for i in range(n)]
for _ in range(n):
    l,r=map(int,input().split())
    if l!=-1:
        tree[_].left=tree[l]
        has_p[l]=0
    if r!=-1:
        tree[_].right=tree[r]
        has_p[r]=0

for i in range(n):
    if has_p[i]==1:
        NODE=i

if n==1: 
    ans="0 1"
else:   
    ans=str(height(tree[NODE])-1)+" "+str(countLeaf(tree[NODE]))
print(ans)

```



代码运行截图 ==（至少包含有"Accepted"）==

![1710942481265](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1710942481265.png)



### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/



思路：这个是经典的题目 首先要知道函数str.isalpha()，然后建树的过程是关键。大概来说，要用一个stack装树根，还有一个node临时储存一下树根。字母的话，要么装进树根，要么成为树根。如果左括号，就把node进栈，如果右括号，就把栈里面东西弹出



代码

```python
class treeNode:
    def __init__(self,value):
        self.value=value
        self.sons=[]

l=list(input())
stack=[]
node=None 
tree=treeNode(l[0])
for i in l:
    if i.isalpha():
        if stack: 
            node=treeNode(i)
            stack[-1].sons.append(node)
        else:    
            node=treeNode(i)
    elif i =="(":
        if node:
            stack.append(node)
    elif i==")":
        if stack: 
            tree=stack.pop()

def postOrder(tree):
    ans=[]
    for i in tree.sons:
        ans.extend(postOrder(i))
    ans.append(tree.value)
    return "".join(ans)

def preOrder(tree):
    ans=[tree.value]
    for i in tree.sons:
        ans.extend(preOrder(i))
    return "".join(ans)
        
print(preOrder(tree))
print(postOrder(tree))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1711196471693](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1711196471693.png)



### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/



思路：做了很久的一道题。首先在class的时候就需要分成file与dir两种。然后，再定义一个输出的函数。

但是在做题的时候，主要是一些具体的输出困扰了我。首先，总节点要设置成node，但我居然没有想到，被迫加了特判。然后，file里面的东西要用字符串存，dir里面要用node（）化以后存。最后，在空行的输出中，答案是收集了所有数据后判断是否结束，就很自然，但是我是每到*就输出一部分，所以被迫缝补了很狼狈的特判。



代码

```python
class node:
    def __init__(self,value):
        self.value=value
        self.file=[]
        self.dir=[]
        
def output(node,tab):
    totab="|     "
    if node.value!="": 
        print(totab*tab +node.value)
    for i in node.dir:
        output(i,tab+1)
    for j in sorted(node.file):
        print(totab*tab + j)


dataset=0
wrap=False

def start():
    global stack
    global now
    global p
    global dataset
    global wrap
    stack=[node("")]
    now=stack[0]
    p=0
    dataset+=1
    if dataset>1: 
        wrap=True
        
start()      
while True:
    i=input()
    if wrap and i!="#":
        print("")
        wrap=False
    if i=="#":
        break
    if i=="*":
        print(f"DATA SET {dataset}:")
        print("ROOT")
        output(stack[0],0)      
        start()
    
    if i[0]=="f":
        now.file.append(i)
    elif i[0]=="d":
        i=node(i)
        now.dir.append(i)
        stack.append(i)
        p=len(stack)-1
        now=stack[p]
    elif i=="]":
        stack.pop()
        p-=1
        now=stack[p]

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1711196449470](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1711196449470.png)



### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/



思路：



代码

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def build_tree(postfix):
    stack = []
    for char in postfix:
        node = TreeNode(char)
        if char.isupper():
            node.right = stack.pop()
            node.left = stack.pop()
        stack.append(node)
    return stack[0]

def level_order_traversal(root):
    queue = [root]
    traversal = []
    while queue:
        node = queue.pop(0)
        traversal.append(node.value)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return traversal

n = int(input().strip())
for _ in range(n):
    postfix = input().strip()
    root = build_tree(postfix)
    queue_expression = level_order_traversal(root)[::-1]
    print(''.join(queue_expression))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1711197709603](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1711197709603.png)



### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/



思路：



代码

```python
ans=[]        
def build(inorder,postorder):
    if len(inorder)==0:
        return[]
    root=postorder[-1]
    if len(inorder)==1:
        return [root]

    else:
        leftin=inorder[:inorder.index(root)]
        leftpos=postorder[:len(leftin)]
        rightin=inorder[inorder.index(root)+1:]
        rightpos=postorder[len(leftin):len(postorder)-1]
        return ans+[root]+build(leftin,leftpos)+build(rightin,rightpos)
    
inord=list(input())
postord=list(input())
print("".join(build(inord,postord)))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1711286681610](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1711286681610.png)



### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/



思路：



代码

```python
ans=[]    
def build(inorder,preorder):
    if len(inorder)==0:
        return[]
    root=preorder[0]
    if len(inorder)==1:
        return [root]

    else:
        leftin=inorder[:inorder.index(root)]
        leftpre=preorder[1:len(leftin)+1]
        rightin=inorder[inorder.index(root)+1:]
        rightpre=preorder[len(leftin)+1:]
        return ans+build(leftin,leftpre)+build(rightin,rightpre)+[root]
    

while True:
    try:
        preord=list(input())
        inord=list(input())
        ans=[]
        print("".join(build(inord,preord)))
    except EOFError:
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1711293376427](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1711293376427.png)



## 2. 学习总结和收获

虽然花费时间较长，但是还是很有收获的，不过其实可以快点看题解来理解，毕竟基本都是基本题目





