[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Introduction.md) [下一章]()

# PyQt6日期和时间

*最近更新于2021年4月23日*

PyQt6教程的这一部分展示了如何在PyQt6中使用日期和时间。

## QDate，QTime，QDateTime

PyQt6有QDate、QDateTime、QTime类来处理日期和时间。QDate是一个用于处理公历中的日历日期的类。它具有确定日期、比较或操纵日期的方法。QTime类与时钟时间一起工作。它提供了比较时间、确定时间和各种其他时间操作方法的方法。QDateTime是一个类，它将QDate和QTime对象组合成一个对象。

## PyQt当前日期和时间

PyQt6有currentDate、currentTime和currentDateTime方法，用于确定当前日期和时间。

```python
# current_date_time.py
#!/usr/bin/python

from PyQt6.QtCore import QDate, QTime, QDateTime, Qt

now = QDate.currentDate()

print(now.toString(Qt.DateFormat.ISODate))
print(now.toString(Qt.DateFormat.RFC2822Date))

datetime = QDateTime.currentDateTime()

print(datetime.toString())

time = QTime.currentTime()
print(time.toString(Qt.DateFormat.ISODate))
```

该示例以各种格式打印当前日期、日期和时间以及时间。

```python
now = QDate.currentDate()
```

currentDate方法返回当前日期。

```python
print(now.toString(Qt.DateFormat.ISODate))
print(now.toString(Qt.DateFormat.RFC2822Date))
```

通过将值Qt.DateFormat.ISODate和Qt.DateFormat.RFC2822Date传递给toString方法，日期以两种不同的格式打印。

```python
datetime = QDateTime.currentDateTime()
```

currentDateTime返回当前日期和时间。

```python
time = QTime.currentTime()
```

最后，currentTime方法返回当前时间。

```
2021-06-12
12 Jun 2021
Sat Jun 12 22:32:55 2021
22:32:55
```

## PyQt6 UTC时间

我们的星球是一个球体；它绕轴旋转。地球自西向东转，所以太阳在不同的时间和地点升起。地球大约每24小时自转一次。因此，世界被划分为24个时区。在每个时区，都有不同的当地时间。这个当地时间通常会被夏令时进一步修改。

务实的需要是“一个全球时间”。一个全球时间有助于避免时区和夏令时的混淆。选择UTC (Universal Coordinated time)作为主要时间标准。UTC用于航空、天气预报、飞行计划、空中交通管制许可和地图。与当地时间不同，UTC不会随着季节的变化而变化。

```python
# utc_local.py
#!/usr/bin/python

from PyQt6.QtCore import QDateTime, Qt

now = QDateTime.currentDateTime()

print('Local datetime: ', now.toString(Qt.DateFormat.ISODate))
print('Universal datetime: ', now.toUTC().toString(Qt.DateFormat.ISODate))

print(f'The offset from UTC is: {now.offsetFromUtc()} seconds')
```

该示例确定当前世界和本地日期和时间。

```python
print('Local datetime: ', now.toString(Qt.DateFormat.ISODate))
```

currentDateTime方法返回表示为本地时间的当前日期和时间。我们可以使用toLocalTime将通用时间转换为本地时间。

```python
print('Universal datetime: ', now.toUTC().toString(Qt.DateFormat.ISODate))
```

我们使用toUTC方法从日期时间对象中获得世界时间。

```python
print(f'The offset from UTC is: {now.offsetFromUtc()} seconds')
```

offsetFromUtc给出了通用时间和本地时间之间的差值，以秒为单位。

```
Local datetime:  2021-06-12T22:51:45
Universal datetime:  2021-06-12T14:51:45Z
The offset from UTC is: 28800 seconds
```

## PyQt6天数

具体月份的天数由daysInMonth方法返回，一年中的天数由daysInYear方法返回。

```python
# n_of_days.py
#!/usr/bin/python

from PyQt6.QtCore import QDate

now = QDate.currentDate()

d = QDate(1945, 5, 7)

print(f'Days in month: {d.daysInMonth()}')
print(f'Days in year: {d.daysInYear()}')
```

该示例打印所选日期的每月和每年的天数。

```
Days in month: 31
Days in year: 365
```

## PyQt6的天数差值

方法返回从一个日期到另一个日期的天数。

```python
# xmas.py
#!/usr/bin/python

from PyQt6.QtCore import QDate, Qt

now = QDate.currentDate()
y = now.year()

print(f'today is {now.toString(Qt.DateFormat.ISODate)}')

xmas1 = QDate(y-1, 12, 25)
xmas2 = QDate(y, 12, 25)

dayspassed = xmas1.daysTo(now)
print(f'{dayspassed} days have passed since last XMas')

nofdays = now.daysTo(xmas2)
print(f'There are {nofdays} days until next XMas')
```

该示例计算从上一个圣诞节算起的天数和到下一个圣诞节的天数。

```
today is 2021-06-12
169 days have passed since last XMas
There are 196 days until next XMas
```

## PyQt6 datetime算法

我们经常需要在datetime值中添加或减去天、秒或年。

```python
# arithmetic.py
#!/usr/bin/python

from PyQt6.QtCore import QDateTime, Qt

now = QDateTime.currentDateTime()

print(f'Today: {now.toString(Qt.DateFormat.ISODate)}')
print(f'Adding 12 days: {now.addDays(12).toString(Qt.DateFormat.ISODate)}')
print(f'Subtracting 22 days: {now.addDays(-22).toString(Qt.DateFormat.ISODate)}')

print(f'Adding 50 seconds: {now.addSecs(50).toString(Qt.DateFormat.ISODate)}')
print(f'Adding 3 months: {now.addMonths(3).toString(Qt.DateFormat.ISODate)}')
print(f'Adding 12 years: {now.addYears(12).toString(Qt.DateFormat.ISODate)}')
```

该示例确定当前日期时间，并添加或减去日、秒、月和年。

```
Today: 2021-06-12T23:03:47
Adding 12 days: 2021-06-24T23:03:47
Subtracting 22 days: 2021-05-21T23:03:47
Adding 50 seconds: 2021-06-12T23:04:37
Adding 3 months: 2021-09-12T23:03:47
Adding 12 years: 2033-06-12T23:03:47
```

## PyQt6夏令时

夏令时（DST）是一种在夏季提前时钟的做法，这样晚上的日光就会持续更长时间。在立春时，时间向前调整一小时，在秋季时，时间向后调整为标准时间。

```python
# daylight_saving.py
#!/usr/bin/python

from PyQt6.QtCore import QDateTime, QTimeZone, Qt

now = QDateTime.currentDateTime()

print(f'Time zone: {now.timeZoneAbbreviation()}')

if now.isDaylightTime():
    print('The current date falls into DST time')
else:
    print('The current date does not fall into DST time')
```

示例检查datetime是否为夏时制。

```python
print(f'Time zone: {now.timeZoneAbbreviation()}')
```

timezoneacronym方法返回datetime的时区缩写。

```python
if now.isDaylightTime():
...
```

如果datetime是夏时制，则返回isDaylightTime。

```
Time zone: 中国标准时间
The current date does not fall into DST time
```

该项目在欧洲中部城市布拉迪斯拉发（Bratislava）的夏季执行。中欧夏季时间（CEST）比世界时间早2小时。这个时区是一个日光节约时区，在欧洲和南极洲使用。在冬季使用的标准时间是中欧时间（CET）。

## PyQt6 unix时间戳

epoch是被选择为某个特定时代起源的时间上的一个瞬间。例如，在西方基督教国家，时间纪元从耶稣诞生的第0天开始。另一个例子是法国共和历，它使用了12年。这一时期是共和时代的开始，这是在1792年9月22日宣布的，这一天宣布了第一共和国和废除君主制。

计算机也有自己的时代。其中最流行的是Unix时间戳。Unix时间戳是1970年1月1日00:00:00 UTC时间(或1970-01-01T00:00:00Z ISO 8601)。计算机中的日期和时间是根据自该计算机或平台的定义纪元以来所经过的秒数或时钟滴答数确定的。

Unix时间是自Unix纪元以来经过的秒数。

Unix date命令可用于获取Unix时间。现在距离Unix时代已经过去了1623511317秒。

```python
# unix_time.py
#!/usr/bin/python

from PyQt6.QtCore import QDateTime, Qt

now = QDateTime.currentDateTime()

unix_time = now.toSecsSinceEpoch() 
print(unix_time)

d = QDateTime.fromSecsSinceEpoch(unix_time)
print(d.toString(Qt.DateFormat.ISODate))
```

示例打印Unix时间并将其转换回QDateTime。

```python
now = QDateTime.currentDateTime()
```

首先，检索当前日期和时间。

```python
unix_time = now.toSecsSinceEpoch() 
```

toSecsSinceEpoch返回Unix时间。

```python
d = QDateTime.fromSecsSinceEpoch(unix_time)
```

使用fromSecsSinceEpoch，我们将Unix时间转换为QDateTime。

```
1623511317
2021-06-12T23:21:57
```

## PyQt6儒略日

儒略日是指自儒略时期开始的连续天数。它主要由天文学家使用。它不应该与儒略历混淆。儒略时期开始于公元前4713年。公元前4713年1月1日的中午开始，儒略历号为0。

儒略日数（JDN）是指从这个时期开始算起所经过的天数。任何时刻的儒略日（JD）是前一个中午的儒略日数加上自该时刻起当天的一段时间。（Qt不计算这一段时间。）除了天文学，儒略日期经常被军事和大型计算机程序使用。

```python
# julian_day.py
#!/usr/bin/python

from PyQt6.QtCore import QDate, Qt

now = QDate.currentDate()

print('Gregorian date for today:', now.toString(Qt.DateFormat.ISODate))
print('Julian day for today:', now.toJulianDay()) 
```

在这个例子中，我们计算今天的公历日和儒略日。

```python
print('Julian day for today:', now.toJulianDay())
```

儒略日通过toJulianDay()方法返回。

```
Gregorian date for today: 2021-06-12
Julian day for today: 2459378
```

## 历史战役

有了儒略日，就可以进行跨越几个世纪的计算。

```python
# battles.py
#!/usr/bin/python

from PyQt6.QtCore import QDate, Qt

borodino_battle = QDate(1812, 9, 7)
slavkov_battle = QDate(1805, 12, 2)

now = QDate.currentDate()

j_today = now.toJulianDay()
j_borodino = borodino_battle.toJulianDay()
j_slavkov = slavkov_battle.toJulianDay()

d1 = j_today - j_slavkov
d2 = j_today - j_borodino

print(f'Days since Slavkov battle: {d1}')
print(f'Days since Borodino battle: {d2}')
```

该示例计算自两个历史事件以来经过的天数。

```python
borodino_battle = QDate(1812, 9, 7)
slavkov_battle = QDate(1805, 12, 2)
```

我们有两个拿破仑时代的战役日期。

```python
j_today = now.toJulianDay()
j_borodino = borodino_battle.toJulianDay()
j_slavkov = slavkov_battle.toJulianDay()
```

我们计算今天和斯拉夫科夫战役和波罗底诺战役的儒略日。

```python
d1 = j_today - j_slavkov
d2 = j_today - j_borodino
```

我们计算了两场战役结束后的天数。

```
Days since Slavkov battle: 78720
Days since Borodino battle: 76249
```

当我们运行这个脚本时，从斯拉科夫战役到现在已经过去了78720天，从波罗底诺战役到现在已经过去了76249天。

在PyQt6教程的这一部分中，我们使用了日期和时间。
