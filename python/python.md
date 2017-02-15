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

### 官方教程 part 1
> 创建工程：```django-admin startproject mysite```
>
> 运行server：```python manage.py runserver```这里使用的server只能用于开发，不能用于正式的生产环境，正式的生产环境的部署是另一个点的内容
>
> 创建app：```python manage.py startapp polls```
>
> 1. 在polls下的views添加一个函数作为一个页面，函数的输入参数为HttpRquest，函数本身会作为参数，传递给url函数的第二个参数
> 2. 创建urls.py作为路径与页面之间映射，其中路径的映射是通过url中的第一个参数，是一个正则表达式
> 3. 将polls的路径映射至mysite中（通过urls.py,url函数），基础的页面就完成了。运行server后就可以进行访问

### 官方教程 part 2 



### 部署至生产环境
WSGI


## Flask
因为Flask是一个轻量级的框架，所以对于这次的项目更加适用，以下会记录这次使用Flask学习到的点，更加详细的需要到官方网站进行查看。
### debug模式和公网的访问
```
app.run(host='0.0.0.0') # 监听公网ip
app.debug = True # 调试模式开启
app.run()
```
### 路由```@app.route('/')```
1. 可以将url中部分内容作为参数```@app.route('/user/<username>')``` 
2. 访问一个结尾不带斜线的 URL 会被 Flask 重定向到带斜线的规范 URL 去。
3. 通过url_for()构造url，可以更加好的管理url，同时它会帮你处理好特殊字符 ？？ 如何管理？
4. http方法:GET, HEAD, POST, PUT, DELETE， OPTIONS ```@app.route('/login', methods=['GET', 'POST'])```

### 静态文件
```url_for('static', filename='style.css')```
### 模板渲染，使用Jinja2
1. 模板的继承
> 模板继承允许你创建一个基础的骨架模板， 这个模板包含您网站的通用元素，并且定义子模板可以重载的 blocks 。
2. 

### 访问请求数据
1. 环境局部变量，依旧不解？ 
2. 请求对象request
3. 文件上传
...
## 消息闪现
