# Assignment #9: 图论：遍历，及 树算

Updated 1739 GMT+8 Apr 14, 2024

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

### 04081: 树的转换

http://cs101.openjudge.cn/dsapre/04081/



思路：



代码

```python
class fNode:
    def __init__(self, key):
        self.key = key
        self.first = None
        self.next = None
        self.parent = None
        self.height = 0
    
    def put(self, p_key):
        if self.first is None:
            self.first = fNode(p_key)
            self.first.parent = self
            self.first.checkheight()
            return self.first
        pos_node = self.first
        while pos_node.next is not None:
            pos_node = pos_node.next
        pos_node.next = fNode(p_key)
        pos_node.next.parent = pos_node.parent
        return pos_node.next

    def checkheight(self):
        pos_node = self
        while pos_node.parent is not None and \
            pos_node.height >= pos_node.parent.height:
            pos_node.parent.height = pos_node.height+1
            pos_node = pos_node.parent

class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.parent = None
        self.height = 0

    def put_left(self, p_node):
        self.left = p_node
        self.left.parent = self
        self.left.checkheight()
        return self.left
    
    def put_right(self, p_node):
        self.right = p_node
        self.right.parent = self
        self.right.checkheight()
        return self.right

    def checkheight(self):
        pos_node = self
        while pos_node.parent is not None and \
            pos_node.height >= pos_node.parent.height:
            pos_node.parent.height = pos_node.height+1
            pos_node = pos_node.parent

def buildftree(inp):
    root = fNode(0)
    pos_key = 1
    pos_node = root
    for i in inp:
        if i == 'd':
            pos_node = pos_node.put(pos_key)
            pos_key += 1
        else:
            pos_node = pos_node.parent
    return root

def magic(pos_fnode):
    pos_node = Node(pos_fnode.key)
    if pos_fnode.first is not None:
        pos_node.put_left(magic(pos_fnode.first))
    if pos_fnode.next is not None:
        pos_node.put_right(magic(pos_fnode.next))
    return pos_node

inp = input()
froot = buildftree(inp)
#print(froot.height)
root = magic(froot)
print('%d => %d'%(froot.height, root.height))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240512161746979](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240512161746979.png)



### 08581: 扩展二叉树

http://cs101.openjudge.cn/dsapre/08581/



思路：



代码

```python
# 定义节点
class Node:
    # 需要的话后面算法自己加parent！
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None

    # 节点大小比较定义（按需取用）
    def __lt__(self, other):
        return self.key < other.key
    def __gt__(self, other):
        return self.key > other.key
    
    #前序输入
    def prein(self, index, inp):
        pos = index
        if pos >= len(inp) or inp[pos] == '.':
            pos = min(pos+1, len(inp))
        else:
            self.left = Node(inp[pos])
            pos = self.left.prein(pos+1, inp)
        if pos >= len(inp) or inp[pos] == '.':
            pos = min(pos+1, len(inp))
        else:
            self.right = Node(inp[pos])
            pos = self.right.prein(pos+1, inp)
        return pos

# 二叉搜索树
class BinaryTree:
    # 定义根节点
    def __init__(self):
        self.root = None

    # 插入节点总算法
    def preorder_input(self, inp):
        if len(inp) <= 1:
            return
        self.root = Node(inp[0])
        self.root.prein(1,inp)
    
    # 中序序遍历（列表输出）
    def midorder(self,node=None):
        if node is None:
            if self.root:
                node = self.root
            else: return None
        output = []
        if node.left:
            output += self.midorder(node.left)
        output += [node.key]
        if node.right:
            output += self.midorder(node.right)
        return output
    
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
        output += [node.key]
        return output

tree = BinaryTree()
inp = input()
tree.preorder_input(inp)
print(''.join(tree.midorder()))
print(''.join(tree.postorder()))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240512182937786](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240512182937786.png)



### 22067: 快速堆猪

http://cs101.openjudge.cn/practice/22067/



思路：



代码

```python
import heapq
st = []
_set = {  }
hea = []
l_ans = []
#for i in range( 8 ) :
    #print( hea )
while True :
    try :
        l_in = list( str( input() ).split( ) )
    except EOFError:
        break
    #print( l_in , _set , st , hea )
    if l_in[ 0 ] == 'push' :
        st.append( int( l_in[ 1 ] ) )
        if int( l_in[ 1 ] ) in _set :
            _set[ int( l_in[ 1 ] ) ] += 1
        else :
            _set[ int( l_in[ 1 ] ) ] = 1
        heapq.heappush( hea , int( l_in[ 1 ] ) )
    elif l_in[ 0 ] == 'pop' :
        if len( st ) > 0 :
            ele = st.pop()
            _set[ ele ] -= 1
    elif l_in[ 0 ] == 'min' :
        if len( st ) > 0 :
            while _set[ hea[ 0 ] ] == 0 :
                heapq.heappop( hea )
            l_ans.append( hea[ 0 ] )


for i in range( len( l_ans ) ) :
    print( l_ans[ i ] )
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240423231414436](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240423231414436.png)



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123



思路：



代码

```python
import sys
sys.setrecursionlimit( 5000 )

class Vertex:
    def __init__( self, id ):
        self.id = id
        self.connection = {}

    def add_connection( self , nbr , weight = 0 ) :
        self.connection[ nbr ] = weight

class Graph:
    def __init__( self ):
        self.vertices = {}
        self.numVertices = 0

    def add_Vertex( self, name ):
        self.numVertices = self.numVertices + 1
        newVertex = Vertex( name )
        self.vertices[ name ] = newVertex
        return newVertex

    def get_Vertex( self , name ):
        if name in self.vertices:
            return self.vertices[ name ]
        else:
            return None

    def add_bilateral_Edge( self , f , t , weight = 0 ):
        if f not in self.vertices:
            self.add_Vertex( f )
        if t not in self.vertices:
            self.add_Vertex( t )
        self.vertices[ f ].add_connection( t , weight )
        self.vertices[ t ].add_connection( f , weight )


    def search( self , init_n , init_m ) :
        init_Vertex = self.get_Vertex( ( init_n , init_m ) )
        return self.__search( init_Vertex , { init_Vertex } )

    def __search( self , vertex , set_searched ) :
        ans = 0
        if len( set_searched ) == self.numVertices-1 :
            #print( vertex.id , vertex.connection , set_searched )
            return 1
        for i in range( len( vertex.connection.keys() ) ) :
            if not ( self.get_Vertex( list( vertex.connection.keys() )[ i ] ) in set_searched ) :
                ans += self.__search( self.get_Vertex( list( vertex.connection.keys() )[ i ] ) , set_searched.union( { vertex } ) )
        return ans
n_num = int( input() )
l_ans = []
for i in range( n_num ) :
    n , m , init_n , init_m = list( map( int , input(  ).split(  ) ) )
    G = Graph(  )
    l_mov = [ ( 1,2 ) , ( 2,1 ) , ( -1,2 ) , ( -2,1 ) , ( 1,-2 ) , ( 2,-1 ) , ( -1,-2 ) , ( -2,-1 ) ]
    #l_mov = [ ( 1 , 0 ) , ( -1 , 0 ) , ( 0 , 1 ) , ( 0 , -1 ) ]
    for x in range( n ) :
        for y in range( m ) :
            G.add_Vertex( ( x , y ) )
    for x in range( n ) :
        for y in range( m ) :
            for k in range( len( l_mov ) ) :
                mov = l_mov[ k ]
                if ( 0 <= mov[ 0 ]+x <= n-1 ) and ( 0 <= mov[ 1 ]+y <= m-1 ) :
                    G.add_bilateral_Edge( ( x , y ) , ( mov[ 0 ]+x , mov[ 1 ]+y ) )
    #for x in range( m ) :
    #    for y in range( n ) :
    #        print( G. )
    l_ans.append( G.search( init_n , init_m ) )
for i in l_ans :
    print( i )
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240423231241962](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240423231241962.png)



### 28046: 词梯

bfs, http://cs101.openjudge.cn/practice/28046/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

由于这周好多考试，作业没啥时间做（哭泣）

放假回来一定补上……



