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

```
# 範例程式:04_01_02.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
# 範例程式:.py
```

```
