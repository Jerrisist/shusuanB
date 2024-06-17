# Assignment #F: All-Killed 满分

Updated 1844 GMT+8 May 20, 2024

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

### 22485: 升空的焰火，从侧面看

http://cs101.openjudge.cn/practice/22485/



思路：



代码

```python
class Node:
    def __init__( self , value , left , right ) :
        self.value = value
        self.left = left
        self.right = right
        self.height = 0

    def max_height( self ) :
        if self.left != None :
            self.left.height = self.height+1
            l = self.left.max_height( )
        else :
            l = 0
        if self.right != None :
            self.right.height = self.height+1
            r = self.right.max_height( )
        else :
            r = 0
        return max( l , r )+1
N =  int( input() )
dic = { i : Node( i , None , None ) for i in range( 1 , N+1 ) }
for i in range( 1 , N+1 ) :
    l_in = list( map( int , input().split() ) )
    if l_in[ 0 ] != -1 :
        dic[ i ].left = dic[ l_in[ 0 ] ]
    if l_in[ 1 ] != -1 :
        dic[ i ].right = dic[ l_in[ 1 ] ]

h_tree = dic[ 1 ].max_height( )
l_ans = [ -1 for i in range( h_tree ) ]
h_0 = -1
st = [ dic[ 1 ] ]
#print( dic )
while h_0 < h_tree-1 :
    #print( st )
    node = st.pop()
    #print( 'node:' , node.value , node , node.left , node.right )
    if node.height > h_0 :
        l_ans[ h_0+1 ] = node.value
        h_0 += 1
    if node.left != None :
        st.append( node.left )
    if node.right != None :
        st.append( node.right )
print( ' '.join( list( map( str , l_ans ) ) ) )
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240528231924254](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240528231924254.png)



### 28203:【模板】单调栈

http://cs101.openjudge.cn/practice/28203/



思路：



代码

```python
n = int( input() )
l_in = list( map( int , input().split() ) )
st_p = []
l_ans = [ 0 for i in range( n ) ]
for p in range( n ) :
    val = l_in[ p ]
    while len(st_p) > 0 and l_in[ st_p[ -1 ] ] < val :
        p_2 = st_p.pop()
        l_ans[ p_2 ] = p+1
    st_p.append( p )
for p in range( n ) :
    print( l_ans[ p ] , end = ' ' )
print( '\n' )
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240528232144576](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240528232144576.png)



### 09202: 舰队、海域出击！

http://cs101.openjudge.cn/practice/09202/



思路：



代码

```python
def has_cycle(n, edges):
    
    adj = { u:[] for u in range( 1 , n+1 )  }
    for u, v in edges:
        adj[u].append(v)
    
    in_degree = [ 0 ] * (n+1)
    for u , v in edges:
        in_degree[v] += 1
    
    queue = [u for u in range( 1 , n+1 ) if in_degree[ u ] == 0]
    visited = 0
    while queue :
        u = queue.pop(0)
        visited += 1
        for v in adj[ u ]:
            in_degree[ v ] -= 1
            if in_degree[ v ] == 0:
                queue.append( v )
    
    return visited != n

T = int( input() )
l_ans = []
for i in range( T ) :
    n , m = list( map( int , input().split() ) )
    edges = []
    for i in range( m ) :
        edges.append( list( map( int , input().split() ) ) )

    if has_cycle( n , edges) :
        l_ans.append( 'Yes' )
    else :
        l_ans.append( 'No' )
for i in range( T ) :
    print( l_ans[ i ] )
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528232204483](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240528232204483.png)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135/



思路：



代码

```python
n, m = map(int,input().split())
num = [int(input()) for _ in range(n)]
mini, maxi = max(num), sum(num)
def check(mid):
    fajo = 1
    money = 0
    for i in num:
        money += i
        if money > mid:
            money = i
            fajo += 1
    if fajo > m:return False
    else:return True
while mini < maxi:
    mid = (mini+maxi)//2
    if check(mid):maxi = mid
    else:mini=mid+1
print(mini)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528232224390](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240528232224390.png)



### 07735: 道路

http://cs101.openjudge.cn/practice/07735/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 01182: 食物链

http://cs101.openjudge.cn/practice/01182/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	上周其他课程压力有点大，只能这周认真准备机考了……



