# SMTP (Simple Mail Transfer Protocol)
smtp是邮件发送所使用的协议，本章节先讲述smtp的基本流程，再讲我们使用例如163,qq等邮箱发送一封mail是经历怎么样的流程(如何利用smtp协议)，然后通过telnet等工具手动发送mail，进一步熟悉和理解smtp。最后讲述下在linxu下如何使用命令行发送mail

## 1. smtp流程
借助telnet工具讲述smtp的流程

[参考资料-通过telnet使用smtp协议发送邮件](http://blog.163.com/lixiangqiu_9202/blog/static/535750372013929101334921/)

[smtp doc](https://tools.ietf.org/html/rfc5321#page-17)
```
# 使用163的服务器进行代理发送
# 以下 '>' 后面是输入部分, '#' 后面是注释部分
> telnet smtp.163.com 25
220 163.com Anti-spam GT for Coremail System (163com[20141201])
> helo localhost # 或者使用 ehlo localhost, 用于说明自己是谁
250 OK
> auth login # 由于使用第三方代理发送所以需要先授权登录
334 dXNlcm5hbWU6 # username 的 base64 编码
> ... # base64编码后的用户名 可使用 echo -n ***@163.com | base64
334 UGFzc3dvcmQ6 # Password 的 base64 编码
> ... # base64 编码后的授权码
235 Authentication successful
> mail from:<yourname@163.com> # 邮件的发送者
250 Mail OK
> rcpt to:<***@***.com> # 发送到哪里，如果有多个人可以多次输入次命令
250 Mail OK
> data # 开始编写邮件内容
354 End data with <CR><LF>.<CR><LF>
> Subject: subject
> from:<yourname@163.com>
> to:<***@***.com>
> # 正文需要空一行
> body
> . # 以 \r\n.\r\n 表示结束然后邮件就会发送
250 Mail OK
> Quit # 退出服务器 
221 Bye
```

## 2. 邮件发送流程
这里讲述的是邮件从源地址出发送到目的地的流程。核心点在于如何找到接收方的smtp服务器地址。

[参考-邮件服务器：SMTP协议原始命令码和工作原理](https://my.oschina.net/mailit/blog/86512)
```
send to ***@qq.com --> get domain qq.com -> 
query with mx record -> get address list -> 
select one -> connect with smtp deliver email   

1. 根据收件人获取收件方的域名，例如 ***@qq.com 域名为 qq.com
2. 根据 mx record（DNS的其中一项服务）查询对于的 mx 地址，得到对应的地址列表
3. 使用smtp连接其中一个地址，不需要登录直接投递即可
```

## 3. 利用telnet手动发送邮件
这里有两种方式，一种是使用第三方代理进行发送邮件，但是需要认证登录。
另一种手动查询mx记录，获取目的地的smtp服务器地址，然后连接直接进行投递，不需要进行认证登录操作。

### 3.1 借助第三方服务器代理发送
上述的smtp流程的例子就是一个借助第三方服务器代理发送的过程。
但是使用同样的方式对qq邮箱进行操作会返回 530 Error。
原因在于telnet工具是明文传输，而qq对传输过程要求进行ssl加密。
因此需要使用另一个工具进行模拟(openssl)。
以下是使用openssl模拟的说明。

[openssl doc](https://www.openssl.org/docs/manmaster/apps/s_client.html)

[Testing SMTP AUTH connections](https://qmail.jms1.net/test-auth.shtml)

```
# 使用如下的命令进行连接，然后执行smtp协议一样的操作即可：
openssl s_client -crlf -connect smtp.qq.com:465
```

### 3.2 直接发送至目的地服务器

[mx record tools](http://mxtoolbox.com/)
```
1. 例如要发送至一封mail给destname@163.com
2. 使用 mx record tools 查询链接，输入163.com 查询 获得如下列表
    Pref	Hostname	IP Address	TTL	
    10	163mx01.mxmail.netease.com	220.181.14.135	5 hrs	Blacklist Check      SMTP Test
    10	163mx02.mxmail.netease.com	220.181.14.144	5 hrs	Blacklist Check      SMTP Test
    10	163mx03.mxmail.netease.com	220.181.14.156	5 hrs	Blacklist Check      SMTP Test
    50	163mx00.mxmail.netease.com	123.125.50.139	5 hrs	Blacklist Check      SMTP Test
3. 选择其中一个地址进行连接
    telnet 163mx01.mxmail.netease.com 25
    输入：
    helo localhost
    mail from:<...>
    rpct to:<...>
    data
    ...
    .
4. 这样子给163可能会被系统当做垃圾邮件拒收,但是确实已经是投递过去了。相同的方式换qq邮箱是完全可以的。

```

## 4. linux 命令行发送mail
[参考资料 - How to send mail from the command line?](http://askubuntu.com/questions/12917/how-to-send-mail-from-the-command-line)

在linux下使用命令行发送mail的工具有很多，我用得是ssmtp。
配置和使用如下：
```
1. 安装：sudo apt-get install ssmtp mailutils
2. 配置：
    sudo vim /etc/ssmtp/ssmtp.conf
    修改如下：
    root=yourname@qq.com
    rewriterDomain=qq.com
    mailhub=smtp.qq.com:465
    hostname=yourname@qq.com
    UseTLS=Yes
    AuthUser=yourname@qq.com
    AuthPass=*********
    FromLineOverride=YES
3. 使用方式：
使用ssmtp命令：
    ssmtp rpct_to@qq.com
    # 输入内容
    To:...
    From:..
    Subject:

    your content
    # 按下 Ctrl + D 发送

    # 也可以将内容写到文本中使用如下命令发送
    ssmtp rcpt_to@qq.com < mail.txt
使用mail命令：
    echo content | mail -a From:yourname@qq.com -s "test ssmpt" rcpt_to@163.com
```

## 5. Python 发送 mail

[参考-SMTP Example](https://docs.python.org/2.7/library/smtplib.html#smtp-example)

[参考-email: Examples](https://docs.python.org/3/library/email-examples.html)

以下是发送python发送mail的两种方式，一种是直接自行构造文本结构，另一种是借助 MIMEText 构造信息结构

```Python
#! /usr/bin/env python
# encoding=utf-8

import smtplib

mail_config = {
    "hostname": "smtp.qq.com",
    "username": "791706848@qq.com",
    "password": "***********"
}

def send(toaddrs, subject, context, sender = mail_config['username']):
    server = smtplib.SMTP_SSL(mail_config['hostname'])
    # server.set_debuglevel(True) # for debug
    server.ehlo('localhost')
    server.login(mail_config['username'], mail_config['password'])
    msg = ("Subject:%s\r\nFrom:%s\r\nTo:%s\r\n\r\n%s")%(subject, sender, ",".join(toaddrs), context)
    server.sendmail(sender, toaddrs, msg)
    server.quit()

if __name__ == "__main__":
    print 'Hello Send email'
    send(['toname@163.com'],'test mail2','send by python with')
```

```Python
#! /usr/bin/env python
# encoding=utf-8

import smtplib

mail_config = {
    "hostname": "smtp.qq.com",
    "username": "791706848@qq.com",
    "password": "***********"
}

# use minetext create message info
def send_mime(toaddrs, subject, context, sender = mail_config['username']):
    from email.mime.text import MIMEText
    msg = MIMEText(context, 'plain','utf-8')
    msg['Subject'] = subject
    msg['From'] = sender
    msg['To'] = ','.join(toaddrs)

    server = smtplib.SMTP_SSL(mail_config['hostname'])
    # server.set_debuglevel(True) # for debug
    server.ehlo('localhost')
    server.login(mail_config['username'], mail_config['password'])
    server.sendmail(sender, toaddrs, msg.as_string())
    server.quit()

if __name__ == "__main__":
    print 'Hello Send email'
    send_mime(['toname@163.com'],'test mail2','send by python with')
```