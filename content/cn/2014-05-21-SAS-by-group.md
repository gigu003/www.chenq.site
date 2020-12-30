---
title: SAS数据步by-group语句
date: '2014-05-21'
slug: 
mathjax: yes
---

By-Group 最常见的功能就是，在一个数据集里利用SET、MERGE、MODIFY 和 UPGRADE语句和By语句进行两个或者多个数据集的合并。当你在用SET、MERGE、MODIFY和UPGRADE语句合并By语句时，数据集里的记录必须是经过排序的。

当在处理这些语句时，SAS每次读取一条记录到程序数据向量。在By-Group语句的作用下，SAS会根据一个或者多个By变量的值在数据集中选择相应的记录。当处理完一个By-Group的记录后，SAS会挪到下一个By-Group继续处理。
### By-Group+条件语句

我们可以通过利用条件语句（IF 或者IF-THEN语句或者SELECT语句）加上SAS临时变量First.variable 或者Last.variable，从而可以有条件地处理记录。
例如，我们可以利用它来执行每一个By-group组的计算，并且在一个By-Group组的第一条记录或者最后一条记录已经写进程序数据向量时写记录到数据集。
<br>下面的程序计算了每一年各个部门的工资，在每一个By-Group开始的时候，它利用IF-THEN语句和自动变量First.variable和Last.variable来重置工资到0，在每个By-group处理结束的时候，写记录到数据集。
```
options pageno=1 nodate linesize=80 pagesize=60; 
data salaries;   
input Department $ Name $ WageCategory $ WageRate;   
datalines;
BAD Carol Salaried 20000
BAD Elizabeth Salaried 5000
BAD Linda Salaried 7000
BAD Thomas Salaried 9000
BAD Lynne Hourly 230
DDG Jason Hourly 200
DDG Paul Salaried 4000
PPD Kevin Salaried 5500
PPD Amber Hourly 150
PPD Tina Salaried 13000
STD Helen Hourly 200
STD Jim Salaried 8000
;
proc print data=salaries;
run;
proc sort data=salaries out=temp;   
by Department;
run;
data budget (keep=Department Payroll);  
set temp;      
by Department;
if WageCategory='Salaried' then YearlyWage=WageRate*12;   
else if WageCategory='Hourly' then YearlyWage=WageRate*2000;  

if first.Department then Payroll=0;   
Payroll+YearlyWage;
if last.Department;
run;

proc print data=budget;   
format Payroll dollar10.;
title 'Annual Payroll by Department';
run;
```


### 数据不是按照字母或者数字排序
在进行BY-Group处理操作时，不仅可以利用按照字母或者数字排序的数据，还可以按照日历月份或者分类等排列的数据。只需要在你使用SET语句时，在By语句后面加个Notsorted选项即可。Notsorted选项的作用就是告诉SAS数据不是按照字母或者数字大小排序的，而是按照By变量的值按组排列的。
**Notsorted**选项不能用在Merge语句、Upgrade语句，或者当SET语句后多于一个数据集的时候。
下面的例子假定数据是按照字符变量Month分组的，IF语句根据Last.Month的值有条件的写记录到数据集。数据步只会在每个By组处理完后写记录到数据集。
```
data total_sale(drop=sales);
set region.sales
by month notsorted;   
total+sales;   
if last.month;
run;
```
 
