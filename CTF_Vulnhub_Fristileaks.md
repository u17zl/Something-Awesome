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

## Exploitation:
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
* However, every page looked like the same, as follow:

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/cola.png)  

Nothing special in these pages.

### KEEP CALM AND DRINK FRISTI
* This is what I saw in index page, so there should have some hints. Let's try http://10.10.10.132/fristi/  

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/login.png) 

* Tried some combo of admin and password, and different forms of SQLi, but I did not be able to login.  
* Never just look at the surface! we should look at the source code. Wow!  

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/login_html.png)  

* There was a message, which was left by a man named eezeepz.
* Then there's the possibility that admin might use eezeepz as his username or password.
* Look though down. We saw a large block of base64-encoded codes:
```
iVBORw0KGgoAAAANSUhEUgAAAW0AAABLCAIAAAA04UHqAAAAAXNSR0IArs4c6QAAAARnQU1BAACx
jwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAARSSURBVHhe7dlRdtsgEIVhr8sL8nqymmwmi0kl
S0iAQGY0Nb01//dWSQyTgdxz2t5+AcCHHAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixw
B4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzkCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL5kc+f
m63yaP7/XP/5RUM2jx7iMz1ZdqpguZHPl+zJO53b9+1gd/0TL2Wull5+RMpJq5tMTkE1paHlVXJJ
Zv7/d5i6qse0t9rWa6UMsR1+WrORl72DbdWKqZS0tMPqGl8LRhzyWjWkTFDPXFmulC7e81bxnNOvb
DpYzOMN1WqplLS0w+oaXwomXXtfhL8e6W+lrNdDFujoQNJ9XbKtHMpSUmn9BSeGf51bUcr6W+VjNd
jJQjcelwepPCjlLNXFpi8gktXfnVtYSd6UpINdPFCDlyKB3dyPLpSTVzZYnJR7R0WHEiFGv5NrDU
12qmC/1/Zz2ZWXi1abli0aLqjZdq5sqSxUgtWY7syq+u6UpINdOFeI5ENygbTfj+qDbc+QpG9c5
uvFQzV5aM15LlyMrfnrPU12qmC+Ucqd+g6E1JNsX16/i/6BtvvEQzF5YM2JLhyMLz4sNNtp/pSkg1
04VajmwziEdZvmSz9E0YbzbI/FSycgVSzZiXDNmS4cjCni+kLRnqizXThUqOhEkso2k5pGy00aLq
i1n+skSqGfOSIVsKC5Zv4+XH36vQzbl0V0t9rWb6EMyRaLLp+Bbhy31k8SBbjqpUNSHVjHXJmC2Fg
tOH0drysrz404sdLPW1mulDLUdSpdEsk5vf5Gtqg1xnfX88tu/PZy7VjHXJmC21H9lWvBBfdZb6Ws
30oZ0jk3y+pQ9fnEG4lNOco9UnY5dqxrhk0JZKezwdNwqfnv6AOUN9sWb6UMyR5zT2B+lwDh++Fl
3K/U+z2uFJNWNcMmhLzUe2v6n/dAWG+mLN9KGWI9EcKsMJl6o6+ecH8dv0Uu4PnkqDl2rGuiS8HK
ul9iMrFG9gqa/VTB8qORLuSTqF7fYU7tgsn/4+zfhV6aiiIsczlGrGvGTIlsLLhiPbnh6KnLDU12q
mD+0cKQ8nunpVcZ21Rj7erEz0WqoZ+5IRW1oXNB3Z/vBMWulSfYlm+hDLkcIAtuHEUzu/l9l867X34
rPtA6lmLi0ZrqX6gu37aIukRkVaylRfqpk+9HNkH85hNocTKC4P31Vebhd8fy/VzOTCkqeBWlrrFhe
EPdMjO3SSys7XVF+qmT5UcmT9+Ss//fyyOLU3kWoGLd59ZKb6Us10IZMjAP5b5AgAL3IEgBc5AsCLH
AHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixwB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzk
CwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL3IEgBc5AsCLHAHgRY4A8Pn9/QNa7zik1qtycQAAAABJR
U5ErkJggg==
```
* Decode it and save it as png file:

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/decode.png)  

* It looks like a password, try 
```
username:eezeepz password:keKkeKKeKKeKkEkkEk
```

* Bingo, we made it and there is a pciture upload function. 

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/upload1.png)  

* Needless to say, a php shell cannot be better.  
* Shell can download here: http://pentestmonkey.net/tools/web-shells/php-reverse-shell  
* Do not forget to modify source code. Edit the following lines of php-reverse-shell.php:  

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/php_shell.png) 

* uploaded it, but got the response of wrong file type, possibly because there is a filter in backend.  

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/php wrong.png)  

* No worries, just added a jpg extension to the php reverse shell, then I launched the netcat listener and then I opened the shell we uploaded. Connection established.  

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/jpg.png) 
![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/shell1.png) 


### Privilege Escalation
* Checked through direcories, see what can be exploited. It seems that there is something interesting in /HOME, try it out:

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/cdhome.png)  

* We do not have access of admin:

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/admindeny.png)  

* There is a old friend, eezeepz. See what inside:  

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/cdeeze.png)  

* There is a notes.txt file that interests me:
```
sh-4.1$ cat notes.txt
cat notes.txt
Yo EZ,

I made it possible for you to do some automated checks, 
but I did only allow you access to /usr/bin/* system binaries. I did
however copy a few extra often needed commands to my 
homedir: chmod, df, cat, echo, ps, grep, egrep so you can use those
from /home/admin/

Don't forget to specify the full path for each binary!

Just put a file called "runthis" in /tmp/, each line one command. The 
output goes to the file "cronresult" in /tmp/. It should 
run every minute with my account privileges.

- Jerry
```
* Nice! We got a hint, just followed it:
```
sh-4.1$ touch /tmp/runthis
sh-4.1$ echo "/home/admin/chmod 777 /home/admin" > /tmp/runthis
```
* Then we have got the privilege of admin:

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/adadmin.png)

* Now inside it I could see a bunch of interesting files, some encrypted files and a python script used to encrypt the files.

![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/adadmin.png)
![img](https://raw.githubusercontent.com/u17zl/Something-Awesome/master/src/python.png)

* It seems that these files are encoded with this python file. Use it to decode:
```
root@kali:~# python cryptpass.py mVGZ3O3omkJLmy2pcuTq
thisisalsopw123
root@kali:~# python cryptpass.py =RFn0AKnlMHMPIzpyuTI0ITG
LetThereBeFristi!
```

* One if them could be password of fristigod, let's have a try

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
