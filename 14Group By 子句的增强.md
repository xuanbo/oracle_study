#	Group By 子句的增强
##	1.Rollup
在`Group By`中使用`Rollup`产生常规分组汇总行，以及分组小计：

    SELECT department_id, job_id, SUM(salary)
    FROM employees
    WHERE department_id < 60
    GROUP BY ROLLUP(department_id, job_id);

![rollup图解](https://github.com/xuanbo/oracle_study/raw/master/rollup.PNG)
`Rollup`后面跟了n个字段，就将进行n+1次分组，从右到左每次减少一个字段进行分组；然后进行`union`

##	2.Cube
在`Group By`中使用`Cube`产生`Rollup`结果集 + 多维度的交叉表数据源:

    SELECT department_id, job_id, SUM(salary)
    FROM employees
    WHERE department_id < 60
    GROUP BY CUBE (department_id, job_id) ;

![cube图解](https://github.com/xuanbo/oracle_study/raw/master/cube.PNG)
`Cube`后面跟了n个字段，就将进行2的N次方的分组运算，然后进行

##	3.GROUPING
`GROUPING`函数：`Rollup`和`Cube`有点抽象，他分别相当于n+1 和 2的n次方常规`Group by`运算；那么在`Rollup`和`Cube`的结果集中如何很明确的看出哪些行是针对那些列或者列的组合进行分组运算的结果的？ 答案是可以使用`Grouping`函数； 没有被`Grouping`到返回1，否则返回0

    SELECT department_id DEPTID, job_id JOB,
    SUM(salary),
    GROUPING(department_id) GRP_DEPT,
    GROUPING(job_id) GRP_JOB
    FROM employees
    WHERE department_id < 50
    GROUP BY ROLLUP(department_id, job_id);

![grouping图解](https://github.com/xuanbo/oracle_study/raw/master/grouping.PNG)
第1行， department_id 和 job_id都被用到了，所以都返回0; 第2行, job_id 没有被用到，所以返回1；第3行，department_id 和job_id 都没有被用到，所以都返回1

##	4.Grouping Sets
使用`Grouping Sets`来代替多次`UNION`:

    SELECT department_id, job_id,
    manager_id,avg(salary)
    FROM employees
    GROUP BY GROUPING SETS ((department_id,job_id), (job_id,manager_id));

![grouping_sets图解](https://github.com/xuanbo/oracle_study/raw/master/grouping_sets.PNG)
