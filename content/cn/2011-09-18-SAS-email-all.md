---
title: 利用SAS群发邮件
date: '2011-09-18'
slug: 
mathjax: yes
---

 今天在网上看到有人用SAS群发邮件，很是震惊，SAS功能这么强大还能群发邮件？于是就找了点资料试了一下，果真可以唉！原来SAS可以利用SMTP功能发邮件，这对于批量处理数据后，把结果一次性发送给不同的用户来说，还是非常有用的！现在把学习的这个功能跟大家分享一下啦！

首先准备下用户信息（用户名、邮箱地址、附件地址）的数据集：

 

数据集：usermessage

name Email  address

张三  ccc@126.com  d:\ddd.pdf

李四  ddd@163.com d:\eee.pdf

王五  eee@hotmail.com d:\xyz.pdf

李五 cqq@gmail.com  d:\zzz.pdf
 
```
OPTIONS Emailauthprotocol=login
Emailsys=smtp
Emailport=25
Emailhost="smtp.163.com"
Emailid="xxxxx@163.com"
Emailpw="**********";

filename mymail email"dddd@126.com"
subject="SAS OUTPUT SYSTEM"
encoding=gb2312;
data _null_;
file mymail;
set usermessage; *从用户信息数据集usermessage读取数据;
put "!EM_TO!" email; *指定邮件发送的地址;
put "!EM_SUBJECT! The output result by SAS"; *指定邮件的主题;

put "!EM_ATTACH!" address; *指定邮件所添加的附件;

put "尊敬的 " name "(先生/女士)："; *把邮件接收者的姓名添加到邮件中;

put " 您好!程序已经运行成功，现在把结果给您发过去，请查看！附件";
put "!EM_SEND!";
put "!EM_NEWMSG!";
return;
run;
```
好了，现在邮件已经被发送到用户信息数据集指定的用户邮箱里了！