#	程序块结构语言PLSQL的组成
##	1.PL/SQL块的组成
*	PL/SQL语言以块为单位，块中可以嵌套子块
*	一个基本的PL/SQL块由3部分组成：定义部分（`DECLARE`），可执行部分（`BEGIN`），异常处理部分（`EXCEPTION`）。

##	2.PL/SQL 语句块结构
![PL/SQL 语句块结构](https://github.com/xuanbo/oracle_study/raw/master/PL_SQL/png/2/PL_SQL_Lan_structure.PNG)

例子：

    DECLARE
      v_date_of_joining DATE;
    BEGIN
      SELECT e.hire_date INTO v_date_of_joining FROM employees e WHERE e.employee_id = 100;
      DBMS_OUTPUT.put_line(v_date_of_joining);
    EXCEPTION
      WHEN no_data_found THEN
        DBMS_OUTPUT.PUT_LINE('No data found');
    END;

##	3.块类型
*	匿名


    [DECLARE]

    BEGIN

      --statements

    [EXCEPTION]

    END;


*	过程


    PROCEDURE name
    IS

    BEGIN
      --statements

    [EXCEPTION]

    END;


*	函数


    FUNCTION name
    RETURN datatype
    IS
    BEGIN
      --statements
      RETURN value;
    [EXCEPTION]

    END;
