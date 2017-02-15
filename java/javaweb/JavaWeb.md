# JavaWeb

## 类似servlet中getOutputStream是否应该自己close
答案是不用，原则是谁开谁关．此outputStream并不是自己开的，自然不应该我来关

[参考 - Should I close the servlet outputstream?](http://stackoverflow.com/questions/1829784/should-i-close-the-servlet-outputstream) 