# 範例程式:p100_1.py
 ```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
for i in range(1,5):
    for j in range(1,5):
        for k in range(1,5):
            if( i != k ) and (i != j) and (j != k):
                print i,j,k
 ```
                
# 範例程式:p100_2.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
i = int(raw_input('净利润:'))
arr = [1000000,600000,400000,200000,100000,0]
rat = [0.01,0.015,0.03,0.05,0.075,0.1]
r = 0
for idx in range(0,6):
    if i>arr[idx]:
        r+=(i-arr[idx])*rat[idx]
        print (i-arr[idx])*rat[idx]
        i=arr[idx]
print r
 ```
# 範例程式:p100_3.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
for i in range(1,85):
    if 168 % i == 0:
        j = 168 / i;
        if  i > j and (i + j) % 2 == 0 and (i - j) % 2 == 0 :
            m = (i + j) / 2
            n = (i - j) / 2
            x = n * n - 100
            print(x)
 ```
 # 範例程式:p100_4.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
year = int(raw_input('year:\n'))
month = int(raw_input('month:\n'))
day = int(raw_input('day:\n'))
 
months = (0,31,59,90,120,151,181,212,243,273,304,334)
if 0 < month <= 12:
    sum = months[month - 1]
else:
    print 'data error'
sum += day
leap = 0
if (year % 400 == 0) or ((year % 4 == 0) and (year % 100 != 0)):
    leap = 1
if (leap == 1) and (month > 2):
    sum += 1
print 'it is the %dth day.' % sum
 ```
 # 範例程式:p100_5.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
l = []
for i in range(3):
    x = int(raw_input('integer:\n'))
    l.append(x)
l.sort()
print l
 ```
# 範例程式:p100_6.py
 ```
 方法一

 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
def fib(n):
    a,b = 1,1
    for i in range(n-1):
        a,b = b,a+b
    return a
 
# 输出了第10个斐波那契数列
print fib(10)

方法二

#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
# 使用递归
def fib(n):
    if n==1 or n==2:
        return 1
    return fib(n-1)+fib(n-2)
 
# 输出了第10个斐波那契数列
print fib(10)

方法三

#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
def fib(n):
    if n == 1:
        return [1]
    if n == 2:
        return [1, 1]
    fibs = [1, 1]
    for i in range(2, n):
        fibs.append(fibs[-1] + fibs[-2])
    return fibs
 
# 输出前 10 个斐波那契数列
print fib(10)
 ```
 # 範例程式:p100_7.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
a = [1, 2, 3]
b = a[:]
print b
 ```
 # 範例程式:p100_8.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
for i in range(1, 10):
    print 
    for j in range(1, i+1):
        print "%d*%d=%d" % (i, j, i*j),
 ```
 # 範例程式:p100_9.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
import time
 
myD = {1: 'a', 2: 'b'}
for key, value in dict.items(myD):
    print key, value
    time.sleep(1) # 暂停 1 秒
 ```
 # 範例程式:p100_10.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
import time
 
print time.strftime('%Y-%m-%d %H:%M:%S',time.localtime(time.time()))
 
# 暂停一秒
time.sleep(1)
 
print time.strftime('%Y-%m-%d %H:%M:%S',time.localtime(time.time()))
 ```
 # 範例程式:p100_11.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
f1 = 1
f2 = 1
for i in range(1,22):
    print '%12ld %12ld' % (f1,f2),
    if (i % 3) == 0:
        print ''
    f1 = f1 + f2
    f2 = f1 + f2
 ```
 # 範例程式:p100_12.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
h = 0
leap = 1
from math import sqrt
from sys import stdout
for m in range(101,201):
    k = int(sqrt(m + 1))
    for i in range(2,k + 1):
        if m % i == 0:
            leap = 0
            break
    if leap == 1:
        print '%-4d' % m
        h += 1
        if h % 10 == 0:
            print ''
    leap = 1
print 'The total is %d' % h
 ```
 # 範例程式:p100_13.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
for n in range(100,1000):
    i = n / 100
    j = n / 10 % 10
    k = n % 10
    if n == i ** 3 + j ** 3 + k ** 3:
        print n
 ```
 # 範例程式:p100_14.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
def reduceNum(n):
    print '{} = '.format(n),
    if not isinstance(n, int) or n <= 0 :
        print '请输入一个正确的数字 !'
        exit(0)
    elif n in [1] :
        print '{}'.format(n)
    while n not in [1] : # 循环保证递归
        for index in xrange(2, n + 1) :
            if n % index == 0:
                n /= index # n 等于 n/index
                if n == 1: 
                    print index 
                else : # index 一定是素数
                    print '{} *'.format(index),
                break
reduceNum(90)
reduceNum(100)
 ```
# 範例程式:p100_15.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
score = int(raw_input('输入分数:\n'))
if score >= 90:
    grade = 'A'
elif score >= 60:
    grade = 'B'
else:
    grade = 'C'
 
print '%d 属于 %s' % (score,grade)

 ```
 # 範例程式:p100_16.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
import datetime
 
if __name__ == '__main__':
 
    # 输出今日日期，格式为 dd/mm/yyyy。更多选项可以查看 strftime() 方法
    print(datetime.date.today().strftime('%d/%m/%Y'))
 
    # 创建日期对象
    miyazakiBirthDate = datetime.date(1941, 1, 5)
 
    print(miyazakiBirthDate.strftime('%d/%m/%Y'))
 
    # 日期算术运算
    miyazakiBirthNextDay = miyazakiBirthDate + datetime.timedelta(days=1)
 
    print(miyazakiBirthNextDay.strftime('%d/%m/%Y'))
 
    # 日期替换
    miyazakiFirstBirthday = miyazakiBirthDate.replace(year=miyazakiBirthDate.year + 1)
 
    print(miyazakiFirstBirthday.strftime('%d/%m/%Y'))
 ```
 # 範例程式:p100_17.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
import string
s = raw_input('请输入一个字符串:\n')
letters = 0
space = 0
digit = 0
others = 0
i=0
while i < len(s):
    c = s[i]
    i += 1
    if c.isalpha():
        letters += 1
    elif c.isspace():
        space += 1
    elif c.isdigit():
        digit += 1
    else:
        others += 1
print 'char = %d,space = %d,digit = %d,others = %d' % (letters,space,digit,others)
 ```
# 範例程式:p100_18.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
Tn = 0
Sn = []
n = int(raw_input('n = '))
a = int(raw_input('a = '))
for count in range(n):
    Tn = Tn + a
    a = a * 10
    Sn.append(Tn)
    print Tn
 
Sn = reduce(lambda x,y : x + y,Sn)
print "计算和为：",Sn
 ```
 # 範例程式:p100_19.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
from sys import stdout
for j in range(2,1001):
    k = []
    n = -1
    s = j
    for i in range(1,j):
            if j % i == 0:
                n += 1
                s -= i
                k.append(i)
    
    if s == 0:
        print j
        for i in range(n):
            stdout.write(str(k[i]))
            stdout.write(' ')
        print k[n]
 ```
 # 範例程式:p100_20.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
tour = []
height = []
 
hei = 100.0 # 起始高度
tim = 10 # 次数
 
for i in range(1, tim + 1):
    # 从第二次开始，落地时的距离应该是反弹高度乘以2（弹到最高点再落下）
    if i == 1:
        tour.append(hei)
    else:
        tour.append(2*hei) 
    hei /= 2
    height.append(hei)
 
print('总高度：tour = {0}'.format(sum(tour)))
print('第10次反弹高度：height = {0}'.format(height[-1]))
 ```
 # 範例程式:p100_21.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
x2 = 1
for day in range(9,0,-1):
    x1 = (x2 + 1) * 2
    x2 = x1
print x1
 ```
 # 範例程式:p100_22.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
for i in range(ord('x'),ord('z') + 1):
    for j in range(ord('x'),ord('z') + 1):
        if i != j:
            for k in range(ord('x'),ord('z') + 1):
                if (i != k) and (j != k):
                    if (i != ord('x')) and (k != ord('x')) and (k != ord('z')):
                        print 'order is a -- %s\t b -- %s\tc--%s' % (chr(i),chr(j),chr(k))
 ```
 # 範例程式:p100_23.py
 ```
 #!/usr/bin/python
# -*- coding: UTF-8 -*-
 
from sys import stdout
for i in range(4):
    for j in range(2 - i + 1):
        stdout.write(' ')
    for k in range(2 * i + 1):
        stdout.write('*')
    print
 
for i in range(3):
    for j in range(i + 1):
        stdout.write(' ')
    for k in range(4 - 2 * i + 1):
        stdout.write('*')
    print
 ```
 # 範例程式:p100_24.py
 ```
 方法一：
 
 方法二：
 
 方法三：
 
 ```
 # 範例程式:p100_25.py
 ```
 ```
 # 範例程式:p100_26.py
 ```
 ```
# 範例程式:p100_27.py
 ```
 ```
 # 範例程式:p100_28.py
 ```
 ```
 # 範例程式:p100_29.py
 ```
 ```
# 範例程式:p100_30.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
# 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
# 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
# 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
# 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
# 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
# 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
# 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
# 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
# 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
# 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
 # 範例程式:p100_1.py
 ```
 ```
