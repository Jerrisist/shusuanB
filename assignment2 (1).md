# Assignment #2: 编程练习

Updated 0953 GMT+8 Feb 24, 2024

2024 spring, Complied by ==吴彦玺==，物理学院



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：安卓window11

Python编程环境：vsCode

C/C++编程环境：无



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/practice/27653/



思路：

​	虽然课上有，但还是靠着记忆重写了一遍。忘记\__str__了所以就用了方法show的来输出。

​	别的都没动，就改个名字

##### 代码

```python
def h(a,b):
    while b != 0:
        oa, ob = a,b
        a = ob
        b = oa % ob
    return a

class Fraction:
    def __init__(self,a,b):
        self.up = a
        self.down = b
    
    def show(self):
        print(str(self.up)+'/'+str(self.down))
    
    def __add__(self,other):
        newup = self.up * other.down + \
            self.down * other.up
        newdown = self.down * other.down
        com = h(newup, newdown)
        return Fraction(newup//com, newdown//com)

a,b,c,d = map(int,input().split())
f1,f2 = Fraction(a,b),Fraction(c,d)
Fraction.show(f1+f2)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240305192912501](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240305192912501.png)



### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110



思路：



##### 代码

```python
n, w = map(int,input().split())
weight = []
value = []
carry_value = 0
for _ in range(n):
    a, b = map(int,input().split())
    value += [a]
    weight += [b]
for i in range(n-1):
    for j in range(i+1,n):
        if value[i]/weight[i] < value[j]/weight[j]:
            value[i], value[j] = value[j], value[i]
            weight[i], weight[j] = weight[j], weight[i]
for num in range(n):
    if weight[num] >= w:
        carry_value += value[num]*w/weight[num]
        break
    w -= weight[num]
    carry_value += value[num]
print('{:.1f}'.format(carry_value))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240307155459176](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240307155459176.png)



### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/



思路：



##### 代码

```python
nCases = int(input())
for _ in range(nCases):
    n, m, hp = map(int,input().split())
    timeline = []
    damage ={}
    for _ in range(n):
        ti, xi = map(int,input().split())
        if ti not in damage:
            timeline += [ti]
            damage[ti]=[xi]
        else: damage[ti] += [xi]
    timeline = sorted(timeline)
    for time in timeline:
        da = sorted(damage[time], reverse = True)
        hp -= sum(da[:m])
        if hp <= 0:
            print(time)
            break
    else:
        print('alive')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240307155533724](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240307155533724.png)



### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B



思路：



##### 代码

```python
useless = input()
result = []
#Primes are True, others are False
#nums[i] shows that if i is a prime
prime = [False, False] +[True] * 999999
for i in range(2, 1001):
    if prime[i]:
        for k in range(2*i, 1000001, i):
            prime[k] = False
for i in map(int, input().split()):
    sqrt_i = i**(1/2)
    result += [['NO'], ['YES']][int(sqrt_i) == sqrt_i and prime[int(sqrt_i)]]
print('\n'.join(result))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240307155607661](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240307155607661.png)



### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A



思路：



##### 代码

```python
cases = int(input())
for i in range(cases):
    n,x=map(int,input().split())
    array = list(map(int,input().split()))
    suum = sum(array)
    if suum % x != 0:
        print(n)
    else:
        for index in range(-(-n)//2):
            if array[index] % x != 0 or array[n-1-index] % x != 0:
                print(n-1-index)
                break
        else: print(-1)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240307164251617](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240307164251617.png)



### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/



思路：



##### 代码

```python
prime = [False, False]+[True for _ in range(10000)]
tp = {}
for pri in range(10002):
    if prime[pri]:
        tp[pri**2] = True
        for n_pri in range(2*pri, 10002, pri):
            prime[n_pri] = False
m,n = map(int,input().split())
for _ in range(m):
    grade = 0
    inp = list(map(int,input().split()))
    for i in inp:
        if i in tp:
            grade += i
    grade /= len(inp)
    if grade == 0:
        print(0)
    else:print('{:.2f}'.format(grade))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240307164321010](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240307164321010.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

​	除了第一题，都是上个学期做过的，所以没记录用时啦

​	这周的收获主要在阅读的讲义上。多掌握了几种排序方法。时间复杂度和部分排序在寒假稍微预习过一点，现在重新看了一遍，记忆更加牢固了



