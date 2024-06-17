# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024

2024 spring, Complied by ==吴彦玺 物理学院==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：win 11

Python编程环境：vscode

C/C++编程环境：



## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/



思路：

​	25min

##### 代码

```python
k = int(input())
missle = list(map(int,input().split()))
cut = [1 for i in range(k)]
for i in range(1,k):
    for j in range(i):
        if missle[i]<=missle[j]:
            cut[i] = max(cut[i],cut[j]+1)
print(max(cut))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240308130933986](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240308130933986.png)



**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147



思路：

​	这道题是第二次做了。需要对递归调用函数自身有基础的理解。

​	12min

##### 代码

```python
def move(sta,mid,end,n):
    if n == 1:
        print(f'1:{sta}->{end}')
    else:
        move(sta,end,mid,n-1)
        print(f'{n}:{sta}->{end}')
        move(mid,sta,end,n-1)

n,a,b,c = input().split()
move(a,b,c,int(n))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240308132806088](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240308132806088.png)



**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253



思路：



##### 代码

```python
import queue

def mput(q,times):
    for t in range(times-1):
        x = q.get()
        q.put(x)

while True:
    n,p,m = map(int,input().split())
    if n == 0 and p == 0 and m == 0:
        break

    q = queue.Queue()
    output = []

    for i in range(p,n+1):
        q.put(i)
    for i in range(1,p):
        q.put(i)

    for j in range(n-1):
        mput(q,m)
        output.append(str(q.get()))
    output.append(str(q.get()))
    print(','.join(output))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240312202449217](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240312202449217.png)



**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554



思路：



##### 代码

```python
n = int(input())
times = [0]+list(map(int,input().split()))
order = list(range(1,n+1))
sum_t = 0
sum_t1 = 0
for i in range(n-2,-2,-1):
    for j in range(i+1):
        if times[order[j]] > times[order[j+1]]:
             order[j],order[j+1]=order[j+1],order[j]
    sum_t += times[order[i+1]]*(n-2-i)
print(' '.join(map(str,order)))
aver_t = '{:.2f}'.format(sum_t/n)
print(aver_t)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240312173504973](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240312173504973.png)



**19963:买学区房**

http://cs101.openjudge.cn/practice/19963



思路：



##### 代码

```python
n = int(input())
a = list(input().split())
price = list(map(int,input().split()))
result = []
for i in range(n):
    p,q = map(int,a[i][1:-1].split(','))
    result.append((p+q)/price[i])
if n % 2 == 0:
    result2 = sorted(result)
    price2 = sorted(price)
    mid = (result2[n//2]+result2[n//2-1])/2
    p_mid = (price2[n//2]+price2[n//2-1])/2
else:
    mid = sorted(result)[n//2]
    p_mid = sorted(price)[n//2]
o = 0
for i in range(n):
    if result[i] > mid and price[i] < p_mid:
        o += 1
print(o)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240312173855255](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240312173855255.png)



**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300



思路：



##### 代码

```python
n = int(input())
model = {}
for i in range(n):
    a,b = input().split('-')
    if a in model:
        model[a].append(b)
    else:
        model[a] = [b]
namebook = sorted(model.keys())
for name in namebook:
    deta = model[name]
    M = []
    B = []
    for i in deta:
        if '.' in i:
            k = float(i[:-1])
        else:
            k = int(i[:-1])
        if i[-1] == 'M':
            M.append(k)
        else:
            B.append(k)
    M.sort()
    B.sort()
    newlist=[]
    for i in range(len(M)):
        newlist.append(str(M[i])+'M')
    for i in range(len(B)):
        newlist.append(str(B[i])+'B')
    output = name + ': ' + (', '.join(newlist))
    print(output)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240312220200468](C:\Users\10121\AppData\Roaming\Typora\typora-user-images\image-20240312220200468.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==



​	作业难度一般；最后一题听同学说可以用lambda写，但还没做。

​	忘记了保留几位小数的语法，可以带上考场（

​	本周尝试实现了几种数据类型，这个也可以带上考场。

​	作业完成得有点晚，下次注意。

