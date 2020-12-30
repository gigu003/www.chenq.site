---
title: COMPRESS函数 
date: '2011-08-16'
slug: 
mathjax: yes
---


下列资料来源于网络：

1. compress() 函数的基本形式

语法：

compress (<source><, chars><, modifiers>)

参数：

source: 指定一个字符串来源

chars: 指定要删除或者保留的字符列表，需用引号

modifiers: 指定修饰符，不区分大小写，用来控制 compress 函数，常用的修饰符及意义见本文的最后部分

2. compress() 函数应用举例

已有数据集 have：
```
data have;
input char $20.; cards; Elek dot Me Yuewei-Liu HOME-027-8765 4321 ; run;
```
例1. 删除空格：可以直接省去第二和第三个 Arguments，也可以明确将空格加入到字符串列表中，也就是第二个 Argument。

data test; set have; char1=compress(char); run; data test; set have; char1=compress(char," "); run;

例2. 使用修饰符删除小写字母：将修饰符设定为”l”，代表 lowcase，即将所有的小写字母加入到要删除的字符列表中；如不用修饰符”l”，也可以直接把所有a-z的小写字母列入要删除的字符串列表当中，效果一样，但显然前者比较简单；本例可以将所有小写字母和大写的”E”从指定的字符串中删除。

data test; set have; char1=compress(char,"E","l"); run; data test; set have; char1=compress(char,"abcdefghijklmnopqrstuvwxyzE"); run;

例3. 保留指定的字符：字符列表的定义与前面类似，只需将”K”或”k”写入修饰符，或者在字符串列表中加入所有数字。本例为保留所有数字和”HOME“中的字符。

data test; set have; char1=compress(char,"HOME","dk"); run; data test; set have; char1=compress(char,"HOME1234567890","k"); run;

附：常用的修饰符及其意义

a/A adds letters of the Latin alphabet (A - Z, a - z) to the list of characters. 所有拉丁字母，包括 a-z A-Z d/D adds numerals to the list of characters.所有数字

f/F  adds the underscore character and letters of the Latin alphabet (A - Z, a - z) to the list of characters.下划线和所有拉丁字母

g/G adds graphic characters to the list of characters.

i/I  ignores the case of the characters to be kept or removed.忽略要删除或保留字符的大小写

k/K  keeps the characters in the list instead of removing them.保留字符串列表中的字符

l/L  adds lowercase letters (a - z) to the list of characters.所有小写拉丁字母

n/N  adds numerals, the underscore character, and letters of the Latin alphabet (A - Z, a - z) to the list of characters.下划线，数字和所有拉丁字母

s/S  adds space characters to the list of characters (blank, horizontal tab, vertical tab, carriage return, line feed, and form feed).定位符，如空格、tab等

t/T  trims trailing blanks from the first and second arguments.去掉第一和第二个 Arguments 里的尾部空格

u/U  adds uppercase letters (A - Z) to the list of characters.所有大写拉丁字母
