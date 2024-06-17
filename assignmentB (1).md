# Assignment #B: 图论和树算

Updated 1709 GMT+8 Apr 28, 2024

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

### 28170: 算鹰

dfs, http://cs101.openjudge.cn/practice/28170/



思路：



代码

```python
def max_area( l , N , M ) :
    l_hav_bee_che = [ [False for j in range(M)] for i in range(N) ]
    max_are = 0
    que = [ None for i in range( M*N ) ]
    #que = [ None for i in range( 25 ) ]
    str_que = 0
    end_que = 0
    l_mov = [ ( 0 , 1 ) , ( 0 , -1 ) , ( 1 , 0 ) , ( -1 , 0 ) ]
    for i in range(N) :
        for j in range(M) :
            if ( not l_hav_bee_che[ i ][ j ] ) and ( l[ i ][ j ] == '.' ) :
                str_que = 0
                end_que = 0
                que[ end_que ] = ( i , j )
                end_que += 1
                l_hav_bee_che[ i ][ j ] = True
                while end_que != str_que :
                    #print(str_que,end_que,que)
                    site = que[ str_que ]
                    str_que += 1
                    for _ in range( len( l_mov ) ) :
                        i_1 = site[ 0 ]+l_mov[ _ ][ 0 ]
                        j_1 = site[ 1 ]+l_mov[ _ ][ 1 ]
                        if ( 0 <= i_1 <= N-1 ) and ( 0 <= j_1 <= M-1 ) :
                    #for i_1 in range( max( 0 , site[0]-1 ) , min( N-1 , site[0]+1 )+1 ) :
                        #for j_1 in range( max( 0 , site[1]-1 ) , min( M-1 , site[1]+1 )+1 ) :
                            if ( not l_hav_bee_che[ i_1 ][ j_1 ] ) and ( l[ i_1 ][ j_1 ] == '.' ) :
                                #if i_1 == 5 and j_1 == 2 :
                                #    print(site)
                                #    print(max( 0 , site[0]-1 ) , min( N-1 , site[1]+1 )+1)
                                #    print(max( 0 , site[0]-1 ) , min( M-1 , site[1]+1 )+1)
                                que[ end_que ] = ( i_1 , j_1 )
                                end_que += 1
                                l_hav_bee_che[ i_1 ][ j_1 ] = True
                #print(end_que)
                #print(que)
                #for i_t in range(N) :
                    #for j_t in range(M):
                        #print( '.' if l_hav_bee_che[i_t][j_t] else '_' , end=''  )
                    #print()
                
                max_are += 1
                #print(max_are)
    #for i in range(N) :
    #    print(l_hav_bee_che[i])
    return max_are
                                
                

l_ans = []
l = []
for i in range( 10 ) :
    l.append( str(input(  )) )
print(max_area( l , 10 , 10 ))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240507205830562](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240507205830562.png)



### 02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/



思路：



代码

```python
def check( s ) :
    lenth = len(s)
    for i in range( lenth ) :
        for j in range( lenth ) :
            if ( s[ i ] == s[ j ] or int(s[ i ]) == int(s[ j ])+j-i or int(s[ i ]) == int(s[ j ])+i-j ) and ( i != j ) :
                #print(i,j)
                return False
    return True


def l_num(  ) :
    l_a = [ [ str(i) for i in range( 1 , 9 ) ] , [  ] , [  ] , [  ] ]
    for i in range( 1 , 4 ) :
        for m in range( len( l_a[ i-1 ] ) ):
            for n in range( len( l_a[ i-1 ] ) ):
                s_0 = l_a[ i-1 ][ m ]+l_a[ i-1 ][ n ]
                #print(s_0)
                if check( s_0 ) :
                    l_a[ i ].append( s_0 )
    return l_a[ 3 ]
l = l_num(  )
l_ans = []
n = int( input() )
for i in range( n ) :
    l_ans.append( l[ int(input())-1 ] )
for i in range( n ) :
    print( l_ans[ i ] )
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240507205553529](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240507205553529.png)



### 03151: Pots

bfs, http://cs101.openjudge.cn/practice/03151/



思路：



代码

```python
global dic_fill
A , B , C = list( map( int , input().split() ) )
dic_fill = { 1 : A , 2 : B }
#一个元组代表A,B所有水
#l_A_e,l_B_e代表另一个为空时其中一个装至这么多的方法
#l_A_f,l_B_f代表另一个为空时其中一个装至这么多的方法
l_A_e = [ None for i in range( A+1 ) ]
l_B_e = [ None for i in range( B+1 ) ]
l_A_f = [ None for i in range( A+1 ) ]
l_B_f = [ None for i in range( B+1 ) ]

l_A_e[ 0 ] = ( 0 , { 1 : 0 , 2 : 0 } , [] )
l_B_e[ 0 ] = ( 0 , { 1 : 0 , 2 : 0 } , [] )

def drop( pot1 , pot2 , dic ) :
    return { pot1 : 0 , pot2 : dic[ pot2 ] }
def fill( pot1 , pot2 , dic ) :
    return { pot1 : dic_fill[ pot1 ] , pot2 : dic[ pot2 ] }
def pour( pot1 , pot2 , dic ) :
    if dic[ pot1 ] + dic[ pot2 ] > dic_fill[ pot2 ] :
        return { pot1 : dic[ pot2 ]+dic[ pot1 ]-dic_fill[ pot2 ] , pot2 : dic_fill[ pot2 ] }
    else :
        return { pot1 : 0 , pot2 : dic[ pot2 ]+dic[ pot1 ] }
def update( tup , l_A_e , l_B_e , l_A_f , l_B_f ) :
    flag = False
    dic = tup[ 1 ]
    if dic[ 1 ] == 0 and l_B_e[ dic[ 2 ] ] == None :
        l_B_e[ dic[ 2 ] ] = tup
        flag = True
    if dic[ 2 ] == 0 and l_A_e[ dic[ 1 ] ] == None :
        l_A_e[ dic[ 1 ] ] = tup
        flag = True
    if dic[ 1 ] == dic_fill[ 1 ] and l_B_f[ dic[ 2 ] ] == None :
        l_B_f[ dic[ 2 ] ] = tup
        flag = True
    if dic[ 2 ] == dic_fill[ 2 ] and l_A_f[ dic[ 1 ] ] == None :
        l_A_f[ dic[ 1 ] ] = tup
        flag = True
    return flag

que = [ ( 0 , { 1 : 0 , 2 : 0 } , [] ) ]
i = 0

flag_0 = False
while len( que ) > i :
    steps , dic , lis = que[ i ]
    i += 1
    if dic[ 1 ] == C or dic[ 2 ] == C :
        flag_0 = True
        break
    if dic[ 1 ] == 0 :
        dic_new = fill( 1 , 2 , dic )
        if update( ( steps+1 , dic_new , lis+[ 'FILL(1)' ] ) , l_A_e , l_B_e , l_A_f , l_B_f ) :
            que.append( ( steps+1 , dic_new , lis+[ 'FILL(1)' ] ) )
        dic_new = pour( 2 , 1 , dic )
        if update( ( steps+1 , dic_new , lis+[ 'POUR(2,1)' ] ) , l_A_e , l_B_e , l_A_f , l_B_f ) :
            que.append( ( steps+1 , dic_new , lis+[ 'POUR(2,1)' ] ) )
    if dic[ 2 ] == 0 :
        dic_new = fill( 2 , 1 , dic )
        if update( ( steps+1 , dic_new , lis+[ 'FILL(2)' ] ) , l_A_e , l_B_e , l_A_f , l_B_f ) :
            que.append( ( steps+1 , dic_new , lis+[ 'FILL(2)' ] ) )
        dic_new = pour( 1 , 2 , dic )
        if update( ( steps+1 , dic_new , lis+[ 'POUR(1,2)' ] ) , l_A_e , l_B_e , l_A_f , l_B_f ) :
            que.append( ( steps+1 , dic_new , lis+[ 'POUR(1,2)' ] ) )
    if dic[ 1 ] == dic_fill[ 1 ] :
        dic_new = drop( 1 , 2 , dic )
        if update( ( steps+1 , dic_new , lis+[ 'DROP(1)' ] ) , l_A_e , l_B_e , l_A_f , l_B_f ) :
            que.append( ( steps+1 , dic_new , lis+[ 'DROP(1)' ] ) )
        dic_new = pour( 1 , 2 , dic )
        if update( ( steps+1 , dic_new , lis+[ 'POUR(1,2)' ] ) , l_A_e , l_B_e , l_A_f , l_B_f ) :
            que.append( ( steps+1 , dic_new , lis+[ 'POUR(1,2)' ] ) )
    if dic[ 2 ] == dic_fill[ 2 ] :
        dic_new = drop( 2 , 1 , dic )
        if update( ( steps+1 , dic_new , lis+[ 'DROP(2)' ] ) , l_A_e , l_B_e , l_A_f , l_B_f ) :
            que.append( ( steps+1 , dic_new , lis+[ 'DROP(2)' ] ) )
        dic_new = pour( 2 , 1 , dic )
        if update( ( steps+1 , dic_new , lis+[ 'POUR(2,1)' ] ) , l_A_e , l_B_e , l_A_f , l_B_f ) :
            que.append( ( steps+1 , dic_new , lis+[ 'POUR(2,1)' ] ) )
    #print( ( steps , dic , lis ) , que )
if flag_0 :
    print( steps )
    for i in lis :
        print( i )
else :
    print('impossible')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240507205640338](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240507205640338.png)



### 05907: 二叉树的操作

http://cs101.openjudge.cn/practice/05907/



思路：



代码

```python
t = int( input() )
l_ans = []
for _ in range( t ) :
    n , m =list( map( int , input().split() ) )
    dic = {}
    for i in range( n ) :
        l_in = list( map( int , input().split() ) )
        dic[ l_in[ 0 ] ] = [ l_in[ 1 ] , l_in[ 2 ] , -1 ]
    for i in range( n ) :
        if dic[ i ][ 0 ] != -1 :
            dic[ dic[ i ][ 0 ] ][ 2 ] = i
        if dic[ i ][ 1 ] != -1 :
            dic[ dic[ i ][ 1 ] ][ 2 ] = i
    for i in range( m ) :
        l_in = list( map( int , input().split() ) )
        if l_in[ 0 ] == 1 :
            node_1 = l_in[ 1 ]
            node_2 = l_in[ 2 ]
            father_1 = dic[ node_1 ][ 2 ]
            father_2 = dic[ node_2 ][ 2 ]
            father_1 , father_2 = father_2 , father_1
            dic[ node_1 ][ 2 ] = father_1
            dic[ node_2 ][ 2 ] = father_2
            if father_1 != father_2 :
                if father_1 != -1 :
                    dic[ father_1 ][ dic[ father_1 ].index( node_2 ) ] = node_1
                if father_2 != -1 :
                    dic[ father_2 ][ dic[ father_2 ].index( node_1 ) ] = node_2
            else :
                dic[ father_1 ][ 0 ] , dic[ father_1 ][ 1 ] = dic[ father_1 ][ 1 ] , dic[ father_1 ][ 0 ]
        elif l_in[ 0 ] == 2 :
            pointer = l_in[ 1 ]
            while dic[ pointer ][ 0 ] != -1 :
                pointer = dic[ pointer ][ 0 ]
            l_ans.append( pointer )
for i in l_ans :
    print( i )
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240507205743129](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240507205743129.png)





### 18250: 冰阔落 I

Disjoint set, http://cs101.openjudge.cn/practice/18250/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 05443: 兔子与樱花

http://cs101.openjudge.cn/practice/05443/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	假期学习不在状态，这周补上吧……



