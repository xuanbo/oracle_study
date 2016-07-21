#	PLSQL中的复杂自定义数据类型
##	1.概述
PLSQL中常用的自定义类型就两种： **记录类型**、**PLSQL内存表类型**（根据表中的数据字段的简单和复杂程度又可分别实现类似于简单数组和记录数组的功能）

##	2.记录类型
#####	记录类型的定义语法
```
TYPE type_name IS RECORD(field_declaration[, field_declaration]…);

identifier type_name;
```
#####	记录类型的定义举例
```
...
TYPE emp_record_type IS RECORD(
	last_name VARCHAR2(25),
	job_id VARCHAR2(10),
	salary NUMBER(8,2)
    );
emp_record emp_record_type;
...
```
#####	`%ROWTYPE`属性
在PLSQL中`%ROWTYPE`表示某张表的记录类型或者是用户指定以的记录类型，使用此属性可以很方便的定义一个变量，其类型与某张表的记录或者自定义的记录类型保持一致。极大的方便了`Select * into ….`的语句使用。
```
DECLARE
  --定义一个记录类型变量
  EMP_VALUE employees%ROWTYPE;
BEGIN
  SELECT * INTO emp_value FROM employees WHERE employee_id = 100;
  --输出变量的值
  DBMS_OUTPUT.PUT_LINE(emp_value.last_name);
END;
```

##	3.PLSQL内存表
PLSQL内存表即`Index By Table`, 这种结构类似于数组，使用主键提供类似于数组那样的元素访问。这种类型必须包括两部分：
1.	使用`BINARY_INTEGER`类型构成的索引主键
2.	另外一个简单类型或者用户自定义类型的字段作为具体的数组元素。这种类型可以自动增长，所以也类似于可变长数组。

#####	语法
```
TYPE type_name IS TABLE OF
{column_type | variable%TYPE
| table.column%TYPE} [NOT NULL]
| table.%ROWTYPE
[INDEX BY BINARY_INTEGER];
identifier type_name;
```

#####	举例
```
...
TYPE ename_table_type IS TABLE OF employees.last_name%TYPE INDEX BY BINARY_INTEGER;
ename_table ename_table_type;
...
```

#####	PLSQL内存表应用举例
下面定义的两个内存表中的元素都是简单数据类型，所以相当于定义了两个简单数组:
```
DECLARE

    TYPE ename_table_type IS TABLE OF employees.last_name%TYPE INDEX BY BINARY_INTEGER;

    TYPE hiredate_table_type IS TABLE OF DATE INDEX BY BINARY_INTEGER;

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
**备注：**对PLSQL内存表中某个元素的访问类似于数组，可以使用下表，因为`BINARY_INTEGER`这种数据类型的值在[-2147483647, 2147483647]范围内，所以下表也可以在这个范围内。

下面定义的两个内存表中的元素记录类型，所以相当于定义了真正的内存表:
```
DECLARE
    TYPE emp_table_type is table of employees%ROWTYPE INDEX BY BINARY_INTEGER;
    my_emp_table emp_table_type;
    v_count NUMBER(3):= 104;
BEGIN
    FOR i IN 100 .. v_count LOOP
        SELECT * INTO my_emp_table(i)
        FROM employees
        WHERE employee_id = i;
    END LOOP;

    FOR i IN my_emp_table.FIRST .. my_emp_table.LAST LOOP
    	DBMS_OUTPUT.PUT_LINE(my_emp_table(i).last_name);
    END LOOP;
END;
```