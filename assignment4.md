# Assignment #4: 排序、栈、队列和树

Updated 0005 GMT+8 March 11, 2024

2024 spring, Complied by ==吴彦玺 物理学院==



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

​	还是练习了下类的写法（假的，从队列头弹出元素还是O（n）），大概花了半个小时，不熟练

​	当然正式考试时这种东西肯定直接用双端队列就好

代码

```python
class Queue:
    def __init__(self):
        self.item = []

    def is_empty(self):
        return self.item==[]
    
    def enqueue(self,value):
        if self.is_empty():
            self.item = [value]
        else:
            self.item.append(value)
    
    def left_pop(self):
        if self.is_empty():
            return None
        else:
            k = self.item[0]
            self.item=self.item[1:]
            return k

    def right_pop(self):
        if self.is_empty():
            return None
        else:
            return self.item.pop()

t = int(input())
for times in range(t):
    n = int(input())
    q = Queue()
    for h in range(n):
        type,c = input().split()
        if type == '1':
            q.enqueue(c)
        elif c=='0':
            q.left_pop()
        else:
            q.right_pop()
    if q.is_empty():
        print('NULL')
    else:
        print(' '.join(q.item))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240315221324864](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240315221324864.png)



### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/



思路：

​	上个学期做的

代码

```python
def polish(a):
    while len(a) != 1:
        k = False
        begin = 1
        for i in range(begin, len(a)):
            if a[i] in ('+', '-', '*', '/'):
                k = False
            elif k:
                if a[i-2] == '+':
                    a[i] = float(a[i-1]) + float(a[i])
                elif a[i-2] == '-':
                    a[i] = float(a[i-1]) - float(a[i])
                elif a[i-2] == '*':
                    a[i] = float(a[i-1]) * float(a[i])
                else:
                    a[i] = float(a[i-1]) / float(a[i])
                del a[i-1], a[i-2]
                #print(a)
                begin = i-2
                break
            else:k = True
    print("%.6f" % a[0])
a = list(input().split())
polish(a)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240317110614999](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240317110614999.png)



### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/



思路：



代码

```python
opers = {'+':1, '-':1, '*':2, '/':2}
def change(inp):
    num = ''
    output = []
    oper = []
    for char in inp:
        if char.isnumeric() or char == '.':
            num += char
        else:
            if num:
                output.append(num)
                num = ''
            if char == '(':
                oper.append(char)
            elif char in '+-*/':
                while oper and oper[-1] in '+-*/' and opers[char] <= opers[oper[-1]]:
                    output.append(oper.pop())
                oper.append(char)
            elif char == ')':
                while oper[-1]!= '(':
                    output.append(oper.pop())
                oper.pop()
    if num:
        output.append(num)
    while oper:
        output.append(oper.pop())
    return ' '.join(output)

n = int(input())
for _ in range(n):
    print(change(input()))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240318224420424](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240318224420424.png)



### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/



思路：

​	纯粹用栈输出，但忘记考虑两个字符串长度不一样的情况。

​	因此花了25min

代码

```python
def compare(tor,tee):
    if len(tor) != len(tee):
        return 'NO'
    stack = []
    end = len(tor)
    index_tor, index_tee = 0, 0
    while index_tee < end:
        if index_tor >= end:
            while stack:
                if stack[-1] != tee[index_tee]:
                    return 'NO'
                index_tee += 1
                stack.pop()                
            return 'YES'
        if stack and stack[-1] == tee[index_tee]:
            index_tee += 1
            stack.pop()
        elif tor[index_tor] == tee[index_tee]:
            index_tee += 1
            index_tor += 1
        else:
            stack.append(tor[index_tor])
            index_tor += 1
    return 'YES'
a = input()
while True:
    try:
        b = input()
        print(compare(a,b))
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240318232444110](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240318232444110.png)



### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/



思路：



代码

```python
class Node:
    def __init__(self):
        self.left = None
        self.right = None

def depth(node):
    if node is None:
        return 0

    return max(depth(node.left),depth(node.right))+1

n = int(input())
nodes = [Node() for _ in range(n)]
for i in range(n):
    l,r = map(int,input().split())
    if l != -1:
        nodes[i].left = nodes[l-1]
    if r != -1:
        nodes[i].right = nodes[r-1]

root = nodes[0]
print(depth(root))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240319154025664](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240319154025664.png)



### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/



思路：

​	用归并排序&计数即可

​	大概40min

代码

```python
def qs(sequ,sta,end,swap):
    if end <= sta+1:
        return swap
    mid = (sta+end)//2
    swap = qs(sequ,sta,mid,swap)
    swap = qs(sequ,mid,end,swap)
    a = sequ[sta:mid]
    index_a = 0
    index_tail = mid
    index_sequ = sta
    while index_tail < end:
        if index_a >= mid-sta:
            break
        if a[index_a] <= sequ[index_tail]:
            sequ[index_sequ] = a[index_a]
            index_a += 1
        else:
            sequ[index_sequ] = sequ[index_tail]
            swap += index_tail-index_sequ
            index_tail += 1
        index_sequ += 1
    else:
        for i in range(index_a,mid-sta):
            sequ[index_sequ] = a[i]
            index_sequ += 1
    #print(sequ,swap)
    return swap

while True:
    n = int(input())
    if n == 0:
        break
    sequ = [int(input()) for times in range(n)]
    output = qs(sequ,0,n,0)
    print(output)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240317105702932](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240317105702932.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	学了树是怎么建的。

​	中序转后序是借鉴了老师发的答案，没想到这个算法，记下来就行了。可以考虑带进考场。

