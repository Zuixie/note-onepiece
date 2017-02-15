# HTML CSS Javascript
## 疑问-HTML的标准，以及javascript的函数标准在哪里

## 文件的拖拽
http://www.cnblogs.com/jscode/archive/2013/05/10/3575024.html
## 
https://www.html5rocks.com/en/tutorials/file/xhr2/

## CSS !important 属性
简单讲,就是当发生冲突的时候优先选择带有 important 属性的风格
```CSS
    .test .test2{
        background: red;
    }
    .test2{
        background: yellow !important; // 正常情况下,test2会被上面的所覆盖,但是加了 important 后就不会被覆盖
    }
```