# time库用法解析
## 时间获取
* time.**time**() -> float
    * 返回系统时钟的浮点数，它是一个从初始纪元 `1970.1.1 00:00:00` 开始计时的**秒数**时间值。

    * 返回值 `time()` 可以作为参数传递给其他函数如gmtime、localtime等

```python
print(time.time())
# 1677164108.2010274
```

* time.**ctime**(seconds) -> string
    * 返回字符串形式表示的时间，其距离初始纪元seconds秒。格式为 `Thu Feb 23 22:31:45 2023`。
    * 等价于`asctime(localtime(seconds))`。
    * 若seconds=0，返回本地时间。

```python
print(time.ctime())
# Thu Feb 23 22:55:08 2023
```
* class time.**struct_time**
    * The time value as returned by `gmtime()`, `localtime()`, `strptime()`, and accepted by `asctime()`, `mktime()`,  `strftime()`. 
| 索引 | 属性 | 含义 | 值 |
| -- | -- | -- | -- |
| 0 | tm_year | 年 | 四位数 |
| 1 | tm_mon | 月 | 1-12 |
| 2 | tm_mday | 日 | 1-31 |
| 3 | tm_hour | 时 | 0-23 |
| 4 | tm_min | 分 | 0-59 |
| 5 | tm_sec | 秒 | 0-61 |
| 6 | tm_wday | 一周的第几日 | 0-6 |
| 7 | tm_yday | 一年的第几日 | 1-366 |
| 8 | tm_isdst | 是否夏令时 | 0、1、-1 |
| | tm_zone | 时区名称缩写 | |
| | tm_gmtoff | 以秒为单位的UTC以东偏离 | |
* time.**gmtime**([seconds]) -> (tm_year, tm_mon, tm_mday, tm_hour, tm_min, tm_sec, tm_wday, tm_yday, tm_isdst)
    * 转换为世界统一时间
    * 将从纪元开始的秒数转换为UTC格式的`struct_time`

* time.**localtime**([seconds]) -> (tm_year,tm_mon,tm_mday,tm_hour,tm_min,tm_sec,tm_wday,tm_yday,tm_isdst)
    * 转换为当地时间.

```python
print(time.gmtime())
print(time.localtime())

# time.struct_time(tm_year=2023, tm_mon=2, tm_mday=23, tm_hour=14, tm_min=55, tm_sec=8, tm_wday=3, tm_yday=54, tm_isdst=0)
# time.struct_time(tm_year=2023, tm_mon=2, tm_mday=23, tm_hour=22, tm_min=55, tm_sec=8, tm_wday=3, tm_yday=54, tm_isdst=0)
```

## 时间格式化输出
* time.**strftime**(format[, tuple]) -> string
    * 转换一个元组或 `struct_time` 表示的由`gmtime()` 或`localtime()` 返回的时间到由 format 参数指定的字符串。如果未提供 t ,则使用由localtime() 返回的当前时间。
    * format 必须是一个字符串。
        *  `%Y`： **带世纪的年份** Year with century as a decimal number.
        * `%m`：**月** Month as a decimal number [01,12].
        * `%d`：**日**  Day of the month as a decimal number [01,31].
        * `%H`：**24小时制**  Hour (24-hour clock) as a decimal number [00,23].
        * `%M`：**分钟**  Minute as a decimal number [00,59].
        * `%S`：**秒**  Second as a decimal number [00,61].
        * `%z`：**时区**  Time zone offset from UTC.
        * `%a`：**星期缩写**  Locale's abbreviated weekday name.
        * `%A`：**星期名称**  Locale's full weekday name.
        * `%b`：**月缩写**  Locale's abbreviated month name.
        * `%B`：**月名称**  Locale's full month name.
        * `%c`：**本地化恰当的日期表示**  Locale's appropriate date and time representation.
        * `%I`：**12小时制**  Hour (12-hour clock) as a decimal number [01,12].
        * `%p`：am或pm  Locale's equivalent of either AM or PM.

```python
t=time.gmtime()
print(time.strftime("时区：%z\n"
                    "本地化恰当时间表示%X\n"
                    "本地化恰当日期表示%x\n"
                    "本地化恰当日期时间表示%c\n"
                    "星期:%a %A\n"
                    "%Y-%m-%d %H:%M:%S\n"
                    ,t))
# 时区：+0800
# 本地化恰当时间表示01:40:08
# 本地化恰当日期表示02/24/23
# 本地化恰当日期时间表示Fri Feb 24 01:40:08 2023
# 星期:Fri Friday
# 2023-02-24 01:40:08

```

* time.**strptime**(string, format) -> struct_time
    * 将字符串转化为struct_time

```python
t="2021 5 12"
print(time.strptime(t,"%Y %m %d"))
# time.struct_time(tm_year=2021, tm_mon=5, tm_mday=12, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=2, tm_yday=132, tm_isdst=-1)

```

## 程序计时

* time.**sleep**(seconds)
    * 将调用它的线程沉睡seconds秒。

* time.**perf_counter**() -> float
    * 返回一个高精度的性能计数器的值，两次调用的差值才有意义。
```python
t1=time.perf_counter()
for i in range(1000):
    pass
t2=time.perf_counter()
print(t2-t1)
#3.3599999999994745e-05
```
