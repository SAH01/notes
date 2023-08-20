# 汉得培训1）SQL培训

## 1.1、oracle函数

### 1.1.1、数值型

```sql
abs()
sign() --- 若为正值返回1，负值返回-1，0返回0
ceil()
floor()
power()		--- power(2.5,2)=6.25
exp()
log(x,y)
ln(y)
mod(x,y)
round()		--- 四舍五入
---
trunc(x,y)
【参数】x,y，数字型表达式,如果y不为整数则截取y整数部分，如果y>0则截取到y位小数，如果y小于0则截取到小数点向左第y位，小数前其它数据用0表示。
select trunc(5555.66666,2.1),trunc(5555.66666,-2.6),trunc(5555.033333)  from dual;
返回：5555.66                    5500               5555
---
sqrt()
sinh(20)	--- 双曲正弦
asin(0.5)		--- 反正弦
```

### 1.1.2、字符型

```sql
ascii('A') --- 返回ascii码
chr(65)		--- A
concat('s1','s2')  		--- 同 s1 || s2
initcap('smith abc aBC')		--- Smith Abc Abc (首字母大写其余小写)
lower() upper() 		--- 首字母大（小）写

-----
INSTR(C1,C2[,I[,J]])	--- 多字节符返回最远端字符的位置
【参数】
C1    被搜索的字符串
C2    希望搜索的字符串
I     搜索的开始位置,默认为1
J     第J次出现的位置,默认为1
InStrB 返回的不是一个字符串在另一个字符串中第一次出现的字符位置，而是字节位置。
-----

length() lengthb()
-----
LPAD(c1,n[,c2])		rpad() //　左（右）追加字符
【参数】
C1 字符串
n 追加后字符总长度 [关键]
c2 追加字符串,默认为空格
-----

LTRIM(c1,[,c2])
【功能】删除左边出现的字符串
【参数】
C1 字符串
c2 追加字符串,默认为空格
RTRIM(c1,[,c2])
-----

REPLACE(c1,c2[,c3])
【功能】将字符表达式值中，部分相同字符串，替换成新的字符串
【参数】
c1   希望被替换的字符或变量 
c2   被替换的字符串
c3   要替换成新的字符串，默认为空(即删除之意，不是空格)
---

TRANSLATE(c1,c2,c3)
【参数】
c1   希望被替换的字符或变量 
c2   查询原始的字符集
c3   替换新的字符集，将c2对应顺序字符，替换为c3对应顺序字符
如果c3长度大于c2，则c3长出后面的字符无效
如果c3长度小于c2，则c2长出后面的字符均替换为空(删除)
如果c3长度为0，则返回空字符串。
如果c2里字符重复，按首次位置为替换依据
select TRANSLATE('he love you','he','i'),
TRANSLATE('重庆的人','重庆的','上海男'),
TRANSLATE('重庆的人','重庆的重庆','北京男士们'),
TRANSLATE('重庆的人','重庆的重庆','1北京男士们'),
TRANSLATE('重庆的人','1重庆的重庆','北京男士们') from dual;
返回：i love you,上海男人,北京男人,1北京人,京男士人

-----
SOUNDEX(c1)
【功能】返回字符串参数的语音表示形式
---
SUBSTR(c1,n1[,n2])
【功能】取子字符串，从n1开始取n2个字符。
SUBSTRB(c1,n1[,n2]) // 按字节取
---
TRIM(c1 from c2)
【功能】删除左边和右边出现的字符串

```

### 1.1.3、日期函数

```sql 
sysdate
add_months(d1,n1) --- 当前日期+n1个月后的日期
last_day(d1) --- 返回日期所在月份最后一天日期
months_between(d1,d2) --- 两个日期直接的月数
next_day(d1[,c1]) --- next_day(sysdate,'星期日') 下周星期日 from dual; 
下周某天日期
```

### 1.1.3、转换函数

> 1、TO_CHAR(x[[,c2],C3])
>
> 2、TO_DATE(X[,c2[,c3]])
>
> 3、TO_NUMBER(X[[,c2],c3])

### 1.1.4、聚组函数

```
STDDEV([distinct|all]x) 		--- 标准差
VARIANCE([distinct|all]x)		--- 方差
count() 
max()
min()
avg()
sum()
```

### 1.1.5、其他函数

1、**decode**

```sql
select empno,
       decode(empno,
              7369,
              'smith',
              7499,
              'allen',
              7521,
              'ward',
              7566,
              'jones',
              'unknow') as name
  from emp
  where rownum <= 10 
  
 /*输出结果 7369 smith 7499 allen 7521 ward 7566 jones 7654
 unknow 7698 unknow 7782 unknow 7788 unknow 7839 unknow 7844unknow*/

```

2、**NULLIF (expr1, expr2)**

```sql
expr1和expr2相等返回NULL，不相等返回expr1
```

3、**rownum 当前行号**

4、**case when**

```sql
SELECT xqn, 
       CASE
          WHEN xqn = 1  THEN '星期一'
          WHEN xqn = 2  THEN '星期二'
          WHEN xqn = 3  THEN '星期三'
	  else '星期三以后'
       END 星期
FROM xqb
--
SELECT xqn, 
decode(xqn,1,'星期一',2,'星期二',3,'星期三','星期三以后') 星期
FROM xqb

```



## 1.2、Oracle SQL PPT 

### 1.2.1、select

> 1、取别名使用双引号与不使用双引号的区别

双引号：会将别名解析成双引号里的内容，不使用双引号的话，即使别名全部命名成小写，也会被解析成大写字母。所以，双引号一般会用在最外层的select子句中，保证列名的大小写是你想要的结果。【简言之，双引号会保留原始格式；无双引号全部解析为大写字母。】

> 2、字符串连接符：||

SELECT last_name **||' is a '||**job_id  AS  "Employee Details" FROM employees;

![image-20230307123116071](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142251129.png)

### 1.2.2、条件限制

1、< > 不等于

2、BETWEEN...AND...

3、order by 字段1,字段2 desc

### 1.2.3、单行函数

> **Oracle**数据库中的数据是大小写敏感的 

数据类型显式转换函数：

![image-20230307131659256](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142251130.png)

decode和case 条件函数

```sql
SELECT last_name, job_id, salary,
CASE job_id 
WHEN 'IT_PROG' THEN 1.10*salary
WHEN 'ST_CLERK' THEN 1.15*salary
WHEN 'SA_REP' THEN 1.20*salary
ELSE salary 
END "REVISED_SALARY"
FROM employees;


SELECT last_name, job_id, salary,
    DECODE(job_id, 'IT_PROG', 1.10*salary,
                                 'ST_CLERK', 1.15*salary,
                                 'SA_REP', 1.20*salary,
                                  salary)  REVISED_SALARY
FROM employees;

```

### 1.2.4、分组计算

> **SELECT 查询语句中同时选择分组计算函数表达式和其他独立字段时 ，其他字段必须出现在Group By子句中，否则不合法。**
>
> **不能在Where条件中使用分组计算函数表达式，当出现这样的需求的时候，使用Having子句。**

### 1.2.5、merge

MERGE 目标表
USING 数据源（数据表，子查询，视图）
ON 匹配条件
WHEN MATCH THEN 操作语句 **；（结束的这个分号必不可少）**

### 1.2.6、事务

```
当如下事件发生是，会隐式的执行Commit动作：
1、数据定义语句被执行的时候，比如新建一张表：Create Table …
2、数据控制语句被执行的时候，比如赋权 GRANT …( 或者 DENY)
3、正常退出PLSQL DEVELOPER, 而没有显式执行COMMIT或者ROLLBACK 语句 。

当如下事件发生时，会隐式执行Rollback 动作：
1、非正常退出PLSQL  DEVELOPER,或者发生系统错误。
```

### 1.2.7、数据库对象-表

| **数据类型**           | **描述**                                                     |
| ---------------------- | ------------------------------------------------------------ |
| VARCHAR2(size)         | 可变长字符串                                                 |
| CHAR(size)             | 定长字符串                                                   |
| NUMBER(p,s)            | 可变长数值                                                   |
| DATE                   | 日期时间                                                     |
| LONG                   | 可变长大字符串，最大可到2G                                   |
| CLOB                   | 可变长大字符串数据，最大可到4G                               |
| RAW and LONG RAW       | 二进制数据                                                   |
| BLOB                   | 大二进制数据，最大可到4G                                     |
| BFILE                  | 存储于外部文件的二进制数据，最大可到4G                       |
| ROWID                  | 64进制18位长度的数据，用以标识行的地址                       |
| TIMESTAMP              | 精确到分秒级的日期类型（9i以后提供的增强数据类型）           |
| INTERVAL YEAR TO MONTH | 表示几年几个月的间隔（9i以后提供的增强数据类型-极其少见）    |
| INTERVAL DAY TO SECOND | 表示几天几小时几分几秒的间隔（9i以后提供的增强数据类型-极其少见） |

**复制表：**

CREATE TABLEA as select  * from tableb

**保留表结构：**

CREATE TABLEA as select * from tableb where 1=2

**更新记录**，先查询是否资源被占用：

update table1 set field1 = ‘111’ where field2 = 2 ;

select * from table1 where field2 = 2 for update NoWait ;

更改表名：

RENAME oldtablename to  newtableName;

### 1.2.8、约束

NOT NULL      （非空约束）
UNIQUE        （唯一性约束）
PRIMARY KEY   （主键约束）
FOREIGN KEY   （外键约束）
CHECK         （自定义约束）

### 1.2.9、视图

```sql
CREATE VIEW 	empvu80
 AS SELECT  employee_id, last_name, salary
    FROM    employees
    WHERE   department_id = 80;
---
CREATE VIEW	dept_sum_vu
  (name, minsal, maxsal, avgsal)
AS SELECT	 d.department_name, MIN(e.salary), 
             MAX(e.salary),AVG(e.salary)
   FROM      employees e, departments d
   WHERE     e.department_id = d.department_id 
   GROUP BY  d.department_name;
```

### 1.2.10、序列

```
创建序列
语法 CREATE SEQUENCE 序列名 [相关参数]
参数说明
INCREMENT BY :序列变化的步进，负值表示递减。(默认1)
START WITH:序列的初始值 。(默认1)
MAXvalue:序列可生成的最大值。(默认不限制最大值，NOMAXVALUE)
MINVALUE:序列可生成的最小值。(默认不限制最小值，NOMINVALUE)
CYCLE:用于定义当序列产生的值达到限制值后是否循环(NOCYCLE:不循环，CYCLE:循环)。
CACHE:表示缓存序列的个数，数据库异常终止可能会导致序列中断不连续的情况，默认值为20，如果不使用缓存可设置NOCACHE


```

> **序列的使用**
> **currval** 表示序列的当前值，新序列必须使用一次nextval 才能获取到值，否则会报错
> **nextval** 表示序列的下一个值。新序列首次使用时获取的是该序列的初始值，从第二次使用时开始按照设置的步进递增
> 查询序列的值：select seq_name.[currval,nextval] from dual;
> 查看所有已创建的序列：select * from user_sequences
> SQL语句中使用：**insert into table (id) values (seq_name.nextval)**

索引：

```sql
CREATE INDEX emp_last_name_idx ON employees(last_name);
```

> (1) create index 索引名 on 表(字段名);     //创建B树索引，一般用的OLTP中
>
> (2) create bitmap index 索引名 on 表(字段名);   //创建位图索引，一般用的OLAP中
>
> (3) create index 索引名 on 表名 (substr(字段,1,10))  
>
> //创建函数索引，有些时候呢，咱们的搜索条件要加上函数，这种情况呢，普通索引就不能解发了，就要创建函数索引
>
> (4) create index 索引名 on 表名 (字段1,字段2......字段n);
>
> //复合索引,当条件中，经常去查询多个条件时，可以把多个条件放在一个索引中
>
> (5)  create index 索引名 on 表(字段 desc);   //排序后创建索引
>
> (6) create index 索引名 on 表名 (字段) reverse; //反转索引，消除热点块
>
> (7) create index 索引名 on 表名(字段) local;   //创建分区本地索引
>
> (8) create index 索引名 on 表名 (字段) global;   //创建分区全局索引

### 1.2.11、集合操作

> UNION： 去除重复记录。
>
> UNION  ALL  ：保留重复记录。
>
> INTERSECT   取交集
>
> MINUS  取差集

### 1.2.12、其他

非相关子查询当作一张表来用：

```sql
SELECT a.last_name, a.salary, a.department_id, b.salavg
  FROM employees a,(SELECT department_id, 
		   	 AVG(salary) salavg
          	     FROM employees
                     GROUP BY department_id) b
 WHERE a.department_id = b.department_id
   AND a.salary > b.salavg;
```

in 和 exists

>    1.子查询结果集小，用IN
>    2.外表小，子查询表大，用EXISTS
>
> IN一般是用于非相关子查询，而EXISTS一般用于相关子查询。

## 1.3、数据库设计规范

1.	每一个Table都应该有primary key，除非有足够合理的理由；
2.	每一个Table都应该用id字段表示记录，除非有足够合理的理由；
3.	字段长度应该尽量长，除非有足够合理的理由；
    如description字段， 我们要求定义为varchar2(1000);
    所有user name相关的信息，如 buyer，sales，我们要求定义为varchar2(100);
4.	英语是数据库对象命名的缺省语言，不允许在命名中使用拼音；除了模块名之外，命名尽量使用全称，除非长度不够；如果使用简称，也应按照一定的规范进行，下面给出几种常用的简称建议：
    TRANSCATION  - TRX
    JOURNAL ENTRY - JE
    REPORT  -  RPT
    APPROVAL -  APPRV
5.	名称一般应使用复数，除非明确知道表中仅有一条记录。

----

### 1.3.1、PLSQL命名规则

**程序命名规则：**

1.	DB function的PLSQL程序，使用P_<function code>方式命名
    如库存过账程序 function p_inv25001
2.	PLSQL package类型的程序， 使用 <package name>_pkg方式命名。
3.	Trigger类型的程序，使用<trigger name>_trg方式。

**变量命名规则：**
以下对三类plsql变量的命名做出规定

1.	函数或过程的形参： p<variable_name>
如 FUNCTION P_inv25001(pnode varchar2,pmasuser varchar2,…)
2.	package或function的局部变量：v<variable_name>, variable_name的第一个字母大写
如 vItemnum, 
3.	package的全局变量：g<variable_name>，variable_name的第一个字母大写

### 1.3.2、PLSQL注释规则

除了程序中常规注释外，对所有的PLSQL程序应按照下列要求加上注释，这主要为了明确的记录程序的更改历史
1.	行注释
在对程序作出重要更改后，在被修改的行右侧加上注释，形式为
    --modified by xxxx, xxxx-xx-xx,  [修改原因]

2.	头注释
      在程序修改测试通过后，须在程序头上按下列方式记录更改历史
        create or replace function .....
       /*
      
        *** xxxx-xx-xx modified by  xxxx
      
      mod1, [brief description]       
      
      mod2, [brief description]
      
        *** xxxx-xx-xx modified by  yyyy
        1.   mod1, [brief description]
        2.   mod2, [brief description]
      
        *** xxxx-xx-xx modified by  zzzz
        1.   mod1, [brief description]
        2.   mod2, [brief description]
      
       */
      begin
      
      end;
