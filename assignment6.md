# Assignment #6: "树"算：Huffman,BinHeap,BST,AVL,DisjointSet

Updated 2214 GMT+8 March 24, 2024

2024 spring, Complied by ==吴彦玺 物理学院==



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

​	深夜做题使人快乐

​	约15min，模板题，修了些小bug：

​	attention：

​	1.houxu()一定要先判断self.left、self.right是不是None，不然会报错

​	2.最后把列表合成字符串输出时要用map把列表里的每个数字转成str

代码

```python
class Node:
    def __init__(self,value=None):
        self.value = value
        self.left = None
        self.right = None

    def houxu(self):
        out = []
        if self.left:
            out += self.left.houxu()
        if self.right:
            out += self.right.houxu()
        out.append(self.value)
        return out
    
class Tree:
    def __init__(self,rootval):
        self.root = Node(rootval)

    def add(self,val):
        pos_node = self.root
        while True:
            if val > pos_node.value:
                if pos_node.right:
                    pos_node = pos_node.right
                else:
                    pos_node.right = Node(val)
                    break
            else:
                if pos_node.left:
                    pos_node = pos_node.left
                else:
                    pos_node.left = Node(val)
                    break

    def treehouxu(self):
        return self.root.houxu()
    
n = int(input())
inp = list(map(int,input().split()))
tree = Tree(inp[0])
for i in inp[1:]:
    tree.add(i)
print(' '.join(map(str,tree.treehouxu())))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240330013219791](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240330013219791.png)



### 05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/



思路：

​	15min

​	没啥好说的，用队列实现按层次输出

代码

```python
import queue

class Node:
    def __init__(self,value=None):
        self.value = value
        self.left = None
        self.right = None

class Tree:
    def __init__(self,rootval):
        self.root = Node(rootval)

    def add(self,val):
        pos_node = self.root
        while True:
            if val > pos_node.value:
                if pos_node.right:
                    pos_node = pos_node.right
                else:
                    pos_node.right = Node(val)
                    break
            elif val < pos_node.value:
                if pos_node.left:
                    pos_node = pos_node.left
                else:
                    pos_node.left = Node(val)
                    break
            else:break

    def levelorder(self):
        pos = queue.Queue()
        pos.put(self.root)
        output = []
        while not pos.empty():
            pos_node = pos.get()
            output.append(pos_node.value)
            if pos_node.left:
                pos.put(pos_node.left)
            if pos_node.right:
                pos.put(pos_node.right)
        return output
    
inp = list(map(int,input().split()))
tree = Tree(inp[0])
for i in inp[1:]:
    tree.add(i)
print(' '.join(map(str,tree.levelorder())))
```



代码运行截图 ==（至少包含有"Accepted"）==

<img src="C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240330015443762.png" alt="image-20240330015443762" style="zoom:80%;" />



### 04078: 实现堆结构

http://cs101.openjudge.cn/practice/04078/

练习自己写个BinHeap。当然机考时候，如果遇到这样题目，直接import heapq。手搓栈、队列、堆、AVL等，考试前需要搓个遍。



思路：

​	大于1h，死于debug

​	实现堆结构要注意的点是，绝对不能让最前面占位的数进入判断（第10行要明确范围）

代码

```python
class Minheap:
    def __init__(self):
        self.lis = [0]
        self.length = 0
    
    def heappush(self,val):
        self.lis.append(val)
        self.length += 1
        pos_index = self.length
        while pos_index > 1:
            if self.lis[pos_index] < self.lis[pos_index//2]:
                self.lis[pos_index],self.lis[pos_index//2]=self.lis[pos_index//2],self.lis[pos_index]
                pos_index = pos_index//2
            else:break

    def heappop(self):
        if self.length == 0:
            return None
        if self.length == 1:
            self.length -= 1
            return self.lis.pop()
        val = self.lis.pop()
        self.length -= 1
        output = self.lis[1]
        self.lis[1] = val
        pos_index = 1
        l = self.length
        while pos_index*2+1 <= l:
            if self.lis[pos_index] <= min(self.lis[pos_index*2],self.lis[pos_index*2+1]):
                break
            if self.lis[pos_index*2] <= self.lis[pos_index*2+1]:
                self.lis[pos_index],self.lis[pos_index*2]=self.lis[pos_index*2],self.lis[pos_index]
                pos_index = pos_index*2
            else:
                self.lis[pos_index],self.lis[pos_index*2+1]=self.lis[pos_index*2+1],self.lis[pos_index]
                pos_index = pos_index*2+1
        else:
            if pos_index*2 == l:
                mini = min(self.lis[pos_index],self.lis[pos_index*2])
                maxi = max(self.lis[pos_index],self.lis[pos_index*2])
                self.lis[pos_index],self.lis[pos_index*2]=mini,maxi
        return output

n = int(input())
heapq = Minheap()
for i in range(n):
    inp = input()
    if inp == '2':
        out = heapq.heappop()
        if out:
            print(out)
        continue
    typ, val = map(int,inp.split())
    if typ == 1:
        heapq.heappush(val)
        #print(heapq.lis)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240330112215363](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240330112215363.png)



### 22161: 哈夫曼编码树

http://cs101.openjudge.cn/practice/22161/



思路：



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
        merged = Node(left.weight + right.weight) #note: 合并后，char 字段默认值是空
        merged.left = left
        merged.right = right
        heapq.heappush(heap, merged)
    return heap[0]
def encode_huffman_tree(root):
    codes = {}
    def traverse(node, code):
        if node.char:
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
        if node.char:
            decoded += node.char
            node = root
    return decoded
n = int(input())
characters = {}
for _ in range(n):
    char, weight = input().split()
    characters[char] = int(weight)
huffman_tree = build_huffman_tree(characters)
codes = encode_huffman_tree(huffman_tree)
strings = []
while True:
    try:
        line = input()
        if line:
            strings.append(line)
        else:
            break
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

![image-20240330183318778](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240330183318778.png)



### 晴问9.5: 平衡二叉树的建立

https://sunnywhy.com/sfbj/9/5/359



思路：

​	麻了……试着自己写一遍ATLTree，发现好麻烦

​	由于这道题是在我之前写的树模板上修改的，所以有些多余的东西（比如queue和很多大小判断）

代码

```python
import queue

# 定义节点
class Node:
    # key用于判断，没有即退化为用value判断
    # 需要的话后面算法自己加parent！
    def __init__(self, key, value=None, parent=None, balancer=0):
        self.key = key
        self.value = value if value is not None else key
        self.left = None
        self.right = None
        self.parent = parent
        self.balancer = balancer

    # 节点大小比较定义（按需取用）
    def __lt__(self, other):
        return self.key < other.key
    def __gt__(self, other):
        return self.key > other.key
    def __eq__(self, other):
        return self.key == other.key
    def __le__(self, other):
        return self.key <= other.key
    def __ge__(self, other):
        return self.key >= other.key
    def __ne__(self, other):
        return self.key != other.key
    
    # 前序遍历（以列表格式输出）
    # 中后序自己换位置
    def preorder(self):
        if not self:
            return []
        output = [self.value]
        output += self.left.preorder()
        output += self.right.preorder()
        return output

# 建立AVL树
class AVLTree():
    def __init__(self):
        self.root = None
    
    # 插入节点总算法
    def put(self, key, val=None):
        if self.root:
            self._put(key, val, self.root)
        else:
            self.root = Node(key,val)

    # 插入节点中递归部分
    # 假定无重复点
    def _put(self, val, key, pos_node):
        put_node = Node(val, key)
        if put_node < pos_node:
            if pos_node.left:
                self._put(val, key, pos_node.left)
            else:
                put_node.parent = pos_node
                pos_node.left = put_node
                self.updatebalance(pos_node.left)
        else:
            if pos_node.right:
                self._put(val, key, pos_node.right)
            else:
                put_node.parent = pos_node
                pos_node.right = put_node
                self.updatebalance(pos_node.right)
    
    # 更新树的平衡状况
    def updatebalance(self, node, mode='put'):
        if mode == 'put':
            if node.balancer > 1 or node.balancer < -1:
                self.rebalance(node)
                return
            if node.parent is not None:
                if node is node.parent.left:
                    node.parent.balancer += 1
                else:
                    node.parent.balancer -= 1
                if node.parent.balancer != 0:
                    self.updatebalance(node.parent)

    # 定义左旋
    def rotateleft(self, rotroot):
        newroot = rotroot.right
        rotroot.right = newroot.left
        if newroot.left is not None:
            newroot.left.parent = rotroot
        newroot.parent = rotroot.parent
        if rotroot is self.root:
            self.root = newroot
        else:
            if rotroot.parent.left is rotroot:
                rotroot.parent.left = newroot
            else:
                rotroot.parent.right = newroot
        newroot.left = rotroot
        rotroot.parent = newroot
        rotroot.balancer = rotroot.balancer + 1 - min(newroot.balancer, 0)
        newroot.balancer = newroot.balancer + 1 + max(rotroot.balancer, 0)

    # 定义右旋
    def rotateright(self, rotroot):
        newroot = rotroot.left
        rotroot.left = newroot.right
        if newroot.right is not None:
            newroot.right.parent = rotroot
        newroot.parent = rotroot.parent
        if rotroot is self.root:
            self.root = newroot
        else:
            if rotroot.parent.left is rotroot:
                rotroot.parent.left = newroot
            else:
                rotroot.parent.right = newroot
        newroot.right = rotroot
        rotroot.parent = newroot
        rotroot.balancer = rotroot.balancer - 1 - max(newroot.balancer, 0)
        newroot.balancer = newroot.balancer - 1 + min(rotroot.balancer, 0)

    # 改变树使其平衡
    def rebalance(self,node):
        if node.balancer < 0:
            if node.right.balancer > 0:
                self.rotateright(node.right)
            self.rotateleft(node)
        elif node.balancer > 0:
            if node.left.balancer < 0:
                self.rotateleft(node.left)
            self.rotateright(node)

    # 前序遍历（列表输出，中后序自己换位置）
    def preorder(self,node=None):
        if node is None:
            if self.root:
                node = self.root
            else: return None
        output = [node.value]
        if node.left:
            output += self.preorder(node.left)
        if node.right:
            output += self.preorder(node.right)
        return output

n = int(input())
inp = list(map(int,input().split()))
tree = AVLTree()
for i in inp:
    tree.put(i)
output = tree.preorder()
print(' '.join(list(map(str, output))))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240401175944862](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240401175944862.png)



### 02524: 宗教信仰

http://cs101.openjudge.cn/practice/02524/



思路：

​	还算简单

​	但这个算法耗时较长，两种合并方式时间都是1400ms

代码

```python
# 并查集数据结构，按树高合并
class DisjSet:
    def __init__(self, n):
        # 节点高度
        self.rank = [1 for _ in range(n)]
        # 存储其上节点，尽可能为根节点
        self.parent = [i for i in range(n)]
    
    # 查找某个节点的上级根节点（代表）
    def find(self,x):
        if (self.parent[x] != x):
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    # 合并x，y所在的两棵树
    def union(self, x, y):
        xset = self.find(x)
        yset = self.find(y)
        if xset == yset:
            return
        if self.rank[xset] < self.rank[yset]:
            self.parent[xset] = yset
        elif self.rank[xset] > self.rank[yset]:
            self.parent[yset] = xset
        else:
            self.parent[yset] = xset
            self.rank[xset] = self.rank[xset] + 1

cases = 0
while True:
    cases += 1
    n, m = map(int,input().split())
    if n == 0 and m == 0:
        break
    disj = DisjSet(n)
    for _ in range(m):
        a, b = map(int,input().split())
        disj.union(a-1, b-1)
    output = 0
    for i in range(n):
        output += (disj.find(i) == i)
    print('Case %d: %d'%(cases, output))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240401194342249](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240401194342249.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	作业题目不简单……

​	学AVL学了好几个小时才弄懂……自己写好了代码可以到时候带进考场。



