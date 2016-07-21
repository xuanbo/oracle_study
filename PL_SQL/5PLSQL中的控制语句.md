#	PLSQL中的控制语句
和其他语言一样，控制主要包括**判断**和**循环**

##	1.判断语句
判断语句的语法与其他语言类似：
```
IF condition THEN
	statements;
[ELSIF condition THEN
	statements;]
[ELSE
	statements;]
END IF;
```

```
CASE selector
    WHEN expression1 THEN result1
    WHEN expression2 THEN result2
    ...
    WHEN expressionN THEN resultN
    [ELSE resultN+1;]
END;
```

需要注意的是对`NULL`的判断处理：

| AND | TRUE | FALSE | NULL |
|--------|--------|--------|--------|
|    TRUE    |    TRUE    |    FALSE    |    NULL    |
|    FALSE    |    FALSE    |    FALSE    |    FALSE    |
|    NULL    |    NULL    |    FALSE    |    NULL    |

| OR | TRUE | FALSE | NULL |
|--------|--------|--------|--------|
|    TRUE    |    TRUE    |    TRUE    |    TRUE    |
|    FALSE    |    TRUE    |    FALSE    |    NULL    |
|    NULL    |    TRUE    |    NULL    |    NULL    |

| NOT |  |
|--------|--------|
|    TRUE    |    FALSE    |
|    FALSE    |    TRUE    |
|    NULL    |    NULL    |

**例如:**
```
declare
  increment employees.salary%type;
  fandept   employees.department_id%type;
  lname constant employees.last_name%type default 'KingA';
begin
  select e.department_id
    into fandept
    from employees e
   where e.last_name = lname;
  if fandept = 10 then
    increment := 200;
  elsif fandept = 20 then
    increment := 300;
  else
    increment := 400;
  end if;
  update employees e
     set e.salary = e.salary + increment
   where e.last_name = lname;
  commit;
exception
  when no_data_found then
    Dbms_Output.put_line('not data found');
end;
```
**注意:**是`elsif`而不是`elseif`。

##	2.循环语句
循环语句的语法与其他语言类似：有**基本循环**、**For循环**、**Wihle循环**三种。
```
LOOP
	statement1;
    . . .
    EXIT [WHEN condition];
END LOOP;
```
```
WHILE condition LOOP
    statement1;
    statement2;
    . . .
END LOOP;
```
```
FOR counter IN [REVERSE]
    	lower_bound..upper_bound LOOP
    statement1;
    statement2;
    . . .
END LOOP;
```
**例如:**
 直到型循环：
```
declare
  start_num t_test.acount%type := 200;
  count_num t_test.acount%type := 10;
begin
  loop
    insert into t_test (acount) values (start_num);
    start_num := start_num + 1;
    count_num := count_num - 1;
    exit when count_num = 0;
  end loop;
end;
```
for循环：
```
declare
  start_num t_test.acount%type := 300;
begin
  for v_count in 0 .. 10 loop
    insert into t_test (acount) values (start_num);
    start_num := start_num + 1;
  end loop;
end;
```
while循环：
```
declare
  start_num t_test.acount%type := 400;
  count_num t_test.acount%type := 10;
begin
  while (count_num > 0) loop
    insert into t_test (acount) values (start_num);
    start_num := start_num + 1;
    count_num := count_num - 1;
  end loop;
end;
```

##	3.嵌套循环和Label
```
...
BEGIN
    <<Outer_loop>>
    LOOP
        v_counter := v_counter+1;
        EXIT WHEN v_counter>10;
        <<Inner_loop>>
        LOOP
            ...
            EXIT Outer_loop WHEN total_done = 'YES';
            -- Leave both loops
            EXIT WHEN inner_done = 'YES';
            -- Leave inner loop only
            ...
        END LOOP Inner_loop;
        ...
    END LOOP Outer_loop;
END;
```
Label一般用不着，只有在使用goto语句，或者内部循环需要访问外部的同名变量的时候才需要，而一般这种做法也是不被提倡的。

`goto`语句:
```
declare
  summ number;
  i    number(3) := 100;
begin
  summ := 0;
  <<label>>
  summ := summ + i;
  i    := i - 1;
  if i > 0 then
    goto label;
  end if;
  dbms_output.put_line(summ);
end;
```

嵌套循环：
```
create table tt(t1 number(10), t2 number(10), t3 number(10));

declare
  v_t1 number(10) := 0;
  v_t2 number(10) := 0;
  v_t3 number(10) := 0;
  j    number := 0;
begin
  while (j < 10) loop
    j := j + 1;
    for i in 1 .. 5 loop
      v_t1 := v_t1 + 1;
      v_t2 := v_t2 + 2;
      v_t3 := v_t3 + 3;
      insert into tt values (v_t1, v_t2, v_t3);
    end loop;
  end loop;
end;
```