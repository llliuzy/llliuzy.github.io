# Assignment #4: 排序、栈、队列和树

Updated 0005 GMT+8 March 11, 2024

2024 spring, Complied by ====



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

### 05902: 双端队列

http://cs101.openjudge.cn/practice/05902/



思路：



代码

```python
from collections import deque
t=int(input())
for _ in range(t):
    n=int(input())
    l=deque()
    for _ in range(n):
        typ,num=map(int,input().split())
        flag=0
        if typ==1:
            l.append(str(num))
        else:
            if len(l)>0:                
                if num==0:
                    l.popleft()
                else:
                    l.pop()
    if len(l)==0: 
        print("NULL")
    else:
        print(" ".join(l))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1710661280117](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1710661280117.png)



### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/



思路：



代码

```python
math=["+","-","*","/"]

def portland(l):
    if len(l)==3:
        x=l[1]+l[0]+l[2]
        x=eval(x)
        return x
    else:
        for i in range(len(l)-2):
            if l[i] in math and l[i+1] not in math and l[i+2] not in math:
                x=l[i+1]+l[i]+l[i+2]
                x=eval(x)
                l.pop(i)
                l.pop(i)
                l[i]=str(x)
                return portland(l)

l=list(input().split())
print("{:.6f}".format(portland(l)))

```



代码运行截图 ==（至少包含有"Accepted"）==

![1710661296331](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1710661296331.png)



### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/



思路：



代码

```python
def infixToPostfix(row):
    precedence={"+":1,"-":1,"*":2,"/":2}
    stack=[];ans=[];number=""
    for i in row:
        if i.isnumeric() or i==".":
            number+=i
        else:
            if number:
                nownum=number
                number=""
                ans.append(nownum)
            if i=="(":
                stack.append(i)
            elif i in "+-*/":
                while stack and stack[-1] in "+-*/" and precedence[i]<=precedence[stack[-1]]:
                    ans.append(stack.pop())
                stack.append(i)
            elif i==")":
                while stack and stack[-1]!="(":
                    ans.append(stack.pop())
                stack.pop()
    if number:
        ans.append(number)
    while stack:
        ans.append(stack.pop())
    print(" ".join(ans))

n=int(input())
for _ in range(n):
    row=list(input())
    infixToPostfix(row)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1710663604289](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1710663604289.png)



### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/



思路：



代码

```python
x=list(input())
while True:
    try:
        flag=0
        out=list(input())
        stack=x
        i=0
        bank=[]
        if len(stack)!=len(out):
            flag=1
        while out:           
            if (not bank) and i<len(stack):
                bank.append(stack[i])
                i+=1
            else:
                if bank[-1]!=out[0] and i<len(stack):
                    bank.append(stack[i])
                    i+=1
                elif bank[-1]==out[0]:
                    bank.pop()
                    out=out[1:]
                else:
                    flag=1
                    break
              
        if flag==0:
            print("YES")
        else:
            print("NO")
                
    
    except EOFError:
        break

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1710596910885](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1710596910885.png)



### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/



思路：



代码

```python
class treeNode:
    def __init__(self):
        self.left=None
        self.right=None

def count_height(node):
    if node==None:
        return 0
    else:
        return max(count_height(node.left),count_height(node.right))+1
    
n=int(input())
tree=[treeNode() for _ in range(n)]
hasParent=[i for i in range(n)]
ans=0
for i in range(n):
    left,right=map(int,input().split())
    if left!=-1: 
        tree[i].left=tree[left-1]
        hasParent.remove(left-1)
    if right!=-1: 
        tree[i].right=tree[right-1]
        hasParent.remove(right-1)
    if left==right==-1:
        ans+=1

print(f"{count_height(tree[hasParent[0]])}")


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1710596973355](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1710596973355.png)



### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/



思路：



代码

```python
import bisect
while True:
    n=int(input())
    if n==0:
        break
    else:
        ans=0;l=[]
        for _ in range(n):
            t=int(input())
            ans+=len(l)-(bisect.bisect_left(l,t))
            bisect.insort_left(l, t) 
        print(ans)
            

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![1710597582080](C:\Users\31861\Documents\WeChat Files\wxid_24oti6txro6z22\FileStorage\Temp\1710597582080.png)



## 2. 学习总结和收获

每日选做尽力在做了但是到目前为止只AC了41道 （Orz

尝试建立了github网站但是还不太会用，网上找不到比较好的学习资源，准备以后有整块时间了再研究





