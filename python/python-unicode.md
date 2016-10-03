# Python - Unicode (Python的编码问题)

## 关于 python 的字符集

[Unicode HOWTO-Python2](https://docs.python.org/2.7/howto/unicode.html#python-2-x-s-unicode-support)
[Unicode HOWTO-Python3](https://docs.python.org/3/howto/unicode.html)

### Unicode 和 utf-8
Unicode是一种规范，像是一个字典，规定了一个16进制的整数对应一个字符。utf-8通俗讲来说是unicode实现的一种方案。
例如unicode规定‘汉’对于的整数为0x6C49，使用utf-8对‘汉’进行存储和传输的时候，通过一定的算法将 0x6C49 转换为 E6 B1 89 进行存储。 

- ucs2, ucs4

    Unicode的学名是"Universal Multiple-Octet Coded Character Set"，简称为UCS。UCS可以看作是"Unicode Character Set"的缩写。
    - ucs2 :使用两个字节进行编码，就是使用一个16位二进制数表示一个unicode编码
    - ucs4 :使用四个字节进行编码

### Python2

- str 类型，表示字节
- unicode 类型，表示字符。关于字符和字节的关系就是，字节是字符的具体存储方式

```Python
# 简单的示例
>>> str('中')
'\xe4\xb8\xad' # 字节
>>> u'中' 
u'\u4e2d' # 字符
```

- encode(): 编码，将unicode转化为str
- decode(): 解码，将str转化为unicode
- 使用``` sys.setdefaultencoding("utf-8")```可以解决大部分因为编码出现错误的原理
```Python
# 1. python2 默认使用的编码格式是 ascii
>>> sys.getdefaultencoding()
'ascii'

# 2.1 str可以使用encode()方法，其实是系统会使用默认编码格式先进行 decode() 转化为 unicode 再使用 encode() 进行编码
s = '中'
s.encode('utf8') ==> s.decode(sys.getdefaultencoding()).encode('utf8')
## 意思是将'中'编码为'utf-8'标准的字节码，由于'中'的类型是本身就是字节码，系统会先使用默认的编码格式(ascii)将其先转化为unicode,
## 错误就在这个时候发生，因为在代码第二句设置# -*- coding: utf-8 -*-的情况下，就是将'\xe4\xb8\xad'这份数据使用ascii转为unicode，
## 但是ascii的范围是 0x0~0x7f，第一个数据0xe4明显超出范围所以就报错了，如果将ascii换为utf-8自然就不会报错了。

# 2.2 使用unicode()创建unicode对象时
u = unicode('中') ==> unicode('中', encoding=sys.getdefaultencoding()) 
## 意思就是将'中'的字节码转为uncicode字符码，原理同2.1的解释，就是在代码第二句设置# -*- coding: utf-8 -*-的情况下，就是将'\xe4\xb8\xad'这份数据使用ascii转为unicode
```

- 读取文件
```Python
# 1. open 函数
f = open('filename') # f.read() 返回是 str 

# 2. codecs.open
import codecs
f = codecs.open('unicode.rst', encoding='utf-8') # f.read() 返回的是 unicode

# 3. io.open
import io
f = io.open('unicode.rst', encoding='utf-8') # f.read() 返回的是 unicode
```

## Python3
Python3 重新设计了字符这一块，使用bytes类型表示字节，使用str表示字符，默认使用utf-8进行编码。
``` Python
>>> '中'
'中'
>>> '中'.encode('utf8')
b'\xe4\xb8\xad'
```