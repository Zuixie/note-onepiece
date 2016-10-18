# Python 

## 升级版本
- [ubuntu安装多版本python](http://mbless.de/blog/2016/01/09/upgrade-to-python-2711-on-ubuntu-1404-lts.html) 
- [升级python](http://serverfault.com/questions/669859/how-can-i-upgrade-python-to-2-7-9-on-ubuntu-14-4)
- [hostname 'mysite.com' doesn't match either of '*.myhost.com', 'myhost.com'](https://stackoverflow.com/questions/18578439/using-requests-with-tls-doesnt-give-sni-support/18579484#18579484)

## 模块打包
核心在于```__init__.py```文件的作用
[关于modules的官方文档](https://docs.python.org/3.5/tutorial/modules.html)

1. ```__init__.py``` 的文件存在将会使得python以package对该该目录
2. 当 import 包时系统会通过sys.path中的路径进行搜索
3. 在```__init__.py```文件中，import该目录下的模块时
```
moudlea/
    __init__.py
    a.py
在 __init__.py 中 import moudlea.a 就可以在正式文件中通过import moudlea，然后使用 moudlea.a
同样也可以使用 from moudlea.a import funca ， 然后在调用处就可以使用moudlea.funca
``` 
4. import * from package
当在调用时使用```from moudlea import *```时，实际是import __init__.py文件中定义```__all__```变量
```
moudlea/
    __init__.py
    a.py
当在 __init__.py 文件中 写入 __all__ = ["a"] 就可以在from moudlea import * 后使用 a.funca()
还有一种写法是如下所示，__all__变量可以不用定义，同样可以直接使用a.funca()
import moudlea.a
from moudlea import * 
```

## metaclass
类也是对象，也是一种类，类定义了实例的行为，而metaclass定义了类的行为
[stackoverflow 对python metaclass的解释](http://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python)

## django
