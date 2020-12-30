---
title: 利用SAS批量导入一个文件夹内的多个excle表 
date: '2012-03-05'
slug: 
mathjax: yes
---

```
%let dirs=C:\aa;  

%macro get_filenames(location);
filename _dir_ "%bquote(&location.)";
data filenames(keep=memname tmp);
  handle=dopen( '_dir_' );
  if handle > 0 then do;
   count=dnum(handle);
    do i=1 to count;
     tmp=cats("tmp",i);
     tmp=Tranwrd(tmp," ","");
     memname=dread(handle,i);
    
     output filenames;
    end;
  end;
  rc=dclose(handle);
run;
filename _dir_ clear;
%mend;
%get_filenames(&dirs);  

%macro readExl(name=,tmp=);
PROC IMPORT OUT=&tmp

DATAFILE= "&dirs\&name"
DBMS=EXCEL REPLACE;
SHEET="Sheet1$";
GETNAMES=YES;
MIXED=NO;
SCANTEXT=YES;
USEDATE=YES;
SCANTIME=YES;
RUN;
%mend;

data _null_;
   set Filenames;
   call execute('%readExl(name= '||memname||',tmp='||tmp||')');
run;

proc datasets library=work nolist;
    delete target;
run;

%macro mergeData(tmp=);
    proc append base=target data=&tmp force;
    run;
%mend;

data _null_;
   set Filenames;
   call execute('%mergeData(tmp='||tmp||')');   
run;

%macro deleteTmp(tmp=);
    proc datasets library=work nolist;
       delete &tmp;
    run;
%mend;

data _null_;
   set Filenames;
   call execute('%deleteTmp(tmp='||tmp||')');    
run;

```