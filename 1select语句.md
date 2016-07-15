#	SELECT 语句
##	1.格式
	SELECT * | {[DISTINCT] column | expression [alias],...}  
	FROM table;	
	
*	SELECT 表示要取哪些列
*	FROM 表示要从哪些表中取

SQL语句中的数学表达式：对于数值和日期型字段，可以进行 “加减乘除”   
例如：  
	SELECT name, salary, salary + 300 FROM employees;
*结果：*  
<table>
	<tr>
		<td>name</td>
		<td>salary</td>
		<td>last_name</td>
	</tr>
	<tr>
		<td>张三</td>
		<td>5000</td>
		<td>5300</td>
	</tr>
	<tr>
		<td>李四</td>
		<td>6000</td>
		<td>6300</td>
	</tr>
</table>

##	2.关于NULL的概念
NULL表示 不可用、未赋值、不知道、不适用 ， 它既不是0 也不是空格。一个数值与NULL进行四则运算，其结果是NULL。

##	3.字符串连接操作符： "||"
	SELECT name || ' salary is ' || salary as INFO   
	FROM employees;   
*结果：*  
<table>
	<tr>
		<td>INFO</td>
	</tr>
	<tr>
		<td>张三 salary is 5000</td>
	</tr>
	<tr>
		<td>李四 salary is 6000</td>
	</tr>
</table>

##	4.DISTINCT 去除重复行
	SELECT salary FROM employees; 默认情况，返回所有行，包括重复行  
*结果：*  
 <table>
	<tr>
		<td>salary</td>
	</tr>
	<tr>
		<td>5000</td>
	</tr>
	<tr>
		<td>6000</td>
	</tr>
	<tr>
		<td>5000</td>
	</tr>
	<tr>
		<td>6000</td>
	</tr>
</table>

	SELECT DISTINCT salary FROM employees; 使用DISTINCT消除重复结果行		

*结果：*  
 <table>
	<tr>
		<td>salary</td>
	</tr>
	<tr>
		<td>5000</td>
	</tr>
	<tr>
		<td>6000</td>
	</tr>
</table>

##	5.组合查询下回分解