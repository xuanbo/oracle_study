#	分组计算函数和GROUP BY子句
##	1.分组计算函数(常用)

*	求和 (SUM)
*	求平均值(AVG)
*	计数（COUNT）
*	求标准差(STDDEV)
*	求方差(VARIANCE)
*	求最大值(MAX)
*	求最小值(MIN)

SQL中使用分组计算函数 的语法

	SELECT [column,] group_function(column), ...
	FROM table
	[WHERE condition]
	[GROUP BY column]
	[ORDER BY column];

##	2.使用GROUP BY 子句进行分组
可以按照某一个字段分组，也可以按照多个字段的组合进行分组

	select  a.department_id, avg(a.salary)
	from A a
	group by a.department_id

*注意：*

*	使用分组后，select中列出的列命不能是未分组的字段，但可以是对未分组的字段用统计函数进行计算
*	不能在Where 条件中使用分组计算函数表达式，当出现这样的需求的时候，使用Having 子句。
*	分组计算函数也可嵌套使用。
*	使用group by后order by应该放在其后


