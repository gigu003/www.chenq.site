---
title: SAS访问外部数据文件
date: '2010-06-03'
slug: 
mathjax: yes
---

SAS访问外部数据文件有四种方法，分别为：
1. 通过IMPORT过程 
1. 通过libname语句和库引擎 
1. 通过Access过程
1. 通过ODBC或者远程软件平台。
这里做个读书笔记，备忘。
## IMPORT过程
语法和选项说明：
```
PROC IMPORT   *DATAFILE= 规定要读入外部文件的地址及名称  TABLE=规定外部数据文件的表名.
DATAFILE=“filename”|TABLE="tablename"
OUT=SAS-data-set                          *OUT= 规定要输出的SAS数据集.
<DBMS=identifier><REPLACE>;  *DBMS=规定外部数据文件格式的标示名  REPLACE=规定替换已存在文件.
1.1导入EXCEL数据表
PROC  IMPORT  OUT=TEMP               *读取excel数据，输入数据集work.temp.
DATAFILE="d:\data\abc.xls"           *规定要读入外部文件的地址和名称为"d:\data\abc.xls".
DBMS=EXCEL2000 REPLACE;              *指定外部数据文件为EXCEL2000，规定替换已存在文件.
SHEET="'sheet1$'";                   *导入表sheet1.
GETNAMES=YES | NO;　　　　*获取变量名:如果EXCEL第一行是变量名的话则选择yes，提取变量名.
MIXED=YES | NO;          *将数值数据转换成字符数据值，作为一列，其中包含混合数据类型。此选项仅适用于从Excel导入数据。默认的是否定的，这意味着，数字数据将被识别为缺失值。如果混MIXED=YES，那么引擎将指派该列为SAS字符类型，并把所有的数字数据转换为字符数据值进行存储。.
SCANTEXT=YES | NO;        *会自动扫描，以最大的宽度作为改列字符变量的宽度。如果SCANTEXT=NO，则在不设定TEXTSIZE的情况下，默认长度为255。.
USEDATE=YES | NO; *使用日期变量。USEDATE=NO时，默认输入的将是日期时间变量，时间为该日的0点0分0秒.
SCANTIME=YES | NO;
TEXTSIZE=x;      *1<x<32767,当TEXTSIZE与SCANTEXT=YES同时出现时，字符变量的宽度有SCANTEXT决定，即扫描该列所有数据得到，但是，其宽度不超过TEXTSIZE的最大值。eg.如有一个字符串为"ABC1234567890"，其宽度为13，但是我设置的TEXTSIZE=10，那么该列的宽度为10，只能导入左侧10位，剩余右侧的将丢失。所以，一般优先使用SCANTEXT=YES，而不适用TEXTSIZE。.
RUN;
```

### 导入ACCESS数据库
```
proc import out=work.dists 　　*读取ACCESS数据库，输入数据集work.dists.
datatable="abc"　　            *规定读入的ACCESS数据库的表名为abc.
dbms=access replace;　       *指定外部数据文件为ACCESS，规定替换已存在文件.
database="d:\data\cyst.mdb";   *规定要读入外部文件的地址和名称为"d:\data\cyst.mdb".
uid='***';                    *指定数据库用户名.
pwd='***';                    *规定ACCESS密码.
run;
```
## ACCESS过程：

ACCESS过程对外部数据文件的访问和读写，必须通过两步来完成：

第一步：创建访问描述器 access descriptor.

第二步：创建基于外部数据文件的数据视窗 view.

创建访问描述器：语句格式：
```
Proc Access dbms=dbf|xls；
create libref.membername.access;
required database description statements;
optional editing statements;
run;
```
实例：
```
proc access dbms=xls;
create work.cyst.access;
path=‘d:\data\cyst.xls’；
getnames=yes;
scantype=yes;
list all;
run;
```
创建数据视窗view：
```
proc access dbms=dbf | xls  accdesc=libref.access-descriptor;
create   libref.membername.view;
select    column-list;
optional editing statements;
run;
```
实例：
```
proc access dbms=xls   accdesc=work.cyst;
create work.aaa.view;
select all;
list view;
run;
```