# Assignment #1: 拉齐大家Python水平

Updated 0940 GMT+8 Feb 19, 2024

2024 spring, Complied by吴彦玺，物理学院



**说明：**

1）数算课程的先修课是计概，由于计概学习中可能使用了不同的编程语言，而数算课程要求Python语言，因此第一周作业练习Python编程。如果有同学坚持使用C/C++，也可以，但是建议也要会Python语言。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：macOS Ventura 13.4.1 (c)

Python编程环境：vsCode Python

C/C++编程环境：无



## 1. 题目

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/



思路：

​	没啥好说的

##### 代码

```python
tbnqs = list(0 for _ in range(31))
tbnqs[1], tbnqs[2] = 1,1
for i in range(3,31):
    tbnqs[i] = tbnqs[i-1]+tbnqs[i-2]+tbnqs[i-3]
n = int(input())
print(tbnqs[n])
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240305183124310](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240305183124310.png)



### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A



思路：

​	上学期的初学代码，乐，你就说过没过嘛（

##### 代码

```python
s = input()
h = s.find('h')
if h == -1:
    print('NO')
else:
    e = s.find('e', h)
    if e == -1:
        print('NO')
    else:
        l1 = s.find('l', e)
        if l1 == -1:
            print('NO')
        else:
            l2 = s.find('l', l1 + 1)
            if l2 == -1:
                print('NO')
            else:
                o = s.find('o', l2)
                if o == -1:
                    print('NO')
                else:print('YES')
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240305183345830](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240305183345830.png)



### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A



思路：

​	乐

##### 代码

```python
str = input().lower()
list = list(str)
element_to_move = ['a', 'e', 'i', 'o', 'u', 'y']
for element in element_to_move:
    list = [x for x in list if x != element]
def addpoint(str):
    return "." + str
list_2 = map(addpoint, list)
print(''.join(list_2))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240305183625832](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240305183625832.png)



### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/



思路：

​	先做质数表，再挑

##### 代码

```python
prilist = [False,False,True]+list(True for _ in range(9998))
prime = [2]
for num in range(3,10001):
    for pri in prime:
        if num % pri == 0:
            prilist[num] = False
            break
    else:
        prime.append(num)

n = int(input())
for num in prime:
    if prilist[n-num]:
        print(num,n-num)
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240305184639322](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240305184639322.png)



### 23563: 多项式时间复杂度

http://cs101.openjudge.cn/practice/23563/



思路：

​	以前做的忘了

##### 代码

```python
idk = [i for i in input().split('+')]
maxi = 0
for item in idk:
    word = ''
    for i in item:
        if i != ' ':
            word += i
    if word[0] == 'n':
        word = '1'+word
    a,b = map(int, word.split('n^'))
    if a:
        maxi = max(b, maxi)
print('n^'+str(maxi))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240305184628082](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240305184628082.png)



### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/



思路：

​	维护一个最大票数maxi即可

​	本来想用rank列表存票数的，但由于名字不知道，所以改成字典了

##### 代码

```python
rank = {}
output = []
maxi = 0
vote = list(map(int,input().split()))

for i in vote:
    if i in rank:
        rank[i] += 1
    else: rank[i] = 1
    if rank[i] > maxi:
        maxi = rank[i]
        output = [i]
    elif rank[i] == maxi:
        output.append(i)

output.sort()
ans = list(map(str,output))
print(' '.join(ans))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240305190337301](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240305190337301.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“数算pre每日选做”、CF、LeetCode、洛谷等网站题目。==

​	我三月份才开始学数算，似乎有点晚了……

​	这个作业就是练练手感吧，上学期学过计概，所以没什么问题。



