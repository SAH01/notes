# 汉得培训2）PLSQL

规范：不超过30，字母开头，不分大小写，不用 ‘-’

数据类型：

```bash
VARCHAR2 (maximum_length)
NUMBER [(precision, scale)]
DATE
CHAR [(maximum_length)]
LONG
LONG RAW
BOOLEAN
BINARY_INTEGER
PLS_INTEGER
ROWID
```

异常：

```sql
sys_raise_app_error_pkg.raise_sys_others_error(p_message=> '币种不能重复',
                                               p_created_by=> 1,
                                               p_package_name=> 'prj_project_xxx_pkg',
                                               p_procedure_function_name => 'lock_hls_bp_master');
                                               
     raise_application_error(sys_raise_app_error_pkg.c_error_number,
                              sys_raise_app_error_pkg.g_err_line_id);
```

匿名：

```sql
Declare
  test1 varchar2(10) := '靠谱杨';
  v_sname student_777.sname%type := '佚名';
Begin
  select s.sname
  into v_sname
  from STUDENT_777 s where s.sno = '101';
  dbms_output.put_line('这是测试直接赋值:  ' || test1);
  dbms_output.put_line('这是测试查询赋值:  ' || v_sname);
Exception
  when no_data_found then
    dbms_output.put_line('未找到！');
End;
/* 这是测试直接赋值:  靠谱杨
这是测试查询赋值:  李军 */
```

函数：

```sql
-- 传入两个参数,返回最大值
CREATE OR REPLACE FUNCTION FUN_MAX (P_NUM1 IN NUMBER, 
									P_NUM2 IN NUMBER DEFAULT 99)
RETURN NUMBER    -- 函数的返回类型
IS
BEGIN
	IF P_NUM1>P_NUM2 THEN 
		RETURN P_NUM1;	
	ELSE 
		RETURN P_NUM2; 
	END IF;
END;

-- 三种调用方式

SELECT FUN_MAX(20) FROM DUAL; ---创建时给了默认值

-- 或者
BEGIN
  dbms_output.put_line(FUN_MAX(12,20));
END;

-- 或者
DECLARE
  v_num NUMBER;
BEGIN
  v_num := FUN_MAX(12,20);
  dbms_output.put_line(v_num);
END;
-- 输出 99
```

【SELECT 1 FROM DUAL （dual在oracle中是一张伪表，帮助完成一个select语句。）】

存储过程：

```sql
/*CREATE [OR REPLACE] PROCEDURE 过程名[(参数1 [IN|OUT|IN OUT] 数据类型,
									 参数2 [IN|OUT|IN OUT] 数据类型……)]
IS|AS
PL/SQL过程体;*/
--
-- 不能给函数和存储过程的参数指定数据的大小或者精度
create or replace procedure test_p (str1  in varchar2,
                                    str2  in varchar2)
    as begin
       DBMS_OUTPUT.PUT_LINE('str1:' || str1);
       DBMS_OUTPUT.PUT_LINE('str2:' || str2);
    end test_p;
-- 调用
declare
    s1 varchar2(10) := '测试1';
    s2 varchar2(10) := '测试2';
begin
    test_p(s1,s2);
end;
/*
str1:测试1
str2:测试2
*/

-- in 和 out

create or replace procedure test_p (pstr1  in varchar2,
                                    pstr2  out varchar2)
    as begin
       DBMS_OUTPUT.PUT_LINE('这是传入值pstr1:' || pstr1);
       select s.sname into pstr2 from STUDENT_777 s where s.sno = '101';
    end test_p;
-- 调用
declare
    s1 varchar2(10) := '测试1';
    s2 varchar2(10);
begin
    test_p(s1,s2);
    DBMS_OUTPUT.PUT_LINE('这是返回值:' || s2);
end;

/*
这是传入值pstr1:测试1
这是返回值:李军
*/
```

# 1、PLSQL变量和SQL语句

```sql
-- [变量名] [数据类型（大小）] := [初始值]
v_sal_increase employees.salary%TYPE := 800;

-- &p_annual_sal 在Plsql Developer的SQL window 执行环境中，可用于提示用户输入一个具体的值。
v_sal NUMBER(9,2) := &p_annual_sal;

-- 输出
DBMS_OUTPUT.PUT_LINE('111')
```

系统内置变量类型：

**BOOLEAN**

**数值**

**DATE**

大对象BLOB

BLOB

**字符串**

**BFILE**

![image-20230309092632535](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142254683.png)



# 2、控制语句

选择

```sql
-- 中间的条件是ELSIF 最后一个条件是ELSE 最后加END IF;
IF condition THEN
statements;
[ELSIF condition THEN
statements;]
[ELSE
statements;]
END IF;

-- 最后加 ELSE[condition] END；
CASE selector
WHEN expression1 THEN result1
WHEN expression2 THEN result2
...
WHEN expressionN THEN resultN
[ELSE resultN+1;]
END;
```

AND：FALSE>NULL>TRUE

OR：TRUE>NULL>FALSE

NOT：NULL -> NULL

![image-20230308092715037](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142254570.png)



**循环**

```sql
-- 基本循环、For循环、Wihle循环 结尾都是END LOOP;
LOOP
statement1;
. . .
EXIT WHEN [condition];
END LOOP;

FOR counter IN [REVERSE 上下限] LOOP
statement1;
statement2;
. . .
END LOOP;

WHILE condition LOOP
statement1;
statement2;
. . .
END LOOP;
```



for 循环两种

1、指定循环 1...10

```sql
FOR i IN 1..3 LOOP
    INSERT INTO locations(location_id, city, country_id)
    VALUES((v_location_id + i), v_city, v_country_id );
END LOOP;
```

跳出循环 EXIT WEHEN  [code];

2、游标循环

# 3、复杂数据类型

【简单理解：记录是多维数组，表是一维数组。】

**记录类型**:其类型与某张表的记录或者自定义的记录类型保持一致，极大地方便了INSERT语句的使用。

```sql
DECLARE
	emp_rec employees%ROWTYPE;
BEGIN
	SELECT * INTO emp_rec
	FROM employees
	WHERE employee_id = &employee_number;
    INSERT INTO retired_emps(empno, ename, job, mgr, hiredate,
    leavedate, sal, comm, deptno)
    VALUES (emp_rec.employee_id, emp_rec.last_name, emp_rec.job_id,
    emp_rec.manager_id, emp_rec.hire_date, SYSDATE, emp_rec.salary,
    emp_rec.commission_pct, emp_rec.department_id);
    COMMIT;
END;
```



**PLSQL内存表**类型（根据表中的数据字段的简单和复杂程度又可分别实现类似于简单数组和记录数组的功能）

```sql
-- 记录
-- %ROWTYPE属性：%ROWTYPE表示某张表的记录类型或者是用户指定的记录类型
TYPE emp_record_type IS RECORD
(last_name VARCHAR2(25),
job_id VARCHAR2(10),
salary NUMBER(8,2));
emp_record emp_record_type;

-- 内存表【相当于数组，一维单一数据类型】
/*PLSQL内存表即 Index By Table , 这种结构类似于数组，使用主键提供类似于数组那样的元素访问。*/

TYPE ename_table_type IS TABLE OF employees.last_name%TYPE
	INDEX BY BINARY_INTEGER;
ename_table ename_table_type;

--
DECLARE
	TYPE ename_table_type IS TABLE OF
	employees.last_name%TYPE
	INDEX BY BINARY_INTEGER;
	
	TYPE hiredate_table_type IS TABLE OF DATE
	INDEX BY BINARY_INTEGER;
	
	ename_table ename_table_type;
	hiredate_table hiredate_table_type;
BEGIN
	ename_table(1) := 'CAMERON';
	hiredate_table(8) := SYSDATE + 7;
	IF ename_table.EXISTS(1) THEN
	INSERT INTO ...
...
END;
```

表操作：

```sql
count：返回PL/SQL表中行的当前数目。
delete：删除表中的行。
exists：如果指定的表项在表中存在那么返回ture。
first：返回表中第一行的索引。
last：返回表中最后一行的索引。
next：返回表中指定行的下一行的索引。
prior：返回表中指定行的上一行的索引
```



# 4、游标

1、隐式游标

Oracle服务器使用隐式游标来解析和执行我们提交的SQL语句；

几个属性：

> SQL%ROWCOUNT 	受最近的SQL语句影响的行数
>
> SQL%FOUND 	最近的SQL语句是否影响了一行以上的 数据
> SQL%NOTFOUND	 最近的SQL语句是否未影响任何数据
>
> SQL%ISOPEN 	对于隐式游标而言永远为FALSE 【是否打开游标】

2、显式游标

![image-20230308100201623](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142254982.png)

==for 循环+游标(游标可以带参数)==：

```sql
DECLARE
	CURSOR emp_cursor IS
		SELECT last_name, department_id
		FROM employees;
BEGIN
	FOR emp_record IN emp_cursor LOOP
	-- implicit open and implicit fetch occur
	IF emp_record.department_id = 80 THEN
	...
	END LOOP; -- implicit close occurs
END;
```



-------



==FOR UPDATE NOWAIT语句==：有的时候我们打开一个游标是为了更新或者删除一些记录，这种情况下我们希望在打开游标的时候即锁定相关记录，应该使用`for update nowait`语句，倘若锁定失败我们就停止不再继续，以免出现长时间等待资源的死锁情况。

==当前游标位置==：WHERE CURRENT OF cursor_name

```sql
SET salary = emp_record.salary * 1.10
WHERE CURRENT OF sal_cursor;
```

# 5、例外处理

> 1、Oracle 内部错误抛出的例外：
>
> 这又分为预定义例外（有错误号+常量定义）
>
> 非预定义例外（仅有错误号，无常量定义）
> 2、程序员显式的抛出的例外

```sql
-- 预定义的意外
/*
not data_found（没有找到数据）
too_many_rows（select...into语句匹配多个行）
zero_divide（被零除）
value_error（算术或转换错误）
timeout_on_resource（在等待资源时发生超时）：发生的典型场景就是分布式数据库
*/

ACCESS_INTO_NULL                     未定义对象  
CASE_NOT_FOUND                       CASE 中若未包含相应的 WHEN ，并且没有设置 ELSE 时  
COLLECTION_IS_NULL                  集合元素未初始化  
CURSER_ALREADY_OPEN              游标已经打开  
DUP_VAL_ON_INDEX                    唯一索引对应的列上有重复的值  
INVALID_CURSOR                        在不合法的游标上进行操作  
INVALID_NUMBER                        内嵌的 SQL 语句不能将字符转换为数字  
NO_DATA_FOUND                        使用 select into 未返回行，或应用索引表未初始化的元素时  
TOO_MANY_ROWS                       执行 select into 时，结果集超过一行  
ZERO_DIVIDE                              除数为 0  
SUBSCRIPT_BEYOND_COUNT        元素下标超过嵌套表或 VARRAY 的最大值  
SUBSCRIPT_OUTSIDE_LIMIT         使用嵌套表或 VARRAY 时，将下标指定为负数  
VALUE_ERROR                             赋值时，变量长度不足以容纳实际数据  
LOGIN_DENIED                            PL/SQL 应用程序连接到 oracle 数据库时，提供了不正确的用户名或密码  
NOT_LOGGED_ON                        PL/SQL 应用程序在没有连接 oralce 数据库的情况下访问数据  
PROGRAM_ERROR                        PL/SQL 内部问题，可能需要重装数据字典＆ pl./SQL 系统包  
ROWTYPE_MISMATCH                  宿主游标变量与 PL/SQL 游标变量的返回类型不兼容  
SELF_IS_NULL                             使用对象类型时，在 null 对象上调用对象方法  
STORAGE_ERROR                        运行 PL/SQL 时，超出内存空间  
SYS_INVALID_ID                         无效的 ROWID 字符串  
TIMEOUT_ON_RESOURCE          Oracle 在等待资源时超时 

```

![image-20230308101629717](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142254627.png)

==OTHERS的处理==，Oracle 提供了两个内置函数 `SQLCODE` 和 `SQLERRM` 分别用来返回Oracle 错误号和错误描述。

v_error_code := SQLCODE ;	-- **错误号**
v_error_message := SQLERRM ; 	-- **错误信息**

```sql
WHEN OTHERS THEN
ROLLBACK;
v_error_code := SQLCODE ;
v_error_message := SQLERRM ;
INSERT INTO errors
VALUES(v_error_code, v_error_message);
```



==非oracle预定义例外==：

PRAGMA pragma 杂注

```sql
DEFINE p_deptno = 10
DECLARE
	e_emps_remaining EXCEPTION;	-- 自定义例外名
	PRAGMA EXCEPTION_INIT (e_emps_remaining, -2292); -- 指定desc和code
BEGIN
	DELETE FROM departments
	WHERE department_id = &p_deptno;
	COMMIT;
EXCEPTION
	WHEN e_emps_remaining THEN
		DBMS_OUTPUT.PUT_LINE ('Cannot remove dept ' ||
		TO_CHAR(&p_deptno) || '. Employees exist. ');
	WHEN OTHERS THEN
        ROLLBACK;
        v_error_code := SQLCODE ;
        v_error_message := SQLERRM ;
        INSERT INTO errors
        VALUES(v_error_code, v_error_message);
END;
```

用户自定义例外：

```sql
EXCEPTION
	WHEN NO_DATA_FOUND THEN
		RAISE_APPLICATION_ERROR (-20201,
		'Manager is not a valid employee.');
```

# 6、存储过程和函数

## 6.1、存储过程

in 可赋值 可提供默认值 default xxx 【默认模式】

out 不可赋默认值

in out 不可赋默认值

```sql
/*CREATE [OR REPLACE] PROCEDURE 过程名[(参数1 [IN|OUT|IN OUT] 数据类型,参数2 [IN|OUT|IN OUT] 数据类型……)]
IS|AS
PL/SQL过程体;*/

-- 不能给函数和存储过程的参数指定数据的大小或者精度
create or replace procedure test_p (str1  in varchar2,
                                    str2  in varchar2)
    as begin
       DBMS_OUTPUT.PUT_LINE('str1:' || str1);
       DBMS_OUTPUT.PUT_LINE('str2:' || str2);
    end test_p;
-- 调用
declare
    s1 varchar2(10) := '测试1';
    s2 varchar2(10) := '测试2';
begin
    test_p(s1,s2);
end;
/*输出
str1:测试1
str2:测试2
*/

-- in 和 out

create or replace procedure test_p (pstr1  in varchar2,
                                    pstr2  out varchar2)
    as begin
       DBMS_OUTPUT.PUT_LINE('这是传入值pstr1:' || pstr1);
       select s.sname into pstr2 from STUDENT_777 s where s.sno = '101';
    end test_p;
-- 调用
declare
    s1 varchar2(10) := '测试1';
    s2 varchar2(10);
begin
    test_p(s1,s2);
    DBMS_OUTPUT.PUT_LINE('这是返回值:' || s2);
end;

/*
这是传入值pstr1:测试1
这是返回值:李军
*/
```

一个例子：

```sql
CREATE OR REPLACE PROCEDURE query_emp
	(p_id IN employees.employee_id%TYPE default 123,
	p_name OUT employees.last_name%TYPE,
	p_salary OUT employees.salary%TYPE,
	p_comm OUT employees.commission_pct%TYPE)
IS BEGIN
	SELECT last_name, salary, commission_pct
	INTO p_name, p_salary, p_comm
	FROM employees
	WHERE employee_id = p_id;
END query_emp;
```

存储过程参数传递：

![image-20230309103805463](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142255707.png)

删除存储过程：DROP PROCEDURE procedure_name

## 6.2、函数

```sql
CREATE OR REPLACE FUNCTION get_sal
(p_id IN employees.employee_id%TYPE)
	RETURN NUMBER
IS
	v_salary employees.salary%TYPE :=0;
BEGIN
	SELECT salary
	INTO v_salary
	FROM employees
	WHERE employee_id = p_id;
	RETURN v_salary;
END get_sal;
```

哪些SQL语句可以使用函数：

> Select 语句
> Where条件和Having子句
> CONNECT BY, START WITH, ORDER BY, 和GROUP BY 子句
> INSERT的Values子句
> UPDATE的Set子句

定义在SQL中使用的函数的限制：

![image-20230308110116710](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142255772.png)

## 6.3、权限

定义者权限 ：函数执行时，对表的访问默认使用定义者权限。

`AUTHID CURRENT_USER`关键字以调用者权限访问，只能调用函数，没有权限访问表。

# 7、包

![image-20230309104612526](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142255528.png)

![image-20230308111322069](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142255128.png)

Package中的向前声明特性：在Package body中，一个函数中调用另一个函数（也在该Package中），则另一个函数必须在前面先定义；如果你非要调用在程序代码中后定义的函数，可把这个函数设置成公有函数，在包说明部分说明；



==Package中的初始化过程代码：==

> Package中可以写一段初始化过程代码，这段代码只是一个Session中加载时被执行一次，一般用于一些复杂变量的初始化（比如某个公有变量的初始化值是需要通过一段负责的SQL来获取的）；如果我们没有这样的初始化代码需求，那么一般可以在这部分写个NULL;就可以了。

![image-20230308112341977](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142255450.png)

Package中变量持久状态：

全局变量的值不能直接在包外访问。

不同session变量互不影响。

# 8、动态SQL

动态SQL可以使用Oracle 内置包 **DBMS_SQL** 来执行，也可以使用EXECUTE IMMEDIATE 语句来执行。

```sql

-- DBMS_SQL
CREATE OR REPLACE PROCEDURE delete_all_rows
(p_tab_name IN VARCHAR2, p_rows_del OUT NUMBER)
IS
cursor_name INTEGER;
BEGIN
	cursor_name := DBMS_SQL.OPEN_CURSOR;
	DBMS_SQL.PARSE(cursor_name, 'DELETE FROM '||p_tab_name,DBMS_SQL.NATIVE );
	p_rows_del := DBMS_SQL.EXECUTE (cursor_name);
	DBMS_SQL.CLOSE_CURSOR(cursor_name);
END;

-- EXECUTE IMMEDIATE
CREATE PROCEDURE del_rows
(p_table_name IN VARCHAR2,
p_rows_deld OUT NUMBER)
IS
BEGIN
	EXECUTE IMMEDIATE 'delete from'||p_table_name;
	p_rows_deld := SQL%ROWCOUNT;
END;
```

# 9、DDL

如果想在程序中执行DDL,可使用Oracle 内置包：**DBMS_DDL**。

# 10、UTL_FILE读写文件

```sql
create directory hand_file_test_ycw as  'D:\HandFileTest';
--
create or replace procedure sal_status_pro(p_filedir  IN VARCHAR2,
                                           p_filename IN VARCHAR2) is
  v_filehandle UTL_FILE.FILE_TYPE;
  CURSOR emp_info IS -- 创建游标
    SELECT last_name, salary, department_id
      FROM employees
     ORDER BY department_id;
  v_newdeptno employees.department_id%TYPE;
  v_olddeptno employees.department_id%TYPE := 0;
BEGIN
  v_filehandle := UTL_FILE.FOPEN(p_filedir, p_filename, 'w'); -- 打开文件
  UTL_FILE.PUTF(v_filehandle, 'SALARY REPORT: GENERATED ON %s\n', SYSDATE);
  UTL_FILE.NEW_LINE(v_filehandle);
  FOR v_emp_rec IN emp_info LOOP
    v_newdeptno := v_emp_rec.department_id;
    IF v_newdeptno <> v_olddeptno THEN
      UTL_FILE.PUTF(v_filehandle,
                    'DEPARTMENT: %s\n',
                    v_emp_rec.department_id);
    END IF;
    UTL_FILE.PUTF(v_filehandle,
                  ' EMPLOYEE: %s earns: %s\n',
                  v_emp_rec.last_name,
                  v_emp_rec.salary);
    v_olddeptno := v_newdeptno;
  END LOOP;
  UTL_FILE.PUT_LINE(v_filehandle, '*** END OF REPORT ***');
  UTL_FILE.FCLOSE(v_filehandle); -- 关闭
EXCEPTION
  WHEN UTL_FILE.INVALID_FILEHANDLE THEN
    RAISE_APPLICATION_ERROR(-20001, 'Invalid File.');
  WHEN UTL_FILE.WRITE_ERROR THEN
    RAISE_APPLICATION_ERROR(-20002, 'Unable to write to file');
end sal_status_pro;

--
begin
  sal_status_pro(p_filedir  => hand_file_test_ycw,
                 p_filename => 'HandTechFile1.txt');
end;
```

# 11、大对象操作

> Oracle数据库里面的LOB有四种类型：
> 1、CLOB ：字符大对象，存储在数据库内部；
> 2、NCLOB：多字节字符大对象，存储在数据库内部；
> 3、BLOB：二进制大对象，存储在数据库内部；
> 4、BFILE：二进制文件，存储在数据库外部；

# 12、触发器

> 数据库触发器 Trigger的概念：
> 对数据库对象的操作可引发很多事件，比如before Insert,before update 等等，但这些事件产生的时候我们可以写响应代码来完成一些基于事件的操作，通常这些操作被写成一段Plsql程序；那么这些更具体的数据库对象上的事件相关的程序呢就称为数据库Trigger
>
> **做一件事的时候自动触发另一件事的发生。**

```sql
创建Trigger： Trigger的定义语句里面涉及到如下关键因素：
时机：Before 或者 After 或 Instead of
事件：Insert 或 Update 或 Delete
对象：表名（或视图名）
类型：Row 或者 Statement级；
条件：满足特定Where条件才执行；
内容：通常是一段PLSQL块代码；
重点注意：
Instead of : 用Trigger的内容替换 事件本身的动作
Row级：SQL语句影响到的每一行都会引发Trigger
Statement级：一句SQL语句引发一次，不管它影响多少行（甚至0行）
```

# 13、数据库依赖关系

> 数据库里面有些对象的定义是依赖于其他对象的，比如View 依赖于Table , Procedure 中有Select 其他Table或者视图的语句，可能依赖于Table 或者视图； 在这里**View 或者 Procedure就被称为依赖对象**，而**table则被称为引用对象**。

![image-20230308143701413](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202303142255773.png)



-------

# 14、【PLSQL代码开发规范】

不建议把SQL语句写在一行上。

```sql
SELECT column_name
INTO v_variable
FROM table_name;
```

## 14.1、包头

1、包头命名规则

- 全拼英文单词
- 以_分词
- 以_pkg结尾

2、包头信息

- 创建作者、时间和目标等信息。

    - \- - Author : jinxiao.lin

          -- Created : 2008-2-21 10:02:14
          
          -- Purpose : BGT609.fmb 预算日记账导入

- 后来修改者的痕迹

    - \- -jinxiao.lin 2008-03-14，Purpose:yy {

          function check_journal_interface(p_batch_id number)  return number;- -}

## 14.2、包体

全局变量以g_开头。

==必备的几个全局变量==：

`g_error_log  varchar2(2000);` 主要用来输出程序出错时的错误信息，方面追踪和调试。

`g_transaction_logic_error_desc  varchar2(2000);` 主要用来提示业务逻辑的错误信息。它是**g_error_log**的一个组成部分，出现在log日志。

`g_this_package_name varchar2(30) := <PACKAGE名>; ` 在程序错误的时候使用，它是**g_error_log**一个组成部分，用于迅速指定出错的PACKAGE名称，方便定位。

`g_form_error_message_code  varchar2(30);`主要用来提示PACKAGE运行错误信息。用来返回给FORM前台，mas_message_pkg.mas_error所使用。

==编程模式：Function和Procedure的选择==

> PACKAGE代码逻辑可以严格区分为两层，传统MVC（Model-View-Controller）模式中的MODEL层和CONTROLLER层。
>
> MODEL层：每一个数据库相关原子操作CRUD（Create, read, update and delete）。都独立封装为一个function。
>
> CONTROLLER层：即业务逻辑层。调用MODEL层方法，不出现直接操作数据库的程序语句。全部以procedure的方式来写。

**CRUD -> FUNCTION**

**业务逻辑 -> PROCEDURE**

此种处理方式，严格封装了数据库操作，实现了数据库原子操作和业务逻辑处理的分离。

---

==PACKAGE 必备方法==：

```sql
  procedure txn_log(p_log_text in varchar2) is
    pragma autonomous_transaction;
  begin
    if g_enable_log = 'Y' then
      insert into bgt_budget_error_logs
        (user_id, message,message_date)
      values
        (sys_global.user_id,p_log_text, sysdate);
      commit;
    end if;
  end txn_log;
  -- 此方法在异常发生时被调用，它记录所有异常发生的情景。
```

## 14.3、Function编写规范

主要用于封装数据库原子操作。

命名：全拼英文，以_分开。

return：规定number【0正常，-2未找到数据，-1重大错误】

参数传递：读取和返回变量，全部使用参数，返回值接口统一。

参数命名：全拼英文，p_开头，用下划线分割。

局部变量：全拼英文，v_开头。

==必备局部变量==：`v_this_method_name constant varchar2(30) :=<function名>`此局部变量在程序错误的时候使用，它是g_error_log一个组成部分，用于迅速指定出错的FUNCTION名称，方便定位。

`v_return number := 0;`

==异常处理模板：==

```sql
exception
    when no_data_found then
      g_error_log := g_this_package_name || '.' || v_this_method_name || ': ' ||
                     sqlerrm || ' . ' ||
                     dbms_utility.format_error_backtrace;
      txn_log(g_error_log);
      v_return := -2;
      return v_return;
    when others then
      g_error_log := g_this_package_name || '.' || v_this_method_name || ': ' ||
                     sqlerrm || ' . ' ||
                     dbms_utility.format_error_backtrace;
      txn_log(g_error_log);
      v_return := -1;
      return v_return;
/*
	即不发生异常时，返回0。
	发生no_data_found exception时，返回-2。
	发生others exception时，返回-1。
*/

```

==一个完整的function实例：==

```sql
 function get_company_id(p_company_code varchar2, p_company_id out number)
    return number is
    v_return number := 0;
    v_this_method_name constant varchar2(30) := 'GET_COMPANY_ID';
  begin
    --通过公司code来获取公司id，公司code是唯一索引
    select t.company_id
      into p_company_id
      from fnd_companies t
     where t.company_code = p_company_code;
    return v_return;
  exception
    when no_data_found then
      g_error_log := g_this_package_name || '.' || v_this_method_name || ': ' ||
                     sqlerrm || ' . ' ||
                     dbms_utility.format_error_backtrace;
      txn_log(g_error_log);
      v_return := -2;
      return v_return;
    when others then
      g_error_log := g_this_package_name || '.' || v_this_method_name || ': ' ||
                     sqlerrm || ' . ' ||
                     dbms_utility.format_error_backtrace;
      txn_log(g_error_log);
      v_return := -1;
      return v_return;
  end get_company_id;
```

## 14.4、procedure编写规范

前面同Function

==异常处理==：

Function对应的数据库原子操作，它的异常处理通常是在本方法内捕获，然后以返回值的方式，通知它的调用方法。

Procedure对应的业务逻辑操作，**它的异常处理是主动抛出**，代表了此业务逻辑错误会导致程序无法继续执行，所有的procedure的异常由PACKAGE入口方法统一捕获和处理。

