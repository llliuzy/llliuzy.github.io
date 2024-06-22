---
TREE_QUESTIONS Learning
by Apocalypse from PKU
---

### 1.基本概念及实现

**节点Node**：往往是class的对象

**边Edge**

**根节Root**

**路径Path**

**子节点Children**：二叉树可以设left、right；多个可以用列表实现，也可用First child / Next sibling representation化归为二叉树

**父节点Parent**

**兄弟节点Sibling**

**字树Subtree**

**叶节点Leaf Node**

**层级Level**：根节点的层级为零

**高度Height**

**二叉树深度**

### 2.基本操作模块及实现

**1.二叉树深度：**

```python
def tree_depth(node):
    if node is None:
        return 0
    left_depth = tree_depth(node.left)
    right_depth = tree_depth(node.right)
    return max(left_depth, right_depth) + 1
```



---

**2.二叉树的读取与建立：**

输入为每个节点的子节点：

```python
nodes = [TreeNode() for _ in range(n)]

for i in range(n):
    left_index, right_index = map(int, input().split())
    if left_index != -1:
        nodes[i].left = nodes[left_index]
    if right_index != -1:
        nodes[i].right = nodes[right_index]
```

但要注意，这里的index随题目要求而改变，即从0开始还是从1开始的问题，可能要-1

---

括号嵌套树的解析建立：

```python
def parse_tree(s):
    stack = []
    node = None
    for char in s:
        if char.isalpha():  # 如果是字母，创建新节点
            node = TreeNode(char)
            if stack:  # 如果栈不为空，把节点作为子节点加入到栈顶节点的子节点列表中
                stack[-1].children.append(node)
        elif char == '(':  # 遇到左括号，当前节点可能会有子节点
            if node:
                stack.append(node)  # 把当前节点推入栈中
                node = None
        elif char == ')':  # 遇到右括号，子节点列表结束
            if stack:
                node = stack.pop()  # 弹出当前节点
    return node  # 根节点
```

---



**3.二叉树叶节点计数**：

```python
def count_leaves(node):
    if node is None:
        return 0
    if node.left is None and node.right is None:
        return 1
    return count_leaves(node.left) + count_leaves(node.right)
```

---



**4.树的遍历：**

前/后序遍历：

```python
def preorder(node):
    output = [node.value]
    for child in node.children:
        output.extend(preorder(child))
    return ''.join(output)

def postorder(node):
    output = []
    for child in node.children:
        output.extend(postorder(child))
    output.append(node.value)
    return ''.join(output)
```

---

中序遍历：

```python

```

---

分层遍历：（使用bfs）

```python
import deque

class TreeNode:
    def __init__(self, value):
        self.value = value
        self.children = []

def level_Order(root):
    queue = deque()
    queue.append(root)
    
    while len(queue) != 0: # 这里是一个特殊的BFS,以层为单位
        n = len(queue)     
        while n > 0: #一层层的输出结果
            point = queue.popleft()
            print(point.value, end=" ") # 这里的输出是一行
            queue.extend(point.children)
            n -= 1
            
        print()   #要加上 end的特殊语法
```

---



**5.二叉搜索树：**

​	建立：

```py
class TreeNode:
    def __init__(self, value):
        self.val = value
        self.left = None
        self.right = None

def insert(root, value):
    if root is None:
        return TreeNode(value)
    elif value > root.val:
        root.right = insert(root.right, value)  # 递归
    else:
        root.left = insert(root.left, value)
    return root
    
N = int(input())
arr = list(map(int, input().split()))
root = None
for value in arr:
    root = insert(root, value)
```

