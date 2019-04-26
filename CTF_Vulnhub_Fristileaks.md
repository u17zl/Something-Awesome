# Something Awesome
## Introduction：
FristiLeaks is a VM created by Ar0xA and the difficulty rating is descriped as basic. This CTF challenge involves penetrating methodologies such as port scanning, server fingerprinting, webshell reverse, SSRF, etc. The challenge goal is to get root access and catch flag.

## VulnHub – FristiLeaks_1.3
### VM download:
* Download (Mirror): https://download.vulnhub.com/fristileaks/FristiLeaks_1.3.ova
* Download (Torrent): https://download.vulnhub.com/fristileaks/FristiLeaks_1.3.ova.torrent
### VM environment:
* Virtualbox
* VMware Workstation player
### Settings:
User will need to manually edit the s MAC address to: 08:00:27:A5:A6:76

## Exploit:
### IP address:

![image text](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/1.png)  

The target IP is 192.168.0.110

### Port Scanning:
Running Nmap  
```
nmap -p- -sS -sV -vv 192.168.0.110
```  
over the VM revealed that the only common port that was open, was port 80, which it identified as running Apache on CentOS:    
```
PORT   STATE SERVICE REASON         VERSION
80/tcp open  http    syn-ack ttl 64 Apache httpd 2.2.15 ((CentOS) DAV/2 PHP/5.3.3)
```
### Robots.txt
* I browsed IP address and this is what it looks like:  

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/index.png)  

* Let us try if there something in 192.168.0.110/Robots.txt

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/robots.png)  

* robots.txt: 3 disallowed entries:
```
/cola
/sisi
/beer
```
* However, every page look like the same, as follow:

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/cola.png)  
Nothing special in these pages.

### KEEP CALM AND DRINK FRISTI
* This is what I saw in index page, so there should have some hints. Let's try http://10.10.10.132/fristi/  

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/login.png) 

* Tried some combo of admin and password, and different forms of SQLi, but I did not be able to login.  
* Never just look at the surface! we should look at the source code. Wow!  

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/login_html.png)  

image.png

看到一有一条信息，这是一个叫eezeepz的人留下来的。

那么那就有这种可能：他也许用eezeepz当做他的用户名或者密码。

再向下看。我们看到了一大块用base64编码的段落

image.png

这里我使用nano 使它变成单行，方便命令行编码

base64 -d /tmp/encoded.txt

image.png

这是一个PNG格式的图画，保存为PNG

base64 -d /tmp/encoded.txt > decoded.png

然后可以用任意工具查看，这里用feh

image.png

看起来像是个密码！赶紧试试 username:eezeepz password:keKkeKKeKKeKkEkkEk

image.png

这时候 不用多说 上传sell

Sell 可以在这里下载http://pentestmonkey.net/tools/web-shells/php-reverse-shell

cp /usr/share/webshells/php/php-reverse-shell.php reverse-shell.php
vi reverse-shell.php
做一些必要的修改，ip地址和监听端口。

image.png

现在设置 netcat 监听 建立连接：

nc -nlvp 8888

image.png

看来只有png, jpg, gif 能上传

image.png

修改一下后缀加上.jpg

image.png

上传成功！

现在打开上传的sell

image.png

现在已经得到了一个低权限权限

image.png

权限提升

image.png

看一下目录，看看有什么可以挖掘的东西，个人对HOME比较感兴趣，进去试试

image.png

居然马上看到关键人物eezeepz！继续向前看

image.png

文件很多 挑特别的看，notes.txt比较显眼，打开试试。

image.png
我们得到了提示，照着做就行了！在/tmp下创建一个＂runtis＂文件

image.png

赋予权限
image.png

现在我们可以阅读 /home/admin 下的内容了

image.png

有几个文件。依次看一下 cryptpass.py

image.png

Cryptepass.txt

image.png

whoisyourgodnow.txt

image.png

看样子应该是用了py文件去加密的。我们重写一下文件：

image.png

解密试试

image.png

分别得到

1.mVGZ3O3omkJLmy2pcuTq ：thisisalsopw123

2.=RFn0AKnlMHMPIzpyuTI0ITG ：LetThereBeFristi!

这有可能是用户fristgod 的密码我们换一下用户试试！

image.png

失败了….查了一下网上是这样解释的：跟 su 命令的实现有关； B环境上su的实现应该是判断标准输入是不是tty ； 而A环境上su的实现则允许从其他文件读取密码。方法如下：

Python -c ‘import pty;pty.spawn(“/bin/sh”)’

image.png

接下来就可以正常使用了。

image.png

现在我们已经成功进入fristigod账户

Ls试试：

image.png

没有东西…

-la试试

image.png

原来都藏起来了。。到.secret_admin_stuff看看

image.png

继续 ls -la 查看具体信息

image.png

发现这个是个root的文件权限应该是不够的

image.png

我们能回去看看history有没有一些线索

image.png

可以看到 “fristigod”用户一直sudo来执行命令

试试 sudo -l

image.png

让输入密码，上面我们得到了两个密码

image.png

呃。。。再试试

image.png

成功了...居然是一个密码....好吧。

image.png

image.png

image.png

image.png

专栏
