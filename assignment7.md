# Assignment #7: April 月考

Updated 1557 GMT+8 Apr 3, 2024

2024 spring, Complied by ==吴彦玺 物理学院==



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

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/



思路：

​	5min

代码

```python
inp = input()
output = ['']
for i in inp:
    if i != ' ':
        output[-1] = output[-1] + i
    else:
        output.append('')
output.reverse()
print(' '.join(output))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240407210632422](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240407210632422.png)



### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/



思路：

​	9min

​	事实上这道题用不用队列无所谓，因为内存数是固定的，用一个固定长列表即可

代码

```python
m, n = map(int,input().split())
inp = list(map(int,input().split()))
dic = [1001 for _ in range(m)]
search = 0
index = 0
for i in inp:
    if i in dic:
        continue
    search += 1
    dic[index] = i
    index = (index+1) % m
print(search)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240407211549307](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240407211549307.png)



### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/



思路：

​	想复杂了，14min

​	注意考虑k=0等特殊情况

​	期末第三题真的会有这么简单嘛……感觉上周作业明明做得很难受……

代码

```python
n, k = map(int,input().split())
inp = list(map(int,input().split()))
inp.sort()
if k == 0 and inp[0] == 1:
    print(-1)
elif k == 0:
    print(1)
elif k<n and inp[k-1] == inp[k]:
    print(-1)
else:
    print(inp[k-1])
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240407213040361](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240407213040361.png)



### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/



思路：

​	27min

​	在用已有树模板建树还是不建树重新写方法纠结了一下，浪费了点时间

​	发现queue竟然自己还没整理好用法，整一下

代码

```python
import queue

class Node:
    def __init__(self,name=None):
        self.value = name
        self.left = None
        self.right = None

# 二叉树
class BinaryTree:
    # 定义根节点
    def __init__(self):
        self.root = None

    # 后序遍历（列表输出，中后序自己换位置）
    def postorder(self,node=None):
        if node is None:
            if self.root:
                node = self.root
            else: return None
        output = []
        if node.left:
            output += self.postorder(node.left)
        if node.right:
            output += self.postorder(node.right)
        output += [node.value]
        return output
    
n = int(input())
inp = input()
que = queue.Queue()
for i in inp:
    if i == '0':
        que.put(Node('B'))
    else:
        que.put(Node('I'))
while que.qsize() > 1:
    new = Node()
    new.left = que.get()
    new.right = que.get()
    le = new.left.value
    ri = new.right.value
    if le == ri:
        new.value = le
    else:
        new.value = 'F'
    que.put(new)

tree = BinaryTree()
tree.root = que.get()
print(''.join(tree.postorder()))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240408171509222](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240408171509222.png)



### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/



思路：

​	约1h……

​	原来有散客的嘛……

​	总之复习了queue的用法，但听群同学说deque更好，决定以后改用

代码

```python
import queue

t = int(input())
find_group = {}
for group in range(t):
    for i in map(int,input().split()):
        find_group[i] = group

range_of_groups = queue.Queue()

range_in_groups = [queue.Queue() for _ in range(t+1)]

while True:
    inp = input()
    if inp == 'STOP':
        break
    if inp == 'DEQUEUE':
        head = range_of_groups.queue[0]
        print(range_in_groups[head].get())
        if range_in_groups[head].empty():
            range_of_groups.get()
    else:
        num = int(inp[8:])
        if num in find_group:
            group = find_group[num]
        else:
            group = t
        range_in_groups[group].put(num)
        if range_in_groups[group].qsize() == 1:
            range_of_groups.put(group)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240408203720034](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240408203720034.png)



### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/



思路：

​	几乎一小时…………

​	浪费时间的点有很多，大部分是debug，比如这棵树root的查找，之类

​	因为每个节点的值都不一样，可以在建树时用nodedict字典存节点，便于快速找到节点（或者判断树内没有此节点）

代码

```python
#20:39-21：35
class Node:
    def __init__(self, value):
        self.value = value
        self.first = None
        self.next = None
        self.parent = None

    def put(self,child):
        child.parent = self
        if self.first is not None:
            pos_node = self.first
            while pos_node.next is not None:
                pos_node = pos_node.next
            pos_node.next = child
        else:
            self.first = child

    def nodepreorder(self):
        node_range = [self]
        pos_node = self.first
        while pos_node is not None:
            node_range.append(pos_node)
            pos_node = pos_node.next
        node_range.sort(key = lambda x: x.value)
        output = []
        for pos_node in node_range:
            if pos_node is not self:
                output += pos_node.nodepreorder()
            else:
                output += [self.value]
        return output

class Nodeset:
    def __init__(self):
        self.root = None
        self.nodedict = {}
    
    def find(self, value):
        if value not in self.nodedict:
            self.nodedict[value] = Node(value)
        return self.nodedict[value]

    def put(self, parentvalue, childvalue):
        parent = self.find(parentvalue)
        child = self.find(childvalue)
        parent.put(child)
        if self.root in (None, child, parent):
            pos_node = parent
            while pos_node.parent is not None:
                pos_node = pos_node.parent
            self.root = pos_node
    
    def preorder(self):
        if self.root is None:
            return ''
        else:
            output = '\n'.join(list(map(str, self.root.nodepreorder())))
            return output

n = int(input())
faketree = Nodeset()
for _ in range(n):
    inp = list(map(int,input().split()))
    parentvalue = inp[0]
    for childvalue in inp[1:]:
        faketree.put(parentvalue, childvalue)
output = faketree.preorder()
print(output)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240408214015358](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240408214015358.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	没有很多时间学数算，期中压力比较大……

​	不过作业算是独立完成，虽然用时偏长



