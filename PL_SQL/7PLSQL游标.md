#	PLSQL中的游标
##	1.游标概论
游标是一个私有的SQL工作区域，Oracle数据库中有两种游标，分别是隐式游标和显式游标，隐式游标不易被用户和程序员察觉和意识到，实际上Oracle服务器使用隐式游标来解析和执行我们提交的SQL语句； 而显式游标是程序员在程序中显式声明的；通常我们说的游标均指显式游标。

**隐式游标的几个有用属性：**

| 属性 | 说明 |
|--------|--------|
|    SQL%ROWCOUNT    |    受最近的SQL语句影响的行数    |
|    SQL%FOUND    |    最近的SQL语句是否影响了一行以上的数据    |
|    SQL%NOTFOUND    |    最近的SQL语句是否未影响任何数据    |
|    SQL%ISOPEN    |    对于隐式游标而言永远为FALSE    |

**显式游标：**
对于返回多行结果的SQL语句的返回结果，可使用显式游标独立的处理器中每一行的数据。

##	2.显式游标
在程序中对显式游标控制的一般过程：

*	声明游标
*	打开游标
*	提取当前行到变量
*	关闭游标

**举例：**
```
DECLARE
  row       employees%rowtype;
  increment number(4);

  CURSOR cursor_emp IS
    SELECT * FROM employees;
	-- 声明游标
BEGIN

  OPEN cursor_emp;
  -- 打开游标

  LOOP

    FETCH cursor_emp
      INTO row;
      -- 提取当前行到变量

    if row.department_id = 10 then
      increment := 100;
    elsif row.department_id = 20 then
      increment := 200;
    else
      increment := 300;
    end if;
    update employees
       set salary = salary + increment
     where last_name = row.last_name;
    Exit when cursor_emp%NOTFOUND;
  END LOOP;

  CLOSE cursor_emp;
	-- 关闭游标

END;
```

#####	游标For循环
如果你觉得像前面那个例子那样对一个游标的遍历很麻烦的话，可以考虑使用For循环，For循环省去了游标的声明、打开、提取、测试、关闭等语句，对程序员来说很方便，语法如下：
```
FOR record_name IN cursor_name LOOP
    statement1;
    statement2;
    . . .
END LOOP;
```
使用方式一：
```
BEGIN
    FOR emp_record IN (SELECT last_name, department_id
    FROM employees) LOOP
        -- implicit open and implicit fetch occur
        IF emp_record.department_id = 80 THEN
        	...
        END IF;
    END LOOP; -- implicit close occurs
END;
```
使用方式二：
```
DECLARE
    CURSOR emp_cursor IS
        SELECT last_name, department_id
        FROM employees;
BEGIN
    FOR emp_record IN emp_cursor LOOP
    -- implicit open and implicit fetch occur
        IF emp_record.department_id = 80 THEN
        	...
        END IF;
    END LOOP; -- implicit close occurs
END;
```

#####	带参数的游标
```
DECLARE
    CURSOR emp_cursor(
    	p_deptno NUMBER,
        p_job VARCHAR2
    ) IS
        SELECT employee_id, last_name
        FROM employees
        WHERE department_id = p_deptno
        AND job_id = p_job;
BEGIN
    OPEN emp_cursor (80, 'SA_REP');
    	. . .
    CLOSE emp_cursor;

    OPEN emp_cursor (60, 'IT_PROG');
    	. . .
    CLOSE emp_cursor;
END;
```

#####	`FOR UPDATE NOWAIT`语句
有的时候我们打开一个游标是为了更新或者删除一些记录，这种情况下我们希望在打开游标的时候即锁定相关记录，应该使用`for update nowait`语句，倘若锁定失败我们就停止不再继续，以免出现长时间等待资源的死锁情况。
```
. . .
DECLARE
    CURSOR emp_cursor IS
        SELECT employee_id, last_name, department_name
        FROM employees, departments
        WHERE employees.department_id = departments.department_id
        	AND employees.department_id = 80
        FOR UPDATE OF salary NOWAIT;
. . .
```

#####	`WHERE CURRENT OF cursor`语句
我们经常要逐条处理游标中的每一条记录，在循环体内做`Update`或者`Delete`时需要有`Where`指向游标的当前记录， 有没有简单一点的的`Where`条件写法呢？
```
DECLARE
    CURSOR sal_cursor IS
        SELECT e.department_id, employee_id, last_name, salary
        FROM employees e, departments d
        WHERE d.department_id = e.department_id and d.department_id = 60
        FOR UPDATE OF salary NOWAIT;
BEGIN
    FOR emp_record IN sal_cursor LOOP
        IF emp_record.salary < 5000 THEN
            UPDATE employees
            SET salary = emp_record.salary * 1.10
            WHERE CURRENT OF sal_cursor;
        END IF;
    END LOOP;
END;
```