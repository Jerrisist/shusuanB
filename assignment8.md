# Assignment #8: 图论：概念、遍历，及 树算

Updated 1919 GMT+8 Apr 8, 2024

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

### 19943: 图的拉普拉斯矩阵

matrices, http://cs101.openjudge.cn/practice/19943/

请定义Vertex类，Graph类，然后实现



思路：

​	之前做过……

代码

```python
n, m = map(int,input().split())
matrix = [[0]*n for _ in range(n)]
for _ in range(m):
    a, b = map(int,input().split())
    matrix[a][a] += 1
    matrix[b][b] += 1
    matrix[a][b] = matrix[b][a] = -1
for row in range(n):
    print(' '.join(map(str,matrix[row])))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240416185827632](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240416185827632.png)



### 18160: 最大连通域面积

matrix/dfs similar, http://cs101.openjudge.cn/practice/18160



思路：

​	之前也做过……

代码

```python
nCases = int(input())
for _ in range(nCases):
    n, m = map(int,input().split())
    lake = []
    for _ in range(n):
        lake.append(['.'] + list(input()) + ['.'])
    lake.append(['.']*(m+2))
    sm = 0
    for i_b in range(n):
        for j_b in range(1,m+1):
            if lake[i_b][j_b] == 'W':
                lake[i_b][j_b] = '.'
                s = 1
                dfs = [(i_b, j_b)]
                while len(dfs) != 0:
                    posi = dfs[0]
                    del dfs[0]
                    for delta_i in (-1,0,1):
                        for delta_j in (-1,0,1):
                            if lake[posi[0]+delta_i][posi[1]+delta_j] == 'W':
                                s += 1
                                dfs.append((posi[0]+delta_i,posi[1]+delta_j))
                                lake[posi[0]+delta_i][posi[1]+delta_j] = '.'
                sm = max(s,sm)
    print(sm)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240416185927662](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240416185927662.png)



### sy383: 最大权值连通块

https://sunnywhy.com/sfbj/10/3/383



思路：



代码

```python
from collections import defaultdict

def dfs(node, graph, visited, values):
    visited[node] = True
    current_value = values[node]
    for neighbor in graph[node]:
        if not visited[neighbor]:
            current_value += dfs(neighbor, graph, visited, values)
    return current_value

def max_connected_block_value(n, m, values, edges):
    graph = defaultdict(list)
    for edge in edges:
        graph[edge[0]].append(edge[1])
        graph[edge[1]].append(edge[0])

    visited = [False] * n
    max_value = 0
    for i in range(n):
        if not visited[i]:
            block_value = dfs(i, graph, visited, values)
            max_value = max(max_value, block_value)

    return max_value

n, m = map(int, input().split())
values = list(map(int, input().split()))
edges = [list(map(int, input().split())) for _ in range(m)]

result = max_connected_block_value(n, m, values, edges)
print(result)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240416190450189](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240416190450189.png)



### 03441: 4 Values whose Sum is 0

data structure/binary search, http://cs101.openjudge.cn/practice/03441



思路：



代码

```python
from collections import defaultdict

def find_quadruplets_sum_zero(n, A, B, C, D):
    sum_ab = defaultdict(int)
    for a in A:
        for b in B:
            sum_ab[a + b] += 1
    
    count = 0
    for c in C:
        for d in D:
            if -c - d in sum_ab:
                count += sum_ab[-c - d]
    
    return count

# Read input
n = int(input())
A, B, C, D = [], [], [], []
for _ in range(n):
    a, b, c, d = map(int, input().split())
    A.append(a)
    B.append(b)
    C.append(c)
    D.append(d)

# Find and print the number of quadruplets whose sum is zero
result = find_quadruplets_sum_zero(n, A, B, C, D)
print(result)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240416190614192](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240416190614192.png)



### 04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

Trie 数据结构可能需要自学下。



思路：



代码

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_number = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, number):
        node = self.root
        for digit in number:
            if digit not in node.children:
                node.children[digit] = TrieNode()
            node = node.children[digit]
            if node.is_end_of_number:
                return False
        node.is_end_of_number = True
        return len(node.children) == 0
    
def check_consistency(t, test_cases):
    results = []
    for i in range(t):
        n = test_cases[i][0]
        numbers = test_cases[i][1:]
        
        trie = Trie()
        consistent = True
        for number in numbers:
            if not trie.insert(number):
                consistent = False
                break
        
        if consistent:
            results.append("YES")
        else:
            results.append("NO")
    
    return results

# 读取输入
t = int(input())
test_cases = []
for _ in range(t):
    n = int(input())
    numbers = [input() for _ in range(n)]
    test_cases.append([n] + numbers)

# 检查一致性并输出结果
results = check_consistency(t, test_cases)
for result in results:
    print(result)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240416190725286](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240416190725286.png)



### 04082: 树的镜面映射

http://cs101.openjudge.cn/practice/04082/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	最近期中考试非常紧张…………没有很多时间学数算……考完期中再把这些题目补上



