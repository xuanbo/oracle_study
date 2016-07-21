#	PLSQL变量
##	1.PLSQL的变量类型
*	系统内置的常规简单变量类型： 比如大多数数据库表的字段类型都可以作为变量类型；
*	用户自定义复杂变量类型： 比如记录类型；
*	引用类型：保存了一个指针值；
*	大对象类型（ LOB）：保存了一个指向大对象的地址；

##	2.PLSQL的变量声明
###	2.1声明变量
*	语法:


    identifier [CONSTANT] datatype [NOT NULL]
    [:= | DEFAULT expr];


*	举例:


    DECLARE
        v_hiredate DATE;
        v_deptno NUMBER(2) NOT NULL := 10;
        v_location VARCHAR2(13) := 'Atlanta';
        c_comm CONSTANT NUMBER := 1400;


###	2.2	声明跟某个类型一致的变量`%TYPE`
PLSQL特有的`%TYPE`属性来声明与XX类型一致的变量类型:

    identifier Table.column_name%TYPE;

例如:

	v_name employees.last_name%TYPE;
    v_min_balance v_balance%TYPE := 10;

#####	`DBMS_OUTPUT.PUT_LINE()`介绍
使用DBMS_OUTPUT.PUT_LINE () 输出变量的值：

    DECLARE
      v_date_of_joining DATE;
    BEGIN
      SELECT e.hire_date
        INTO v_date_of_joining
        FROM employees e
       WHERE e.employee_id = 100;
      DBMS_OUTPUT.put_line(v_date_of_joining);
    END;

##	3.PLSQL中的注释语句
*	多行注释类似于java或者C, 使用/\* ... \*/
*	单行注释是在语句后面使用–

##	4.SQL函数在PLSQL的过程语句中的使用
*	大多数SQL函数都可以在PLSQL的过程语句中使用，比如：单行的数值和字符串函数、数据类型转换函数、日期函数、时间函数、求最大、最小值的`GREATEST`, `LEAST`函数等
*	但有些函数在PLSQL的过程语句中是不能使用的，比如：`Decode`函数、分组函数（`AVG`,`MIN`,`MAX`,`COUNT`,`SUM`,`STDDEV`,and`VARIANCE`）等

##	5.块嵌套和变量范围
PLSQL的块是可以嵌套的，变量的作用范围与其他语言类似

    ...
    x BINARY_INTEGER;
    BEGIN
    ...
        DECLARE
        	y NUMBER;
        BEGIN
        	y:= x;
        END;
    ...
    END;

**变量限定词**： 假设我们在块嵌套的程序中，里层和外层有相同的变量声明，而里层的程序要访问外层的同名变量，该怎么办呢？

    <<outer>>
    DECLARE
    	birthdate DATE;
    BEGIN
    ....
        DECLARE
        	birthdate DATE;
        BEGIN
        	...
            outer.birthdate := TO_DATE('03-AUG-1976', 'DD-MON-YYYY');
        END;
    ....
    END;