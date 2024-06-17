# Assignment #5: "树"算：概念、表示、解析、遍历

Updated 2124 GMT+8 March 17, 2024

2024 spring, Complied by ==吴彦玺 物理学院==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：win11

Python编程环境：vscode

C/C++编程环境：无



## 1. 题目

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/



思路：

​	这道题是最后做出来的hhh，还是不知道哪里出错了

​	前前后后花了大半个小时？算了（

代码

```python
class Node:
    def __init__(self, value=None):
        self.val = value
        self.left = None
        self.right = None

n = int(input())
tree = [Node() for _ in range(n)]
if_root = [True for _ in range(n)]
Leaf = 0

def height(i):
    if not i: return -1
    return max(height(i.left), height(i.right))+1


for i in range(n):
    l,r = map(int,input().split())
    if l == -1 and r == -1:
        Leaf += 1
    else:
        if l != -1:
            if_root[l] = False
            tree[i].left = tree[l]
        if r != -1:
            if_root[r] = False
            tree[i].left = tree[r]

for i in range(n):
    if if_root[i]:
        print(height(tree[i]), Leaf)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240322104304330](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240322104304330.png)



### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/



思路：

​	自己想了一个前序后序输出，前序比较简单，但后序就卡了很久，经同学提醒才意识到和前序是一样的。

​	约60min

代码

```python
class Node:
    def __init__(self,value=None,parent=None):
        self.value = value
        self.child = None
        self.sibling = None
        self.parent = parent

    def add_child(self,childname):
        self.child = Node(childname,self)
        return self.child
    
    def add_sibling(self, siblingname):
        self.sibling = Node(siblingname,self.parent)
        return self.sibling

    def head_output(self):
        output = [self.value]
        node_possessing = self.child
        while node_possessing:
            output += node_possessing.head_output()
            node_possessing = node_possessing.sibling
        return output
    
    def tail_output(self):
        output = []
        node_possessing = self.child
        while node_possessing:
            output += node_possessing.tail_output()
            node_possessing = node_possessing.sibling
        output.append(self.value)    
        return output

class Tree:
    def __init__(self,rootname):
        self.root = Node(rootname)

origin_tree = input()
tree = Tree(origin_tree[0])
parents = [tree.root]

for i in origin_tree[1:]:
    if i in ('(',','):
        parents.append(i)
        continue
    elif i == ')':
        while parents[-1] != '(':
            parents.pop()
        parents.pop()
        continue
    if parents[-1] == '(':
        parents.append(parents[-2].add_child(i))
    elif parents[-1] == ',':
        parents.pop()
        parents.append(parents.pop().add_sibling(i))
    
print(''.join(tree.root.head_output()))
print(''.join(tree.root.tail_output()))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240321131346735](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240321131346735.png)



### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/



思路：

​	1h+

​	啊啊啊啊这是啥啊

​	我的代码相比解答来说长了很多，主要长在10-25行，为了让思路更加清晰，我把一些小步骤拆开了单独写，导致重复部分较多。

​	到目前为止，我还是看见一道题新建一个类，命名之类的都是现想的，这是没有必要的。应该总结出一个普适性比较高的类复制到各个题目使用。

代码

```python
shower = '|     '
class dir:
    def __init__(self,dirname,depth=0):
        self.name = dirname
        self.childir = None
        self.files = []
        self.sibdir = None
        self.depth = depth
    
    def add_childir(self,childirname):
        self.childir = dir(childirname,self.depth+1)
        return self.childir
    
    def add_sibdir(self,sibdirname):
        self.sibdir = dir(sibdirname,self.depth)
        return self.sibdir
    
    def add_dir(self,newdirname):
        dir_possessing = self.childir
        if not dir_possessing:
            return self.add_childir(newdirname)
        else:
            while dir_possessing.sibdir:
                dir_possessing = dir_possessing.sibdir
            return dir_possessing.add_sibdir(newdirname)
    
    def add_file(self,filename):
        self.files.append(filename)
    
    def show(self):
        output = [shower*self.depth+self.name]
        dir_possessing = self.childir
        while dir_possessing:
            output += dir_possessing.show()
            dir_possessing = dir_possessing.sibdir
        files_possessing = sorted(self.files)
        for filename in files_possessing:
            output.append(shower*self.depth+filename)
        return output
    
class Root:
    def __init__(self,name):
        self.root = dir('ROOT')
        self.name = name
n = 1
while True:
    tree = Root(n)
    fdirs = [tree.root]
    while True:
        inp = input()
        if inp == '#': break
        if inp[0] == 'd':
            fdirs.append(fdirs[-1].add_dir(inp))
        elif inp[0] == 'f':
            fdirs[-1].add_file(inp)
        elif inp == ']':
            fdirs.pop()
        elif inp == '*':
            print(f'DATA SET {n}:')
            print('\n'.join(tree.root.show())+'\n')
            break
    if inp == '#': break
    n += 1
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240321165504061](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240321165504061.png)



### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/



思路：

​	做了很久……

​	算了也没啥好说的

代码

```python
class Node:
    def __init__(self,name):
        self.name = name
        self.left = None
        self.right = None
    
def show(nodes):
    if not nodes:return ''
    output = ''
    next = []
    for node in nodes:
        output += node.name
        for i in (node.left,node.right):
            if i:next.append(i)
    output += show(next)
    return output
        
n = int(input())
for _ in range(n):
    tail = input()
    stack = []
    for i in tail:
        if i.islower():
            stack.append(Node(i))
        else:
            pos = Node(i)
            pos.right = stack.pop()
            pos.left = stack.pop()
            stack.append(pos)

    output = show(stack)[::-1]
    print(output)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240321181922034](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240321181922034.png)



### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/



思路：

​	约20min

​	利用后序表达式的特点提取根的信息，将中序表达式切片，从而完成递归

​	所有的递归都要考虑边界情况

代码

```python
class Node:
    def __init__(self,name):
        self.val = name
        self.left = None
        self.right = None

def build_tree(mid,back):
    if len(mid) == 0:   return None
    if len(mid) == 1:   return Node(mid)
    parentname = back[-1]
    l,r = mid.split(parentname)
    parent = Node(parentname)
    parent.left = build_tree(l,back[:len(l)])
    parent.right = build_tree(r,back[len(l):-1])
    return parent

def showformer(node):
    if not node:    return ''
    output = node.val
    output += showformer(node.left)
    output += showformer(node.right)
    return output

mid = input()
back = input()
tree = build_tree(mid,back)
print(showformer(tree))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240322101415092](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240322101415092.png)



### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/



思路：

​	和上一题一样，改几个字母就行（怎么是这样的作业，乐），所以只花了几分钟

​	注意 try-except 语句的运用，这个需要记下来

代码

```python
class Node:
    def __init__(self,name):
        self.val = name
        self.left = None
        self.right = None

def build_tree(head,mid):
    if len(mid) == 0:   return None
    if len(mid) == 1:   return Node(mid)
    parentname = head[0]
    l,r = mid.split(parentname)
    parent = Node(parentname)
    parent.left = build_tree(head[1:len(l)+1],l)
    parent.right = build_tree(head[len(l)+1:],r)
    return parent

def showlatter(node):
    if not node:    return ''
    output = showlatter(node.left)
    output += showlatter(node.right)
    output += node.val
    return output

while True:
    try:
        head = input()
        mid = input()
        tree = build_tree(head,mid)
        print(showlatter(tree))
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240322102325718](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240322102325718.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	越做到后面越熟练了，感觉收获很大。

​	try—except语句要记得带进考场



