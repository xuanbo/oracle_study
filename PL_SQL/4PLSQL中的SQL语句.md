#	PLSQL中的SQL语句
##	1.`SELECT INTO`语句
用于把从数据库查询出内容存入变量
```
DECLARE
  v_date_of_joining DATE;
BEGIN
  SELECT e.hire_date
    INTO v_date_of_joining
    FROM employees e
   WHERE e.employee_id = 100;
  DBMS_OUTPUT.put_line(v_date_of_joining);
EXCEPTION
  WHEN no_data_found THEN
    DBMS_OUTPUT.PUT_LINE('No data found');
END;
```
将查出的值赋给`v_date_of_joining`这个变量。

**注意点：**该语句支持单行的查询结果，
如果`where`条件控制不好，导致多行查询结果，则会引发`Too_many_rows`的例外

##	2.`INSERT`语句
```
BEGIN
    INSERT INTO employees(employee_id, first_name, last_name, email, hire_date, job_id, salary)
    VALUES
    (employees_seq.NEXTVAL, 'Ruth', 'Cores', 'RCORES',
    sysdate, 'AD_ASST', 4000);
END;
```

##	3.`UPDATE`语句
```
DECLARE
    v_sal_increase employees.salary%TYPE := 800;
BEGIN
    UPDATE employees
        SET salary = salary + v_sal_increase
    WHERE job_id = 'ST_CLERK';
END;
```

##	4.`DELETE`语句
```
DECLARE
	v_deptno employees.department_id%TYPE := 10;
BEGIN
    DELETE FROM employees
    WHERE department_id = v_deptno;
END;
```

##	5.`MERGE`语句
```
DECLARE
	v_empno employees.employee_id%TYPE := 100;
BEGIN
	MERGE INTO copy_emp c
	USING employees e
	ON (e.employee_id = v_empno)
	WHEN MATCHED THEN
        UPDATE SET
            c.first_name = e.first_name,
            c.last_name = e.last_name,
            c.email = e.email,
            . . .
	WHEN NOT MATCHED THEN
        INSERT VALUES(e.employee_id, e.first_name, e.last_name,
        . . .,e.department_id);
END;
```