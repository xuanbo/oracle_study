#	条件限制和排序
##	1.条件限制的关键词：WHERE

	SELECT *|{[DISTINCT] column|expression [alias],...}  
	FROM table  
	[WHERE condition(s)];	
比如:  
	SELECT name, department_id FROM employees WHERE department_id = 90 ;  
结果:	

<table>
	<tr>
		<td>name</td>
		<td>department_id</td>
	<tr>
	<tr>
		<td>张三</td>
		<td>90</td>
	<tr>
	<tr>
		<td>李四</td>
		<td>90</td>
	<tr>
</table>

##	2.比较操作符：
<table>
	<tr>
		<td>比较操作符</td>
		<td>意义</td>
	</tr>
	<tr>
		<td>=</td>
		<td>等于</td>
	</tr>
	<tr>
		<td>&gt;</td>
		<td>大于</td>
	</tr>
	<tr>
		<td>&gte;</td>
		<td>大于等于</td>
	</tr>
	<tr>
		<td>&lt;</td>
		<td>小于</td>
	</tr>
	<tr>
		<td>&lte;</td>
		<td>小于等于</td>
	</tr>
	<tr>
		<td>&gt;&lt;</td>
		<td>不等于</td>
	</tr>
	<tr>
		<td>BETWEEN ...AND...</td>
		<td>两个值之间</td>
	</tr>
	<tr>
		<td>IN(set)</td>
		<td>在集合范围内以逗号隔开</td>
	</tr>
	<tr>
		<td>like</td>
		<td>匹配字符串，%通配符，匹配任意字符串；_占位符，代表一个任意字符</td>
	</tr>
	<tr>
		<td>IS NULL</td>
		<td>是一个空值</td>
	</tr>
</table>
例如：
<pre>SELECT name, salary FROM employees WHERE salary <= 3000;</pre>
结果：
<table>
	<tr>
		<td>name</td>
		<td>salary</td>
	</tr>
	<tr>
		<td>张三</td>
		<td>2000</td>
	<tr>
	<tr>
		<td>李四</td>
		<td>2500</td>
	<tr>
</table>
如果要搜索统配符本身该怎么办呢？  
这需要使用ESCAPE 标识转义字符：<pre>select * from t_char where a like „%\%%' escape '\';</pre>

##	3.使用ORDER BY 子句进行排序：
*	ASC:升序	(默认，可以省略不写)
*	DEDC:倒序	
例如：	  
	SELECT name, department_id, salary
	FROM employees
	ORDER BY department_id, salary DESC;
结果:	

<table>
	<tr>
		<td>name</td>
		<td>department_id</td>
		<td>salary</td>
	</tr>
	<tr>
		<td>张三</td>
		<td>1</td>
		<td>3000</td>
	</tr>
	<tr>
		<td>李四</td>
		<td>1</td>
		<td>2500</td>
	</tr>
	<tr>
		<td>王五</td>
		<td>2</td>
		<td>5000</td>
	</tr>
	<tr>
		<td>赵六</td>
		<td>2</td>
		<td>4000</td>
	</tr>
</table>
