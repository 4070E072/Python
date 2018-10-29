# 範例程式:a.py
```
def test():
    print('test in a.py')
```
# 範例程式:b.py
```
def test():
    print('test in b.py')

```
# 範例程式:ClearWindow.py
```
##   """
##   
##   Clear Window Extension
##   Version: 0.2
##   
##   Author: Roger D. Serwy
##           roger.serwy@gmail.com
##   
##   Date: 2009-06-14
##   
##   It provides "Clear Shell Window" under "Options"
##   with ability to undo.
##   
##   Add these lines to config-extensions.def
##   

config_extension_def = """
[ClearWindow]
enable=1
enable_editor=0
enable_shell=1
[ClearWindow_cfgBindings]
clear-window=<Control-Key-l>
"""

class ClearWindow:

    menudefs = [
        ('options', [
               ('Clear Shell Window', '<<clear-window>>'),
       ]),]
		 
    def __init__(self, editwin):
        self.editwin = editwin
        self.text = self.editwin.text
        self.text.bind("<<clear-window>>", self.clear_window2)

        self.text.bind("<<undo>>", self.undo_event)  # add="+" doesn't work

    def undo_event(self, event):
        text = self.text
        
        text.mark_set("iomark2", "iomark")
        text.mark_set("insert2", "insert")
        self.editwin.undo.undo_event(event)

        # fix iomark and insert
        text.mark_set("iomark", "iomark2")
        text.mark_set("insert", "insert2")
        text.mark_unset("iomark2")
        text.mark_unset("insert2")
        

    def clear_window2(self, event): # Alternative method
        # work around the ModifiedUndoDelegator
        text = self.text
        text.undo_block_start()
        text.mark_set("iomark2", "iomark")
        text.mark_set("iomark", 1.0)
        text.delete(1.0, "iomark2 linestart")
        text.mark_set("iomark", "iomark2")
        text.mark_unset("iomark2")
        text.undo_block_stop()
        if self.text.compare('insert', '<', 'iomark'):
            self.text.mark_set('insert', 'end-1c')
        self.editwin.set_line_and_column()

    def clear_window(self, event):
        # remove undo delegator
        undo = self.editwin.undo
        self.editwin.per.removefilter(undo)

        # clear the window, but preserve current command
        self.text.delete(1.0, "iomark linestart")
        if self.text.compare('insert', '<', 'iomark'):
            self.text.mark_set('insert', 'end-1c')
        self.editwin.set_line_and_column()
 
        # restore undo delegator
        self.editwin.per.insertfilter(undo)
 


```
# 範例程式:hello.py
```
def main():	#def是用來定義函數的Python關鍵字
    if __name__ == '__main__':		#選擇結構，識別目前執行方式
        print('This program is run directly.')
    elif __name__ == 'hello':		#冒號、換行、縮排表示一個語句區塊的開始
        print('This program is used as a module.')

main()		#呼叫上面定義的函數

```
# 範例程式:HelloWorld.py
```
def main():
    print('Hello world')
main()

```
# 範例程式:set01.py
```
import random
import time

x = list(range(10000))		#產生成列表
y = set(range(10000))		#產生成集合
z = dict(zip(range(1000),range(10000)))		#產生成字典
r = random.randint(0, 9999)	#產生成亂數

start = time.time()
for i in range(9999999):
    r in x 				#測試清單列表中是否包含某個元素
print('list,time used:', time.time()-start)

start = time.time()
for i in range(9999999):
    r in y					#測試集合中是否包含某個元素
print('set,time used:', time.time()-start)

start = time.time()
for i in range(9999999):
    r in z					#測試字典中是否包含某個元素
print('dict,time used:', time.time()-start)

```
# 範例程式:set02.py
```
import random
import time

def RandomNumbers1(number, start, end):
    '''使用列表來產生成number個介於start和end之間的不重複亂數'''
    data = []
    while True:
        element = random.randint(start, end)
        if element not in data:
            data.append(element)
        if len(data) == number:
            break
    return data

def RandomNumbers2(number, start, end):
    '''使用集合來產生成number個介於start和end之間的不重複亂數'''
    data = set()
    while True:
        element = random.randint(start, end)
        data.add(element)
        if len(data) == number:
            return data

start = time.time()
for i in range(10000):
    d1 = RandomNumbers1(500, 1, 10000)
print('Time used:', time.time()-start)

start = time.time()
for i in range(10000):
    d2 = RandomNumbers2(500, 1, 10000)
print('Time used:', time.time()-start)

```
# 範例程式:03_02_01.py
```
def main(n):
    for i in range(n):
        print((' * '*i).center(n*3))
    for i in range(n, 0, -1):
        print((' * '*i).center(n*3))

main(6)

```
# 範例程式:03_02_03.py
```
import time
digits = (1, 2, 3, 4)

start = time.time()
for i in range(1000):
    result = []
    for i in digits:
        for j in digits:
            for k in digits:
                result.append(i*100+j*10+k)
print(time.time()-start)

start = time.time()
for i in range(1000):
    result = []
    for i in digits:
        i = i*100
        for j in digits:
            j = j*10
            for k in digits:
                result.append(i+j+k)
print(time.time()-start)

```
# 範例程式:03_02_03_2.py
```
import time
import math

start = time.time()						#取得目前時間
for i in range(10000000):
    math.sin(i)
print('Time Used:', time.time()-start)	#輸出所耗時間

loc_sin = math.sin
start = time.time()
for i in range(10000000):
    loc_sin(i)
print('Time Used:', time.time()-start)

```
# 範例程式:03_03_01.py
```
def check_permission(func):
    def wrapper(*args, **kwargs):
        if kwargs.get('username')!='admin':
            raise Exception('Sorry. You are not allowed.')
        return func(*args, **kwargs)
    return wrapper

class ReadWriteFile(object):
    #把函數check_permission作為裝飾器使用
    @check_permission
    def read(self, username, filename):
        return open(filename,'r').read()

    def write(self, username, filename, content):
        open(filename,'a+').write(content)
    #把函數check_permission作為普通函數使用
    write = check_permission(write)

t = ReadWriteFile()
print('Originally.......')
print(t.read(username='admin', filename=r'd:\sample.txt'))
print('Now, try to write to a file........')
t.write(username='admin', filename=r'd:\sample.txt', content='\nhello world')
print('After calling to write...........')
print(t.read(username='admin', filename=r'd:\sample.txt'))

```
# 範例程式:03_03_02.py
```
def demo(newitem, old_list=[]):
    old_list.append(newitem)
    return old_list

print(demo('5', [1, 2, 3, 4]))
print(demo('aaa', ['a', 'b']))
print(demo('a'))
print(demo('b'))

```
# 範例程式:03_03_03.py
```
def scope_test():
    def do_local():
        spam = "我是區域變數"

    def do_nonlocal():
        nonlocal spam		#這時要求spam必須是已存在的變數
        spam = "我不是區域變數，也不是全域變數"

    def do_global():
        global spam		#如果全域作用域沒有spam，就自動新建一個
        spam = "我是全域變數"

    spam = "原來的值"
    do_local()
    print("設定區域變數值後：", spam)
    do_nonlocal()
    print("設定nonlocal變數值後：", spam)
    do_global()
    print("設定全域變數值後", spam)

scope_test()
print("全域變數：", spam)

```
# 範例程式:03-01.py
```
age = 24
subject = "電腦"
college = "非重點"
if (age > 25 and subject=="電子資訊工程") or (college=="重點" and subject=="電子資訊工程" ) or (age<=28 and subject=="電腦"):
    print("恭喜，您已獲得敝公司的面試機會!")
else:
    print("抱歉，您未達到面試要求")

```
# 範例程式:03-02.py
```
numbers = []
while True:
    x = input('請輸入一個整數：')
    try:						#異常處理結構的相關細節，詳見第7章
        numbers.append(int(x))
    except:
        print('不是整數')
    while True:
        flag = input('繼續輸入嗎？（yes/no）')
        if flag.lower() not in ('yes', 'no'):	#限定輸入的內容必須為yes或no
            print('只能輸入yes或no')
        else:
            break
    if flag.lower()=='no':
        break

print(sum(numbers)/len(numbers))

```
# 範例程式:03-03.py
```
import time
date = time.localtime()		#取得目前的日期時間
year = date[0]
month = date[1]
day = date[2]
day_month = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
if year%400==0 or (year%4==0 and year%100!=0):	#判斷是否為閏年
    day_month[1] = 29
if month==1:
    print(day)
else:
    print(sum(day_month[:month-1])+day)

```
# 範例程式:03-03_2.py
```
from datetime import date

daysOfMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]

def myCalendar(year, month):
    #取得year年month月1日是周幾
    start = date(year, month, 1).timetuple().tm_wday
    #輸出表頭資訊
    print('{0}年{1}月日曆'.format(year,month).center(56))
    print('\t'.join('日 一 二 三 四 五 六'.split()))
    #取得該月有多少天，如果是2月並且是閏年，就適當調整一下
    day = daysOfMonth[month-1]
    if month==2:
        if year%400==0 or (year%4==0 and year%100!=0):
            day += 1
    #產生資料，根據需求在前面加上空白
    result = [' '*8 for i in range(start+1)]
    result += list(map(lambda d: str(d).ljust(8), range(1, day+1)))
    #列印資料
    for i, day in enumerate(result):
        if i!=0 and i%7==0:
            print()
        print(day, end='')
    print()
def main(year, month=-1):
    if type(year)!=int or year<1000 or year>10000:
        print('Year error')
        return
    if type(month)==int:
        #如果沒有指定月份，就列印全年的日曆
        if month==-1:
            for m in range(1, 13):
                myCalendar(year, m)
        #如果指定了月份，就只列印這一個月的日曆
        elif month in range(1,13):
            myCalendar(year, month)
        else:
            print('Month error')
            return
    else:
        print('Month error')
        return
main(2017)

```
# 範例程式:03-04.py
```
a_list = ['a', 'b', 'mpilgrim', 'z', 'example']
for i, v in enumerate(a_list):
    print('列表的第', i+1, '個元素是：', v)

```
# 範例程式:03-05.py
```
for i in range(1, 101):
    if i % 7 == 0 and i % 5 != 0:
        print(i)

```
# 範例程式:03-06.py
```
for i in range(100, 1000):
	ge =  i % 10
	shi = i // 10 % 10
	bai = i // 100
	if ge**3+shi**3+bai**3 == i:
		print(i)

```
# 範例程式:03-07.py
```
score = [70, 90, 78, 85, 97, 94, 65, 80]
s = 0
for i in score:
	s += i
print(s / len(score))

```
# 範例程式:03-08.py
```
for i in range(1, 10):
    for j in range(1, i+1):
        print('{0}*{1}={2}'.format(i,j,i*j), end='  ')
    print()		#列印空行

```
# 範例程式:03-09.py
```
for i in range(200, 0, -1):
	if i%17 == 0:
		print(i)
		break

```
# 範例程式:03-10.py
```
import math

n = input("Input an integer:")
n = int(n)
m = int(math.sqrt(n)+2)
for i in range(2, m):
	if n%i == 0:
		print('No')
		break
else:
	print('Yes')

```
# 範例程式:03-11.py
```
for ji in range(0, 31):
    if 2*ji + (30-ji)*4 == 90:
        print('ji:', ji, ' tu:', 30-ji)

```
# 範例程式:03-11_2.py
```
def demo(jitu, tui):
    tu = (tui - jitu*2) / 2
    if int(tu) == tu:
        return (int(jitu-tu), int(tu))

print(demo(30, 90))

```
# 範例程式:03-12.py
```
digits = (1, 2, 3, 4)
for i in digits:
    ii = i*100
    for j in digits:
        if j==i:
            continue
        jj = j*10
        for k in digits:
            if k==i or k==j:
                continue
            print(ii+jj+k)

```
# 範例程式:03-13.py
```
def Cni1(n, i):
	import math
	return int(math.factorial(n)/math.factorial(i)/math.factorial(n-i))

def Cni2(n,i):
    if not (isinstance(n,int) and isinstance(i,int) and n>=i):
        print('n and i must be integers and n must be larger than or equal to i.')
        return
    result = 1
    Min, Max = sorted((i,n-i))
    for i in range(n,0,-1):
        if i>Max:
            result *= i
        elif i<=Min:
            result /= i
    return result
print(Cni1(6,2))

```
# 範例程式:03-14.py
```
def licai(base, rate, days):
    result = base				#初始投資金額
    times = 365//days			#整除，用來計算一年可以滾動多少期
    for i in range(times):
        result = result +result*rate/365*days
    return result
print(licai(100000, 0.0385, 14))	#14天理財，利率0.0385，投資10萬

```
# 範例程式:03-15.py
```
from math import pi as PI

def CircleArea(r):
    if isinstance(r, (int, float)): 	#確保接收的參數為數值
        return PI*r*r
    else:
        print('You must give me an integer or float as radius.')

print(CircleArea(3))

```
# 範例程式:03-16.py
```
def demo(*para):
    avg = sum(para) / len(para)		#平均值
    g = [i for i in para if i>avg]		#列表推導式
    return (avg,)+tuple(g)

print(demo(1, 2, 3, 4))

```
# 範例程式:03-17.py
```
def demo(s):
    result = [0, 0]
    for ch in s:
        if 'a'<=ch<='z':
            result[1] += 1
        elif 'A'<=ch<='Z':
            result[0] += 1
    return result

print(demo('aaaabbbbC'))

```
# 範例程式:03-18.py
```
def demo(lst, k):
    x = lst[:k]
    x.reverse()
    y = lst[k:]
    y.reverse()
    r = x+y
    return list(reversed(r))

lst = list(range(1, 21))
print(lst)
print(demo(lst, 5))

```
# 範例程式:03-18_2.py
```
def demo(lst, k):
    temp = lst[:]
    for i in range(k):
        temp.append(temp.pop(0))
    return temp

```
# 範例程式:03-19.py
```
def demo(t):
    a, b = 1, 1
    while b < t:
        a, b = b, a+b
    else:
        return b

print(demo(50))

```
# 範例程式:03-20.py
```
import random

def demo(lst):
    m = min(lst)
    result = (m,)
    positions = [index for index, value in enumerate(lst) if value==m]
    result = result + tuple(positions)
    return result

x = [random.randint(1, 20) for i in range(50)]
print(x)
print(demo(x))

```
# 範例程式:03-21.py
```
def demo(t):
    result = [[1], [1, 1]]
    line = [1, 1]
    for i in range(2, t):
        r = []
        for j in range(0, len(line)-1):
            r.append(line[j]+line[j+1])
        line = [1]+r+[1]
        result.append(line)
    return result

def output(result):
    for item in result:
        print(item)

output(demo(10))

```
# 範例程式:03-22.py
```
import math

def IsPrime(n):
    m = int(math.sqrt(n))+1
    for i in range(2, m):
        if n%i==0:
            return False
    return True

def demo(n):
    if isinstance(n, int) and n>0 and n%2==0:
        for i in range(3, int(n/2)+1):
            if i%2==1 and IsPrime(i) and IsPrime(n-i):
                print(i, '+', n-i, '=', n)

demo(60)

```
# 範例程式:03-23.py
```
def demo(m, n):
    if m>n:
        m, n = n, m
    p = m*n
    while m!=0:
        r = n%m
        n = m
        m = r
    return (n, int(p/n))

print(demo(20, 30))

```
# 範例程式:03-24.py
```
import random

def demo(x, n):
    if n not in x:
        print(n, ' is not an element of ', x)
        return

    i = x.index(n)			#取得指定元素在列表中的索引
    x[0], x[i] = x[i], x[0]	#將指定元素與第0個元素交換
    key = x[0]
    
    i = 0
    j = len(x) - 1
    while i<j:
        while i<j and x[j]>=key:	#由後向前尋找第一個比指定元素小的元素
            j -= 1
        x[i] = x[j] 
        
        while i<j and x[i]<=key:	#由前向後尋找第一個比指定元素大的元素
            i += 1
        x[j] = x[i]
        
    x[i] = key

x =list(range(1, 10))
random.shuffle(x)			#打亂元素的順序
print(x)
demo(x, 4)
print(x)

```
# 範例程式:03-25.py
```
def Rate(origin, userInput):
    if not (isinstance(origin, str) and isinstance(userInput, str)):
        print('The two parameters must be strings.')
        return
    if len(origin)<len(userInput):
        print('Sorry. I suppose the second parameter string is shorter.')
        return    
    right = 0					#精確符合的字元個數
    for origin_char, user_char in zip(origin, userInput):
        if origin_char==user_char:
            right += 1
    return right/len(origin)

origin = 'Shandong Institute of Business and Technology'
userInput = 'ShanDong institute of business and technolog'
print(Rate(origin, userInput))	#輸出測試結果

```
# 範例程式:03-26.py
```
from random import randint
from math import sqrt

def factoring(n):
    '''對大數進行質因數分解'''
    if not isinstance(n, int):
        print('You must give me an integer')
        return
    #開始分解，把所有質因數都加到result列表
    result = []
    for p in primes:
        while n!=1:
            if n%p == 0:
                n = n/p
                result.append(p)
            else:
                break
        else:
            result = map(str, result)
            result = '*'.join(result)
            return result
    #考慮參數本身就是質數的情況
    if not result:
        return n

testData = [randint(10, 100000) for i in range(50)]
#亂數中的最大數
maxData = max(testData)
#小於maxData的所有質數
primes = [ p for p in range(2, maxData) if 0 not in [ p% d for d in range(2, int(sqrt(p))+1)] ]

for data in testData:
    r = factoring(data)
    print(data, '=', r)
    #測試分解結果是否正確
    print(data==eval(r))

```
# 範例程式:03-27.py
```
from functools import reduce
from math import gcd

def isCoPrime(p):
    '''判斷p中每個元組的第1個數（即mi）之間是否互為質數'''
    for index, item1 in enumerate(p):
        for item2 in p[index+1:]:
            if gcd(item1[0], item2[0])!=1:
                return False
    return True

def extEuclid(Mi, mi):
    '''暴力窮舉法，求Mi對mi的乘法逆元，也可擴展歐幾里得演算法快速求解'''
    for i in range(1, mi):
        if i*Mi % mi == 1:
            return i

def chineseRemainder(p):
    '''p為[(3, 2), (7, 1), (13, 5),(mi, ai) ...]形式的參數，其中3/7/13為商，2/1/5為餘數'''
    #先判斷資料中的mi是否互為質數，如果不是則提示資料錯誤並退出
    if not isCoPrime(p):
        return 'Data error.'        
    #切片淺複製，臨時變數，防止修改實參中的資料
    pp = p[:]
    #求M=m1*m2*m3*...*mn
    ppp = [item[0] for item in pp]
    M = reduce(lambda x,y: x*y, ppp)
    for index, item in enumerate(pp):
        Mi = int(M/item[0])
        bi = extEuclid(Mi, item[0])
        pp[index] = item+(Mi, bi)
    #求解最終結果，sum(ai*bi*Mi) mod M
    result = sum([item[1]*item[2]*item[3] for item in pp])
    result = result % M
    #考慮特殊情況，不允許結果為1
    if result==1:
        result = result+M
    return result

data = [[(3,2), (5,3), (7,2)],
        [(5,1), (3,2)],
        [(5,1), (3,1)],
        [(5,4), (3,2)],
        [(7,2), (8,4), (9,3)],
        [(5,2), (6,4), (7,4)],
        [(3,2), (5,3), (7,4)]]
for p in data:
    print(p)
    print(chineseRemainder(p))

```
# 範例程式:03-27_2.py
```
def chineseRemainder(p):
    '''p為[(3, 2), (7, 1), (13, 5), ...]形式的參數，其中3/7/13為商，2/1/5為餘數'''
    #檢查資料是否合法，若有相同商對應至不同餘數，則認為給的資料不合法
    for index1, pair1 in enumerate(p):
        for pair2 in p[index1+1:]:
            if pair1[0]==pair2[0] and pair1[1]!=pair2[1]:
                print('Data Error.')
                return
    #對給定資料按商從大到小排序
    p = sorted(p, key=lambda x:x[0], reverse=True)
    #產生巢狀列表
    possibleValues = list(map(lambda x: list((i*x[0]+x[1] for i in range(1,10000))), p))
    #尋找第一個共同包含的數，該數即為符合條件的最小數
    for value in possibleValues[0]:
        flag = True
        for rest in possibleValues[1:]:
            if value not in rest:
                flag = False
        if flag:
            print(value)
            return
    else:
        print('Can not find a number')

p = [[(5,3), (9,3), (13,3),(17,3)],
     [(9,7), (5,2), (4,3)],
     [(3,2), (5,3), (7,2)],
     [(3,2), (4,1)],
     [(2,1), (4,3), (5,2), (7,3), (9,4)],
     [(2,1), (3,2), (5,4), (6,5), (7,0)]]
for pp in p:
    chineseRemainder(pp)

```
# 範例程式:03-28.py
```
import random

def hongbao(total, num):
    #total表示擬發紅包的總金額
    #num表示擬發紅包的數量
    each = []
    #已發紅包總金額
    already = 0
    for i in range(1, num):
        #為目前搶紅包的人隨機分配金額
        #至少給剩下的每個人留一分錢
        t = random.randint(1, (total-already)-(num-i))
        each.append(t)
        already = already+t
    #剩餘所有的錢發給最後一個人
    each.append(total-already) 
    return each

if __name__=='__main__':
    total = 5
    num = 5
    #模擬30次
    for i in range(30):
        each = hongbao(total, num)
        print(each) 

```
# 範例程式:03-29.py
```
def convert(YearMonthDay):
    if not isinstance(YearMonthDay, str):
        return 'Type Error. Must be str'
    if YearMonthDay.count('-') != 2:
        return 'Parameter Error. Must contains 2 -'
    data = YearMonthDay.split('-')
    if (len(data[0]) != 4) or (len(data[1]) not in (1, 2)) or (len(data[2]) not in (1, 2)):
        return 'Parameter Error. Must be YYYY-MM-DD'
    try:
        year, month, day = map(int, data)
        quarter = [[3, 4, 5], [6, 7, 8], [9, 10, 11], [12, 1, 2]]
        for q, m in enumerate(quarter):
            if month in m:
                return str(year) + str(q+1)
    except:
        return 'Parameter Error. Must be YYYY-MM-DD, and all be digits'

print(convert('2016-a9-27'))

```
# 範例程式:03-30.py
```
def conv(lst1, lst2):
    '''計算兩個列表所表示的訊號的摺積，並返回一個列表'''
    result = []
    #翻轉第一個列表
    lst1.reverse()
    length1 = len(lst1)
    length2 = len(lst2)
    #移動翻轉後的第一個列表，直到“「完全移入”」
    for i in range(1, length1+1):
        t = lst1[length1-i:]
        #計算重疊“「面積”」
        v = sum((item1*item2 for item1, item2 in zip(t,lst2)))
        result.append(v)
    #繼續移動翻轉後的第一個列表，直到“「完全移出”」
    for i in range(1, length2):
        t = lst2[i:]
        v = sum((item1*item2 for item1, item2 in zip(lst1,t)))
        result.append(v)
    return result

def mul(lst):
    '''把列表中的數字轉換為普通整數的形式'''
    result = ''
    c = 0
    for item in lst[::-1]:
        item = item + c
        #計算目前位數的餘數，以及向前一位進位的數字
        n, c = str(item%10), item //10
        #使用字串記錄臨時結果
        result += n
    if c:
        result += str(c)
    return eval(result[::-1])

def main(num1, num2):
    lst1 = list(map(int, str(num1)))
    lst2 = list(map(int, str(num2)))
    result = conv(lst1, lst2)
    print(mul(result)==num1*num2)

from random import randint
for i in range(100):
    num1 = randint(1, 99999999)
    num2 = randint(1, 99999999999)
    main(num1, num2)

```
# 範例程式:03-31.py
```
from random import randint

def guess():
    #隨機產生一個整數
    value = randint(1,1000)
    #最多允許猜5次
    maxTimes = 5
    for i in range(maxTimes):
        prompt = 'Start to GUESS:' if i==0 else 'Guess again:'
        #加上異常處理結構，防止輸入不是數字的情況
        try:
            x = int(input(prompt))
            #猜對了
            if x == value:
                print('Congratulations!')
            elif x > value:
                print('Too big')
            else:
                print('Too little')
        except:
            print('Must input an integer between 1 and 999')
    else:
        #次數用完還沒猜對，遊戲結束，提示正確答案
        print('Game over. FAIL.')
        print('The value is ', value)

guess()

```
# 範例程式:03-32.py
```
def demo(v, n):
    assert 0<v<10, 'v must between 1 and 9'
    assert type(n)==int, 'n must be integer'
    result, t = 0, 0
    for i in range(n):
        t = t*10 + v
        result += t
    return result

print(demo(3, 4))

```
# 範例程式:03-33.py
```
from itertools import cycle

def demo(lst, k):
    #切片，以免影響原來的資料
    t_lst = lst[:]
    #遊戲一直進行到只剩下最後一個人
    while len(t_lst)>1:
        #建立cycle物件
        c = cycle(t_lst)
        #從1到k報數
        for i in range(k):
            t = next(c)
        #一個人出局，圈子縮小
        index = t_lst.index(t)
        t_lst = t_lst[index+1:] + t_lst[:index]
        #測試用，查看每次一個人出局之後剩餘人數的編號
        print(t_lst)
    #遊戲結束
    return t_lst[0]

lst = list(range(1,11))
print(demo(lst, 3))

```
# 範例程式:03-34.py
```
def hannuo(num, src, dst, temp=None):
    #宣告用來記錄移動次數的變數為全域變數
    global times
    #確認參數類型和範圍
    assert type(num) == int, 'num must be integer'
    assert num > 0, 'num must > 0'
    #只剩最後或只有一個盤子需要移動，這也是函數遞迴呼叫的結束條件
    if num == 1:
        print('The {0} Times move:{1}==>{2}'.format(times, src, dst))
        times += 1
    else:
        #遞迴呼叫函數本身，
        #先把除了最後一個盤子之外的所有盤子，移動到臨時柱子上
        hannuo(num-1, src, temp, dst)
        #把最後一個盤子直接移動到目標柱子上
        hannuo(1, src, dst)
        #把除了最後一個盤子之外的其他盤子，從臨時柱子上移動到目標柱子上
        hannuo(num-1, temp, dst, src)
#用來記錄移動次數的變數
times = 1
#A表示最初放置盤子的柱子，C是目標柱子，B是臨時柱子
hannuo(3, 'A', 'C', 'B')

```
# 範例程式:03-35.py
```
def main(n):
    '''參數n表示數字的位元數，例如n=3時，返回495'''
    #待測試數字範圍的起點和結束值
    start = 10**(n-1)+2
    end = start*10-20
    #依序測試每位數
    for i in range(start, end):
        i = str(i)
        #由這幾個數字組成的最大數
        big = ''.join(sorted(i,reverse=True))
        big = int(big)
        #由這幾個數字組成的最小數
        little = ''.join(sorted(i))
        little = int(little)
        if big-little==int(i):
            print(i)
n = 4
main(n)

```
# 範例程式:03-36.py
```
from random import randint
from itertools import permutations

#4個數字和2個運算子可能組成的運算式形式
exps = ('((%s %s %s) %s %s) %s %s',
        '(%s %s %s) %s (%s %s %s)', 
        '(%s %s (%s %s %s)) %s %s',
        '%s %s ((%s %s %s) %s %s)',
        '%s %s (%s %s (%s %s %s))')
ops = r'+-*/'

def test24(v):
    result = []
    #Python允許函數的巢狀定義
    #這個函數對字串運算式求值，並驗證是否等於24
    def check(exp):
        try:
            #有可能會出現除0異常，所以放到異常處理結構中
            return int(eval(exp)) == 24
        except:
            return False
    #全排列，列舉4個數所有可能的順序
    for a in permutations(v):
        #找尋4個數的目前排列能實現24的運算式
        t = [exp % (a[0], op1, a[1], op2, a[2], op3, a[3]) for op1 in ops for op2 in ops for op3 in ops for exp in exps if check(exp %(a[0], op1, a[1], op2, a[2], op3, a[3]))]
        if t:
            result.append(t)
    return result

for i in range(20):
    print('='*20)
    #產生亂數進行測試
    lst = [randint(1, 14) for j in range(4)]
    r = test24(lst)
    if r:
        print(r)
    else:
        print('No answer for ', lst)

```
# 範例程式:03-37.py
```
import random

def doubleColor():
    red = random.sample(range(1,34), 6)
    blue = random.choice(range(1, 17))
    return str(red)+'-'+str(blue)

print(doubleColor())

```
# 範例程式:03-38.py
```
def isValid(s, col):
    '''本函數用來檢查最後一個皇后的位置是否合法'''
    #目前皇后的行號
    row = len(s)
    #檢查目前的皇后們是否有衝突
    for r, c in enumerate(s):
        #如果該列已有皇后，或者某個皇后與目前皇后的水平與垂直距離相等
        #就表示目前皇后的位置不合法，不允許放置
        if c == col or abs(row - r) == abs(col - c):
            return False
 
    return True
 
def queen(n, s=()):
    '''本函數返回的結果是每個皇后所在的列號'''
    #已是最後一個皇后，保存本次結果
    if len(s) == n:
        return [s]
 
    res = []
    for col in range(n):
        if not isValid(s, col): continue
        for r in queen(n, s + (col,)):
            res.append(r)
 
    return res

#形式轉換，最終結果中包含每個皇后所在的行號和列號
result = [[(r, c) for r, c in enumerate(s)] for s in queen(8)]
#輸出合法結果的數量
print(len(result))
#輸出所有可能的結果，也就是所有皇后的擺放位置
#結果中每個皇后的位置是一個元組，裡面兩個數字分別是行號和列號
for r in result:
    print(r)

```
# 範例程式:04_01_01.py
```
class Car(object):		#定義一個類別，派生繼承自object類別
	def infor(self):	#定義成員方法
		print(" This is a car ")

```
# 範例程式:04_01_02.py
```
class A:
	def __init__(self, value1 = 0, value2 = 0):	#建構函數
		self._value1 = value1
		self.__value2 = value2			#私有成員
	def setValue(self, value1, value2):	#成員方法
		self._value1 = value1
		self.__value2 = value2	#類別內部可以直接存取私有成員
	def show(self):				#成員方法
		print(self._value1)
		print(self.__value2)

```
# 範例程式:04_01_03.py
```
class Car(object):
    price = 100000				#屬於類別的資料成員
    def __init__(self, c):
        self.color = c			#屬於物件的資料成員

car1 = Car("Red")				#產生實體物件
car2 = Car("Blue")
print(car1.color, Car.price)	#存取物件和類別的資料成員
Car.price = 110000				#修改類別屬性
Car.name = 'QQ'					#動態增加類別屬性
car1.color = "Yellow"			#修改實例屬性
print(car2.color, Car.price, Car.name)
print(car1.color, Car.price, Car.name)
def setSpeed(self, s):
    self.speed = s
car1.setSpeed = types.MethodType(setSpeed, car1)	#動態為物件增加成員方法
car1.setSpeed(50)				#呼叫物件的成員方法
print(car1.speed)

```
# 範例程式:04_01_04.py
```
class Root:
	__total = 0
	def __init__(self, v):	#建構函數
			self.__value = v
			Root.__total += 1

	def show(self):		#普通的實例方法
			print('self.__value:', self.__value)
			print('Root.__total:', Root.__total)

	@classmethod			#修飾器，宣告類別方法
	def classShowTotal(cls):	#類別方法
			print(cls.__total)

	@staticmethod			#修飾器，宣告靜態方法
	def staticShowTotal():	#靜態方法
			print(Root.__total)

```
# 範例程式:04_01_05.py
```
class Test:
	def __init__(self, value):
		self.__value = value

	def __get(self):
		return self.__value

	def __set(self, v):
		self.__value = v

	def __del(self):		#刪除物件的私有資料成員
		del self.__value

	value = property(__get, __set, __del)	#可讀、可寫、可刪除的屬性

	def show(self):
		print(self.__value)

```
# 範例程式:04-01.py
```
#基礎類別必須繼承object，否則衍生類別將無法使用super()函數
class Person(object): 
    def __init__(self, name = '', age = 20, sex = 'man'):
        self.setName(name)		#透過呼叫方法進行初始化
        self.setAge(age)		#這樣可以對參數進行更好的控制
        self.setSex(sex)

    def setName(self, name):
        if not isinstance(name, str):
            print('name must be string.')
            #如果資料不合法，就使用預設值
            self.__name = ''
            return
        self.__name = name

    def setAge(self, age):
        if type(age) != int:
            print('age must be integer.')
            self.__age = 20
            return
        self.__age = age

    def setSex(self, sex):
        if sex not in ('man', 'woman'):
            print('sex must be "man" or "woman"')
            self.__sex = 'man'
            return
        self.__sex = sex

    def show(self):
        print(self.__name, self.__age, self.__sex, sep='\n')

#衍生類別
class Teacher(Person):
    def __init__(self, name='', age = 30, sex = 'man', department = 'Computer'):
        #呼叫基礎類別建構方法，初始化相關的私有資料成員
        super(Teacher, self).__init__(name, age, sex)
        #也可以這樣初始化基礎類別的私有資料成員
        #Person.__init__(self, name, age, sex)
        #初始化衍生類別的資料成員
        self.setDepartment(department) 

    def setDepartment(self, department):
        if type(department) != str:
            print('department must be a string.')
            self.__department = 'Computer'
            return
        self.__department = department

    def show(self):
        super(Teacher, self).show()
        print(self.__department)

if __name__ =='__main__':
    #建立基礎類別物件
    zhangsan = Person('Zhang San', 19, 'man')
    zhangsan.show()
    print('='*30)

    #建立衍生類別物件
    lisi = Teacher('Li si', 32, 'man', 'Math')
    lisi.show()
    #呼叫繼承的方法修改年齡
    lisi.setAge(40)
    lisi.show()

```
# 範例程式:04-03.py
```
class simNumpyArray(object):
    def __init__(self, p):
        '''可接收列表、元組、range物件等類型的資料，並且每個元素都必須為數字'''
        if type(p) not in (list, tuple, range):
            print('data type error')
            return
        for item in p:
            #下面這行用來判斷參數類型，可以改成這樣
            #if isinstance(item, (int, float, complex)):
            if type(item) not in (int, float, complex):
                print('data type error')
                return
        self.__data = [list(p)]
        self.__row = 1
        self.__col = len(p)

    #解構函數
    def __del__(self):
        del self.__data

    #修改大小，首先檢查給定的大小參數是否合適
    def reshape(self, size):
        '''參數必須是元組或列表，如(row,col)或[row,col]
           row或col其中一個可以為-1，表示自動計算
        '''
        if not (isinstance(size, list) or isinstance(size, tuple)):
            print('size parameter error')
            return
        if len(size) != 2:
            print('size parameter error')
            return
        if (not isinstance(size[0],int)) or (not isinstance(size[1],int)):
            print('size parameter error')
            return
        if size[0] != -1 and size[1] != -1 and size[0]*size[1] != self.__row*self.__col:
            print('size parameter error')
            return
        #行數或列數為-1，表示該值自動計算
        if size[0] == -1:
            if size[1] == -1 or (self.__row*self.__col)%size[1] != 0:
                print('size parameter error')
                return
        if size[1] == -1:
            if size[0] == -1 or (self.__row*self.__col)%size[0] != 0:
                print('size parameter error')
                return
                
        #重新合併資料
        data = [t for i in self.__data for t in i]
        #修改大小
        if size[0] == -1:
            self.__row = int(self.__row*self.__col/size[1])
            self.__col = size[1]
        elif size[1] == -1:
            self.__col = int(self.__row*self.__col/size[0])
            self.__row = size[0]
        else:
            self.__row = size[0]
            self.__col = size[1]            
        self.__data = [[data[row*self.__col+col] for col in range(self.__col)] for row in range(self.__row)]

    #在交互模式下直接使用變數名稱作為運算式，查看值時呼叫該函數
    def __repr__(self):
        #return repr('\n'.join(map(str, self.__data)))
        for i in self.__data:
            print(i)
        return ''

    #以print()函數輸出值時呼叫該函數
    def __str__(self):
        return '\n'.join(map(str, self.__data))
    
    #屬性，矩陣轉置
    @property
    def T(self):
        b = simNumpyArray([t for i in self.__data for t in i])
        b.reshape((self.__row, self.__col))
        b.__data = list(map(list,zip(*b.__data)))
        b.__row, b.__col = b.__col, b.__row
        return b

    #通用程式碼，適用於矩陣與整數、實數、複數的加、減、乘、除、整除、指數
    def __operate(self, n, op):
        b = simNumpyArray([t for i in self.__data for t in i])
        b.reshape((self.__row, self.__col))
        b.__data = [[eval(str(j)+op+str(n)) for j in item] for item in b.__data]
        return b

    #通用程式碼，適用於矩陣之間的加、減
    def __matrixAddSub(self, n, op):
        c = simNumpyArray([1])
        c.__row = self.__row
        c.__col = self.__col
        c.__data = [[eval(str(x[i])+op+str(y[i])) for i in range(len(x))] for x,y in zip(self.__data, n.__data)]
        return c
    
    #所有元素統一加一個數字，或者兩個矩陣相加
    def __add__(self, n):
        #若參數是整數或實數，則返回矩陣
        #其中每個元素為原矩陣中元素與該整數或實數相加的結果
        if type(n) in (int, float, complex):
            return self.__operate(n, '+')
        elif isinstance(n, simNumpyArray):
            #如果參數為同類型矩陣，且大小一致，則為兩個矩陣的對應元素相加
            if n.__row==self.__row and n.__col==self.__col:
                return self.__matrixAddSub(n, '+')
            else:
                print('two matrix must be the same size')
                return
        else:
            print('data type error')
            return
            
    #所有元素統一減一個數字，或者兩個矩陣相減
    def __sub__(self, n):
        #若參數是整數或實數，則返回矩陣
        #其中每個元素為原矩陣中元素與該整數或實數相減的結果
        if type(n) in (int, float, complex):
            return self.__operate(n, '-')
        elif isinstance(n, simNumpyArray):
            #如果參數為同類型矩陣，且大小一致，則為兩個矩陣的對應元素相減
            if n.__row==self.__row and n.__col==self.__col:
                #先產生一個實體的臨時物件，內容為[1]
                return self.__matrixAddSub(n, '-')
            else:
                print('two matrix must be the same size')
                return
        else:
            print('data type error')
            return
            
    #所有元素統一乘一個數字，或者兩個矩陣相乘
    def __mul__(self, n):
        #若參數是整數或實數，則返回矩陣
        #其中每個元素為原矩陣中元素與該整數或實數相乘的結果
        if type(n) in (int, float, complex):
            return self.__operate(n, '*')
        elif isinstance(n, simNumpyArray):
            #如果參數為同類型矩陣，且第一個矩陣的列數等於第二個矩陣的行數
            if n.__row==self.__col:     
                data = []
                for row in self.__data:
                    t = []
                    for ii in range(n.__col):
                        col = [c[ii] for c in n.__data]
                        tt = sum([i*j for i,j in zip(row,col)])
                        t.append(tt)
                    data.append(t)
                c = simNumpyArray([t for i in data for t in i])
                c.reshape((self.__row, n.__col))
                return c
            else:
                print('size error.')
                return
        else:
            print('data type error')
            return
        
    #所有元素統一除以一個數字，此處採用Python 3.x編寫，真除法
    def __truediv__(self, n):
        if type(n) in (int, float, complex):
            return self.__operate(n, '/')
        else:
            print('data type error')
            return

    #矩陣元素與數字計算整商
    def __floordiv__(self, n):
        if type(n) in (int, float, complex):
            return self.__operate(n, '//')
        else:
            print('data type error')
            return
        
    #矩陣與數字的指數運算
    def __pow__(self, n):
        if type(n) in (int, float, complex):
            return self.__operate(n, '**')
        else:
            print('data type error')
            return

    #測試兩個矩陣是否相等
    def __eq__(self, n):
        if isinstance(n, simNumpyArray):
            if self.__data == n.__data:
                return True
            else:
                return False
        else:
            print('data type error')
            return

    #測試矩陣本身是否小於另一個矩陣
    def __lt__(self, n):
        if isinstance(n, simNumpyArray):
            if self.__data < n.__data:
                return True
            else:
                return False
        else:
            print('data type error')
            return

    #成員測試運算子
    def __contains__(self, v):
        if v in self.__data:
            return True
        else:
            return False

    #支援反覆運算
    def __iter__(self):
        return iter(self.__data)

    #通用方法，計算三角函數
    def __triangle(self, method):
        try:
            b = simNumpyArray([t for i in self.__data for t in i])
            b.reshape((self.__row, self.__col))
            b.__data = [[eval("__import__('math')."+method+"("+str(j)+")") for j in item] for item in b.__data]
            return b
        except:
            return 'method error'
        
    #屬性，對所有元素求正弦
    @property
    def Sin(self):        
        return self.__triangle('sin')

    #屬性，對所有元素求餘弦
    @property
    def Cos(self):
        return self.__triangle('cos')

```
# 範例程式:04-07.py
```
class DirectedGraph(object):
    def __init__(self, d):
        if isinstance(d, dict):
            self.__graph = d
        else:
            self.__graph = dict()
            print('Sth error')

    def __generatePath(self, graph, path, end, results):
        current = path[-1]
        if current == end:
            results.append(path)
        else:
            for n in graph[current]:
                if n not in path:
                    self.__generatePath(graph, path + [n], end, results)

    def searchPath(self, start, end):
        self.__results = []
        self.__generatePath(self.__graph, [start], end, self.__results)
        self.__results.sort(key = lambda x:len(x))    #按照所有路徑的長度排序
        print('The path from ',self.__results[0][0], ' to ', self.__results[0][-1], ' is:')
        for path in self.__results:
            print(path)

d = {'A':['B', 'C', 'D'],
     'B':['E'],
     'C':['D', 'F'],
     'D':['B', 'E', 'G'],
     'E':['D'],
     'F':['D', 'G'],
     'G':['E']}
g = DirectedGraph(d)
g.searchPath('A', 'D')
g.searchPath('A', 'E')

```
# 範例程式:BinaryTree.py
```
class BinaryTree:
    def __init__(self, value):
        self.__left = None
        self.__right =  None
        self.__data = value

    #解構函數
    def __del__(self):
        del self.__data

    def insertLeftChild(self, value):	#建立左子樹
        if self.__left:
            print('__left child tree already exists.')
        else:
            self.__left = BinaryTree(value)
            return self.__left
        
    def insertRightChild(self, value):	#建立右子樹
        if self.__right:
            print('Right child tree already exists.')
        else:
            self.__right = BinaryTree(value)
            return self.__right
        
    def show(self):
        print(self.__data)

    def preOrder(self):			#前序遍歷
        print(self.__data)			#輸出根節點的值
        if self.__left:
            self.__left.preOrder()	#遍歷左子樹
        if self.__right:
            self.__right.preOrder()	#遍歷右子樹

    def postOrder(self):			#後序遍歷
        if self.__left:
            self.__left.postOrder()
        if self.__right:
            self.__right.postOrder()
        print(self.__data)

    def inOrder(self):			#中序遍歷
        if self.__left:
            self.__left.inOrder()
        print(self.__data)
        if self.__right:
            self.__right.inOrder()

if __name__ == '__main__':
    print('Please use me as a module.')

```
# 範例程式:MyArray.py
```
class MyArray:
    '''All the elements in this array must be numbers'''
    def __IsNumber(self, n):
        if not isinstance(n, (int, float, complex)):
            return False
        return True

    #建構函數，進行必要的初始化
    def __init__(self, *args):
        if not args:
            self.__value = []
        else:
            for arg in args:
                if not self.__IsNumber(arg):
                    print('All elements must be numbers')
                    return
            self.__value = list(args)

    #解構函數，釋放內部封裝的列表
    def __del__(self):
        del self.__value

    #重載運算子+
    #陣列的每個元素都與數字n相加，或兩個陣列相加，返回新陣列
    def __add__(self, n):
        if self.__IsNumber(n):
            #陣列的所有元素都與數字n相加
            b = MyArray()
            b.__value = [item+n for item in self.__value]
            return b
        elif isinstance(n, MyArray):
            #兩個等長的陣列對應元素相加
            if len(n.__value)==len(self.__value):
                c = MyArray()
                c.__value = [i+j for i, j in zip(self.__value, n.__value)]
                #for i, j in zip(self.__value, n.__value):
                #    c.__value.append(i+j)
                return c
            else:
                print('Lenght not equal')                
        else:
            print('Not supported')

    #重載運算子-
    ##陣列的每個元素都與數字n相減，返回新陣列
    def __sub__(self, n):
        if not self.__IsNumber(n):
            print('- operating with ', type(n), ' and number type is not supported.')
            return
        b = MyArray()
        b.__value = [item-n for item in self.__value]
        return b

    #重載運算子*
    #陣列的每個元素都與數字n相乘，返回新陣列
    def __mul__(self, n):
        if not self.__IsNumber(n):
            print('* operating with ', type(n), ' and number type is not supported.')
            return
        b = MyArray()
        b.__value = [item*n for item in self.__value]
        return b

    #重載運算子/
    #陣列的每個元素都與數字n相除，返回新陣列
    def __truediv__(self, n):
        if not self.__IsNumber(n):
            print(r'/ operating with ', type(n), ' and number type is not supported.')
            return
        b = MyArray()
        b.__value = [item/n for item in self.__value]
        return b

    #重載運算子//
    #陣列的每個元素都與數字n整除，返回新陣列
    def __floordiv__(self, n):
        if not isinstance(n, int):
            print(n, ' is not an integer')
            return
        b = MyArray()
        b.__value = [item//n for item in self.__value]
        return b

    #重載運算子%
    #陣列的每個元素都與數字n求餘數，返回新陣列
    def __mod__(self, n):
        if not self.__IsNumber(n):
            print(r'% operating with ', type(n), ' and number type is not supported.')
            return
        b = MyArray()
        b.__value = [item%n for item in self.__value]
        return b

    #重載運算子**
    #陣列的每個元素都與數字n進行指數計算，返回新陣列
    def __pow__(self, n):
        if not self.__IsNumber(n):
            print('** operating with ', type(n), ' and number type is not supported.')
            return
        b = MyArray()
        b.__value = [item**n for item in self.__value]
        return b

    def __len__(self):        
        return len(self.__value)

    #直接使用該類別物件作為運算式，藉以查看物件的值
    def __repr__(self):
        #equivalent to return `self.__value`
        return repr(self.__value)

    #支援print()函數查看物件的值
    def __str__(self):
        return str(self.__value)

    #增加元素
    def append(self, v):
        if not self.__IsNumber(v):
            print('Only number can be appended.')
            return
        self.__value.append(v)

    #取得指定下標的元素值，支援以列表或元組指定多個下標
    def __getitem__(self, index): 
        length = len(self.__value)
        #如果指定單個整數作為下標，則直接返回元素值
        if isinstance(index, int) and 0<=index<length: 
            return self.__value[index]
        #以列表或元組指定多個整數下標
        elif isinstance(index, (list,tuple)):
            for i in index:
                if not (isinstance(i,int) and 0<=i<length):
                    return 'index error'
            result = []
            for item in index:
                result.append(self.__value[item])
            return result
        else:
            return 'index error'

    #修改元素值，支援以列表或元組指定多個下標，同時修改多個元素值
    def __setitem__(self, index, value):
        length = len(self.__value)
        #如果下標合法，則直接修改元素值
        if isinstance(index, int) and 0<=index<length:
            self.__value[index] = value
        #支援以列表或元組指定多個下標
        elif isinstance(index, (list,tuple)):
            for i in index:
                if not (isinstance(i,int) and 0<=i<length):
                    raise Exception('index error')
            #如果下標和值都是列表或元組，並且個數一樣
            #則分別為多個下標的元素修改值
            if isinstance(value, (list,tuple)):
                if len(index) == len(value):
                    for i, v in enumerate(index):
                        self.__value[v] = value[i]
                else:
                    raise Exception('values and index must be of the same length')
            #如果指定多個下標和一個普通值，則把多個元素修改為相同的值
            elif isinstance(value, (int,float,complex)):
                for i in index:
                    self.__value[i] = value
            else:
                raise Exception('value error')
        else:
            raise Exception('index error')

    #支援成員測試運算子in，測試陣列是否包含某個元素
    def __contains__(self, v):        
        if v in self.__value:
            return True
        return False

    #模擬向量內積
    def dot(self, v):
        if not isinstance(v, MyArray):
            print(v, ' must be an instance of MyArray.')
            return
        if len(v) != len(self.__value):
            print('The size must be equal.')
            return
        return sum([i*j for i,j in zip(self.__value, v.__value)])
        #b = MyArray()
        #for m, n in zip(v.__value, self.__value):
        #    b.__value.append(m * n)
        #return sum(b.__value)

    #重載運算子==，測試兩個陣列是否相等
    def __eq__(self, v):
        if not isinstance(v, MyArray):
            print(v, ' must be an instance of MyArray.')
            return False
        if self.__value == v.__value:
            return True
        return False

    #重載運算子<，比較兩個陣列大小
    def __lt__(self, v):
        if not isinstance(v, MyArray):
            print(v, ' must be an instance of MyArray.')
            return False
        if self.__value < v.__value:
            return True
        return False

if __name__ == '__main__':
    print('Please use me as a module.')

```
# 範例程式:myQueue.py
```
class myQueue:
    #建構函數，預設佇列大小為10
    def __init__(self, size = 10):
        self._content = []
        self._size = size
        self._current = 0

    #解構函數
    def __del__(self):
        del self._content

    def setSize(self, size):
        if size < self._current:
            #如果縮小佇列，應刪除後面的元素
            for i in range(size, self._current)[::-1]:
                del self._content[i]
            self._current = size
        self._size = size

    #推入
    def put(self, v):
        if self._current < self._size:
            self._content.append(v)
            self._current = self._current+1
        else:
            print('The queue is full')

    #彈出
    def get(self):
        if self._content:
            self._current = self._current-1
            return self._content.pop(0)
        else:
            print('The queue is empty')

    def show(self):
        if self._content:
            print(self._content)
        else:
            print('The queue is empty')

    #佇列清空
    def empty(self):
        self._content = []
        self._current = 0

    def isEmpty(self):
        if not self._content:
            return True
        else:
            return False

    def isFull(self):
        if self._current == self._size:
            return True
        else:
            return False

if __name__ == '__main__':
    print('Please use me as a module.')

```
# 範例程式:mySet.py
```
#自訂集合類別
class Set(object):
    def __init__(self, data=None):
        if data==None:
            self.__data = []
        else:
            if not hasattr(data, '__iter__'):
                #提供的資料不可反覆運算，產生實體失敗
                raise Exception('必須提供可反覆運算的資料類型')
            temp = []
            for item in data:
                #集合中的元素必須可雜湊
                #這裡的可雜湊，與第14章介紹的安全雜湊演算法並不一樣
                hash(item)
                if not item in temp:
                    temp.append(item)
            self.__data = temp

    #解構函數
    def __del__(self):
        del self.__data

    #增加元素，要求元素必須可雜湊
    def add(self, value):
        hash(value)
        if value not in self.__data:
            self.__data.append(value)
        else:
            print('元素已存在，操作被忽略')

    #刪除元素
    def remove(self, value):
        if value in self.__data:
            self.__data.remove(value)
            print('刪除成功')
        else:
            print('元素不存在，刪除操作被忽略')

    #隨機彈出並返回一個元素
    def pop(self):
        if not self.__data:
            print('集合已空，彈出操作被忽略')
            return
        import random
        item = random.choice(self.__data)
        print(item)
        self.__data.remove(item)

    #運算子重載，集合差集運算
    def __sub__(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        #空集合
        result = Set()
        #如果一個元素屬於目前集合而非另一個集合，則新增
        for item in self.__data:
            if item not in anotherSet.__data:
                result.__data.append(item)
        return result
    
    #提供方法，集合差集運算
    def difference(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        return self - anotherSet

    
    #|運算子重載，集合聯集運算
    def __or__(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        result = Set(self.__data)
        for item in anotherSet.__data:
            if item not in result.__data:
                result.__data.append(item)
        return result
    
    #提供方法，集合聯集運算
    def union(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        return self | anotherSet
    
    #&運算子重載，集合交集運算
    def __and__(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        result = Set()
        for item in self.__data:
            if item in anotherSet.__data:
                result.__data.append(item)
        return result
        
    #^運算子重載，集合對稱差集
    def __xor__(self, anotherSet):
        return (self-anotherSet) | (anotherSet-self)

    #提供方法，集合對稱差集運算
    def symetric_difference(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        return self ^ anotherSet

    #==運算子重載，判斷兩個集合是否相等
    def __eq__(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        if sorted(self.__data) == sorted(anotherSet.__data):
            return True
        else:
            return False

    #>運算子重載，集合包含關係
    def __gt__(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        if self != anotherSet:
            flag1 = True
            for item in self.__data:
                if item not in anotherSet.__data:
                    #目前集合中有的元素不屬於另一個集合
                    flag1 = False
                    break
            flag2 = True
            for item in anotherSet.__data:
                if item not in self.__data:
                    #另一個集合中有的元素不屬於目前集合
                    flag2 = False
                    break
            if  not flag1 and flag2:
                return True
            return False

    #>=運算子重載，集合包含關係
    def __ge__(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        return self==anotherSet or self>anotherSet
    
    #提供方法，判斷目前集合是否為另一個集合的真子集
    def issubset(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        if self < anotherSet:
            return True
        else:
            return False

    #提供方法，判斷目前集合是否為另一個集合的超集合
    def issuperset(self, anotherSet):
        if not isinstance(anotherSet, Set):
            raise Exception('類型錯誤')
        if self > anotherSet:
            return True
        else:
            return False


    #提供方法，清空集合所有元素
    def clear(self):
        while self.__data:
            del self.__data[-1]
        print('集合已清空')

    #運算子重載，使得集合可反覆運算
    def __iter__(self):
        return iter(self.__data)

    #運算子重載，支援in運算子
    def __contains__(self, value):
        if value in self.__data:
            return True
        else:
            return False

    #支援內建函數len()
    def __len__(self):
        return len(self.__data)
        
    #直接查看該類別物件時，呼叫此函數
    def __repr__(self):
        return '{'+str(self.__data)[1:-1]+'}'

    #以print()函數輸出該類別物件時，呼叫此函數
    def __str__(self):
        return '{'+str(self.__data)[1:-1]+'}'

```
# 範例程式:Stack.py
```
class Stack:
    def __init__(self, size = 10):
        self._content = []		#使用列表存放堆疊元素
        self._size = size		#初始堆疊大小
        self._current = 0		#堆疊元素的個數初始化為0

    #解構函數
    def __del__(self):
        del self._content

    def empty(self):
        self._content = []
        self._current = 0
        
    def isEmpty(self):
        return not self._content

    def setSize(self, size):
        #如果縮小堆疊空間，則刪除指定大小之後的既有元素
        if size < self._current:
            for i in range(size, self._current)[::-1]:
                del self._content[i]
            self._current = size
        self._size = size
    
    def isFull(self):
        return self._current == self._size
        
    def push(self, v):
        if self._current < self._size:
            self._content.append(v)
            self._current = self._current + 1	#堆疊的元素個數加1
        else:
            print('Stack Full!')
            
    def pop(self):
        if self._content:
            self._current = self._current - 1	#堆疊的元素個數減1
            return self._content.pop()
        else:
            print('Stack is empty!')
            
    def show(self):
        print(self._content)

    def showRemainderSpace(self):
        print('Stack can still PUSH ', self._size-self._current, ' elements.')

if __name__ == '__main__':
    print('Please use me as a module.')

```
# 範例程式:05_01_02.py
```
from string import ascii_letters
from random import choice
from time import time

letters = ''.join([choice(ascii_letters) for i in range(999999)])
def positions_of_character(sentence, ch):	#使用字串物件的find()方法
    result = []
    index = 0
    index = sentence.find(ch, index+1)
    while index != -1:
        result.append(index)
        index = sentence.find(ch, index+1)
    return result

def demo(s, c):		#普通方法，逐個字元比較
    result = []
    for i,ch in enumerate(s):
        if ch == c:
            result.append(i)
    return result

start = time()
positions = positions_of_character(letters, 'a')
print(time()-start)

start = time()
p = demo(letters, 'a')
print(time()-start)

```
# 範例程式:05_01_02_3.py
```
import timeit

#使用列表推導式產生10000個字串
strlist = ['This is a long string that will not keep in memory.' for n in range(10000)]

#使用字串物件的join()方法連接多個字串
def use_join():
    return ''.join(strlist)

#使用運算子+連接多個字串
def use_plus():
    result = ''
    for strtemp in strlist:
        result = result+strtemp
    return result

if __name__ == '__main__':
    #重複執行次數
    times = 1000
    jointimer = timeit.Timer('use_join()', 'from __main__ import use_join')
    print('time for join:', jointimer.timeit(number=times))
    plustimer = timeit.Timer('use_plus()', 'from __main__ import use_plus')
    print('time for plus:', plustimer.timeit(number=times))

```
# 範例程式:05-01.py
```
def crypt(source, key):
    from itertools import cycle
    result = ''
    temp = cycle(key)
    for ch in source:
        result = result + chr(ord(ch) ^ ord(next(temp)))
    return result

source = 'Shandong Institute of Business and Technology'
key = 'Dong Fuguo'

print('Before Encrypted:'+source)
encrypted = crypt(source, key)
print('After Encrypted:'+encrypted)
decrypted = crypt(encrypted, key)
print('After Decrypted:'+decrypted)

```
# 範例程式:05-02.py
```
import random
import string
import codecs

#常用中文字Unicode編碼表（部分），完整清單詳見配套原始程式碼
StringBase = '\u7684\u4e00\u4e86\u662f\u6211\u4e0d\u5728\u4eba'
#轉換為中文
StringBase = ''.join(StringBase.split('\\u')) 
                     
def getEmail():
    #常見功能變數名稱尾碼，可以隨意擴展該列表
    suffix = ['.com', '.org', '.net', '.cn']
    characters = string.ascii_letters+string.digits+'_'
    username = ''.join((random.choice(characters) for i in range(random.randint(6,12))))
    domain = ''.join((random.choice(characters) for i in range(random.randint(3,6))))
    return username+'@'+domain+random.choice(suffix)

def getTelNo():
    return ''.join((str(random.randint(0,9)) for i in range(11)))

def getNameOrAddress(flag):
    '''flag=1表示返回隨機姓名，flag=0表示返回隨機地址'''
    result = ''
    if flag==1:
        #大部分中國人姓名在2-4個中文字
        rangestart, rangeend = 2, 5
    elif flag==0:
        #假設位址在10-30個中文字之間
        rangestart, rangeend = 10, 31
    else:
        print('flag must be 1 or 0')
        return ''
    for i in range(rangestart, rangeend):
        result += random.choice(StringBase)
    return result

def getSex():
    return random.choice(('男', '女'))

def getAge():
    return str(random.randint(18,100))

def main(filename):
    with codecs.open(filename, 'w', 'utf-8') as fp:
        fp.write('Name,Sex,Age,TelNO,Address,Email\n')
        #隨機產生200個人的資訊
        for i in range(200):
            name = getNameOrAddress(1)
            sex = getSex()
            age = getAge()
            tel = getTelNo()
            address = getNameOrAddress(0)
            email = getEmail()
            line = ','.join([name, sex, age, tel, address, email]) + '\n'
            fp.write(line)
            
def output(filename):
    with codecs.open(filename, 'r', 'utf-8') as fp:
        while True:
            line = fp.readline()
            if not line:
                return
            line = line.split(',')
            for i in line:
                print(i, end=',')
            print()
            
if __name__=='__main__':
    filename = 'information.txt'
    main(filename)
    output(filename)

```
# 範例程式:05-03.py
```
import re

telNumber = '''Suppose my Phone No. is 0535-1234567, yours is 010-12345678, his is 025-87654321.'''
pattern = re.compile(r'(\d{3, 4})-(\d{7, 8})')
index = 0
while True:
    matchResult = pattern.search(telNumber, index)		#從指定位置開始比對
    if not matchResult:
        break
    print('-'*30)
    print('Success:')
    for i in range(3):
        print('Searched content:', matchResult.group(i),\
        ' Start from:', matchResult.start(i), 'End at:', matchResult.end(i),\
          ' Its span is:', matchResult.span(i))
    index = matchResult.end(2)			#指定下次比對的開始位置

```
# 範例程式:05-05.py
```
import sys
import re

def checkFormats(lines, desFileName):
    fp = open(desFileName, 'w')
    for i, line in enumerate(lines):
        print('='*30)
        print('Line:', i+1)        
        if line.strip().startswith('#'):
            print(' '*10+'Comments.Pass.')
            fp.write(line)
            continue
        flag = True        
        #check operator symbols
        symbols = [',', '+', '-', '*', '/', '//', '**', '>>', '<<', '+=', '-=', '*=', '/=']
        temp_line = line
        for symbol in symbols:
            pattern = re.compile(r'\s*'+re.escape(symbol)+r'\s*')
            temp_line = pattern.split(temp_line)
            sep = ' '+symbol+' '
            temp_line = sep.join(temp_line)
        if line != temp_line:
            flag = False
            print(' '*10+'You may miss some blank spaces in this line.')        
        #check import statement
        if line.strip().startswith('import'):
            if ',' in line:
                flag = False
                print(' '*10+"You'd better import one module at a time.")
                temp_line = line.strip()
                modules = temp_line[temp_line.index(' ')+1:]
                modules = modules.strip()
                pattern = re.compile(r'\s*,\s*')
                modules = pattern.split(modules)
                temp_line = ''
                for module in modules:
                    temp_line += line[:line.index('import')]+'import '+module+'\n'
                line = temp_line
            pri_line = lines[i-1].strip()
            if pri_line and (not pri_line.startswith('import')) and \
               (not pri_line.startswith('#')):
                flag = False
                print(' '*10+'You should add a blank line before this line.')
                line = '\n'+line
            after_line = lines[i+1].strip()
            if after_line and (not after_line.startswith('import')):
                flag = False
                print(' '*10+'You should add a blank line after this line.')
                line =line+'\n'
        #check if there is a blank line before new funtional code block
        #including the class/function definition
        if line.strip() and not line.startswith(' ') and i > 0:
            pri_line = lines[i-1]
            if pri_line.strip() and pri_line.startswith(' '):
                flag = False
                print(' '*10+"You'd better add a blank line before this line.")
                line = '\n'+line                
        if flag:
            print(' '*10 + 'Pass.')
        fp.write(line)
    fp.close()
            
if __name__ == '__main__':
    fileName = sys.argv[1]	#命令列參數
    fileLines = []
    with open(fileName, 'r') as fp:
        fileLines = fp.readlines()
    desFileName = fileName[:-3]+'_new.py'
    checkFormats(fileLines, desFileName)
    #check the ratio of comment lines to all lines
    comments = [line for line in fileLines if line.strip().startswith('#')]
    ratio = len(comments)/len(fileLines)
    if ratio <= 0.3:
        print('='*30)
        print('Comments in the file is less than 30%.')
        print('Perhaps you should add some comments at appropriate position.')

```
# 範例程式:FindIdentifiersFromPyFile.py
```
import re
import os
import sys

classes = {}
functions = []
variables = {'normal':{}, 'parameter':{}, 'infor':{}}

'''This is a test string:
atest, btest = 3, 5
to verify that variables in comments will be ignored by this algorithm
'''

def _identifyClassNames(index, line):
    '''parameter index is the line number of line,
     parameter line is a line of code of the file to check'''
    pattern = re.compile(r'(?<=class\s)\w+(?=.*?:)')
    matchResult = pattern.search(line)
    if not matchResult:
        return
    className = matchResult.group(0)
    classes[className] = classes.get(className, [])
    classes[className].append(index)

def _identifyFunctionNames(index, line):
    pattern = re.compile(r'(?<=def\s)(\w+)\((.*?)\)(?=:)')
    matchResult = pattern.search(line)
    if not matchResult:
        return
    functionName = matchResult.group(1)
    functions.append((functionName, index))
    parameters = matchResult.group(2).split(r', ')
    if parameters[0] == '':
        return
    for v in parameters:
        variables['parameter'][v] = variables['parameter'].get(v, [])
        variables['parameter'][v].append(index)
    
def _identifyVariableNames(index, line):
    #find normal variables, including the case: a, b = 3, 5
    pattern = re.compile(r'\b(.*?)(?=\s=)')
    matchResult = pattern.search(line)
    if matchResult:
        vs = matchResult.group(1).split(r', ')
        for v in vs:
            #consider the case 'if variable == value'
            if 'if ' in v:
                v = v.split()[1]
            #consider the case: 'a[3] = 3'
            if '[' in v:
                v = v[0:v.index('[')]
            variables['normal'][v] = variables['normal'].get(v, [])
            variables['normal'][v].append(index)
    #find the variables in for statements
    pattern = re.compile(r'(?<=for\s)(.*?)(?=\sin)')
    matchResult = pattern.search(line)
    if matchResult:
        vs = matchResult.group(1).split(r', ')
        for v in vs:
            variables['infor'][v] = variables['infor'].get(v, [])
            variables['infor'][v].append(index)

def output():
    print('='*30)
    print('The class names and their line numbers are:')
    for key, value in classes.items():
        print(key, ':', value)
    print('='*30)
    print('The function names and their line numbers are:')
    for i in functions:
        print(i[0], ':', i[1])
    print('='*30)
    print('The normal variable names and their line numbers are:')
    for key, value in variables['normal'].items():
        print(key, ':', value)
    print('-'*20)
    print('The parameter names and their line numbers in functions are:')
    for key, value in variables['parameter'].items():
        print(key, ':', value)
    print('-'*20)
    print('The variable names and their line numbers in for statements are:')
    for key, value in variables['infor'].items():
        print(key, ':', value)

#suppose the lines of comments less than 50
def comments(index):
    for i in range(50):
        line = allLines[index + i].strip()
        if line.endswith('"""') or line.endswith("'''"):
            return i+1
        
if __name__ == '__main__':
    fileName = sys.argv[1]	#命令列參數
    if not os.path.isfile(fileName):
        print('Your input is not a file.')
        sys.exit(0)			#退出目前程式
    if not fileName.endswith('.py'):
        print('Sorry. I can only check Python source file.')
        sys.exit(0)
    allLines = []
    with open(fileName, 'r') as fp:
        allLines = fp.readlines()        
    index = 0    
    totalLen = len(allLines)
    while index < totalLen:
        line = allLines[index]
        #strip the blank characters at both end of line
        line = line.strip()
        #ignore the comments starting with '#'
        if line.startswith('#'):
            index += 1
            continue
        #ignore the comments between ''' or """
        if line.startswith('"""') or line.startswith("'''"):
            index += comments(index)
            continue
        #identify identifiers
        _identifyClassNames(index+1, line)
        _identifyFunctionNames(index+1, line)
        _identifyVariableNames(index+1, line)
        index += 1
    output()

```
# 範例程式:06_04_01.py
```
import os 

def visitDir(path): 
    if not os.path.isdir(path):
        print('Error:"', path, '" is not a directory or does not exist.')
        return
    for lists in os.listdir(path): 
        sub_path = os.path.join(path, lists) 
        print(sub_path)
        if os.path.isdir(sub_path):  
            visitDir(sub_path)			#遞迴呼叫

```
# 範例程式:06_04_01_2.py
```
import os

def visitDir2(path):
    if not os.path.isdir(path):
        print('Error:"', path, '" is not a directory or does not exist.')
        return
    list_dirs = os.walk(path) 
    for root, dirs, files in list_dirs:		#遍歷該元組的目錄和檔案資訊
        for d in dirs: 
            print(os.path.join(root, d))		#獲取得完整路徑
        for f in files: 
            print(os.path.join(root, f))		#獲取得檔案絕對路徑

```
# 範例程式:06-01.py
```
s = 'Hello world\n文字檔的讀取方法\n文字檔的寫入方法\n'
with open('sample.txt', 'a+') as f:
      f.write(s)

```
# 範例程式:06-02.py
```
fp = open('sample.txt')
print(fp.read(4))	#從目前位置讀取前4個字元
print(fp.read(18))	#英文字和中文字一樣對待
print(fp.read())	#從目前位置讀取後面的所有內容
fp.close()			#關閉檔案物件

```
# 範例程式:06-03.py
```
with open('sample.txt') as fp:
    while True:
        line = fp.readline()
        if not line:
            break
        print(line)

```
# 範例程式:06-05.py
```
with open('data.txt', 'r') as fp:
    data = fp.readlines()				#讀取所有行
data = [line.strip() for line in data]	#刪除每行兩側的空白字元
data = ','.join(data)				#合併所有行
data = data.split(',')				#分割得到所有數字
data = [int(item) for item in data]	#轉換為數字
data.sort()							#昇冪排序
data = ','.join(map(str,data))		#將結果轉換為字串
with open('data_asc.txt', 'w') as fp:	#將結果寫入檔案
    fp.write(data)

```
# 範例程式:06-07.py
```
fp = open('sample.txt')

result = [0, '']
for line in fp:
    t = len(line)
    if t > result[0]:
        result[0] = t
        result[1] = line
fp.close()

print(result)

```
# 範例程式:06-08.py
```
from os.path import isfile as isfile
from time import time as time

Result = {}
AllLines = []
#FileName = r'XueshengKaoQin.pyw'
FileName = input('Please input the file to check, including full path:')

#Read the content of given file
#Remove all the whitespace string  of every line,
#preserving only one space character between words or operators
#note:The last line does not contain the '\n' character
def PreOperate():
    global AllLines
    with open(FileName, 'r', encoding='utf-8') as fp:
        for line in fp:
            line = ' '.join(line.split())
            AllLines.append(line)

#Check if the current position is still the duplicated one
def IfHasDuplicated(Index1):
    for item in Result.values():
        for it in item:
            if Index1 == it[0]:
                return it[1] #return the span
    return False

#If the current line Index2 is in a span of duplicated lines, return True, else False
def IsInSpan(Index2):
    for item in Result.values():
        for i in item:
            if i[0] <= Index2 < i[0] + i[1]:
                return True
    return False

def MainCheck():
    global Result
    TotalLen = len(AllLines)
    Index1 = 0
    while Index1 < TotalLen - 1:
        #speed up
        span = IfHasDuplicated(Index1)
        if span:
            Index1 += span
            continue
        Index2 = Index1 + 1
        while Index2 < TotalLen:
            #speed up, skip the duplicated lines
            if IsInSpan(Index2):
                Index2 +=1
                continue
            src = ''
            des = ''
            for i in range(10):                
                if Index2+i >= TotalLen:
                    break                
                src += AllLines[Index1+i]
                des += AllLines[Index2+i]
                if src == des:
                    t = Result.get(Index1, [])
                    for tt in t:
                        if tt[0] == Index2:
                            tt[1] = i+1
                            break
                    else:
                        t.append([Index2, i+1])
                    Result[Index1] = t
                else:
                    break
            t = Result.get(Index1, [])
            for tt in t:
                if tt[0] == Index2:
                    Index2 += tt[1]
                    break
            else:
                Index2 +=1
                
        #Optimize the Result dictionary, remove the items with span<3
        Result[Index1] = Result.get(Index1, [])
        for n in Result[Index1][::-1]: #Note: here must use the reverse slice,
            if n[1] < 3:               
                Result[Index1].remove(n)
        if not Result[Index1]:
            del Result[Index1]

        #Compute the min span of duplicated codes of line Index1,modify the step Index1
        a = [ttt[1] for ttt in Result.get(Index1, [[Index1, 1]])]
        if a:
            Index1 += max(a)
        else:
            Index1 += 1

#Output the result
def Output():    
    print('-'*20)    
    print('Result:')
    for key, value in Result.items():
        print('The original line is: \n {0}'.format(AllLines[key]))
        print('Its line number is {0}'.format(key+1))
        print('The duplicated line numbers are:')
        for i in value:
            print('    Start:', i[0], '    Span:', i[1])
        print('-'*20)
    print('-'*20)

if isfile(FileName):
    start = time()
    PreOperate()
    MainCheck()
    Output()
    print('Time used:', time() - start)

```
# 範例程式:06-09.py
```
import pickle

n = 7
i = 13000000
a = 99.056
s = '中國人民 123abc'
lst = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
tu = (-5, 10, 8)
coll = {4, 5, 6}
dic = {'a':'apple', 'b':'banana', 'g':'grape', 'o':'orange'}
f = open('sample_pickle.dat', 'wb')	#以寫入模式打開啟二進位檔案
try:
    pickle.dump(n, f)		#對象物件個數
    pickle.dump(i, f)		#寫入整數
    pickle.dump(a, f)		#寫入實數
    pickle.dump(s, f)		#寫入字串
    pickle.dump(lst, f)		#寫入列表
    pickle.dump(tu, f)		#寫入元組
    pickle.dump(coll, f)		#寫入集合
    pickle.dump(dic, f)		#寫入字典
except:
    print('寫文件入檔案異常！')
finally:
    f.close()

```
# 範例程式:06-10.py
```
import pickle

f = open('sample_pickle.dat', 'rb')
n = pickle.load(f)			#讀取檔案的資料個數
for i in range(n):
	x = pickle.load(f)
	print(x)
f.close()

```
# 範例程式:06-11.py
```
import struct

n = 1300000000
x = 96.45
b = True
s = 'a1@中國'
sn = struct.pack('if?', n, x, b)	#序列化，i表示整數，f表示實數，?表示邏輯值
f = open('sample_struct.dat', 'wb')
f.write(sn)
f.write(s.encode())				#字串需要編碼為位元組，再寫入檔案
f.close()

```
# 範例程式:06-12.py
```
import struct

f = open('sample_struct.dat', 'rb')
sn = f.read(9)
tu = struct.unpack('if?', sn)		#以指定的格式反序列化
print(tu)
n, x, b1 = tu
print('n=',n, 'x=',x, 'b1=',b1)
s = f.read(9)
s = s.decode()					#解碼字串
print('s=', s)

```
# 範例程式:06-13.py
```
import os

file_list = os.listdir(".")
for filename in file_list:         
	pos = filename.rindex(".") 
	if filename[pos+1:] == "html":
		newname = filename[:pos+1]+"htm" 
		os.rename(filename, newname)
		print(filename+"更名為："+newname)

```
# 範例程式:06-14.py
```
import sys
import zlib
import os.path

filename = sys.argv[1]
if os.path.isfile(filename):
    fp = open(filename, 'rb')
    contents = fp.read()
    fp.close()
    print(zlib.crc32(contents.encode()))
else:
    print('file not exists')

```
# 範例程式:06-15.py
```
def is_gif(fname):
    f = open(fname, 'r')
    first4 = tuple(f.read(4))
    f.close()
    return first4 == ('G', 'I', 'F', '8')

```
# 範例程式:06-16.py
```
from xlwt import *

book = Workbook()				#建立新的Excel檔案
sheet1 = book.add_sheet("First")	#增加新的worksheet
al = Alignment()
al.horz = Alignment.HORZ_CENTER	#對齊方式
al.vert = Alignment.VERT_CENTER
borders = Borders()
borders.bottom = Borders.THICK	#邊框樣式
style = XFStyle()
style.alignment = al
Style.borders = borders
row0 = sheet1.row(0)			#擷取第0行列
row0.write(0, 'test', style=style)	#寫入儲存格
book.save(r'D:\test.xls')		#儲存檔案

```
# 範例程式:06-18_1.py
```
from ctypes import *
from time import sleep
from datetime import datetime

#方便呼叫Windows底層API函數
user32 = windll.user32
kernel32 = windll.kernel32
psapi = windll.psapi

#即時查看目前視窗
def getProcessInfo():
    global windows
    #取得目前位於桌面最頂端的視窗控制碼
    hwnd = user32.GetForegroundWindow()
    pid = c_ulong(0)
    #取得處理程序ID
    user32.GetWindowThreadProcessId(hwnd, byref(pid))
    processId = str(pid.value)
    #取得可執行檔名稱
    executable = create_string_buffer(512)
    h_process = kernel32.OpenProcess(0x400|0x10, False, pid)
    psapi.GetModuleBaseNameA(h_process, None, byref(executable), 512)
    #取得視窗標題
    windowTitle = create_string_buffer(512)
    user32.GetWindowTextA(hwnd, byref(windowTitle), 512)
    #關閉控制碼
    kernel32.CloseHandle(hwnd)
    kernel32.CloseHandle(h_process)
    #更新最近兩個視窗清單
    windows.pop(0)
    windows.append([executable.value.decode('big5'),windowTitle.value.decode('big5')])

def main():
    global windows
    windows = [None, None]
    while True:
        getProcessInfo()
        #如果使用者切換視窗，則進行提示
        if windows[0] != windows[1]:
            print('='*30)
            print(str(datetime.now())[:19],windows[0],'==>',windows[1])
        sleep(0.2)
if __name__ == '__main__':
    main()

```
# 範例程式:06-18_2.py
```
from ctypes.wintypes import *
from ctypes import *

kernel32 = windll.kernel32

class tagPROCESSENTRY32(Structure):	#定義結構體
    _fields_ = [('dwSize',		DWORD),
             ('cntUsage',			DWORD),
             ('th32ProcessID',		DWORD),
             ('th32DefaultHeapID',	POINTER(ULONG)),
             ('th32ModuleID',		DWORD),
             ('cntThreads',			DWORD),
             ('th32ParentProcessID',	DWORD),
             ('pcPriClassBase',		LONG),
             ('dwFlags',			DWORD),
             ('szExeFile',			c_char * 260)]
def killProcess(processNames):
    #建立處理程序快照
    hSnapshot = kernel32.CreateToolhelp32Snapshot(15, 0)
    fProcessEntry32 = tagPROCESSENTRY32()
    if hSnapshot:
        fProcessEntry32.dwSize = sizeof(fProcessEntry32)
        hasmore = kernel32.Process32First(hSnapshot, byref(fProcessEntry32))
        #列舉處理程序
        while hasmore:
            #可執行檔
            processName = (fProcessEntry32.szExeFile)
            #處理程序ID
            processID = fProcessEntry32.th32ProcessID
            if processName.decode().lower() in processNames:
                #取得處理程序控制碼
                hProcess = kernel32.OpenProcess(1, False, processID)
                #結束處理程序
                kernel32.TerminateProcess(hProcess,0)
            #取得下一個處理程序
            hasmore = kernel32.Process32Next(hSnapshot, byref(fProcessEntry32))

#待刪除的處理程序清單
processNames = ('notepad.exe', 'mspaint.exe')
killProcess(processNames)

```
# 範例程式:06-18_3.py
```
from ctypes import *
from ctypes import wintypes
from time import sleep

#呼叫Windows系統動態連結程式庫user32.dll
user32 = windll.user32
p = wintypes.POINT()
buffer = create_string_buffer(255)

while True:
    sleep(0.5)
    #取得滑鼠位置
    user32.GetCursorPos(byref(p))
    #取得滑鼠所在位置的視窗控制碼
    HWnd = user32.WindowFromPoint(p)
    #註解的程式碼本來可以實現星號密碼查看，在Win7以後的系統失效了
    #dwStyle = user32.GetWindowLongA(HWnd, -16) #-16是GWL_STYLE訊息的值
    #user32.SetWindowWord(HWnd, -16, 0)
    sleep(0.2)
    #取得視窗文字
    user32.SendMessageA(HWnd, 13, 255, byref(buffer)) #13是WM_GETTEXT訊息的值
    #user32.SetWindowLongA(HWnd, -16, dwStyle)
    print(buffer.value.decode('big5'))

```
# 範例程式:06-20.py
```
import os
import filecmp
import shutil
import sys

def autoBackup(scrDir, dstDir):
    if ((not os.path.isdir(scrDir)) or (not os.path.isdir(dstDir)) or
      (os.path.abspath(scrDir)!=scrDir) or (os.path.abspath(dstDir)!=dstDir)):
        usage()
    for item in os.listdir(scrDir):
        scrItem = os.path.join(scrDir, item)
        dstItem = scrItem.replace(scrDir,dstDir)
        if os.path.isdir(scrItem):
            #建立新增的資料夾，保證目的資料夾的結構與原始資料夾一致
            if not os.path.exists(dstItem):
                os.makedirs(dstItem)
                print('make directory'+dstItem)
            #遞迴呼叫本身函數
            autoBackup(scrItem, dstItem)
        elif os.path.isfile(scrItem):
            #只複製新增或修改過的檔案
            if ((not os.path.exists(dstItem)) or
               (not filecmp.cmp(scrItem, dstItem, shallow=False))):
                shutil.copyfile(scrItem, dstItem)
                print('file:'+scrItem+'==>'+dstItem)

def usage():
    print('scrDir and dstDir must be existing absolute path of certain directory')
    print('For example:{0} c:\\olddir c:\\newdir'.format(sys.argv[0]))
    sys.exit(0)
    
if __name__=='__main__':
    if len(sys.argv)!=3:
        usage()
    scrDir, dstDir= sys.argv[1], sys.argv[2]
    autoBackup(scrDir, dstDir)

```
# 範例程式:06-21.py
```
import os

totalSize = 0
fileNum = 0
dirNum = 0

def visitDir(path):
    global totalSize
    global fileNum
    global dirNum
    for lists in os.listdir(path):
        sub_path = os.path.join(path, lists)
        if os.path.isfile(sub_path):
            fileNum = fileNum+1				#統計檔案數量
            totalSize = totalSize+os.path.getsize(sub_path)	#統計檔案總大小
        elif os.path.isdir(sub_path):
            dirNum = dirNum+1				#統計資料夾數量
            visitDir(sub_path)				#遞迴遍歷子資料夾

def main(path):
    if not os.path.isdir(path):
        print('Error:"', path, '" is not a directory or does not exist.')
        return
    visitDir(path)

def sizeConvert(size):					#單位換算
    K, M, G = 1024, 1024**2, 1024**3
    if size >= G:
        return str(size/G)+'G Bytes'
    elif size >= M:
        return str(size/M)+'M Bytes'
    elif size >= K:
        return str(size/K)+'K Bytes'
    else:
        return str(size)+'Bytes'

def output(path):
    print('The total size of '+path+' is:'+sizeConvert(totalSize)+'('+str(totalSize)+' Bytes)')
    print('The total number of files in '+path+' is:',fileNum)
    print('The total number of directories in '+path+' is:',dirNum)

if __name__=='__main__':
    path = r'd:\temp'
    main(path)
    output(path)

```
# 範例程式:06-22.py
```
from os.path import isdir, join
from os import listdir

AllLines = []				#儲存所有程式碼
NotRepeatedLines = []		#保存不重複的程式碼行列
file_num = 0				#檔案數量
code_num = 0 				#程式碼總列數

def LinesCount(directory):
    global AllLines
    global NotRepeatedLines
    global file_num
    global code_num

    for filename in listdir(directory):
        temp = join(directory, filename)
        if isdir(temp):			#遞迴遍歷子資料夾
            LinesCount(temp)
        elif temp.endswith('.cpp'):	#只考慮.cpp 檔案
            file_num += 1
            with open(temp, 'r') as fp:
                while True:
                    line = fp.readline()
                    if not line:
                        break
                    if line not in NotRepeatedLines:
                        NotRepeatedLines.append(line)	#記錄非重複列
                    code_num += 1		#記錄所有程式碼列
path = r'D:\Code\Cpp'
print('總列數：{0}，不重複列數：{1}'.format(code_num, len(NotRepeatedLines)))
print('檔案數量：{0}'.format(file_num))

```
# 範例程式:06-23.py
```
from os.path import isdir, join, splitext
from os import remove, listdir
import sys

filetypes = ['.tmp', '.log', '.obj', '.txt']	#指定要刪除的檔案類型

def delCertainFiles(directory):
    if not isdir(directory):
        return
    for filename in listdir(directory):
        temp = join(directory, filename)
        if isdir(temp):
            delCertainFiles(temp)			#遞迴呼叫
        elif splitext(temp)[1] in filetypes:	#檢查檔案類型
            remove(temp)					#刪除檔案
            print(temp, ' deleted....')

def main():
    directory = r'c:\windows\temp'
    #directory = sys.argv[1]
    delCertainFiles(directory)

main()

```
# 範例程式:06-24.py
```
import openpyxl
from openpyxl import Workbook
fn = r'f:\test.xlsx'				#檔案名稱
wb = Workbook()						#建立工作簿
ws = wb.create_sheet(title='你好，世界')	#建立工作表
ws['A1'] = '這是第一個儲存格' 		#設定儲存格
ws['B1'] = 3.1415926
wb.save(fn)							#儲存Excel檔案
wb = openpyxl.load_workbook(fn)		#開啟既有的Excel檔案
ws = wb.worksheets[1]				#開啟指定索引的工作表
print(ws['A1'].value) 				#讀取並輸出指定儲存格的值
ws.append([1,2,3,4,5])				#加入一列資料
ws.merge_cells('F2:F3')				#合併儲存格
ws['F2'] = "=sum(A2:E2)" 			#寫入公式
for r in range(10,15):
    for c in range(3,8):
        _ = ws.cell(row=r, column=c, value=r*c)	#寫入儲存格資料
wb.save(fn)

```
# 範例程式:06-24_2.py
```
import openpyxl
from openpyxl import Workbook
import random

#產生隨機資料
def generateRandomInformation(filename):
    workbook = Workbook()
    worksheet = workbook.worksheets[0]
    worksheet.append(['姓名','課程','成績'])
    #中文名字中的第一、第二、第三個字
    first = tuple('趙錢孫李')
    middle = tuple('偉昀琛東')
    last = tuple('坤豔志')
    #課程名稱
    subjects = ('語文','數學','英語')
    #隨機產生200筆資料
    for i in range(200):
        line = []
        r = random.randint(1,100)
        name = random.choice(first)
        #按一定機率產生只有兩個字的中文名字
        if r>50:
            name = name + random.choice(middle)
        name = name + random.choice(last)
        #依序產生姓名、課程名稱和成績
        line.append(name)
        line.append(random.choice(subjects))
        line.append(random.randint(0,100))
        worksheet.append(line)
    #保存資料，產生Excel 2007格式的檔案
    workbook.save(filename)

def getResult(oldfile, newfile):
    #用來存放結果資料的字典
    result = dict()
    #開啟原始資料
    workbook = openpyxl.load_workbook(oldfile)
    worksheet = workbook.worksheets[0]
    #遍歷原始資料
    #跳過第0列的表頭
    for row in worksheet.rows[1:]:
        #姓名，課程名稱，本次成績
        name, subject, grade= row[0].value, row[1].value, row[2].value
        #取得目前姓名對應的課程名稱和成績
        #如果result字典沒有此姓名，則返回空字典
        t = result.get(name, {})
        #取得學生目前課程的成績，若不存在，返回0
        f = t.get(subject, 0)
        #只保留學生該課程的最高成績
        if grade > f:
            t[subject] = grade
            result[name] = t
    #建立Excel檔案
    workbook1 = Workbook()
    worksheet1 = workbook1.worksheets[0]
    worksheet1.append(['姓名','課程','成績'])
    #將result字典的結果寫入Excel檔案
    for name, t in result.items():
        for subject, grade in t.items():
            worksheet1.append([name, subject, grade])
    workbook1.save(newfile)

if __name__ == '__main__':
    oldfile = r'D:\test.xlsx'
    newfile = r'D:\result.xlsx'
    generateRandomInformation(oldfile)
    getResult(oldfile, newfile)

```
# 範例程式:06-26.py
```
import random
import os
import tkinter
import tkinter.ttk
from docx import Document

columnsNumber = 4

def main(rowsNumber=20, grade=4):
    if grade < 3:
        operators = '＋－'
        biggest = 20
    elif grade <= 4:
        operators = '＋－×÷'
        biggest = 100
    elif grade == 5:
        operators = '＋－×÷('
        biggest = 100
        
    document = Document()
    #建立表格
    table = document.add_table(rows=rowsNumber, cols=columnsNumber)
    #遍歷每個儲存格
    for row in range(rowsNumber):
        for col in range(columnsNumber):
            first = random.randint(1, biggest)
            second = random.randint(1, biggest)
            operator = random.choice(operators)            
            if operator != '(':
                if operator == '－':
                    #如果是減法心算題，確保結果為正數
                    if first < second:
                        first, second = second, first
                r = str(first).ljust(2, ' ') + ' ' + operator  + str(second).ljust(2, ' ') + '='
            else:
                #產生帶括弧的心算題，需要3個數字和2個運算子
                third = random.randint(1, 100)
                while True:
                    o1 = random.choice(operators)
                    o2 = random.choice(operators)
                    if o1 != '(' and o2 != '(':
                        break
                rr = random.randint(1, 100)
                if rr > 50:
                    if o2 == '－':
                        if second < third:
                            second, third = third, second
                    r = str(first).ljust(2, ' ') + o1 + ' ( ' \
                       + str(second).ljust(2, ' ') + o2 + str(third).ljust(2, ' ') + ')='
                else:
                    if o1 == '－':
                        if first < second:
                            first, second = second, first
                    r = '(' + str(first).ljust(2, ' ') + o1 \
                       + str(second).ljust(2, ' ') + ')' \
                       + o2 + str(third).ljust(2, ' ') + '='
            #取得指定儲存格，並寫入心算題
            cell = table.cell(row, col)
            cell.text = r
    document.save('kousuan.docx') 
    os.startfile('kousuan.docx')

if __name__ == '__main__':
    #tkinter程式設計細節，請參考第15章
    app = tkinter.Tk()
    app.title('KouSuan------by Dong Fuguo')
    app['width'] = 300
    app['height'] = 150
    labelNumber = tkinter.Label(app, text='Number:', justify=tkinter.RIGHT, width=50)
    labelNumber.place(x=10, y=40, width=50,height=20)
    comboNumber = tkinter.ttk.Combobox(app, values=(100,200,300,400,500), width=50)
    comboNumber.place(x=70, y=40, width=50, height=20)
    
    labelGrade = tkinter.Label(app, text='Grade:', justify=tkinter.RIGHT, width=50)
    labelGrade.place(x=130, y=40, width=50,height=20)
    comboGrade = tkinter.ttk.Combobox(app, values=(1,2,3,4,5), width=50)
    comboGrade.place(x=200, y=40, width=50, height=20)

    def generate():
        number = int(comboNumber.get())
        grade = int(comboGrade.get())
        main(number, grade)
    buttonGenerate = tkinter.Button(app, text='GO', width=40, command=generate)
    buttonGenerate.place(x=130, y=90, width=40, height=30)

    app.mainloop()

```
# 範例程式:06-27.py
```
#docx檔案題庫包含很多段，每段一個題目，格式為：問題。（答案）
#資料庫datase.db中，tiku資料表包含kechengmingcheng,zhangjie,timu,daan四個欄位
#資料庫有關知識，請查看本書第8章
import sqlite3
from docx import Document

doc = Document('《Python程式設計》題庫.docx')

#連接資料庫
conn = sqlite3.connect('database.db')
cur = conn.cursor()

#先清空原來的題庫，可省略
cur.execute('delete from tiku')
conn.commit()

for p in doc.paragraphs:
    text = p.text
    if '（' in text and '）' in text:
        index = text.index('（')
        #分離問題和答案
        question = text[:index]
        if '___' in question:
            question = '填空題：' + question
        else:
            question = '判斷題：' + question
        answer = text[index+1:-1]
        #將資料寫入資料庫
        sql = 'insert into tiku(kechengmingcheng,zhangjie,timu,daan) values("Python程式設計","未分類","'+question+'","'+answer+'")'
        cur.execute(sql)
conn.commit()
#關閉資料庫連接
conn.close()

```
# 範例程式:06-28.py
```
from docx import Document
import re

result = {'li':[], 'fig':[], 'tab':[]}
doc = Document(r'd:\doc\05.docx')

for p in doc.paragraphs:			#遍歷檔案所有段落
    t = p.text					#獲得每一段的文字
    if re.match('例\d+-\d+ ', t):	#例題
        result['li'].append(t)
    elif re.match('圖\d+-\d+ ', t):	#插圖
        result['fig'].append(t)
    elif re.match('表\d+-\d+ ', t):	#表格
        result['tab'].append(t)

for key in result.keys():		#輸出結果
    print('='*30)
    for value in result[key]:
        print(value)

```
# 範例程式:06-29.py
```
from zipfile import ZipFile
from os import listdir
from os.path import isfile, isdir, join

def addFileIntoZipfile(srcDir, fp):
    for subpath in listdir(srcDir):
        subpath = join(srcDir, subpath)
        if isfile(subpath):
            fp.write(subpath)				#寫入檔案
        elif isdir(subpath):
            fp.write(subpath)				#寫入資料夾
            addFileIntoZipfile(subpath, fp)	#遞迴呼叫

def zipCompress(srcDir, desZipfile):
    fp = ZipFile(desZipfile, mode='a')		#以附加模式開啟或建立zip檔
    addFileIntoZipfile(srcDir, fp)
    fp.close()

paths = [r'd:\temp\python']
for path in paths:
    zipCompress(path, 'test.zip')

```
# 範例程式:06-30.py
```
import os
import sys
import zipfile				#zipfile是標準庫
try:
    from unrar import rarfile	#嘗試匯入擴展庫，如果沒有就臨時安裝
except:
    path = '"'+os.path.dirname(sys.executable)+'\\scripts\\pip" install --upgrade pip'
    os.system(path)
    path = '"'+os.path.dirname(sys.executable)+'\\scripts\\pip" install unrar'
    os.system(path)
    from unrar import rarfile    

def decryptRarZipFile(filename):
    if filename.endswith('.zip'):
        fp = zipfile.ZipFile(filename)
    elif filename.endswith('.rar'):
        fp = rarfile.RarFile(filename)
    desPath = filename[:-4]	#解壓縮的目的資料夾
    if not os.path.exists(desPath):
        os.mkdir(desPath)
    try:					#嘗試不用密碼解壓縮
        fp.extractall(desPath)
        fp.close()
        print('No password')
        return
    except: 				#使用密碼字典進行暴力破解
        try:
            fpPwd = open('pwddict.txt')
        except:
            print('No dict file pwddict.txt in current directory.')
            return
        for pwd in fpPwd:
            pwd = pwd.rstrip()
            try:
                if filename.endswith('.zip'):
                    for file in fp.namelist():	#重新編碼再解碼，避免中文亂碼
                        fp.extract(file, path=desPath, pwd=pwd.encode())
                        os.rename(desPath+'\\'+file,
                                  desPath+'\\'+file.encode('cp437').decode('gbk'))
                    print('Success! ====>'+pwd)
                    fp.close()
                    break
                elif filename.endswith('.rar'):
                    fp.extractall(path=desPath, pwd=pwd)
                    print('Success! ====>'+pwd)
                    fp.close()
                    break
            except:
                pass
        fpPwd.close()

if __name__ == '__main__':
    filename = sys.argv[1]
    if os.path.isfile(filename) and filename.endswith(('.zip', '.rar')):
        decryptRarZipFile(filename)
    else:
        print('Must be Rar or Zip file')

```
# 範例程式:06-31.py
```
import os
import sys
from tkinter import Tk, Button
from tkinter import filedialog
from tkinter import simpledialog

try:
    import openpyxl
except:
    #先把pip升級到最新版本
    path = '"'+os.path.dirname(sys.executable)+'\\scripts\\pip" install --upgrade pip'
    os.system(path)
    #安裝openpyxl擴展庫
    path = '"'+os.path.dirname(sys.executable)+'\\scripts\\pip" install openpyxl'
    os.system(path)
    import openpyxl

def merge(start):
    #顯示開啟檔案對話方塊，開啟待合併的Excel 2007+檔
    opts= {'filetypes':[('Excel 2007', '.xlsx')]}
    filename = filedialog.askopenfilename(**opts)
    #如果未選擇檔案，便不執行後面的程式碼
    if not filename:
        return
    #分割路徑和檔案名稱
    filepath, tempfilename = os.path.split(filename)
    shotname = os.path.splitext(tempfilename)[0]
    #產生新的檔案名稱
    newFile = filepath + '\\' + shotname + '_merge.xlsx'
    #建立新的Excel 2007+檔案
    workbook = openpyxl.Workbook()
    #增加新的worksheet
    worksheet = workbook.worksheets[0]
    data = openpyxl.load_workbook(filename)
    for sheetnum, sheet in enumerate(data.worksheets):
        #根據設定的表頭列數，設定讀取的起始列
        #第一個sheet讀取表頭，後面的sheet忽略表頭
        if sheetnum == 0:
            rowStart = 0
        else:
            rowStart = start
        #遍歷原sheet，根據情況忽略表頭
        for row in sheet.rows[rowStart:]:
            line = [col.value for col in row]
            worksheet.append(line)
    #儲存新檔
    workbook.save(newFile)
    #開啟剛剛建立的新文件
    os.startfile(newFile)
    
#按一下按鈕後執行的函數，參數a表示Excel檔案每個worksheet預期的表頭列數
def callback():
    kw = {'initialvalue':1, 'minvalue':0, 'maxvalue':10}
    headerNum = simpledialog.askinteger('表頭列數', '請輸入表頭列數',**kw)
    if headerNum != None:
        merge(headerNum)
    
root = Tk()
root.title("合併sheet")
Button(root, text="合併WorkSheets", bg='blue', bd=2,width=28,command=callback).pack()

root.mainloop()

```
# 範例程式:06-32.py
```
from openpyxl import Workbook

def main(txtFileName):
    new_XlsxFileName = txtFileName[:-3] + 'xlsx'
    wb = Workbook()
    ws = wb.worksheets[0]
    with open(txtFileName) as fp:
        for line in fp:
            line = line.strip().split(',')
            ws.append(line)
    wb.save(new_XlsxFileName)

main('test.txt')

```
# 範例程式:demo.py
```
filename = 'demo.py'
with open(filename, 'r') as fp:
    lines = fp.readlines()
maxLength = max(map(len,lines))
for index, line in enumerate(lines):
    newLine = line.rstrip()
    newLine = newLine + ' '*(maxLength+5-len(newLine))
    newLine = newLine + '#' + str(index+1) + '\n'
    lines[index] = newLine
with open(filename[:-3]+'_new.py', 'w') as fp:
    fp.writelines(lines)

```
# 範例程式:demo_new.py
```
filename = 'demo.py'                                        #1
with open(filename, 'r') as fp:                             #2
    lines = fp.readlines()                                  #3
maxLength = max(map(len,lines))                             #4
for index, line in enumerate(lines):                        #5
    newLine = line.rstrip()                                 #6
    newLine = newLine + ' '*(maxLength+5-len(newLine))      #7
    newLine = newLine + '#' + str(index+1) + '\n'           #8
    lines[index] = newLine                                  #9
with open(filename[:-3]+'_new.py', 'w') as fp:              #10
    fp.writelines(lines)                                    #11

```
# 範例程式:07_01_03.py
```
import os
import stat

def remove_readonly(func, path):		#定義回呼函數
    os.chmod(path, stat.S_IWRITE)		#刪除檔案的唯讀屬性
    func(path)						#再次呼叫剛剛失敗的函數

def del_dir(path, onerror=None):
    for file in os.listdir(path):
        file_or_dir = os.path.join(path,file)
        if os.path.isdir(file_or_dir) and not os.path.islink(file_or_dir):
            del_dir(file_or_dir)		#遞迴刪除子資料夾及其檔案
        else:
            try:
                os.remove(file_or_dir)	#嘗試刪除該檔，
            except:					#刪除失敗
                if onerror and callable(onerror):
                    onerror(os.remove, file_or_dir)	#自動呼叫回呼函數
                else:
                    print('You have an exception but did not capture it.')
    os.rmdir(path)					#刪除資料夾

del_dir("E:\\old", remove_readonly)	#呼叫函數，並指定回呼函數

```
# 範例程式:07-02.py
```
#要測試的模組，在本書第4章
import Stack
#Python單元測試標準庫
import unittest

class TestStack(unittest.TestCase):
    def setUp(self):
        #測試之前以附加模式開啟指定檔案
        self.fp = open('D:\\test_Stack_result.txt', 'a+')

    def tearDown(self):
        #測試結束後關閉檔案
        self.fp.close()
        
    def test_isEmpty(self):
        try:
            s = Stack.Stack()
            #確保函數返回結果為True
            self.assertTrue(s.isEmpty())
            self.fp.write('isEmpty passed\n')
        except Exception as e:
            self.fp.write('isEmpty failed\n')

    def test_empty(self):
        try:
            s = Stack.Stack(5)
            for i in ['a', 'b', 'c']:
                s.push(i)
            #測試清空堆疊操作是否正常
            s.empty()
            self.assertTrue(s.isEmpty())
            self.fp.write('empty passed\n')
        except Exception as e:
            self.fp.write('empty failed\n')

    def test_isFull(self):
        try:
            s = Stack.Stack(3)
            s.push(1)
            s.push(2)
            s.push(3)
            self.assertTrue(s.isFull())
            self.fp.write('isFull passed\n')
        except Exception as e:
            self.fp.write('isFull failed\n')

    def test_pushpop(self):
        try:
            s = Stack.Stack()
            s.push(3)
            #確保推入堆疊後立刻彈出，以得到原來的元素
            self.assertEqual(s.pop(), 3)
            s.push('a')
            self.assertEqual(s.pop(), 'a')
            self.fp.write('push and pop passed\n')
        except Exception as e:
            self.fp.write('push or pop failed\n')

    def test_setSize(self):
        try:
            s = Stack.Stack(8)
            for i in range(8):
                s.push(i)
            self.assertTrue(s.isFull())
            #測試擴大堆疊空間是否正常
            s.setSize(9)
            s.push(8)
            self.assertTrue(s.isFull())
            self.assertEqual(s.pop(), 8)
            #測試縮小堆疊空間是否正常
            s.setSize(4)
            self.assertTrue(s.isFull())
            self.assertEqual(s.pop(), 3)
            self.fp.write('setSize passed\n')
        except Exception as e:
            self.fp.write('setSize failed\n')
        
if __name__ == '__main__':
    unittest.main()

```
# 範例程式:07-02_2.py
```
import sys
import logging

old = sys.stderr					#記下原輸出目的地
fp = open('log_test.txt', 'a')		#建立日誌檔
sys.stderr = fp					#把訊息輸出到指定檔案

logging.debug('Debugging information')	#輸出訊息到檔案
logging.info('Informational message')	#一般會忽略debug()和info()的訊息
logging.warning('Warning:config file %s not found', 'server.conf')
logging.error('Error occurred')
logging.critical('Critical error -- shutting down')

sys.stderr = old					#恢復成標準輸出
fp.close()						#關閉檔案

```
# 範例程式:07-02_3.py
```
from time import time

class Timer(object):
    def __enter__(self):
        self.start = time()
        return self

    def __exit__(self, *args):
        self.end = time()
        self.seconds = self.end-self.start

def isPrime(n):
    if n == 2:
        return True
    for i in range(2, int(n**0.5)+2):
        if n%i == 0:
            return False
    return True

with Timer() as t:
    for i in range(1000):
        isPrime(99999999999999999999999)
print(t.seconds)

```
# 範例程式:07-02_4.py
```
from memory_profiler import profile

@profile				#修飾器
def isPrime(n):
    if n == 2:
        return True
    for i in range(2, int(n**0.5)+2):
        if n%i == 0:
            return False
    return True

isPrime(99999999999999999999999)

```
# 範例程式:07-03.py
```
import string
from random import choice

characters = string.ascii_letters + string.digits
selected = [choice(characters) for i in range(1000)]
ch = choice(selected)
print(ch, ':', selected.count(ch))

```
# 範例程式:doctest_demo.py
```
def add(value1, value2):
    #下面一對三引號之間是測試程式碼，doctest會搜索這些程式碼並執行
    #然後根據執行結果與預期結果的符合程度，測試程式碼是否正確
    '''return the addition of two numbers or the concatenation of two string/list/tuple
    >>> add(3, 5)
    8
    >>> add(3.0, 5.0)
    8.0
    >>> add([1,2], [3, 4])
    [1, 2, 3, 4]
    >>> add((1,), (2, 3, 4))
    (1, 2, 3, 4)
    >>> add(1, [3])
    Traceback (most recent call last):
        ...
    TypeError: value1 and value2 must be of the same type
    >>> add(1, '2')
    Traceback (most recent call last):
        ...
    TypeError: value1 and value2 must be of the same type
    >>> add([1], (2,))
    Traceback (most recent call last):
        ...
    TypeError: value1 and value2 must be of the same type
    >>> add('1234', [1,2,3,4])
    Traceback (most recent call last):
        ...
    TypeError: value1 and value2 must be of the same type
    >>> add({1,2,3}, {3,4,5})
    {1, 2, 3, 4, 5}
    >>> add({1:1}, {2:2})
    Traceback (most recent call last):
        ...
    TypeError: value1 and value2 must be the type of int,float,str,list,tuple or set
    '''
    #下面是正式的功能
    if type(value1) not in (int, float, str, list, tuple, set):
        raise TypeError('value1 and value2 must be the type of int,float,str,list,tuple or set')
    if type(value1) != type(value2):
        raise TypeError('value1 and value2 must be of the same type')
    if type(value1) == set:
        return value1 | value2
    else:
        return value1 + value2

if __name__ == "__main__":    
    import doctest
    doctest.testmod()
    print(add(3,5))

```
# 範例程式:IsPrime.py
```
import pdb

n=37
pdb.set_trace()
for i in range(2, n):
    if n%i==0:
        print('No')
        break
else:
    print('Yes')

```
# 範例程式:IsPrime2.py
```
#import pdb

n=37
#pdb.set_trace()
for i in range(2, n):
    if n%i==0:
        print('No')
        break
else:
    print('Yes')

```
# 範例程式:Stack.py
```
class Stack:
    def __init__(self, size = 10):
        self._content = []		#使用列表存放堆疊元素
        self._size = size		#初始堆疊大小
        self._current = 0		#堆疊元素的個數初始化為0

    #解構函數
    def __del__(self):
        del self._content

    def empty(self):
        self._content = []
        self._current = 0
        
    def isEmpty(self):
        return not self._content

    def setSize(self, size):
        #如果縮小堆疊空間，則刪除指定大小之後的既有元素
        if size < self._current:
            for i in range(size, self._current)[::-1]:
                del self._content[i]
            self._current = size
        self._size = size
    
    def isFull(self):
        return self._current == self._size
        
    def push(self, v):
        if self._current < self._size:
            self._content.append(v)
            self._current = self._current + 1	#堆疊的元素個數加1
        else:
            print('Stack Full!')
            
    def pop(self):
        if self._content:
            self._current = self._current - 1	#堆疊的元素個數減1
            return self._content.pop()
        else:
            print('Stack is empty!')
            
    def show(self):
        print(self._content)

    def showRemainderSpace(self):
        print('Stack can still PUSH ', self._size-self._current, ' elements.')

if __name__ == '__main__':
    print('Please use me as a module.')

```
# 範例程式:08_01_02.py
```
import sqlite3

conn = sqlite3.connect(":memory:")
cur = conn.cursor()
cur.execute("create table people (name_last, age)")
who = "Dong"
age = 38
# 以問號作為預留位置
cur.execute("insert into people values (?, ?)", (who, age))
# 以命名變數作為預留位置
cur.execute("select * from people where name_last=:who and age=:age", 
          {"who": who, "age": age})
print(cur.fetchone())

```
# 範例程式:08_01_02_2.py
```
import sqlite3

#自訂反覆運算器，按順序產生小寫字母
class IterChars:
    def __init__(self):
        self.count = ord('a')
    def __iter__(self):
        return self
    def __next__(self):
        if self.count > ord('z'):
            raise StopIteration
        self.count += 1
        return (chr(self.count - 1),)

conn = sqlite3.connect(":memory:")
cur = conn.cursor()
cur.execute("create table characters(c)")
#建立反覆運算器物件
theIter = IterChars()
#插入記錄，每次插入一個英文小寫字母
cur.executemany("insert into characters(c) values (?)", theIter)
#讀取並顯示所有記錄
cur.execute("select c from characters")
print(cur.fetchall())

```
# 範例程式:08_01_02_3.py
```
import sqlite3
import string

#包含yield語句的函數，可用來建立產生器物件
def char_generator():
    for c in string.ascii_lowercase:
        yield (c,)

conn = sqlite3.connect(":memory:")
cur = conn.cursor()
cur.execute("create table characters(c)")
#使用產生器物件得到參數列表
cur.executemany("insert into characters(c) values (?)", char_generator())
cur.execute("select c from characters")
print(cur.fetchall())

```
# 範例程式:08_01_02_4.py
```
import sqlite3

persons = [
    ("Hugo", "Boss"),
    ("Calvin", "Klein")
    ]
conn = sqlite3.connect(":memory:")
# 建立資料表
conn.execute("create table person(firstname, lastname)")
# 插入資料
conn.executemany("insert into person(firstname, lastname) values (?, ?)", persons)
# 顯示資料
for row in conn.execute("select firstname, lastname from person"):
    print(row)
print("I just deleted", conn.execute("delete from person").rowcount, "rows")

```
# 範例程式:08_01_02_5.py
```
import sqlite3

conn = sqlite3.connect("D:/addressBook.db")
cur = conn.cursor()		#建立指標
#cur.execute("create table addressList (name , sex , phon , QQ , address)")

cur.execute('''insert into addressList(name , sex , phon , QQ , address) values('王小丫' ,  '女' ,  '13888997011' ,  '66735' ,  '北京市' )''')
cur.execute('''insert into addressList(name, sex, phon, QQ, address) values('李莉', '女', '15808066055', '675797', '天津市')''')
cur.execute('''insert into addressList(name, sex, phon, QQ, address) values('李星草', '男', '15912108090', '3232099', '昆明市')''')
conn.commit()			#提交交易，把資料寫入資料庫
conn.close()

```
# 範例程式:08_01_02_6.py
```
import sqlite3

conn=sqlite3.connect('D:/addressBook.db')
cur=conn.cursor()
cur.execute('select * from addressList')
li = cur.fetchall()			#返回所有查詢結果
for line in li:
    for item in line:
        print(item, end=' ')
    print()
conn.close()

```
# 範例程式:08_01_03.py
```
import sqlite3

conn = sqlite3.connect("D:\\test.db")
c = conn.cursor()
#c.execute('''create table stocks(date text, trans text, symbol text, qty real, price real)''')
c.execute("""insert into stocks values ('2006-01-05','BUY','RHAT',100,35.14)""")
conn.commit()

conn.row_factory = sqlite3.Row
c = conn.cursor()
c.execute('select * from stocks')
r = c.fetchone()
print(type(r))
print(tuple(r))
print(r[2])
print(r.keys())
print(r['qty'])
for field in r:
    print(field)

c.close()

```
# 範例程式:09-03.py
```
import socket
import threading
import time

activeDegree = dict()
flag = 1
def main():
    global activeDegree
    global flag
    #取得本機IP地址
    HOST = socket.gethostbyname(socket.gethostname())
    #建立原始通訊端，適用Windows平台
    #對於其他作業系統，要把socket.IPPROTO_IP替換為socket.IPPROTO_ICMP
    s = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_IP)
    s.bind((HOST, 0))
    #設定在捕獲的封包中含有IP標頭
    s.setsockopt(socket.IPPROTO_IP, socket.IP_HDRINCL, 1)
    #啟用混合模式，捕捉所有封包
    s.ioctl(socket.SIO_RCVALL, socket.RCVALL_ON)
    #開始捕捉封包
    while flag:
        c = s.recvfrom(65565)
        host = c[1][0]
        activeDegree[host] = activeDegree.get(host, 0)+1
        #假設本機IP位址為10.2.1.8
        if c[1][0]!='10.93.2.31': 
            print(c)
    #關閉混合模式
    s.ioctl(socket.SIO_RCVALL, socket.RCVALL_OFF)
    s.close()
t = threading.Thread(target=main)
t.start()
time.sleep(60)
flag = 0
t.join()
for item in activeDegree.items():
    print(item)

```
# 範例程式:09-04.py
```
import socket
import multiprocessing

def ports(ports_service):
    #取得常用連接埠對應的服務名稱
    for port in list(range(1,100))+[143, 145, 113, 443, 445, 3389, 8080, 521, 5000]:
        try:
            ports_service[port] = socket.getservbyport(port)
        except socket.error:
            pass

def ports_scan(host, ports_service):
    ports_open = []
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        #逾時時間的不同，將影響掃描結果的精確度
        sock.settimeout(0.01)
    except socket.error:
        print('socket creation error')
        sys.exit()
    for port in ports_service:
        try:
            #嘗試連結指定連接埠
            sock.connect((host,port))
            #記錄開啟的連接埠
            ports_open.append(port)
            sock.close()
        except socket.error:
            pass
    return ports_open

if __name__=='__main__':
    m = multiprocessing.Manager()
    ports_service = dict()
    results = dict()
    ports(ports_service)
    #建立處理程序池，允許最多8個處理程序同時執行
    pool = multiprocessing.Pool(processes=8)
    net = '10.9.1.'
    for host_number in map(str, range(80,83)):
        host = net+host_number
        #建立一個新的處理程序，同時記錄執行結果
        results[host] = pool.apply_async(ports_scan, (host, ports_service))
        print('starting '+host+'...')
    #關閉處理程序池，close()必須在join()之前執行
    pool.close()
    #等待池中的處理程序全部執行結束
    pool.join()

    #列印輸出結果
    for host in results:
        print('='*30)
        print(host,'.'*10)
        for port in results[host].get():
            print(port, ':', ports_service[port])

```
# 範例程式:09-04_2.py
```
from time import sleep
from socket import gethostbyname
from datetime import datetime

def get_ipAddresses(url):
    ipAddresses = [0]
    while True:
        sleep(0.5)				#暫停0.5秒
        ip = gethostbyname(url)
        if ip != ipAddresses[-1]:	#目標主機IP位址發生變化
            ipAddresses.append(ip)
            print(str(datetime.now())[:19]+'===>'+ip)
get_ipAddresses(r'www.microsoft.com')

```
# 範例程式:09-04_3.py
```
import socket
import nmap

nmScan = nmap.PortScanner()			#建立連接埠掃描物件
ip = socket.gethostbyname('www.microsort.com')	#取得目標主機的IP位址
nmScan.scan(ip,'80')				#掃描指定連接埠
print(nmScan[ip]['tcp'][80]['state'])	#查看連接埠狀態

```
# 範例程式:09-05.py
```
import sys
import multiprocessing
import re
import os
import urllib.request as lib

def craw_links(url, depth, keywords, processed):
    '''url:the url to craw
     depth:the current depth to craw
     keywords:the tuple of keywords to focus
     pool:process pool
    '''
    contents = []
    if url.startswith(('http://', 'https://')):        
        if url not in processed:
            #mark this url as processed
            processed.append(url)
        else:
            #avoid processing the same url again
            return
        print('Crawing '+url+'...')
        fp = lib.urlopen(url)
        #Python3 returns bytes, so need to decode. 
        contents = fp.read()
        contents_decoded = contents.decode('UTF-8')
        fp.close()
        pattern = '|'.join(keywords)
        #if this page contains certain keywords, save it to a file
        flag = False
        if pattern:
            searched = re.search(pattern, contents_decoded)
        else:
            #if the keywords to filter is not given, save current page
            flag = True
        if flag or searched:
            with open('craw\\'+url.replace(':','_').replace('/','_'), 'wb') as fp:
                fp.write(contents)
        #find all the links in the current page
        links = re.findall('href="(.*?)"', contents_decoded)
        #craw all links in the current page
        for link in links:
            #consider the relative path
            if not link.startswith(('http://','https://')):                
                try:
                    index = url.rindex('/')
                    link = url[0:index+1]+link
                except:
                    pass                
            if depth>0 and link.endswith(('.htm','.html')):
                craw_links(link, depth-1, keywords, processed)
                
if __name__=='__main__':   
    processed = []   
    keywords = ('datetime','KeyWord2')
    if not os.path.exists('craw') or not os.path.isdir('craw'):
        os.mkdir('craw')
    craw_links(r'https://docs.python.org/3/library/index.html', 1, keywords, processed)

```
# 範例程式:09-07.py
```
import os.path
from flask import Flask
from flask.ext.mail import Mail, Message

app = Flask(__name__)
#以126免費郵件為例
app.config['MAIL_SERVER'] = 'smtp.126.com'
app.config['MAIL_PORT'] = 25
app.config['MAIL_USE_TLS'] = True
#如果電子郵件帳號是abcd@126.com，便應填寫abcd
app.config['MAIL_USERNAME'] = 'your own username of your email'
app.config['MAIL_PASSWORD'] = 'your own password of the username'

def sendEmail(From, To, Subject, Body, Html, Attachments):
    '''To:must be a list'''
    msg = Message(Subject, sender=From, recipients=To)
    msg.body = Body
    msg.html = Html
    for f in Attachments:
        with app.open_resource(f) as fp:
            msg.attach(filename=os.path.basename(f), data=fp.read(),
                     content_type = 'application/octet-stream')
    mail = Mail(app)
    with app.app_context():
        mail.send(msg)

if __name__=='__main__':
    #From的電子郵件帳號必須與前面相同
    From = '<your email address>'
    #這是筆者的QQ帳號，大家測試時一定要記得修改啊
    To = ['<306467355@qq.com>']
    Subject = 'hello world'
    Body = 'Only a test.'
    Html = '<h1>test test test.</h1>'
    Attachments =['c:\\python35\\python.exe']
    sendEmail(From, To, Subject, Body, Html, Attachments)

```
# 範例程式:client.py
```
import socket

#伺服端主機IP位址和連接埠號
HOST = '127.0.0.1'
PORT = 50007
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
try:
    #連接伺服器
    s.connect((HOST, PORT))
except Exception as e:
    print('Server not found or not open')
    sys.exit()
while True:
    c = input('Input the content you want to send:')
    #傳送資料
    s.sendall(c.encode())
    #從伺服端接收資料
    data = s.recv(1024)
    data = data.decode()
    print('Received:', data)
    if c.lower() == 'bye':
        break
#關閉連接
s.close()

```
# 範例程式:flask_test.py
```
from flask import Flask

app = Flask(__name__)
@app.route("/")
def hello():    
    return "Hello World! 你好嗎？" 

if __name__ == "__main__":
    app.run()

```
# 範例程式:ftpClient.py
```
import socket
import sys
import re
import struct
import getpass

def main(serverIP):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((serverIP, 10600))
    userId = input('請輸入帳號名稱：')
    #以getpass模組的getpass()方法取得密碼，不顯示
    userPwd = getpass.getpass('請輸入密碼：')
    message = userId+','+userPwd
    sock.send(message.encode())
    login = sock.recv(100)
    #驗證是否登錄成功
    if login == b'error':
        print('帳號名稱或密碼錯誤')
        return
    #整數編碼大小
    intSize = struct.calcsize('I')
    while True:
        #接收用戶端命令，其中##>是提示符號
        command = input('##> ').lower().strip()
        #未輸入任何有效字元，提前進入下一次迴圈，等待使用者繼續輸入
        if not command:
            continue
        #向伺服端傳送命令
        command = ' '.join(command.split())
        sock.send(command.encode())
        #退出
        if command in ('quit', 'q'):
            break
        #查看檔案列表
        elif command in ('list', 'ls', 'dir'):
            #先接收位元組字串大小，再根據情況接收適當數量的位元組字串
            loc_size = struct.unpack('I', sock.recv(intSize))[0]
            files = eval(sock.recv(loc_size).decode())
            for item in files:
                print(item)
        #切換至上一層目錄
        elif ''.join(command.split()) == 'cd..':
            print(sock.recv(100).decode())
        #查看目前工作目錄
        elif command in ('cwd', 'cd'):
            print(sock.recv(1024).decode())
        #切換至子資料夾
        elif command.startswith('cd '):
            print(sock.recv(100).decode())
        #從伺服器下載檔案
        elif command.startswith('get '):
            isFileExist = sock.recv(20)
            #檔案不存在
            if isFileExist != b'ok':
                print('error')
            #檔案存在，開始下載
            else:
                print('downloading.', end='')
                fp = open(command.split()[1], 'wb')
                while True:
                    #顯示進度
                    print('.', end='')
                    data = sock.recv(4096)
                    if data == b'overxxxx':
                        break
                    fp.write(data)
                    sock.send(b'ok')
                fp.close()
                print('ok')
                
        #無效命令
        else:
            print('無效命令')
    sock.close()

if __name__ == '__main__':
    if len(sys.argv) != 2:
        print('Usage:{0} serverIPAddress'.format(sys.argv[0]))
        exit()
    serverIP = sys.argv[1]
    #以規則運算式判斷伺服器位址是否為合法的IP位址
    if re.match(r'^\d{1,3}.\d{1,3}.\d{1,3}.\d{1,3}$', serverIP):
        main(serverIP)
    else:
        print('伺服器位址不合法')
        exit()

```
# 範例程式:ftpServer.py
```
import socket
import threading
import os
import struct

#使用者帳號、密碼、主目錄
#也可以把這些資料存放到資料庫
users = {'zhangsan':{'pwd':'zhangsan1234', 'home':r'c:\python 3.5'},
         'lisi':{'pwd':'lisi567', 'home':'c:\\'}}

def server(conn,addr, home):
    print('新用戶端：'+str(addr))
    #進入目前帳號主目錄
    os.chdir(home)
    while True:
        data = conn.recv(100).decode().lower()
        #顯示用戶端輸入的每一道命令
        print(data)
        #用戶端退出
        if data in ('quit', 'q'):
            break
        #查看目前資料夾的檔案列表
        elif data in ('list', 'ls', 'dir'):
            files = str(os.listdir(os.getcwd()))
            files = files.encode()
            #先傳送位元組字串大小，再傳送位元組字串
            conn.send(struct.pack('I', len(files)))
            conn.send(files)
        #切換至上一層目錄
        elif ''.join(data.split()) == 'cd..':
            cwd = os.getcwd()
            newCwd = cwd[:cwd.rindex('\\')]
            #考慮根目錄的情況
            if newCwd[-1] == ':':
                newCwd += '\\'
            #限定使用者主目錄
            if newCwd.lower().startswith(home):
                os.chdir(newCwd)
                conn.send(b'ok')
            else:
                conn.send(b'error')
        #查看目前的目錄
        elif data in ('cwd', 'cd'):
            conn.send(str(os.getcwd()).encode())
        elif data.startswith('cd '):
            #指定最大分隔次數，考慮目的資料夾帶有空格的情況
            #只允許以相對路徑進行跳轉
            data = data.split(maxsplit=1)
            if len(data) == 2 and  os.path.isdir(data[1]) \
               and data[1]!=os.path.abspath(data[1]):
                os.chdir(data[1])
                conn.send(b'ok')
            else:
                conn.send(b'error')
        #下載檔案
        elif data.startswith('get '):
            data = data.split(maxsplit=1)
            #檢查檔案是否存在
            if len(data) == 2 and os.path.isfile(data[1]):
                conn.send(b'ok')
                fp = open(data[1], 'rb')
                while True:
                    content = fp.read(4096)
                    #傳送檔案結束
                    if not content:
                        conn.send(b'overxxxx')
                        break
                    #傳送檔案內容
                    conn.send(content)
                    if conn.recv(10) == b'ok':
                        continue
                fp.close()
            else:
                conn.send(b'no')
        #無效命令
        else:
            pass
            
    conn.close()
    print(str(addr)+'關閉連接')

#建立Socket，監聽本地連接埠，並等待用戶端連接
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.bind(('', 10600))
sock.listen(5)
while True:
    conn, addr = sock.accept()
    #驗證用戶端輸入的帳號和密碼是否正確
    userId, userPwd = conn.recv(1024).decode().split(',')
    if userId in users and users[userId]['pwd'] == userPwd:
        conn.send(b'ok')
        #為每個用戶端連接建立與啟動一個執行緒
        #參數為連接、用戶端位址、帳號主目錄
        home = users[userId]['home']
        t = threading.Thread(target=server, args=(conn,addr,home))
        t.daemon = True
        t.start()
    else:
        conn.send(b'error')

```
# 範例程式:server.py
```
import socket

words = {'how are you?':'Fine,thank you.', 'how old are you?':'38',
        'what is your name?':'Dong FuGuo', "what's your name?":'Dong FuGuo',
        'where do you work?':'SDIBT', 'bye':'Bye'}
HOST = ''
PORT = 50007
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
#繫結socket
s.bind((HOST, PORT))
#開始監聽用戶端連接
s.listen(1)
print('Listening at port:',PORT)
conn, addr = s.accept()
print('Connected by', addr)
while True:
    data = conn.recv(1024)
    data = data.decode()
    if not data:
        break
    print('Received message:', data)
    conn.sendall(words.get(data, 'Nothing').encode())
conn.close()
s.close()

```
# 範例程式:showIP.py
```
import socket
import uuid

ip = socket.gethostbyname(socket.gethostname())	#本機IP地址
node = uuid.getnode()
macHex = uuid.UUID(int=node).hex[-12:]
mac = []
for i in range(len(macHex))[::2]:
    mac.append(macHex[i:i+2])
mac = ':'.join(mac)							#網卡實體位址
print('IP:', ip)
print('MAC:', mac)

```
# 範例程式:sockMiddle.py
```
import sys
import socket
import threading

def middle(conn, addr):
    #面對伺服器的Socket
    sockDst = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sockDst.connect((ipServer,portServer))
    while True:
        data = conn.recv(1024).decode()
        print('收到用戶端訊息：'+data)
        if data == '不要發給伺服器':
            conn.send('該訊息已被代理伺服器過濾'.encode())
            print('該訊息已過濾')
        elif data.lower() == 'bye':
            print(str(addr)+'用戶端關閉連接')
            break
        else:
            sockDst.send(data.encode())
            print('已轉發伺服器')
            data_fromServer = sockDst.recv(1024).decode()
            print('收到伺服器回覆的訊息：'+data_fromServer)
            if data_fromServer == '不要發給用戶端':
                conn.send('該訊息已被代理伺服器修改'.encode())
                print('訊息已被篡改')
            else:
                conn.send(b'Server reply:'+data_fromServer.encode())
                print('已轉發伺服器訊息給用戶端')
        
    conn.close()
    sockDst.close()

def main():
    sockScr = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sockScr.bind(('', portScr))
    sockScr.listen(200)
    print('代理已啟動')
    while True:
        try:
            conn, addr = sockScr.accept()
            t = threading.Thread(target=middle, args=(conn, addr))
            t.start()
            print('新客戶：'+str(addr))
        except:
            pass
        
if __name__ == '__main__':
    try:
        #(本機IP地址,portScr)<==>(ipServer,portServer)
        #代理伺服器監聽連接埠
        portScr = int(sys.argv[1])
        #伺服器IP位址與連接埠號
        ipServer = sys.argv[2]
        portServer = int(sys.argv[3])
        main()
    except:
        print('Sth error')

```
# 範例程式:sockMiddle_client.py
```
import sys
import socket

def main():
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((ip, port))
    while True:
        data = input('What do you want to ask:')
        sock.send(data.encode())
        print(sock.recv(1024).decode())
        if data.lower() == 'bye':
            break
    sock.close()

if __name__ == '__main__':
    try:
        #代理伺服器的IP位址和連接埠號
        ip = sys.argv[1]
        port = int(sys.argv[2])
        main()
    except:
        print('Sth error')

```
# 範例程式:sockMiddle_server.py
```
import sys
import socket
import threading

#回覆訊息，原樣返回
def replyMessage(conn):
    while True:
        data = conn.recv(1024)
        conn.send(data)
        if data.decode().lower() == 'bye':
            break
    conn.close()

def main():
    sockScr = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sockScr.bind(('', port))
    sockScr.listen(200)
    while True:
        try:
            conn, addr = sockScr.accept()
            #只允許特定主機存取本伺服器
            if addr[0] != onlyYou:
                conn.close()
                continue
            #建立並啟動執行緒
            t = threading.Thread(target=replyMessage, args=(conn,))
            t.start()        
        except:
            print('error')

if __name__ == '__main__':
    try:
        #取得命令列參數，port為伺服器監聽連接埠
        #只允許IP位址為onlyYou的主機存取
        port = int(sys.argv[1])
        onlyYou = sys.argv[2]
        main()
    except:
        print('Must give me a number as port')

```
# 範例程式:udp_receiver.py
```
import socket
#使用IPV4協定，以UDP協定傳輸資料
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
#繫結連接埠和埠號，空字串表示本機任何可用的IP位址
s.bind(('', 5000))
while True:
    data, addr = s.recvfrom(1024)
    #顯示接收的內容
    print('received message:{0} from PORT {1} on {2}'.format(data.decode(),
                                                     addr[1], addr[0]))
    if data.decode().lower() == 'bye':
        break
s.close( )

```
# 範例程式:udp_sender.py
```
import socket
import sys
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
#假設192.168.0.103是接收端機器的IP位址
s.sendto(sys.argv[1].encode() , ("10.93.2.31" ,5000))
s.close( )

```
# 範例程式:10-01.py
```
from threading import Thread
import time

def func1(x, y):
    for i in range(x, y):
        print(i, end=' ')
    print()
    time.sleep(10)		#等待10秒

t1 = Thread(target=func1, args=(15, 20))	#建立執行緒物件，args是傳遞給函數的參數
t1.start()			#啟動執行緒
t1.join(5)			#等待中的執行緒t1執行結束，或等待5秒鐘
t2 = Thread(target=func1, args=(5, 10))
t2.start()

```
# 範例程式:10-02.py
```
from threading import Thread
import time
def func1():
    time.sleep(10)

t1 = Thread(target=func1)
print('t1:',t1.isAlive())	#執行緒未未運行，返回False
t1.start()
print('t1:',t1.isAlive())	#執行緒還在運行，返回True
t1.join(5)				#join()方法因逾時而結束
print('t1:',t1.isAlive())	#執行緒還在運行，返回True
t1.join()					#等待中的執行緒結束
print('t1:',t1.isAlive())	#執行緒已結束，返回False

```
# 範例程式:10-04.py
```
import threading
import time

#自訂執行緒類別
class mythread(threading.Thread):
    def __init__(self):
        threading.Thread.__init__(self)
    #重寫run()方法
    def run(self):
        global x
        #取得鎖，如果成功則進入臨界區
        lock.acquire()
        x = x+3
        print(x)
        #退出臨界區，釋放鎖
        lock.release()
        
lock = threading.RLock()
#也可以使用Lock類別實現加鎖和執行緒同步
#lock = threading.Lock()

#存放多個執行緒的列表
tl = []
for i in range(10):
    #建立執行緒並加入列表
    t = mythread()
    tl.append(t)

#多個執行緒互斥存取的變數
x = 0
#啟動列表的所有執行緒
for i in tl:
    i.start()

```
# 範例程式:10-05.py
```
import threading
from random import randint
from time import sleep

#自訂生產者執行緒類別
class Producer(threading.Thread):
    def __init__(self, threadname):
        threading.Thread.__init__(self,name=threadname)
    def run(self):
        global x
        while True:
            #取得鎖
            con.acquire()
            #假設共用列表中，最多只能容納20個元素
            if len(x) == 20:
                #如果共用列表已滿，生產者等待
                con.wait()
                print('Producer is waiting.....')
            else:
                print('Producer:', end=' ')
                #產生新元素，附加至共用列表
                x.append(randint(1, 1000))
                print(x)
                sleep(1)
                #喚醒等待條件的執行緒
                con.notify()
            #釋放鎖
            con.release()
        
#自訂消費者執行緒類別
class Consumer(threading.Thread):
    def __init__(self, threadname):
        threading.Thread.__init__(self, name =threadname)
    def run(self):
        global x
        while True:
            #取得鎖
            con.acquire()
            if not x:
                #等待
                con.wait()
                print('Consumer is waiting.....')
            else:
                print(x.pop(0))
                print(x)
                sleep(2)
                con.notify()
            con.release()
        
#建立Condition物件，以及生產者執行緒和消費者執行緒
con = threading.Condition()
x = []
p = Producer('Producer')
c = Consumer('Consumer')
p.start()
c.start()
p.join()
c.join()

```
# 範例程式:10-06.py
```
import threading
import time
import queue 

#自訂生產者執行緒類別
class Producer(threading.Thread):
    def __init__(self, threadname):
        threading.Thread.__init__(self, name = threadname)
    def run(self):
        global myqueue
        #在佇列尾部附加元素
        myqueue.put(self.getName())
        print(self.getName(), ' put ', self.getName(), ' to queue.')

class Consumer(threading.Thread):
    def __init__(self, threadname):
        threading.Thread.__init__(self, name = threadname)
    def run(self):
        global myqueue
        #在佇列頭部取得元素
        print(self.getName(), ' get ', myqueue.get(), ' from queue.')

myqueue = queue.Queue()

#建立生產者執行緒和消費者執行緒
plist = []
clist = []
for i in range(10):
    p = Producer('Producer' + str(i))
    plist.append(p)
    c = Consumer('Consumer' + str(i))
    clist.append(c)

#依序啟動生產者執行緒和消費者執行緒
for p, c in zip(plist, clist):
    p.start()
    p.join()
    c.start()
    c.join()

```
# 範例程式:10-08.py
```
from multiprocessing import Process
import os

def f(name):
    print('module name:', __name__)
    print('parent process:', os.getppid())	#查看父處理序ID
    print('process id:', os.getpid())		#查看目前處理序ID
    print('hello', name)

if __name__ == '__main__':
    p = Process(target=f, args=('bob',))	#建立處理序
    p.start()			#啟動處理序
    p.join()			#等待處理序執行結束

```
# 範例程式:10-08_2.py
```
from multiprocessing import Pool
from statistics import mean

def f(x):
    return mean(x)

if __name__ == '__main__':
    x = [list(range(10)), list(range(20,30)), list(range(50,60)), list(range(80,90))]
    with Pool(5) as p:		#建立包含5個處理序的處理序池
        print(p.map(f, x))		#並行執行

```
# 範例程式:10-09.py
```
import multiprocessing as mp

def foo(q):
    q.put('hello world!')			#把資料放入佇列

if __name__ == '__main__':
    mp.set_start_method('spawn')	#Windows系統建立子處理序的預設方式
    q = mp.Queue()
    p = mp.Process(target=foo, args=(q,))  #建立處理序，把Queue物件作為參數傳遞
    p.start()
    p.join()
    print(q.get())				#從佇列取得資料

```
# 範例程式:10-09_2.py
```
import multiprocessing as mp

def foo(q):
    q.put('hello world')

if __name__ == '__main__':
    ctx = mp.get_context('spawn')
    q = ctx.Queue()
    p = ctx.Process(target=foo, args=(q,))
    p.start()
    p.join()
    print(q.get())

```
# 範例程式:10-10.py
```
from multiprocessing import Process, Pipe

def f(conn):
    conn.send('hello world')		#對管道傳送資料
    conn.close()				#關閉管道

if __name__ == '__main__':
    parent_conn, child_conn = Pipe()	#建立管道物件
    p = Process(target=f, args=(child_conn,))	#將管道的一方作為參數，傳遞給子處理序
    p.start()
    p.join()
    print(parent_conn.recv())		#透過管道的另一方取得資料
    parent_conn.close()

```
# 範例程式:10-11.py
```
from multiprocessing import Process, Value, Array

def f(n, a):
    n.value = 3.1415927
    for i in range(len(a)):
        a[i] = a[i]*a[i]

if __name__ == '__main__':
    num = Value('d', 0.0)					#實數
    arr = Array('i', range(10))			#整數型陣列
    p = Process(target=f, args=(num, arr))	#建立處理序物件
    p.start()
    p.join()
    print(num.value)
    print(arr[:])

```
# 範例程式:10-12.py
```
from multiprocessing import Process, Manager

def f(d, l, t):
    d['name'] = 'Dong Fuguo'
    d['age'] = 38
    d['sex'] = 'Male'
    d['affiliation'] = 'SDIBT'
    l.reverse()
    t.value = 3    

if __name__ == '__main__':
    with Manager() as manager:
        d = manager.dict()
        l = manager.list(range(10))
        t = manager.Value('i', 0)
        p = Process(target=f, args=(d, l, t))
        p.start()
        p.join()
        for item in d.items():
            print(item)
        print(l)
        print(t.value)

```
# 範例程式:10-13.py
```
from multiprocessing import Process, Lock

def f(l, i):
    l.acquire()			#取得鎖
    try:
        print('hello world', i)
    finally:
        l.release()		#釋放鎖

if __name__ == '__main__':
    lock = Lock()		#建立鎖物件
    for num in range(10):
        Process(target=f, args=(lock, num)).start()

```
# 範例程式:10-14.py
```
from multiprocessing import Process, Event

def f(e, i):
    if e.is_set():
        e.wait()
        print('hello world', i)
        e.clear()
    else:
        e.set()

if __name__ == '__main__':
    e = Event()
    for num in range(10):
        Process(target=f, args=(e,num)).start()

```
# 範例程式:demo.py
```
import threading

#自訂執行緒類別
class mythread(threading.Thread):
    def __init__(self, threadname):
        threading.Thread.__init__(self, name = threadname)
        
    def run(self):
        global myevent
        #根據Event物件是否已設定，做出不同的回應
        if myevent.isSet():
            #清除標誌
            myevent.clear()
            #等待
            myevent.wait()
            print(self.getName()+' set')
        else:
            print(self.getName()+' not set')
            #設定標誌
            myevent.set()

myevent = threading.Event()
#設定標誌
myevent.set()

for i in range(10):
    t = mythread(str(i))
    t.start()

```
# 範例程式:threaddaomon.py
```
import threading
import time

class mythread(threading.Thread):	#繼承Thread類別，建立自訂的執行緒類別
    def __init__(self, num, threadname):
        threading.Thread.__init__(self, name=threadname)
        self.num = num
    def run(self): 				#重寫run()方法
        time.sleep(self.num)
        print(self.num)

t1 = mythread(1, 't1')	#建立自訂執行緒類別物件，daemon預設為False
t2 = mythread(5, 't2')
t2.daemon = True		#設定執行緒物件t2的daemon屬性為True
print(t1.daemon)
print(t2.daemon)
t1.start()			#啟動執行緒
t2.start()

```
# 範例程式:FileSplit.py
```
import os
import os.path
import time

def FileSplit(sourceFile, targetFolder):
    if not os.path.isfile(sourceFile):		#原始檔案必須存在
        print(sourceFile, ' does not exist.')
        return
    if not os.path.isdir(targetFolder):		#目的資料夾不存在，則建立
        os.mkdir(targetFolder)
    tempData = []		#存放臨時資料
    number = 1000		#切分後的每個小檔案包含1000列
    fileNum = 1			#切分後的檔案編號
    with open(sourceFile, 'r') as srcFile:
        dataLine = srcFile.readline().strip()
        while dataLine:
            for i in range(number):			#讀取1000列文字
                tempData.append(dataLine) 
                dataLine = srcFile.readline() 
                if not dataLine:
                    break
            desFile = os.path.join(targetFolder, sourceFile[0:-4] + str(fileNum) + '.txt')
            with open(desFile, 'a+') as f:	#建立一個小檔案
                f.writelines(tempData)
            tempData = []
            fileNum = fileNum + 1			#小檔案編號加1

if __name__ == '__main__':
    sourceFile = 'test.txt'				#指定原始檔案
    targetFolder = 'test'					#指定存放切分後，小檔案的資料夾
    FileSplit(sourceFile, targetFolder)

```
# 範例程式:Hadoop_Map.py
```
import os
import re
import time

def Map(sourceFile):
    if not os.path.exists(sourceFile):
        print(sourceFile, ' does not exist.')
        return    
    pattern = re.compile(r'[0-9]{4}/[0-9]{1,2}/[0-9]{1,2}')
	#pattern = re.compile(r'[0-9]{1,2}/[0-9]{1,2}/[0-9]{4}')
    result = {}
    with open(sourceFile, 'r') as srcFile:
        for dataLine in srcFile:
            r = pattern.findall(dataLine)
            if r:
                print(r[0], ',', 1)		#將中間結果輸出到標準控制台
Map('test.txt')    

```
# 範例程式:Hadoop_Reduce.py
```
import os
import sys

def Reduce(targetFile):
    result = {}
    for line in sys.stdin:		#從標準控制台中獲取得中間結果資料
        riqi, shuliang = line.strip().split(',')
        result[riqi] = result.get(riqi, 0)+1
    with open(targetFile, 'w') as fp:
        for k,v in result.items():
            fp.write(k + ':' + str(v) + '\n')
Reduce('result.txt')

```
# 範例程式:isPrime.py
```
from pyspark import SparkConf, SparkContext
from pyspark.sql import SQLContext
from random import random

conf = SparkConf().setAppName("isPrime")
sc = SparkContext(conf=conf)
sqlCtx = SQLContext(sc)
def isPrime(n):
    if n<2:
        return False
    if n==2:
        return True
    if not n&1:
        return False
    for i in range(3, int(n**0.5)+2, 2):
        if n%i == 0:
            return False
    return True
rdd = sc.parallelize(range(100000000))
result = rdd.filter(isPrime).count()
print('='*30)
print(result)

```
# 範例程式:isPrime2.py
```
import time

def isPrime(n):
    if n<2:
        return False
    if n==2:
        return True
    if not n&1:
        return False
    for i in range(3, int(n**0.5)+2, 2):
        if n%i == 0:
            return False
    return True

num = 0
start = time.time()
for n in range(100000000):
    if isPrime(n):
        num += 1
print(num)
print(time.time()-start)

```
# 範例程式:Map.py
```
import os
import re
import threading
import time

def Map(sourceFile):				#這段程式碼僅適用於配套檔案，
    if not os.path.exists(sourceFile):	#或者類似的Windows升級日誌
        print(sourceFile, ' does not exist.')
        return    
    pattern = re.compile(r'[0-9]{4}/[0-9]{1,2}/[0-9]{1,2}')
    result = {}
    with open(sourceFile, 'r') as srcFile:
        for dataLine in srcFile:
            r = pattern.findall(dataLine)	#找尋符合日期格式的字串
            if r:
                result[r[0]] = result.get(r[0], 0) + 1
    desFile = sourceFile[0:-4] + '_map.txt'
    with open(desFile, 'a+') as fp:		#中間的臨時結果
        for k, v in result.items():
            fp.write(k + ':' + str(v) + '\n')

if __name__ == '__main__':
    desFolder = 'test'
    files = os.listdir(desFolder)
    def Main(i):					#使用多執行緒
        Map(desFolder + '\\' + files[i])
    fileNumber = len(files)
    for i in range(fileNumber):
        t = threading.Thread(target = Main, args =(i,))
        t.start()    

```
# 範例程式:pi.py
```
from pyspark import SparkConf, SparkContext
from pyspark.sql import SQLContext
from random import random

conf = SparkConf().setAppName("pi")
sc = SparkContext(conf=conf)
sqlCtx = SQLContext(sc)

def sample(p):
    x, y = random(), random()
    return 1 if x*x + y*y < 1 else 0

NUM_SAMPLES = 100000		#數值越大結果越準確
count = sc.parallelize(range(NUM_SAMPLES))
count = count.map(sample).reduce(lambda a, b: a + b)
print('='*30)
print("Pi is roughly %f" % (4.0 * count / NUM_SAMPLES))
print('='*30)

```
# 範例程式:Reduce.py
```
from os.path import isdir
from os import listdir

def Reduce(sourceFolder, targetFile):
    if not isdir(sourceFolder):
        print(sourceFolder, ' does not exist.')
        return
    result = {}
    #Deal only with the mapped files
    allFiles = [sourceFolder+'\\'+f for f in listdir(sourceFolder) if f.endswith('_map.txt')]
    for f in allFiles:
        with open(f, 'r') as fp:
            for line in fp:
                line = line.strip()
                if not line:
                    continue
                key, value = line.split(':')	#結合Map.py，理解此處的邏輯
                result[key] = result.get(key,0) + int(value)
    with open(targetFile, 'w') as fp:		#建立結果檔
        for k,v in result.items():
            fp.write(k + ':' + str(v) + '\n')

if __name__ == '__main__':
    Reduce('test', 'test\\result.txt')

```
# 範例程式:12_01_02.py
```
def getBezier(self, P0, P1, P2, P3, t):
    a0 = (1-t)**3
    a1 = 3 * (1-t)**2 * t
    a2 = 3 * t**2 * (1-t)
    a3 = t**3

    x = a0*P0[0] + a1*P1[0] + a2*P2[0] + a3*P3[0]
    y = a0*P0[1] + a1*P1[1] + a2*P2[1] + a3*P3[1]
    z = a0*P0[2] + a1*P1[2] + a2*P2[2] + a3*P3[2]

    return (x, y, z)

def Draw(self):
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
    glLoadIdentity()
    #平移
    glTranslatef(-3.0, 0.0, -8.0)
    #指定三次貝茲曲線的四個控制點座標
    P0 = (-4, -2, -9)
    P1 = (-0.5, 3, 0)
    P2 = (2, -3, 0)
    P3 = (4.5, 2, 0)
    #指定模式，繪製連續的折線
    glBegin(GL_LINE_STRIP)
    #設定頂點顏色
    glColor3f(0.0, 0.0, 0.0)
    #使用100段直線條的拼接，以便逼近三次貝茲曲線
    for i in range(101):
        #參數t必須是介於[0,1]之間的實數
        t = i/100.0
        p = self.getBezier(P0, P1, P2, P3, t)
        glVertex3f(*p)
        
    #結束本次繪製
    glEnd()       
        
    glutSwapBuffers()

```
# 範例程式:12_01_03.py
```
# -*- coding:utf-8 -*-
# Filename: TextureMapping.py
# --------------------
# Function description:
# PyOpenGL demo
# --------------------
# Author: Dong Fuguo
# QQ: 306467355
# Email: dongfuguo2005@126.com
#--------------------
# Date: 2014-12-29, Updated on 2016-5-1
# --------------------

import sys
from OpenGL.GL import *
from OpenGL.GLUT import *
from OpenGL.GLU import *
from PIL import Image

class MyPyOpenGLTest:
    def __init__(self, width=640, height=480, title=b'MyPyOpenGLTest'):
        glutInit(sys.argv)
        glutInitDisplayMode(GLUT_RGBA | GLUT_DOUBLE | GLUT_DEPTH)
        glutInitWindowSize(width, height)
        self.window = glutCreateWindow(title)
        glutDisplayFunc(self.Draw)
        #指定鍵盤事件處理函數
        glutKeyboardFunc(self.KeyPress)
        glutIdleFunc(self.Draw)
        self.InitGL(width, height)
        #繞各坐標軸旋轉的角度
        self.x = 0.0
        self.y = 0.0
        self.z = 0.0

    def KeyPress(self, key, x, y):        
        if key==b'a':
            self.x += 0.3
        elif key==b's':
            self.x -= 0.3
        elif key==b'j':
            self.y += 0.3
        elif key==b'k':
            self.y -= 0.3
        elif key==b'g':
            self.z += 0.3
        elif key==b'h':
            self.z -= 0.3

    #繪製圖形
    def Draw(self):
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        glLoadIdentity()
        #沿z軸平移
        glTranslate(0.0, 0.0, -5.0)
        #分別繞x,y,z軸旋轉
        glRotatef(self.x, 1.0, 0.0, 0.0)
        glRotatef(self.y, 0.0, 1.0, 0.0)
        glRotatef(self.z, 0.0, 0.0, 1.0)

        #開始繪製立方體的每個面，同時設定紋理映射
        #繪製四邊形
        glBegin(GL_QUADS)
        #設定紋理坐標
        glTexCoord2f(0.0, 0.0)
        #繪製頂點
        glVertex3f(-1.0, -1.0, 1.0)
        glTexCoord2f(1.0, 0.0)
        glVertex3f(1.0, -1.0, 1.0)
        glTexCoord2f(1.0, 1.0)
        glVertex3f(1.0, 1.0, 1.0)
        glTexCoord2f(0.0, 1.0)
        glVertex3f(-1.0, 1.0, 1.0)

        glTexCoord2f(1.0, 0.0)
        glVertex3f(-1.0, -1.0, -1.0)
        glTexCoord2f(1.0, 1.0)
        glVertex3f(-1.0, 1.0, -1.0)
        glTexCoord2f(0.0, 1.0)
        glVertex3f(1.0, 1.0, -1.0)
        glTexCoord2f(0.0, 0.0)
        glVertex3f(1.0, -1.0, -1.0)

        glTexCoord2f(0.0, 1.0)
        glVertex3f(-1.0, 1.0, -1.0)
        glTexCoord2f(0.0, 0.0)
        glVertex3f(-1.0, 1.0, 1.0)
        glTexCoord2f(1.0, 0.0)
        glVertex3f(1.0, 1.0, 1.0)
        glTexCoord2f(1.0, 1.0)
        glVertex3f(1.0, 1.0, -1.0)
        
        glTexCoord2f(1.0, 1.0)
        glVertex3f(-1.0, -1.0, -1.0)
        glTexCoord2f(0.0, 1.0)
        glVertex3f(1.0, -1.0, -1.0)
        glTexCoord2f(0.0, 0.0)
        glVertex3f(1.0, -1.0, 1.0)
        glTexCoord2f(1.0, 0.0)
        glVertex3f(-1.0, -1.0, 1.0)
        
        glTexCoord2f(1.0, 0.0)
        glVertex3f(1.0, -1.0, -1.0)
        glTexCoord2f(1.0, 1.0)
        glVertex3f(1.0, 1.0, -1.0)
        glTexCoord2f(0.0, 1.0)
        glVertex3f(1.0, 1.0, 1.0)
        glTexCoord2f(0.0, 0.0)
        glVertex3f(1.0, -1.0, 1.0)

        glTexCoord2f(0.0, 0.0)
        glVertex3f(-1.0, -1.0, -1.0)
        glTexCoord2f(1.0, 0.0)
        glVertex3f(-1.0, -1.0, 1.0)
        glTexCoord2f(1.0, 1.0)
        glVertex3f(-1.0, 1.0, 1.0)
        glTexCoord2f(0.0, 1.0)
        glVertex3f(-1.0, 1.0, -1.0)

        #結束繪製
        glEnd()
        #刷新螢幕，產生動畫效果
        glutSwapBuffers()

    #載入紋理
    def LoadTexture(self):
        img = Image.open('sample_texture.bmp')
        width, height = img.size
        img = img.tobytes('raw', 'RGBX', 0, -1)
        glBindTexture(GL_TEXTURE_2D, glGenTextures(1))
        glPixelStorei(GL_UNPACK_ALIGNMENT,1)
        glTexImage2D(GL_TEXTURE_2D, 0, 4, width, height, 0, GL_RGBA,
                     GL_UNSIGNED_BYTE,img)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_CLAMP)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_CLAMP)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_NEAREST)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST)
        glTexEnvf(GL_TEXTURE_ENV, GL_TEXTURE_ENV_MODE, GL_DECAL)
        
    def InitGL(self, width, height):
        self.LoadTexture()
        glEnable(GL_TEXTURE_2D)
        glClearColor(1.0, 1.0, 1.0, 0.0)
        glClearDepth(1.0)
        glDepthFunc(GL_LESS)
        glShadeModel(GL_SMOOTH)
        #背景剔除，消隱
        glEnable(GL_CULL_FACE)
        glCullFace(GL_BACK)
        glEnable(GL_POINT_SMOOTH)
        glEnable(GL_LINE_SMOOTH)
        glEnable(GL_POLYGON_SMOOTH)
        glMatrixMode(GL_PROJECTION)
        glHint(GL_POINT_SMOOTH_HINT,GL_NICEST)
        glHint(GL_LINE_SMOOTH_HINT,GL_NICEST)
        glHint(GL_POLYGON_SMOOTH_HINT,GL_FASTEST)
        glLoadIdentity()
        gluPerspective(45.0, float(width)/float(height), 0.1, 100.0)
        glMatrixMode(GL_MODELVIEW)
    def MainLoop(self):
        glutMainLoop()

if __name__ == '__main__':
    w = MyPyOpenGLTest()
    w.MainLoop()

```
# 範例程式:12-01.py
```
import sys
from math import pi as PI
from math import sin, cos
from OpenGL.GL import *
from OpenGL.GLU import *
from OpenGL.GLUT import *
##下載64 bit PyOpenGL安装包
##http://www.lfd.uci.edu/~gohlke/pythonlibs/#pyopengl
##pip install file_name.whl
class MyPyOpenGLTest:
    #重寫建構函數，初始化OpenGL環境，指定顯示模式以及用來繪圖的函數
    def __init__(self, width = 640, height = 480, title = 'MyPyOpenGLTest'.encode('big5')):
        glutInit(sys.argv)
        glutInitDisplayMode(GLUT_RGBA | GLUT_DOUBLE | GLUT_DEPTH)
        glutInitWindowSize(width, height)
        self.window = glutCreateWindow(title)
        #指定繪製函數
        glutDisplayFunc(self.Draw)
        glutIdleFunc(self.Draw)
        self.InitGL(width, height)

    #根據特定的需求，進一步完成OpenGL的初始化
    def InitGL(self, width, height):
        #初始化視窗背景為白色
        glClearColor(1.0, 1.0, 1.0, 0.0)
        glClearDepth(1.0)
        glDepthFunc(GL_LESS)
        #光滑渲染
        glEnable(GL_BLEND)
        glShadeModel(GL_SMOOTH)
        glEnable(GL_POINT_SMOOTH)
        glEnable(GL_LINE_SMOOTH)
        glEnable(GL_POLYGON_SMOOTH)        
        glMatrixMode(GL_PROJECTION)
        #反走樣，也稱抗鋸齒
        glHint(GL_POINT_SMOOTH_HINT,GL_NICEST)
        glHint(GL_LINE_SMOOTH_HINT,GL_NICEST)
        glHint(GL_POLYGON_SMOOTH_HINT,GL_FASTEST)
        glLoadIdentity()
        #透視投影變換
        gluPerspective(45.0, float(width)/float(height), 0.1, 100.0)
        glMatrixMode(GL_MODELVIEW)

    #定義自己的繪圖函數
    def Draw(self):
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        glLoadIdentity()
        #平移
        glTranslatef(-3.0, 2.0, -8.0)
        #繪製二維圖形，z座標為0
        #指定模式，繪製多邊形
        glBegin(GL_POLYGON)
        #設定頂點顏色
        glColor3f(1.0, 0.0, 0.0)
        #繪製多邊形頂點
        glVertex3f(0.0, 1.0, 0.0)
        glColor3f(0.0, 1.0, 0.0)
        glVertex3f(1.0, -1.0, 0.0)
        glColor3f(0.0, 0.0, 1.0)
        glVertex3f(-1.0, -1.0, 0.0)
        #結束本次繪製
        glEnd()
        
        glTranslatef(3, -1, 0.0)
        
        #繪製三維圖形，三維線條
        glBegin(GL_LINES)
        glColor3f(1.0, 0.0, 0.0)
        glVertex3f(1.0, 1.0, -1.0)
        glColor3f(0.0, 1.0, 0.0)
        glVertex3f(-1.0, -1.0, 3.0)
        glEnd()

        glTranslatef(-0.3, 1, 0)

        #使用折線繪製圓
        glBegin(GL_LINE_LOOP)
        n = 100
        theta = 2*PI/n
        r = 0.8
        for i in range(100):
            x = r*cos(i*theta)
            y = r*sin(i*theta)
            glVertex3f(x, y, 0)
        glEnd()
        
        glutSwapBuffers()

    #訊息主迴圈
    def MainLoop(self):
        glutMainLoop()

if __name__ == '__main__':
    #產生實體視窗物件，執行程式，啟動訊息主迴圈
    w = MyPyOpenGLTest()
    w.MainLoop()

```
# 範例程式:12-02.py
```
# -*- coding:utf-8 -*-
# Filename: TextureMapping.py
# --------------------
# Function description:
# PyOpenGL demo
# --------------------
# Author: Dong Fuguo
# QQ: 306467355
# Email: dongfuguo2005@126.com
#--------------------
# Date: 2014-12-29, Updated on 2016-12-1
# --------------------

import sys
from OpenGL.GL import *
from OpenGL.GLUT import *
from OpenGL.GLU import *
from PIL import Image

class MyPyOpenGLTest:
    #初始化OpenGL環境
    def __init__(self, width = 640, height = 480, title = b'MyPyOpenGLTest'):
        glutInit(sys.argv)
        glutInitDisplayMode(GLUT_RGBA | GLUT_DOUBLE | GLUT_DEPTH)
        glutInitWindowSize(width, height)
        self.window = glutCreateWindow(title)
        glutDisplayFunc(self.Draw)
        glutIdleFunc(self.Draw)
        self.InitGL(width, height)
        #繞各坐標軸旋轉的角度
        self.x = 0.0
        self.y = 0.0
        self.z = 0.0

    #繪製圖形
    def Draw(self):
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        glLoadIdentity()
        #沿z軸平移
        glTranslate(0.0, 0.0, -5.0)
        #分別繞x,y,z軸旋轉
        glRotatef(self.x, 1.0, 0.0, 0.0)
        glRotatef(self.y, 0.0, 1.0, 0.0)
        glRotatef(self.z, 0.0, 0.0, 1.0)

        #開始繪製立方體的每個面，同時設定紋理映射
        #繪製四邊形
        glBegin(GL_QUADS)
        #繪製頂點，並設定紋理坐標
        glTexCoord2f(0.0, 0.0)        
        glVertex3f(-1.0, -1.0, 1.0)
        glTexCoord2f(1.0, 0.0)
        glVertex3f(1.0, -1.0, 1.0)
        glTexCoord2f(1.0, 1.0)
        glVertex3f(1.0, 1.0, 1.0)
        glTexCoord2f(0.0, 1.0)
        glVertex3f(-1.0, 1.0, 1.0)

        #繪製立方體的第二個面
        glTexCoord2f(1.0, 0.0)
        glVertex3f(-1.0, -1.0, -1.0)
        glTexCoord2f(1.0, 1.0)
        glVertex3f(-1.0, 1.0, -1.0)
        glTexCoord2f(0.0, 1.0)
        glVertex3f(1.0, 1.0, -1.0)
        glTexCoord2f(0.0, 0.0)
        glVertex3f(1.0, -1.0, -1.0)

        #繪製立方體的第三個面
        glTexCoord2f(0.0, 1.0)
        glVertex3f(-1.0, 1.0, -1.0)
        glTexCoord2f(0.0, 0.0)
        glVertex3f(-1.0, 1.0, 1.0)
        glTexCoord2f(1.0, 0.0)
        glVertex3f(1.0, 1.0, 1.0)
        glTexCoord2f(1.0, 1.0)
        glVertex3f(1.0, 1.0, -1.0)

        #繪製立方體的第四個面
        glTexCoord2f(1.0, 1.0)
        glVertex3f(-1.0, -1.0, -1.0)
        glTexCoord2f(0.0, 1.0)
        glVertex3f(1.0, -1.0, -1.0)
        glTexCoord2f(0.0, 0.0)
        glVertex3f(1.0, -1.0, 1.0)
        glTexCoord2f(1.0, 0.0)
        glVertex3f(-1.0, -1.0, 1.0)

        #繪製立方體的第五個面
        glTexCoord2f(1.0, 0.0)
        glVertex3f(1.0, -1.0, -1.0)
        glTexCoord2f(1.0, 1.0)
        glVertex3f(1.0, 1.0, -1.0)
        glTexCoord2f(0.0, 1.0)
        glVertex3f(1.0, 1.0, 1.0)
        glTexCoord2f(0.0, 0.0)
        glVertex3f(1.0, -1.0, 1.0)

        #繪製立方體的第六個面
        glTexCoord2f(0.0, 0.0)
        glVertex3f(-1.0, -1.0, -1.0)
        glTexCoord2f(1.0, 0.0)
        glVertex3f(-1.0, -1.0, 1.0)
        glTexCoord2f(1.0, 1.0)
        glVertex3f(-1.0, 1.0, 1.0)
        glTexCoord2f(0.0, 1.0)
        glVertex3f(-1.0, 1.0, -1.0)

        #結束繪製
        glEnd()
        
        #刷新螢幕，產生動畫效果
        glutSwapBuffers()
        
        #修改各坐標軸的旋轉角度
        self.x += 0.2
        self.y += 0.3
        self.z += 0.1

    #載入紋理
    def LoadTexture(self):
        #底下是紋理圖形檔
        img = Image.open('sample_texture.bmp')
        width, height = img.size
        img = img.tobytes('raw', 'RGBX', 0, -1)
        glBindTexture(GL_TEXTURE_2D, glGenTextures(1))
        glPixelStorei(GL_UNPACK_ALIGNMENT,1)
        glTexImage2D(GL_TEXTURE_2D, 0, 4, width, height, 0, GL_RGBA, GL_UNSIGNED_BYTE,img)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_CLAMP)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_CLAMP)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_NEAREST)
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST)
        glTexEnvf(GL_TEXTURE_ENV, GL_TEXTURE_ENV_MODE, GL_DECAL)
        
    def InitGL(self, width, height):
        self.LoadTexture()
        glEnable(GL_TEXTURE_2D)
        glClearColor(1.0, 1.0, 1.0, 0.0)
        glClearDepth(1.0)
        glDepthFunc(GL_LESS)
        glShadeModel(GL_SMOOTH)
        #背景剔除，消隱
        glEnable(GL_CULL_FACE)
        glCullFace(GL_BACK)
        glEnable(GL_POINT_SMOOTH)
        glEnable(GL_LINE_SMOOTH)
        glEnable(GL_POLYGON_SMOOTH)
        glMatrixMode(GL_PROJECTION)
        glHint(GL_POINT_SMOOTH_HINT,GL_NICEST)
        glHint(GL_LINE_SMOOTH_HINT,GL_NICEST)
        glHint(GL_POLYGON_SMOOTH_HINT,GL_FASTEST)
        glLoadIdentity()
        gluPerspective(45.0, float(width)/float(height), 0.1, 100.0)
        glMatrixMode(GL_MODELVIEW)
        
    def MainLoop(self):
        glutMainLoop()

if __name__ == '__main__':
    w = MyPyOpenGLTest()
    w.MainLoop()

```
# 範例程式:12-03.py
```
import sys
from OpenGL.GL import *
from OpenGL.GLU import *
from OpenGL.GLUT import *

class MyPyOpenGLTest:
    #重寫建構函數，初始化OpenGL環境，指定顯示模式以及用來繪圖的函數
    def __init__(self, width = 640, height = 480, title = b'Normal_Light'):
        glutInit(sys.argv)
        glutInitDisplayMode(GLUT_RGBA | GLUT_DOUBLE | GLUT_DEPTH)
        glutInitWindowSize(width, height)
        self.window = glutCreateWindow(title)
        #指定繪製函數
        glutDisplayFunc(self.Draw)
        glutIdleFunc(self.Draw)
        self.InitGL(width, height)

    #根據特定的需求，進一步完成OpenGL的初始化
    def InitGL(self, width, height):
        #初始化視窗背景為白色
        glClearColor(1.0, 1.0, 1.0, 0.0)
        glClearDepth(1.0)
        glDepthFunc(GL_LESS)
        #設定燈光與材質屬性
        mat_sp = (1.0, 1.0, 1.0, 1.0)
        mat_sh = [50.0]
        light_position = (-0.5, 1.5, 1, 0)
        yellow_l = (1, 1, 0, 1)
        ambient = (0.1, 0.8, 0.2, 1.0)
        glMaterialfv(GL_FRONT, GL_SPECULAR, mat_sp)
        glMaterialfv(GL_FRONT, GL_SHININESS, mat_sh)
        glLightfv(GL_LIGHT0, GL_POSITION, light_position)
        glLightfv(GL_LIGHT0, GL_DIFFUSE, yellow_l)
        glLightfv(GL_LIGHT0, GL_SPECULAR, yellow_l)
        glLightModelfv(GL_LIGHT_MODEL_AMBIENT, ambient)
        #啟用光照模型
        glEnable(GL_LIGHTING)
        glEnable(GL_LIGHT0)
        glEnable(GL_DEPTH_TEST)
        #光滑渲染
        glEnable(GL_BLEND)
        glShadeModel(GL_SMOOTH)
        glEnable(GL_POINT_SMOOTH)
        glEnable(GL_LINE_SMOOTH)
        glEnable(GL_POLYGON_SMOOTH)        
        glMatrixMode(GL_PROJECTION)
        #反走樣，也稱反鋸齒
        glHint(GL_POINT_SMOOTH_HINT,GL_NICEST)
        glHint(GL_LINE_SMOOTH_HINT,GL_NICEST)
        glHint(GL_POLYGON_SMOOTH_HINT,GL_FASTEST)
        glLoadIdentity()
        #透視投影變換
        gluPerspective(45.0, float(width)/float(height), 0.1, 100.0)
        glMatrixMode(GL_MODELVIEW)

    #定義自己的繪圖函數
    def Draw(self):
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        glLoadIdentity()
        #平移
        glTranslatef(-1.5, 2.0, -8.0)                
        #繪製三維圖形，三維線條
        glBegin(GL_LINES)
        #設定頂點顏色
        glColor3f(1.0, 0.0, 0.0)
        #設定頂點法線
        glNormal3f(1.0, 1.0, 1.0)
        glVertex3f(1.0, 1.0, -1.0)
        glColor3f(0.0, 1.0, 0.0)
        glNormal3f(-1.0, -1.0, -1.0)
        glVertex3f(-1.0, -1.0, 3.0)
        glEnd()

        #球
        glColor3f(0.8, 0.3, 1.0)
        glTranslatef(0, -1.5, 0)
        #第一個參數是球的半徑，後面兩個參數是分段數
        glutSolidSphere(1.0,40,40)
        
        glutSwapBuffers()

    #訊息主迴圈
    def MainLoop(self):
        glutMainLoop()

if __name__ == '__main__':
    #產生實體視窗物件，執行程式，啟動訊息主迴圈
    w = MyPyOpenGLTest()
    w.MainLoop()

```
# 範例程式:12-04.py
```
from PIL import Image
import os

def searchLeft(width, height, im):
    for w in range(width):		#由左向右掃描
        for h in range(height):	#由下向上掃描
            color = im.getpixel((w, h))	#取得圖形指定位置的像素顏色
            if color != (255, 255, 255):
                return w			#返回橢圓邊界最左端的x坐標

def searchRight(width, height, im):
    for w in range(width-1, -1, -1):	#由右向左掃描
        for h in range(height):
            color = im.getpixel((w, h))
            if color != (255, 255, 255):
                return w 			#返回橢圓邊界最右端的x坐標
            
def searchTop(width, height, im):
    for h in range(height-1, -1, -1):
        for w in range(width):
            color = im.getpixel((w,h))
            if color != (255, 255, 255):
                return h			#返回橢圓邊界最上端的y坐標

def searchBottom(width, height, im):
    for h in range(height):
        for w in range(width):
            color = im.getpixel((w,h))
            if color != (255, 255, 255):
                return h			#返回橢圓邊界最下端的y坐標

#巡訪指定資料夾中所有的bmp圖形檔，假設圖形為白色背景，橢圓為其他任意顏色
images = [f for f in os.listdir('testimages') if f.endswith('.bmp')]
for f in images:
    f = 'testimages\\'+f
    im = Image.open(f)
    width, height = im.size		#取得圖形大小
    x0, x1 = searchLeft(width, height, im), searchRight(width, height, im)
    y0, y1 = searchBottom(width, height, im), searchTop(width, height, im)
    center = ((x0+x1)//2, (y0+y1)//2)
    im.putpixel(center, (255,0,0))	#把橢圓中心像素畫成紅色
    im.save(f[0:-4]+'_center.bmp')	#儲存為新圖形檔
im.close()

```
# 範例程式:12-05.py
```
from PIL import Image, ImageDraw, ImageFont

def redraw(f, v1, v2):
    start = int(600*v1)
    end = int(600*v2)    
    im = Image.open(f)
    for w in range(start):			#繪製紅色區域
        for h in range(36, 61):			#具體數值需根據圖形大小進行調整
            im.putpixel((w,h), (255, 0, 0))
    for w in range(start, end):		#繪製綠色區域
        for h in range(36, 61):
            im.putpixel((w,h), (0, 255, 0))
    for w in range(end, 600):			#繪製品紅色區域
        for h in range(36, 61):
            im.putpixel((w,h), (255, 0, 255))
    draw = ImageDraw.Draw(im)
    font = ImageFont.truetype('simsun.ttc', 18)
    draw.text((start//2,38), 'A', (0,0,0), font=font)	#在各自區域內居中顯示字母
    draw.text(((end-start)//2+start,38), 'B',(0,0,0), font=font)
    draw.text(((600-end)//2+end,38), 'C', (0,0,0), font=font)
    im.save(f)						#保存圖片

redraw(r'd:\biaotou1.png', 0.1, 0.9)

```
# 範例程式:12-06.py
```
from PIL import Image, ImageDraw, ImageFont
import random
import string

#所有可能的字元，主要是英文字母和數字
characters = string.ascii_letters+string.digits

#取得指定長度的字串
def selectedCharacters(length):
    '''length:the number of characters to show'''
    result = ""
    for i in range(length):
        result += random.choice(characters)
    return result

def getColor():
    '''get a random color'''
    r = random.randint(0,255)
    g = random.randint(0,255)
    b = random.randint(0,255)
    return (r,g,b)

def main(size=(200,100), characterNumber=6, bgcolor=(255,255,255)):
    imageTemp = Image.new('RGB', size, bgcolor)
    #設定字體和字型大小
    font = ImageFont.truetype('c:\\windows\\fonts\\TIMESBD.TTF', 48)
    draw = ImageDraw.Draw(imageTemp)
    text = selectedCharacters(characterNumber)
    width, height = draw.textsize(text, font)
    #繪製驗證碼字串
    offset = 2
    for i in range(characterNumber):
        offset += width//characterNumber
        position = (offset, (size[1]-height)//2+random.randint(-10,10))
        draw.text(xy=position, text=text[i], font=font, fill=getColor())    
    #對驗證碼圖片進行基礎變換，這裡採用簡單的點運算
    imageFinal = Image.new('RGB', size, bgcolor)
    pixelsFinal = imageFinal.load()
    pixelsTemp = imageTemp.load()
    for y in range(0, size[1]):
        offset = random.randint(-1,1)
        for x in range(0, size[0]):
            newx = x+offset
            if newx>=size[0]:
                newx = size[0]-1
            elif newx<0:
                newx = 0
            pixelsFinal[newx,y] = pixelsTemp[x,y]
    draw = ImageDraw.Draw(imageFinal)
    #繪製干擾噪點像素
    for i in range(int(size[0]*size[1]*0.07)):
        draw.point((random.randint(0,size[0]), random.randint(0,size[1])), fill=getColor())
    #繪製干擾線條
    for i in range(8):
        start = (0, random.randint(0, size[1]-1))
        end = (size[0], random.randint(0, size[1]-1))
        draw.line([start, end], fill=getColor(), width=1)
    #繪製干擾弧線
    for i in range(8):
        start = (-50, -50)
        end = (size[0]+10, random.randint(0, size[1]+10))
        draw.arc(start+end, 0, 360, fill=getColor())
    #儲存驗證碼圖片
    imageFinal.save("result.jpg")
    imageFinal.show()

if __name__=="__main__":
    main((200,100), 8, (255,255,255))

```
# 範例程式:12-07.py
```
from PIL import Image
import os

gifFileName = 'bike.gif'
#以Image模組的open()方法開啟gif動態圖形時，預設是第一幀
im = Image.open(gifFileName)
pngDir = gifFileName[:-4]
#建立存放每幀圖片的資料夾
os.mkdir(pngDir)

try:
    while True:
        #儲存目前幀圖片
        current = im.tell()
        im.save(pngDir+'\\'+str(current)+'.png')
        #取得下一幀圖片
        im.seek(current+1)
except EOFError:
    pass

```
# 範例程式:12-08.py
```
from PIL import Image
from math import floor

def textureMap(srcTextureFile, dstSurfaceFile, dstWidth, dstHeight):
    '''srcTextureFile:原始圖片
       dstSurfaceFile:模擬目標物體表面
       dstWidth:目標物體表面寬度
       dstHeight:目標物體表面高度
    #開啟原始圖片
    srcTexture = Image.open(srcTextureFile)
    #建立指定尺寸的目標物體表面
    dstSurface = Image.new('RGBA', (dstWidth, dstHeight))
    srcWidth, srcHeight = srcTexture.size
    #根據目標物體表面尺寸，計算並取得原始圖片中對應位置的像素值
    for w in range(dstWidth):
        for h in range(dstHeight):
            x, y = floor(w/dstWidth*srcWidth), floor(h/dstHeight*srcHeight)
            dstSurface.putpixel((w,h), srcTexture.getpixel((x,y)))
    dstSurface.save(dstSurfaceFile)
    dstSurface.close()
    srcTexture.close()
    #也可以嘗試下面的寫法，更簡單一些
    '''
    srcTexture = Image.open(srcTextureFile)
    srcTexture = srcTexture.resize((dstWidth,dstHeight))
    srcTexture.save(dstSurfaceFile)
    srcTexture.close()


#測試
textureMap('sample.jpg', r'new.jpg', 200, 250)

```
# 範例程式:12-09.py
```
from random import randint
from PIL import Image

#根據原始24位元BMP影像檔，產生指定數量含有隨機噪點的臨時圖形
def addNoise(fileName, num):
    if not fileName.endswith('.bmp'):
        print('Must be bmp image')
        return
    for i in range(num):
        im = Image.open(fileName)
        width, height = im.size
        n = randint(1, 20)
        for j in range(n):
            w = randint(0, width-1)
            h = randint(0, height-1)
            im.putpixel((w,h), (0,0,0))
        im.save(fileName[:-4]+'_'+str(i+1)+'.bmp')

#根據多個含有隨機噪點的圖形，對應位置像素計算平均值，以產生結果圖形
def mergeOne(fileName, num):
    if not fileName.endswith('.bmp'):
        print('Must be bmp image')
        return
    ims = [Image.open(fileName[:-4]+'_'+str(i+1)+'.bmp') for i in range(num)]
    im = Image.new('RGB', ims[0].size, (255,255,255))
    for w in range(im.size[0]):
        for h in range(im.size[1]):
            r, g, b = [0]*3
            for tempIm in ims:
                value = tempIm.getpixel((w,h))
                r += value[0]
                g += value[1]
                b += value[2]
            r = r//num
            g = g//num
            b = b//num
            im.putpixel((w,h), (r,g,b))
    im.save(fileName[:-4]+'_result.bmp')

#對比合併後的圖形和原始圖形之間的相似度
def compare(fileName):
    im1 = Image.open(fileName)
    im2 = Image.open(fileName[:-4]+'_result.bmp')
    width, height = im1.size
    total = width * height
    right = 0
    expectedRatio = 0.05
    for w in range(width):
        for h in range(height):
            r1, g1, b1 = im1.getpixel((w,h))
            r2, g2, b2 = im2.getpixel((w,h))
            if (abs(r1-r2),abs(g1-g2),abs(b1-b2)) < (255*expectedRatio,)*3:
                right += 1
    return (total, right)

if __name__ == '__main__':
    #產生32個臨時圖形後進行融合，並對比融合後的圖形與原始圖形的相似度
    addNoise('test.bmp', 32)
    mergeOne('test.bmp', 32)
    result = compare('test.bmp')
    print('Total number of pixels:{0[0]},right number:{0[1]}'.format(result))

```
# 範例程式:12-10.py
```
from PIL import Image

def qipan(fileName, width, height, color1, color2):
    #產生空白圖形
    im = Image.new('RGB',(width,height))
    for h in range(height):
        for w in range(width):
            #填充顏色交叉的圖案
            if (int(h/height*8)+int(w/width*8)) % 2 == 0:
                im.putpixel((w,h), color1)
            else:
                im.putpixel((w,h), color2)
    #儲存圖形檔
    im.save(fileName)

if __name__=='__main__':
    fileName = 'qipan.jpg'
    qipan(fileName, 500, 500, (128,128,128), (10,10,10))

```
# 範例程式:13_02_03.py
```
import numpy as np
from scipy import signal, misc
import matplotlib.pyplot as plt
#need to pip install matplotlib first

image = misc.ascent()		#二維圖形陣列，lena圖形
w = np.zeros((50, 50))		#全0二維陣列，摺積核
w[0][0] = 1.0				#修改參數，調整濾波器
w[49][25] = 1.0				#可以根據需求調整
image_new = signal.fftconvolve(image, w)	#使用FFT演算法進行摺積

plt.figure()
plt.imshow(image_new)		#顯示濾波後的圖形
plt.gray()
plt.title('Filtered image')
plt.show()

```
# 範例程式:13_02_03_2.py
```
import numpy as np
from scipy import signal, misc
import matplotlib.pyplot as plt
#need to pip install matplotlib first

image = misc.ascent()
w = signal.gaussian(50, 10.0)
image_new = signal.sepfir2d(image, w, w)

plt.figure()
plt.imshow(image_new)		#顯示濾波後的圖形
plt.gray()
plt.title('Filtered image')
plt.show()

```
# 範例程式:13_05_01.py
```
import numpy as np
import pylab as pl

t = np.arange(0.0, 2.0*np.pi, 0.01)	#產生陣列，0到2π之間，以0.01為步長
s = np.sin(t)			#對陣列的所有元素求正弦值，得到新陣列
pl.plot(t,s)			#畫圖，以t為橫坐標，s為縱坐標
pl.xlabel('x')			#設定坐標軸標籤
pl.ylabel('y')
pl.title('sin')		#設定圖形標題
pl.show()				#顯示圖形

```
# 範例程式:13_05_02.py
```
import numpy as np
import pylab as pl

a = np.arange(0, 2.0*np.pi, 0.1)
b = np.cos(a)
pl.scatter(a,b)			#繪製散點圖
pl.show()

```
# 範例程式:13_05_02_2.py
```
import matplotlib.pylab as pl
import numpy as np

x = np.random.random(100)
y = np.random.random(100)
pl.scatter(x,y,s=x*500,c=u'r',marker=u'*')	#s指大小，c指顏色，marker指符號形狀
pl.show()

```
# 範例程式:13_05_03.py
```
import numpy as np
import matplotlib.pyplot as plt

#The slices will be ordered and plotted counter-clockwise.
labels = 'Frogs', 'Hogs', 'Dogs', 'Logs'
sizes = [15, 30, 45, 10]
colors = ['yellowgreen', 'gold', '#FF0000', 'lightcoral']
explode = (0, 0.1, 0, 0.1)			#使餅狀圖的第2片和第4片裂開

fig = plt.figure()
ax = fig.gca()
ax.pie(np.random.random(4), explode=explode, labels=labels, colors=colors,
       autopct='%1.1f%%', shadow=True, startangle=90,
       radius=0.25, center=(0, 0), frame=True)
ax.pie(np.random.random(4), explode=explode, labels=labels, colors=colors,
       autopct='%1.1f%%', shadow=True, startangle=90,
       radius=0.25, center=(1, 1), frame=True)
ax.pie(np.random.random(4), explode=explode, labels=labels, colors=colors,
       autopct='%1.1f%%', shadow=True, startangle=90,
       radius=0.25, center=(0, 1), frame=True)
ax.pie(np.random.random(4), explode=explode, labels=labels, colors=colors,
       autopct='%1.1f%%', shadow=True, startangle=90,
       radius=0.25, center=(1, 0), frame=True)
ax.set_xticks([0, 1])				#設定坐標軸刻度
ax.set_yticks([0, 1])
ax.set_xticklabels(["Sunny", "Cloudy"])	#設定坐標軸刻度上顯示的標籤
ax.set_yticklabels(["Dry", "Rainy"])
ax.set_xlim((-0.5, 1.5))				#設定坐標軸跨度
ax.set_ylim((-0.5, 1.5))
#Set aspect ratio to be equal so that pie is drawn as a circle.
ax.set_aspect('equal')

plt.show()

```
# 範例程式:13_05_04.py
```
import numpy as np
import pylab as pl
import matplotlib.font_manager as fm

myfont = fm.FontProperties(fname=r'C:\Windows\Fonts\msjh.ttf') #設定字體
t = np.arange(0.0, 2.0*np.pi, 0.01)	#引數取值範圍
s = np.sin(t)						#計算正弦函數值
z = np.cos(t)						#計算餘弦函數值
pl.plot(t, s, label='正弦')
pl.plot(t, z, label='餘弦')
pl.xlabel('x-變數', fontproperties=myfont, fontsize=16)		#設定x標籤
pl.ylabel('y-正弦餘弦函數值', fontproperties=myfont, fontsize=16)
pl.title('sin-cos函數圖形', fontproperties=myfont, fontsize=24) #圖形標題
pl.legend(prop=myfont)				#設定圖例
pl.show()

```
# 範例程式:13_05_05.py
```
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 2*np.pi, 500)
y = np.sin(x)
z = np.cos(x*x)
plt.figure(figsize=(8,5))
#標籤前後加上$，代表以內嵌的LaTex引擎顯示為公式
plt.plot(x,y,label='$sin(x)$',color='red',linewidth=2)	#紅色，2個像素寬
plt.plot(x,z,'b--',label='$cos(x^2)$')		#藍色，虛線
plt.xlabel('Time(s)')
plt.ylabel('Volt')
plt.title('Sin and Cos figure using pyplot')
plt.ylim(-1.2,1.2)
plt.legend()							#顯示圖例
plt.show()

```
# 範例程式:13_05_06.py
```
import numpy as np
import matplotlib.pyplot as plt

x= np.linspace(0, 2*np.pi, 500)	#建立引數陣列
y1 = np.sin(x)					#建立函數值陣列
y2 = np.cos(x)
y3 = np.sin(x*x)
plt.figure(1)					#建立圖形
#create three axes
ax1 = plt.subplot(2,2,1)			#第一列第一行圖形
ax2 = plt.subplot(2,2,2)			#第一列第二行圖形
ax3 = plt.subplot(2,1,2)			#第二列
plt.sca(ax1)					#選擇ax1
plt.plot(x,y1,color='red')		#繪製紅色曲線
plt.ylim(-1.2,1.2)				#限制y坐標軸範圍
plt.sca(ax2)					#選擇ax2
plt.plot(x,y2,'b--')			#繪製藍色曲線
plt.ylim(-1.2,1.2)
plt.sca(ax3)					#選擇ax3
plt.plot(x,y3,'g--')
plt.ylim(-1.2,1.2)
plt.show()

```
# 範例程式:13_05_07.py
```
import matplotlib as mpl
from mpl_toolkits.mplot3d import Axes3D
import numpy as np
import matplotlib.pyplot as plt

mpl.rcParams['legend.fontsize'] = 10	#圖例字型大小
fig = plt.figure()
ax = fig.gca(projection='3d')			#三維圖形
theta = np.linspace(-4 * np.pi, 4 * np.pi, 100)
z = np.linspace(-4, 4, 100)*0.3		#測試資料
r = z**3 + 1
x = r * np.sin(theta)
y = r * np.cos(theta)
ax.plot(x, y, z, label='parametric curve')
ax.legend()
plt.show()

```
# 範例程式:13_05_08.py
```
import numpy as np
import matplotlib.pyplot as plt
import mpl_toolkits.mplot3d

x,y = np.mgrid[-2:2:20j, -2:2:20j]
z = 50 * np.sin(x+y)		#測試資料
ax = plt.subplot(111, projection='3d')	#三維圖形
ax.plot_surface(x,y,z,rstride=2, cstride=1, cmap=plt.cm.Blues_r)
ax.set_xlabel('X')			#設定坐標軸標籤
ax.set_ylabel('Y')
ax.set_zlabel('Z')
plt.show()

```
# 範例程式:13_05_08_2.py
```
import pylab as pl
import numpy as np
import mpl_toolkits.mplot3d

rho, theta = np.mgrid[0:1:40j, 0:2*np.pi:40j]
z = rho**2
x = rho*np.cos(theta)
y = rho*np.sin(theta)
ax = pl.subplot(111, projection='3d')
ax.plot_surface(x,y,z)
pl.show()

```
# 範例程式:13_05_09.py
```
from matplotlib.path import Path
from matplotlib.patches import PathPatch
import matplotlib.pyplot as plt


fig, ax = plt.subplots()

#定義繪圖指令與控制點坐標
#其中MOVETO表示將繪製起點移動到指定坐標
#CURVE4表示使用4個控制點繪製3次貝茲曲線
#CURVE3表示使用3個控制點繪製2次貝茲曲線
#LINETO表示從目前位置繪製直線到指定位置
#CLOSEPOLY表示從目前位置繪製直線到指定位置，並閉合多邊形
path_data = [
    (Path.MOVETO, (1.58, -2.57)),
    (Path.CURVE4, (0.35, -1.1)),
    (Path.CURVE4, (-1.75, 2.0)),
    (Path.CURVE4, (0.375, 2.0)),
    (Path.LINETO, (0.85, 1.15)),
    (Path.CURVE4, (2.2, 3.2)),
    (Path.CURVE4, (3, 0.05)),
    (Path.CURVE4, (2.0, -0.5)),
    (Path.CURVE3, (3.5, -1.8)),
    (Path.CURVE3, (2, -2)),
    (Path.CLOSEPOLY, (1.58, -2.57)),
    ]
codes, verts = zip(*path_data)
path = Path(verts, codes)
#按照指令和坐標進行繪圖
patch = PathPatch(path, facecolor='r', alpha=0.9)
ax.add_patch(patch)

# 繪製控制多邊形和連接點
x, y = zip(*path.vertices)
line, = ax.plot(x, y, 'go-')

#顯示網格
ax.grid()
#設定坐標軸刻度大小一致，可以更真實地顯示圖形
ax.axis('equal')
plt.show()

```
# 範例程式:13_05_10.py
```
import sys
import tkinter as Tk
import matplotlib
from numpy import arange, sin, pi
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg, NavigationToolbar2TkAgg
from matplotlib.backend_bases import key_press_handler
from matplotlib.figure import Figure

matplotlib.use('TkAgg')

root = Tk.Tk()
root.title("matplotlib in TK")
#設定圖形尺寸與品質
f = Figure(figsize=(5, 4), dpi=100)
a = f.add_subplot(111)
t = arange(0.0, 3, 0.01)
s = sin(2*pi*t)
#繪製圖形
a.plot(t, s)


#把繪製的圖形顯示到tkinter視窗
canvas = FigureCanvasTkAgg(f, master=root)
canvas.show()
canvas.get_tk_widget().pack(side=Tk.TOP, fill=Tk.BOTH, expand=1)
#把matplotlib繪製圖形的工具列顯示於tkinter視窗
toolbar = NavigationToolbar2TkAgg(canvas, root)
toolbar.update()
canvas._tkcanvas.pack(side=Tk.TOP, fill=Tk.BOTH, expand=1)

#定義並繫結鍵盤事件處理函數
def on_key_event(event):
    print('you pressed %s' % event.key)
    key_press_handler(event, canvas, toolbar)
canvas.mpl_connect('key_press_event', on_key_event)

#按一下事件處理函數
def _quit():
    #結束事件主迴圈，並銷毀應用程式視窗
    root.quit()
    root.destroy()
button = Tk.Button(master=root, text='Quit', command=_quit)
button.pack(side=Tk.BOTTOM)

Tk.mainloop()

```
# 範例程式:13_05_11.py
```
from random import choice
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.widgets import RadioButtons, Button

t = np.arange(0.0, 2.0, 0.01)
s0 = np.sin(2*np.pi*t)
s1 = np.sin(4*np.pi*t)
s2 = np.sin(8*np.pi*t)

fig, ax = plt.subplots()
l, = ax.plot(t, s0, lw=2, color='red')
plt.subplots_adjust(left=0.3)

#定義允許的幾種頻率，並建立單選鈕元件
#其中[0.05, 0.7, 0.15, 0.15]表示元件在視窗的歸一化位置和大小
axcolor = 'lightgoldenrodyellow'
rax = plt.axes([0.05, 0.7, 0.15, 0.15], axisbg=axcolor)
radio = RadioButtons(rax, ('2 Hz', '4 Hz', '8 Hz'))
hzdict = {'2 Hz': s0, '4 Hz': s1, '8 Hz': s2}
def hzfunc(label):
    ydata = hzdict[label]
    l.set_ydata(ydata)
    plt.draw()
radio.on_clicked(hzfunc)

#定義允許的幾種顏色，並建立單選鈕元件
rax = plt.axes([0.05, 0.4, 0.15, 0.15], axisbg=axcolor)
colors = ('red', 'blue', 'green')
radio2 = RadioButtons(rax, colors)
def colorfunc(label):
    l.set_color(label)
    plt.draw()
radio2.on_clicked(colorfunc)

#定義允許的幾種線型，並建立單選鈕元件
rax = plt.axes([0.05, 0.1, 0.15, 0.15], axisbg=axcolor)
styles = ('-', '--', '-.', 'steps', ':')
radio3 = RadioButtons(rax, styles)
def stylefunc(label):
    l.set_linestyle(label)
    plt.draw()
radio3.on_clicked(stylefunc)

#定義按鈕按一下事件處理函數，並在視窗上建立按鈕
def randomFig(event):
    #隨機選擇一個頻率，同時設定單選鈕的選中項
    hz = choice(tuple(hzdict.keys()))
    hzLabels = [label.get_text() for label in radio.labels]
    radio.set_active(hzLabels.index(hz))
    l.set_ydata(hzdict[hz])
    #隨機選擇一個顏色，同時設定單選鈕的選中項
    c = choice(colors)
    radio2.set_active(colors.index(c))
    l.set_color(c)
    #隨機選擇一個線型，同時設定單選鈕的選中項
    style = choice(styles)
    radio3.set_active(styles.index(style))
    l.set_linestyle(style)
    #根據設定的屬性繪製圖形
    plt.draw()
axRnd = plt.axes([0.5, 0.015, 0.2, 0.045])
buttonRnd = Button(axRnd, 'Random Figure')
buttonRnd.on_clicked(randomFig)
#顯示圖形
plt.show()

```
# 範例程式:13_05_12.py
```
from time import sleep
from threading import Thread
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.widgets import Button

fig, ax = plt.subplots()
#設定圖形顯示位置
plt.subplots_adjust(bottom=0.2)
#實驗資料
range_start, range_end, range_step = 0, 1, 0.005
t = np.arange(range_start, range_end, range_step)
s = np.sin(4*np.pi*t)
l, = plt.plot(t, s, lw=2)
#自訂類別，用來封裝兩個按鈕的按一下事件處理函數
class ButtonHandler:
    def __init__(self):
        self.flag = True
        self.range_s, self.range_e, self.range_step = 0, 1, 0.005
    #執行緒函數，用來更新資料並重新繪製圖形
    def threadStart(self):
        while self.flag:
            sleep(0.02)
            self.range_s += self.range_step
            self.range_e += self.range_step
            t = np.arange(self.range_s, self.range_e, self.range_step)
            ydata = np.sin(4*np.pi*t)
            #更新資料
            l.set_xdata(t-t[0])
            l.set_ydata(ydata)
            #重新繪製圖形
            plt.draw()
    def Start(self, event):
        self.flag = True
        #建立並啟動新執行緒
        t = Thread(target=self.threadStart)
        t.start()
    def Stop(self, event):
        self.flag = False
        
callback = ButtonHandler()
#建立按鈕，並設定按一下事件處理函數
axprev = plt.axes([0.81, 0.05, 0.1, 0.075])
bprev = Button(axprev, 'Stop')
bprev.on_clicked(callback.Stop)
axnext = plt.axes([0.7, 0.05, 0.1, 0.075])
bnext = Button(axnext, 'Start')
bnext.on_clicked(callback.Start)

plt.show()

```
# 範例程式:13_05_13.py
```
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.widgets import Slider, Button, RadioButtons

fig, ax = plt.subplots()
#設定繪圖區域位置
plt.subplots_adjust(left=0.1, bottom=0.25)
t = np.arange(0.0, 1.0, 0.001)
#初始振幅與頻率，並繪製初始圖形
a0, f0= 5, 3
s = a0*np.sin(2*np.pi*f0*t)
l, = plt.plot(t, s, lw=2, color='red')
#設定坐標軸刻度範圍
plt.axis([0, 1, -10, 10])

axColor = 'lightgoldenrodyellow'
#建立兩個Slider元件，分別設定位置/尺寸、背景色和初始值
axfreq = plt.axes([0.1, 0.1, 0.75, 0.03], axisbg=axColor)
sfreq = Slider(axfreq, 'Freq', 0.1, 30.0, valinit=f0)
axamp = plt.axes([0.1, 0.15, 0.75, 0.03], axisbg=axColor)
samp = Slider(axamp, 'Amp', 0.1, 10.0, valinit=a0)
#為Slider元件設定事件處理函數
def update(event):
    #取得Slider元件的目前值，並以此更新圖形
    amp = samp.val
    freq = sfreq.val
    l.set_ydata(amp*np.sin(2*np.pi*freq*t))
    plt.draw()
    #fig.canvas.draw_idle()
sfreq.on_changed(update)
samp.on_changed(update)

#建立Adjust按鈕，設定大小、位置和事件處理函數
def adjustSliderValue(event):
    ampValue = samp.val + 0.05
    if ampValue > 10:
        ampValue = 0.1
    samp.set_val(ampValue)

    freqValue = sfreq.val + 0.05
    if freqValue > 30:
        freqValue = 0.1
    sfreq.set_val(freqValue)
    update(event)
axAdjust = plt.axes([0.6, 0.025, 0.1, 0.04])
buttonAdjust = Button(axAdjust, 'Adjust', color=axColor, hovercolor='red')
buttonAdjust.on_clicked(adjustSliderValue)

#建立按鈕元件，以便恢復初始值
resetax = plt.axes([0.8, 0.025, 0.1, 0.04])
button = Button(resetax, 'Reset', color=axColor, hovercolor='yellow')
def reset(event):
    sfreq.reset()
    samp.reset()
button.on_clicked(reset)

plt.show()

```
# 範例程式:14-01.py
```
def KaiSaEncrypt(ch, k):
    if (not isinstance(ch, str)) or len(ch)!=1:
        print('The first parameter must be a character')
        return
    if (not isinstance(k, int)) or (not 1<=k<=25):
        print('The second parameter must be an integer between 1 and 25')
        return
    #把英文字母轉換為後面第k個字母
    if 'a'<=ch<=chr(ord('z')-k):
        return chr(ord(ch)+k)
    #把英文字母首尾相連
    elif chr(ord('z')-k)<ch<='z':
        return chr((ord(ch)-ord('a')+k)%26+ord('a'))
    elif 'A'<=ch<=chr(ord('Z')-k):
        return chr(ord(ch)+k)
    elif chr(ord('Z')-k)<ch<='Z':
        return chr((ord(ch)-ord('A')+k)%26+ord('A'))
    else:
        return ch

def encrypt(plain, k):
    return ''.join([KaiSaEncrypt(ch, k) for ch in plain])

def KaiSaDecrypt(ch, k):
    if (not isinstance(ch, str)) or len(ch)!=1:
        print('The first parameter must be a character')
        return
    if (not isinstance(k, int)) or (not 1<=k<=25):
        print('The second parameter must be an integer between 1 and 25')
        return
    #把英文字母首尾相連，然後將每個字母轉換為前面第k個字母
    if chr(ord('a')+k)<=ch<='z':
        return chr(ord(ch)-k)
    elif 'a'<=ch<chr(ord('a')+k):
        return chr((ord(ch)-k+26))
    elif chr(ord('A')+k)<=ch<='Z':
        return chr(ord(ch)-k)
    elif 'A'<=ch<chr(ord('A')+k):
        return chr((ord(ch)-k+26))
    else:
        return ch

def decrypt(plain, k):
    return ''.join([KaiSaDecrypt(ch, k) for ch in plain])

plainText = 'Explicit is better than implicit.'
cipherText = encrypt(plainText, 5)
print(plainText)
print(cipherText)
print(decrypt(cipherText,5))

```
# 範例程式:14-02.py
```
from string import ascii_uppercase as uppercase
from itertools import cycle

#建立密碼表
table = dict()
for ch in uppercase:
    index = uppercase.index(ch)
    table[ch] = uppercase[index:]+uppercase[:index]

#建立解密密碼表
deTable = {'A':'A'}
start = 'Z'
for ch in uppercase[1:]:
    index = uppercase.index(ch)
    deTable[ch] = chr(ord(start)+1-index)

#解密金鑰
def deKey(key):
    return ''.join([deTable[i] for i in key])

#加密/解密
def encrypt(plainText, key):
    result = []
    #建立cycle物件，支援金鑰字母的循環使用
    currentKey = cycle(key)
    for ch in plainText:
        if 'A'<=ch<='Z':
            index = uppercase.index(ch)
            #取得金鑰字母
            ck = next(currentKey)
            result.append(table[ck][index])
        else:
            result.append(ch)
    return ''.join(result)

key = 'DONGFUGUO'
p = 'PYTHON 3.5.1 PYTHON 2.7.11'
c = encrypt(p, key)
print(p)
print(c)
print(encrypt(c,deKey(key)))

```
# 範例程式:14-03.py
```
def encrypt(plainText, t):
    result = []
    length = len(t)
    #將明文分組
    temp = [plainText[i:i+length] for i in range(0,len(plainText),length)]
    #除最後一組之外的其他組進行換位處理
    for item in temp[:-1]:
        newItem = ''
        for i in t:
            newItem = newItem+item[i-1]
        result.append(newItem)
    return ''.join(result)+temp[-1]

p = 'Errors should never pass silently.'
#加密
c = encrypt(p, (1, 4, 3, 2))
print(c)
#解密
print(encrypt(c, (1, 4, 3, 2)))

```
# 範例程式:14-04_2.py
```
from hashlib import md5
from string import ascii_letters, digits
from itertools import permutations
from time import time

all_letters = ascii_letters + digits + '.,;'		#候選字元集

def decrypt_md5(md5_value):
    if len(md5_value) != 32:					#破解32位MD5值
        print('error')
        return
    
    md5_value = md5_value.lower()				#轉換為小寫MD5值
    
    for k in range(5,10):						#預期的密碼長度
        for item in permutations(all_letters, k):	#暴力測試
            item = ''.join(item)
            print('.', end='')					#顯示進度
            if md5(item.encode()).hexdigest() == md5_value:	#破解成功
                return item

md5_value = 'e7d057704ea5206d8cb61280741238f5'
start = time()
result = decrypt_md5(md5_value)
if result:
    print('\nSuccess:  '+md5_value+'==>'+result)
print('Time used:', time()-start)

```
# 範例程式:14-05.py
```
import string
import random
from Crypto.Cipher import AES

#產生指定長度的金鑰
def keyGenerater(length):
    if length not in (16, 24, 32):
        return None
    x = string.ascii_letters+string.digits
    return ''.join([random.choice(x) for i in range(length)]) 

def encryptor_decryptor(key, mode):
    return AES.new(key, mode, b'0000000000000000')

#以指定金鑰和模式加密給定的資料
def AESencrypt(key, mode, text):
    encryptor = encryptor_decryptor(key, mode)
    return encryptor.encrypt(text)

#以指定金鑰和模式解密給定的資料
def AESdecrypt(key, mode, text):
    decryptor = encryptor_decryptor(key, mode)
    return decryptor.decrypt(text)

if __name__ == '__main__':
    text = '山東省煙臺市 Python3.5 is excellent.'
    key = keyGenerater(16)
    #隨機選擇AES的模式
    mode = random.choice((AES.MODE_CBC, AES.MODE_CFB, AES.MODE_ECB, AES.MODE_OFB))
    if not key:
        print('Something is wrong.')
    else:
        print('key:', key)
        print('mode:', mode)
        print('Before encryption:', text)
        #明文必須是位元組字串形式，且長度為16的倍數
        text_encoded = text.encode()
        text_length = len(text_encoded)
        padding_length = 16 - text_length%16
        text_encoded = text_encoded + b'0'*padding_length        
        text_encrypted = AESencrypt(key, mode, text_encoded)
        print('After encryption:', text_encrypted)
        text_decrypted =AESdecrypt(key, mode, text_encrypted)
        print('After decryption:', text_decrypted.decode()[:-padding_length])

```
# 範例程式:14-06.py
```
import rsa

key = rsa.newkeys(3000)		#產生隨機金鑰
privateKey = key[1]			#私密金鑰
publicKey = key[0]			#公開金鑰

message = '中國山東煙臺.Now is better than never.'
print('Before encrypted:',message)
message = message.encode()

cryptedMessage = rsa.encrypt(message, publicKey)
print('After encrypted:\n',cryptedMessage)

message = rsa.decrypt(cryptedMessage, privateKey)
message = message.decode()
print('After decrypted:',message)

```
# 範例程式:CheckMD5OfFile.py
```
import sys
import hashlib
import os.path

filename = sys.argv[1]
if os.path.isfile(filename):
    fp = open(filename, 'rb')
    contents = fp.read()
    fp.close()
    print(hashlib.md5(contents).hexdigest())
else:
    print('file not exists')

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
