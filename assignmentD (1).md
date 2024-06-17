# Assignment #D: May月考

Updated 1654 GMT+8 May 8, 2024

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

### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：



代码

```python
L, M = map(int, input().split())
road = [1] * (L + 1)
for times in range(M):
    x, y = map(int, input().split())
    for i in range(x, y + 1):
        if road[i] == 1:
            road[i] = 0
tree = 0
for i in road:
    tree += i
print(tree)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240521201939162](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240521201939162.png)



### 20449: 是否被5整除

http://cs101.openjudge.cn/practice/20449/



思路：



代码

```python
A=input()
ni = 0
ans = ''
for i in range(len(A)):
    ni = ni*2 + int(A[i])
    ans += str(int(ni % 5 == 0))
print(ans)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240521202509218](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240521202509218.png)



### 01258: Agri-Net

http://cs101.openjudge.cn/practice/01258/



思路：



代码

```python
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

n = int(input())
ldict = []
for i in range(n):
    inp = list(map(int,input().split()))
    for j in range(i, n):
        ldict.append([i,j,inp[j]])
ldict.sort(key = lambda x:x[2])
dis = DisjSet(n)
num = 0
suml = 0
for line in ldict:
    if dis.find(line[0]) == dis.find(line[1]):
        continue
    dis.union(line[0], line[1])
    num += 1
    suml += line[2]
    if num == n-1:
        print(suml)
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 27635: 判断无向图是否连通有无回路(同23163)

http://cs101.openjudge.cn/practice/27635/



思路：



代码

```python
def find_root( p , l_p ) :
    while p != l_p[ p ] :
        p = l_p[ p ]
    return p
n , m = list( map( int , input().split() ) )
bool_loop = False
n_trees = n
l_parent = [ i for i in range( n ) ]
for i in range( m ) :
    u , v = list( map( int , input().split() ) )
    root_u = find_root( u , l_parent )
    root_v = find_root( v , l_parent )
    if root_u == root_v :
        bool_loop = True
    else :
        l_parent[ root_u ] , l_parent[ root_v ] = min( root_u , root_v ) , min( root_u , root_v )
        n_trees -= 1
if n_trees == 1 :
    print( 'connected:yes' )
else :
    print( 'connected:no' )
if bool_loop :
    print( 'loop:yes' )
else :
    print( 'loop:no' )
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240521234711222](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240521234711222.png)





### 27947: 动态中位数

http://cs101.openjudge.cn/practice/27947/



思路：



代码

```python
import heapq

def find_medians(nums):
    max_heap = []
    min_heap = []
    medians = []

    for num in nums:
        if len(max_heap) == len(min_heap):
            heapq.heappush(max_heap, -heapq.heappushpop(min_heap, num))
        else:
            heapq.heappush(min_heap, -heapq.heappushpop(max_heap, -num))
        if ( len(max_heap) + len(min_heap) ) % 2 == 1:
            medians.append( -max_heap[ 0 ] )

    return medians

T = int( input() )
for _ in range( T ):
    nums = [ int( x ) for x in input().split() ]
    medians = find_medians( nums )
    #print( medians )
    print( len( medians ) )
    print( ' '.join( map( str , medians ) ) )
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240521234815383](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240521234815383.png)



### 28190: 奶牛排队

http://cs101.openjudge.cn/practice/28190/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	时间没有规划好，就做了四道题……期末考前会全部复习完的……





