# MySQL基础

[TOC]




##  数据库的好处

- 持久化数据到本地

- 可以实现结构化查询，方便管理

  

## 数据库相关概念

- DB：数据库，保存一组有组织的数据的容器

- DBMS：数据库管理系统，又称为数据库软件（产品），用于管理DB中的数据

- SQL：结构化查询语言，用于和DBMS通信的语言

  

## 数据库存储数据的特点

- 将数据放到表中，表再放到库中

- 一个数据库中可以有多个表，每个表都有一个的名字，用来标识自己。表名具有唯一性。

- 表具有一些特性，这些特性定义了数据在表中如何存储，类似java中 “类”的设计。

- 表由列组成，我们也称为字段。所有表都是由一个或多个列组成的，每一列类似java 中的”属性”

- 表中的数据是按行存储的，每一行类似于java中的“对象”。

  

## DBMS分类

- 基于共享文件系统的DBMS（Access）

- 基于CS架构，客户机--服务器的DBM（MySQL、Oracle、SqlServer）

  

## MySQL产品的介绍和安装

### MySQL服务的启动和停止

- 方式一：计算机——右击管理——服务

- 方式二：通过管理员身份运行

    ```mysql
    net start 服务名（启动服务） -> net start MySQL）
    net stop 服务名（停止服务）
    ```

  

### MySQL服务的登录和退出   

**登陆前首先确保服务是启动的**	

- 方式一：通过mysql自带的客户端（只限于root用户，不灵活，不推荐）

- 方式二：通过windows自带的客户端
  - 登录：mysql 【-h主机名 -P端口号 】-u用户名 -p密码
  - 退出：exit或ctrl+C

    ```
     mysql -h localhost -P 3306 -u root -p密码 （密码和p之间不能够有空格）
     mysql -u root -p1243 (连接本机，可以缩写)	
    ```



### MySQL的常见命令 

```mysql
# 查看当前所有的数据库
show databases;
# 打开指定的库
use 库名;
# 查看当前库的所有表
show tables;
# 查看其它库的所有表
show tables FROM 库名; #但并不会前往所查看的库
# 查看目前在库里的位置
select database();
# 创建表
create table 表名(

列名 列类型,
列名 列类型，
。。。
);
# 查看表结构
desc 表名; # describe
# 查看表的数据
select * FROM 表名；
```

查看服务器的版本

- 方式一：登录到mysql服务端

    ```mysql
    select version();
    ```

- 方式二：没有登录到mysql服务端，直接在cmd

     ```
     mysql --version
     mysql --V
     ```


### MySQL的语法规范

- 不区分大小写,但建议关键字大写，表名、列名小写

- 每条命令最好用分号结尾

- 每条命令根据需要，可以进行缩进或换行

- 注释

  ```
  单行注释：#注释文字
  单行注释：-- 注释文字（注意--和注释文字之间的空格）
  多行注释：/* 注释文字  */
  ```



### SQL的语言分类

- DQL（Data Query Language）：数据查询语言
select 

- DML(Data Manipulate Language):数据操作语言
insert 、update、delete

- DDL（Data Define Languge）：数据定义语言
create、drop、alter

- TCL（Transaction Control Language）：事务控制语言
  commit、rollback

  

## DQL语言学习



### DQL语言执行顺序总结

```mysql
SELECT 查询列表         7.
FROM 表                1.
连接类型 JOIN 表2       2.这一步生成笛卡尔乘积后的两表
ON 连接条件             3.用连接条件对笛卡尔乘积表进行筛选
WHERE 筛选条件          4.
GROUP BY 分组列表       5.
HAVING 分组后的筛选条件  6.对分组后的表进行再筛选
ORDER BY 排序列表       8.
LIMIT 偏移，条目数      9..
```

### 进阶1：基础查询

#### 基本语法

```mysql
SELECT 查询列表（要查询的东西）#类似于Java中 :System.out.println(要打印的东西);
FROM 表名;
```

#### 特点

- 通过select查询完的结果 ，是一个虚拟的表格，不是真实存在

- 要查询的东西 可以是常量值、可以是表达式、可以是字段、可以是函数

#### 查询举例

##### select 基本用法

```mysql
USE myemployees; #打开要查询的库

# 查询表中的单个字段
SELECT 字段名 FROM 表名;
SELECT last_name FROM employees

# 查询表中的多个字段
SELECT 字段名，字段名 FROM 表名;
SELECT last_name, salary, email FROM employees;

# 查询表中的所有字段
SELECT * FROM 表名
SELECT * FROM employees;

# 在搜索时可以通过 `字段`，将字段和关键字进行区分
SELECT 
  `department_name`,`manager_id`,`location_id` 
FROM
  departments ;
  
# 查询常量值
SELECT 常量值; # 注意：字符型和日期型的常量值必须用单引号引起来，数值型不需要
SELECT 100;
SELECT 'john';

# 查询表达式
SELECT 100%98;

# 查询函数
SELECT 函数名(实参列表);
SELECT VERSION();
```

##### 查询并给变量起别名

```mysql
/*
①便于理解
②如果要查询的字段有重名的情况，使用别名可以区分开
*/
# 方式一： 使用AS关键字
SELECT 100%98 AS 结果;
SELECT last_name AS 姓, first_name AS 名 FROM employees;

# 方式二： 使用空格
SELECT last_name 姓, first_name 名 FROM employees;

# 案例：查询salary，显示结果为out put
SELECT salary AS "out put" FROM employees; -- 通过 " " 将与关键词重复的别名
```

##### 查询时去除重复

```mysql
# 使用DISTINCT修饰字段去重
# 案例： 查询员工表中涉及到的所有的部门编号
SELECT DISTINCT department_id FROM employees;
```

##### + 号的作用

```mysql
/*
java中的+号：
①运算符，两个操作数都为数值型
②连接符，只要有一个操作数为字符串

mysql中的+号：
仅仅有一个功能：运算符

*/
# 两个操作数都为数值型，则做加法运算
sellect 100 + 90;

# 其中一方为字符型，则试图将字符型数值转换成数值型
# 如果转换成功，则继续做加法运算
SELECT '123' + 90;

# 如果抓转换失败，则将字符型数值转换成0
SELECT 'john' + 90;	

# 只要其中一方为null，则结果肯定为null
SELECT null + 10;             
```

##### CONCAT的用法

```mysql
/* 
功能：拼接字符
SELECT concat(字符1，字符2，字符3,...);
*/

# 案例：查询员工名和姓，并拼接成一个字段，显示为姓名

SELECT 
	CONCAT(last_name, first_name) AS 姓名 
FROM
	employees;
```

##### IFNULL/ ISNULL的用法

```mysql
/*
ifnull函数
功能：判断某字段或表达式是否为null，如果为null 返回指定的值，否则返回原本的值
SELECT ifnull(commission_pct,0) FROM employees;

isnull函数
功能：判断某字段或表达式是否为null，如果是，则返回1，否则返回0
*/

SELECT 
	CONCAT(`employee_id`,',',`first_name`,',',`last_name`,',',`email`,',',IFNULL(commission_pct,0))
	AS 'OUT_PUT'
FROM employees;

SELECT 
	CONCAT(`employee_id`,',',`first_name`,',',`last_name`,','
	,`email`,',',IS NULL(commission_pct))
	AS 'OUT_PUT'
FROM employees;
```



### 进阶2：条件查询

条件查询：根据条件过滤原始表的数据，查询到想要的数据

#### 基本语法

```mysql
SELECT 
要查询的字段|表达式|常量值|函数
FROM 
表
WHERE   # 类似于Java中的if
条件 ;
# 执行顺序 FROM -> WHERE -> SELECT
```

#### 条件分类

```mysql
一、条件表达式
示例：salary>10000

条件运算符：
> < >= <= = 
!= <> # 这两个都是不等于

二、逻辑表达式
示例：salary>10000 && salary<20000

逻辑运算符：用于连接条件表达式

and（&&）:两个条件如果同时成立，结果为true，否则为false
or(||)：两个条件只要有一个成立，结果为true，否则为false
not(!)：如果条件成立，则not后为false，否则为true

三、模糊查询

```

#### 按条件表达式筛选

```mysql
# 案例：查询部门编号不等于90号的员工名和部门编号
SELECT 
	last_name, department_id
FROM 
	employees
WHERE 	
	department_id <> 90;

```

#### 按逻辑表达式筛选

```mysql
# 案例：查询部门编号不是在90到110之间，或者工资高于15000的员工信息
SELECT 
	*
FROM 
	employees
WHERE 	
	NOT(department_id<=110 AND department_id>=90) OR salary>15000;
```

#### 模糊查询

##### LIKE和通配符的用法

```mysql
/*
like:一般搭配通配符使用，可以判断字符型或数值型
通配符 % ：任意多个字符，可以为0个字符
_  ：任意单个字符
*/

# 案例1： 查询员工名中包含字符a的员工信息
SELECT 
	first_name
FROM 
	employees
WHERE 	
	first_name LIKE '%a%'
	
# 案例2： 查询员工名中的第三个字符为n，第五个字符为l的员工名和工资
SELECT 
	first_name,salary
FROM 
	employees
WHERE 	
	first_name LIKE '__n_l%'
	
# 案例3： 查询员工姓中第二个字符为_的员工名
SELECT 
	last_name
FROM 
	employees
WHERE 	
	last_name LIKE '_$_%' ESCAPE '$'; #推荐使用 ESCAPE 来标识转义字符\  
    #last_name LIKE '_\_%'
# 案例4：用like查询数值型，查询部门编号大于等100的员工信息
SELECT 
	*
FROM 
	employees
WHERE 
	department_id LIKE '1__';
```

##### BETWEEN AND用法

```mysql
/*
between and 
①使用between and 可以提高语句的简洁度
②between a and b 包含两个临界值a，b
③临界值a，b顺序颠倒，查询条件不一样
④between and  等价于  查询内容 >= a and 查询内容 <= b;
扩展：not between and  a <  and > b
*/

# 案例: 查询员工编号在100到200之间的员工信息
# 以下为两种等价方式
SELECT 
	*
FROM 
	employees
WHERE 	
	employee_id >= 100 AND employee_id <= 120;
#-----------------------------------------------

SELECT 
	*
FROM 
	employees
WHERE 	
	employee_id BETWEEN 100 AND 120;

```

##### IN的用法

```mysql
/*
in： 用与判断某字段的值是否属于in列表中的一项
①使用in提高语句的简洁度
②in列表的值类型必须统一或兼容（123， '123'）
③不支持通配符
*/

# 案例：查询员工的工种编号是 IT_PROG或者AD_VP或者AD_PRES的员工名和对应的工种编号
SELECT 
	last_name, job_id
FROM 
	employees
WHERE 	
	job_id IN('IT_PROG', 'AD_VP', 'AD_PRES'); 
	# 等价于 job_id = 'IT_PROG' or job_id = 'AD_VP' or job_id = 'AD_PRES'
	
```

##### IS NULL /  IS NOT NULL / <=>的用法

```mysql
/*
is null /is not null：用于判断是否为null值
!= 与<> 不能够用于判断NULL值
*/
# 案例：查询没有奖金的员工名和奖金率
SELECT 
	last_name
FROM 
	employees
WHERE 	
	commission_pct IS NULL;

/*
IS NULL: 仅仅可以判断NULL， 可读性较高，建议使用
<=>： 既可以判断NULL值，又可以判断普通的数值，可读性较低
*/

is null PK <=> 

			普通类型的数值	null值		可读性
is null		×			√		√
<=>		√			√		×
```



### 进阶3：排序查询	

#### 基本语法	

```mysql
select
	要查询的东西
FROM
	表
where 
	条件
order by 排序的字段|表达式|函数|别名 asc / desc
```

#### 特点

- asc ：升序，如果不写默认升序
  desc：降序
- 排序列表 支持 单个字段、多个字段、函数、表达式、别名
- order by的位置一般放在查询语句的最后（除limit语句之外）

#### 查询举例

```mysql
# 案例：查询部门编号>=90的员工信息，按入职时间的先后进行排序【按筛选条件】
SELECT 
  * 
FROM
  employees 
WHERE department_id >= 90 
ORDER BY hiredate ASC 

# 案例：按年薪的高低显示员工的信息和年薪，年薪以表达式形式给出【按表达式排序】
SELECT 
  *, salary*12*(1 + IFNULL(commission_pct, 0)) 年薪
FROM
  employees 
ORDER BY salary*12*(1 + IFNULL(commission_pct, 0)) DESC 

# 案例：按年薪的高低显示员工的信息和年薪，年薪以表达式形式给出【按别名排序】
SELECT 
  *, salary*12*(1 + IFNULL(commission_pct, 0)) 年薪
FROM
  employees 
ORDER BY 年薪 DESC 

# 案例： 按姓名的长度显示员工的姓名和工资【按函数排序】
SELECT 
  LENGTH(last_name) 字节长度,
  last_name,
  salary 
FROM
  employees 
ORDER BY LENGTH(last_name) DESC;

# 案例：查询员工信息，要求先按工资升序，再按员工编号降序【按多个字段排序】
# 从前往后，在前一个字段相等时，再遵守后一个字段的排序规则
SELECT 
  * 
FROM
  employees 
ORDER BY salary ASC,
  employee_id DESC ;
```



### 进阶4：常见函数

概念： 类似于Java的方法，将一组逻辑语句封装在方法体中，对外暴露方法名

优点：

- 隐藏了实现细节
- 提高了代码的重用性

调用：select 函数名（实参列表）【from 表】

#### 单行函数1:字符函数
```mysql
#1. concat 拼接字符
SELECR CONCAT(last_name,'_',first_name) 姓名 FROM employee

#2. length 获取参数值的字节数
SELECT LENGTH('john')

SHOW VARIABLES LIKE '%char%' #显示当前环境的编码字符集

#3. upper转换成大写 lower转换成小写
# 将姓变大写，名变小写，然后实现拼接
SELECT CONCAT(UPPER(last_name),'_',LOWER(first_name)) 姓名 FROM employees

#4. substr/substring截取子串，索引从1开始
# 截取从指定索引出后面所有字符
SELECT SUBSTR('李莫愁爱上了陆展元', 7) out_put #截取‘陆展元’
# 截取从指定索引处指定字符长度的字符
SELECT SUBSTR('李莫愁爱上了陆展元', 1, 3) out_put #截取‘李莫愁’ 

#5. instr 返回子串在原字符串的第一个位置索引，若没有找到返回0
SELECT INSTR('杨不悔爱上了殷梨亭','殷梨亭') AS out_put 

#6. trim去前后指定的空格和字符/ltrim去左边空格/rtrim去右边空格
SELECT TRIM('a' FROM 'aaaaaaa张翠山aaaaa')； #可以指定删除的字符

#7. lpad/rpad右填充， 用指定的字符实现左右填充，字符过长也会根据填充长度截断
SELECT LPAD('樱花', 10, '*') AS out_put;

#8. replace 替换
SELECT REPLACE('张无忌爱上了周芷若张无忌爱上了周芷若', '周芷若', '赵敏') AS out_put;
```


#### 单行函数2:数学函数

```mysql
#1. round()(负数先绝对值)四舍五入，并能保留小数点
SELECT ROUND(1.2345, 2) --》 1.23

#2. ceil()向上取整,返回大于等于该参数的最小整数

#3. floor(0向下取整,返回小于等于该参数的最大整数

#4. truncate截断
SELECT TRUNCATE(1.2345, 2) #小数点后保留两位

#5. mod()取余数,

SELECT MOD(10,-3) # = 1，正负号根据被除数定   ---> a-a/b*b

```

#### 单行函数3:日期函数

```mysql
#1. now 返回当前系统日期+时间
SELECT NOW()

#2. curdate()返回当前系统日期，不包含时间
SELECT CURDATE();

#3. curtiem()  返回当前时间，不包含日期
SELECT CURTIME()

#4. 获取日期的指定部分
SELECT YEAR(NOW()) 年
SELECT MONTHNAME(NOW())

#5. str_to_date 将字符通过指定给是转换成日期
SELECT STR_TO_DATE('1998-3-2','%Y-%c-%d') AS out_put --> 1998-03-02
# 应用例子：查询指定入职日期的员工信息，日期输入格式为 4-3 1993
SELECT * FROM employees
WHERE hiredate = STR_TO_DATE('4-3 1992','%c-%d %Y')  

#6. date_format将日期转换成指定格式的字符
SELECT DATE_FORMAT(NOW(), '%y年%m月%d日') AS out_put

# monthname()：以英文形式返回月份
# datediff()：返回两个日期相差的天数
```

![日期格式](MySQL%E5%9F%BA%E7%A1%80.assets/%E6%97%A5%E6%9C%9F%E6%A0%BC%E5%BC%8F.png)

#### 单行函数4:其他函数

```mysql
	version()版本
	database()当前库
	user()当前连接用户
	password('字符')：返回该字符的密码形式
	md5('字符')：返回该字符的md5加密形式
```

#### 单行函数5: 分支判断函数

```mysql
#1. if 处理双分支
if(条件表达式，表达式1，表达是)：如果条件表达式城里，返回表达式1，否则返回表达式2

SELECT last_name, commission_pct, IF(commission_pct IS NULL, '没奖金，呵呵', '有奖金，哈哈') 备注
FROM employees

# case 处理多分支 
#情况1：处理等值判断
case 要判断的字段或者表达式
when 常量值1 then 要显示的值1
when 常量值2 then 要显示的值2
...
else 要显示的值n
end
/*
案例：查询员工的工资，要求
部门号=30，显示的工资为1.1倍
部门号=40，显示的工资为1.2倍
部门号=50，显示的工资为1.3倍
其他部门，显示的工资为原工资
*/
SELECT 
  salary 原工资,
  department_id,
  CASE
    department_id 
    WHEN 30 
    THEN salary * 1.1 
    WHEN 40 
    THEN salary * 1.3 
    WHEN 50 
    THEN salary 
    ELSE salary 
  END AS 新工资 
FROM
  employees ;
#情况2：处理条件判断，类似于 多重if实现区间判断
case 
when 条件1 then 要显示的值1或语句1；
when 条件值2 then 要显示的值2或语句2；
...
else 要显示的值n或语句n
end
/*
#案例：查询工资的工资情况
如果工资>20000， 显示A级别
如果工资>15000， 显示B级别
如果工资>10000， 显示C级别
否则，显示D级别
*/
SELECT 
  salary,
  CASE
    WHEN salary > 20000 
    THEN 'A' 
    WHEN salary > 15000 
    THEN 'B' 
    WHEN salary > 10000 
    THEN 'C' 
    ELSE 'D' 
  END 工资级别 
FROM
  employees 
```

#### 分组函数

功能：用作统计使用，又称为聚合函数或统计函数或组函数

特点：

- sum 求和 avg 平均值：这两个适用于处理数值型，
- max() 最大值 min() 最小值 count() 计算非空值的个数：可以处理任何类型
- 都可以搭配distinct使用，用于统计去重后的结果
- 所有分组函数在处理时都忽略NULL值
- 和分组函数一同查询的字段要求是group by后的字段

```mysql
#1. 简单使用
SELECT SUM(salary) 和, ROUND(AVG(salary), 2) 平均, MAX(salary)最高 FROM employees;

#2. 和distinct搭配
SELECT SUM(DISTINCT salary) FROM employees;

#3. count函数的详细介绍
#用于统计表的行数，运行逻辑为，统计表的每一行的所有字段，每行只要有一个字段非NULL，就会被统计上
SELECT COUNT(*) FROM employees;
#COUNT(字段、*、常量值，一般放1), 相当于在表内加了一列常量，并统计常量个数--》用于统计表的函数
SELECT COUNT(常量) FROM employees;

#总结：建议使用 count(*)用于统计行数

#案例：查询员工表中的最大入职时间和最小入职时间的相差天数
SELECT DATEDIFF(MAX(hiredate),MIN(hiredate)) AS DIFFRENCE
FROM employees;

#案例：查询部门编号为90的员工个数
SELECT COUNT(*) FROM employees
WHERE department_id = 90
```

### 进阶5：分组查询
语法：

SELECT  查询的字段，**分组函数**（要求出现在group by的后面）
FROM 表  
【where 筛选条件】
group by **分组的字段**
【having 分组后的筛选】  
【order by 子句】

**[执行顺序：from->where->group by-> having->order by->select]()**

注意：查询列表必须特殊，要求是分组函数和group by后出现的字段

特点：

- 可以按单个字段分组
- 和分组函数一同查询的字段最好是分组后的字段
- 分组函数做条件肯定是放在HAVING中

|              | 针对的表       | 位置           | 关键字 |
| ------------ | -------------- | -------------- | ------ |
| 分组前筛选： | 原始表         | group by的前面 | where  |
| 分组后筛选： | 分组后的结果集 | group by的后面 | having |

- 可以按多个字段分组，字段之间用逗号隔开（多个字段之间用逗号隔开，没有顺序要求）
- 可以支持排序
- HAVING后可以支持别名




```mysql
1. 简单的分组查询
    #案例： 查询每个工种的最高工资
    SELECT MAX(salary), job_id
    FROM employees
    GROUP BY job_id
```

#### 添加分组前的筛选

```mysql
#案例：查询邮箱中包含a字符的，每个部门的平均工资
SELECT AVG(salary), department_id
FROM employees
WHERE email LIKE '%a%'
GROUP BY department_id;
```

#### 添加分组后的筛选

```mysql
#案例：查询哪个部门的员工大于2
SELECT COUNT(*), department_id
FROM employees
GROUP BY department_id;
HAVING COUNT(*) > 2

#案例：查询每个工种有奖金的员工的最高工资  >12000的工种编号和最高工资
#①查询每个工种有奖金的员工的最高工资
#②最高工资>12000的工种编号和最高工资
SELECT MAX(salary), job_id
FROM employees
WHERE commission_pct IS NOT NULL
GROUP BY job_id
HAVING MAX(salary) > 12000;

#按表达式或函数分组
#案例：按员工姓名的长度分组，查询每一组的员工个数，筛选员工个数>5的有哪些
SELECT  COUNT(*), LENGTH(last_name)
FROM employees
GROUP BY LENGTH(last_name)
HAVING COUNT(*) > 5;

#按多个字段分组，并且支持排序
#案例：查询每个部门每个工种的员工的平均工资-->结果会是部门编号和工种编号相同的放在一起计算平均工资
SELECT job_id, AVG(salary), department_id
FROM employees
GROUP BY department_id, job_id

ORDER BY AVG(salary) DESC;



```

### 进阶6：多表连接查询

笛卡尔乘积现象：表1有m行，表2有n行，多表查询后有m*n行。

发生原因：当连接条件省略或无效则会出现

解决办法：添加上连接条件

```mysql
SELECT NAME, boyName FROM boys, beauty
WHERE beauty.boyfriend_id = boys.id; #通过表名.字段名来区分同名表
```

#### 连接查询的分类分类

- 按年代分类：
  - sql92：仅仅支持内连接
  - sql99：支持内连接+外连接（左外和右外）+交叉连接

- 按功能分：

  - 内连接：
    - 等值连接
    - 非等值连接
    - 自连接

  - 外连接：
    - 左外连接
    - 右外连接
    - 全外连接（MySQL不支持）
  - 交叉连接

#### SQL92语法

##### 等值连接

**语法**：

select 查询列表
from 表1 别名,表2 别名
where 表1.key=表2.key
【and 筛选条件】
【group by 分组字段】
【having 分组后的筛选】
【order by 排序字段】

- 等值连接的结果 = 多个表的交集
- .n表连接，至少需要n-1个连接条件
- 多个表不分主次，没有顺序要求，因为连接原理是每个表中的所有项都进行了一次匹配
- 一般为表起别名，提高阅读性和性能，但如果给表起了别名，就不能使用原来的表名去限定
- 可以搭配前面介绍的所有子句使用，比如排序、分组、筛选

```mysql
#案例：查询员工名和对应的部门名
SELECT last_name, department_name
FROM employees, departments
WHERE employees.`department_id` = departments.`department_id`;

#案例：查询城市名中第二字符为o的部门名和城市名
SELECT department_name, city
FROM departments d, locations l
WHERE d.`location_id` = l.`location_id`
AND city LIKE '_o%';

#多表查询可以分组，可以排序
#案例：查询每个工种的工种名和员工的个数，并且按员工个数降序
SELECT job_title, COUNT(*)
FROM employees e, jobs j
WHERE e.`job_id` = j.`job_id`
GROUP BY job_title
ORDER BY COUNT(*) DESC;
```

##### 非等值连接与自连接

**非等值连接语法：**

```mysql
select 查询列表
from 表1 别名,表2 别名
where 非等值的连接条件
【and 筛选条件】
【group by 分组字段】
【having 分组后的筛选】
【order by 排序字段】
```

**自连接语法：**

```mysql
select 查询列表
from 表 别名1,表 别名2
where 等值的连接条件
【and 筛选条件】
【group by 分组字段】
【having 分组后的筛选】
【order by 排序字段】
```

```mysql
1. 非等值连接
#案例1：查询员工的工资和工资级别
SELECT salary, grade_level
FROM employees e, job_grades g
WHERE salary BETWEEN g.`lowest_sal` AND g.`highest_sal`

2. 自连接，在同一张表找两遍
#案例：查询员工名和上级的名称
SELECT e.last_name, e.manager_id, m.employee_id, m.last_name 
FROM employees e, employees m
WHERE e.`manager_id` = m.`employee_id`
```

#### SQL99语法

##### 等值连接

```mysql
select 字段，...
FROM 表1
【inner|left outer|right outer|cross】join 表2 on  连接条件
【inner|left outer|right outer|cross】join 表3 on  连接条件
【where 筛选条件】
【group by 分组字段】
【having 分组后的筛选条件】
【order by 排序的字段或表达式】

#好处：语句上，连接条件和筛选条件实现了分离，简洁明了！
```

特点：

- inner可以省略
- 筛选条件放在where后面，连接条件放在on后面，提高分离性，便于阅读

```mysql
#案例：查询哪个部门的员工个数>3，显示其部门名和员工个数，并按照个数降序排列
SELECT department_name, COUNT(*) 员工个数
FROM employees e
INNER JOIN departments d
ON e.`department_id` = d.`department_id`
GROUP BY department_name
HAVING 员工个数 > 3
ORDER BY 员工个数 DESC;

#案例：查询员工名，部门名，工种名，并按部门名降序排列
SELECT 
  last_name,
  department_name,
  job_title 
FROM
  employees e 
  INNER JOIN departments d 
    ON e.`department_id` = d.`department_id` 
  INNER JOIN jobs j 
    ON e.`job_id` = j.`job_id` ;

SELECT DISTINCT A.nom, A.prenom
FROM abonne A INNER JOIN emprunt E ON A.id = E.id_abonne
INNER JOIN livre L ON L.id = E.id_livre
WHERE L.titre = 'Le Petit Prince';

```

##### 非等值连接与自连接

```mysql
1. 非等值连接
#案例：查询员工的工资级别
SELECT salary, grade_level
FROM employees e
JOIN job_grades g
ON e.`salary` BETWEEN g.`lowest_sal` AND g.`highest_sal`;

#案例：查询工资级别的个数>20的个数，并且按照工资级别降序
SELECT salary, grade_level, COUNT(*)
FROM employees e
JOIN job_grades g
ON e.`salary` BETWEEN g.`lowest_sal` AND g.`highest_sal`
GROUP BY grade_level
HAVING COUNT(*) > 20
ORDER BY grade_level DESC;

2.自连接
SELECT e.last_name, m.last_name
FROM employees e
INNER JOIN employees m
ON e.`manager_id` = m.`employee_id`;
```

##### 外连接

应用场景：用于查询一个表中有，但是另一个表中没有的记录

特点：

- 查询的结果=主表中所有的行，如果从表和它匹配的将显示匹配行，如果从表没有匹配的则显示null
- 左外连接，left join 左边的是主表
- 右外连接，right join 右边的是主表
- 全外连接，full join两边都是主表
- 左外和右外交换两个表的顺序，可以实现同样的效果
- 要查询的信息主要来自哪个表，则此表为主表

全外连接：内连接的结果+表1中有但表2没有+表2中有但表1中没有的

```mysql
#引入，查询男朋友 不在男神表的女神名
#左外连接实现
SELECT b.`name`, bo.*
FROM beauty b
LEFT OUTER JOIN boys bo
ON b.`boyfriend_id` = bo.`id`
WHERE bo.`id` IS NULL; #非空的筛选条件适合使用从表的主键列
#右外连接实现
SELECT b.`name`, bo.*
FROM boys bo
RIGHT OUTER JOIN beauty b
ON b.`boyfriend_id` = bo.`id`
WHERE bo.`id` IS NULL; #非空的筛选条件适合使用从表的主键列

#案例：查询哪个部门没有员工
SELECT d.*, e.*
FROM departments d
LEFT OUTER JOIN employees e
ON d.`department_id` = e.`department_id`
WHERE e.`employee_id` IS NULL
```

##### 交叉连接

把连接的表进行笛卡尔乘积

```
SELECT b.*, bo.*
FROM beauty b
CROSS JOIN boys bo;

```

#### 多表连接总结

![多表连接总结](MySQL%E5%9F%BA%E7%A1%80.assets/%E5%A4%9A%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%80%BB%E7%BB%93-1627231468485.png)

![多表连接总结2](MySQL%E5%9F%BA%E7%A1%80.assets/%E5%A4%9A%E8%A1%A8%E8%BF%9E%E6%8E%A5%E6%80%BB%E7%BB%932.png)

### 进阶7：子查询

含义：

- 出现在其他语句内部的select语句，称为子查询或内查询
- 在外面的查询语句，称为主查询或外查询

#### 子查询分类
1. 按出现位置分类

- select后面：
  - 仅仅支持标量子查询
- from后面：
  - 表子查询
- <u>***where或having后面：***</u>
  - *<u>**标量子查询**</u>*（单行）
  - <u>***列子查询***</u>（多行）
  - 行子查询
- exists后面：
  - 标量子查询
  - 列子查询
  - 行子查询
  - 表子查询

2. 按结果集的行列
   - 标量子查询（单行子查询）：结果集为一行一列
   - 列子查询（多行子查询）：结果集为多行一列
   - 行子查询：结果集为多行多列
   - 表子查询：结果集为多行多列

特点：

- 子查询都放在小括号内
- 子查询可以放在FROM后面、select后面、where后面、having后面，但一般放在条件的右侧
- 子查询优先于主查询执行，主查询使用了子查询的执行结果
- 子查询根据查询结果的行数不同分为以下两类：
  ① 单行子查询
  	结果集只有一行
  	一般搭配单行操作符使用：> < = <> >= <= 
  	非法使用标量子查询的情况：
  	a、子查询的结果为一组值
  	b、子查询的结果为空
  	
  ② 多行子查询
  	结果集有多行
  	一般搭配多行操作符使用：any/some、all、in、not in
  	in： 属于子查询结果中的任意一个就行
  	any和all往往可以用其他查询代替

#### WHERE与HAVING后面的子查询

##### 标量子查询

```mysql
1. 将子查询条件放在where后面
#案例：返回job_id与141号员工相同，salary比143号员工多的员工姓名，job_id 和工资
SELECT 
  last_name,
  job_id,
  salary 
FROM
  employees 
WHERE job_id = 
  (SELECT 
    job_id 
  FROM
    employees 
  WHERE employee_id = 141) 
  AND salary > 
  (SELECT 
    salary 
  FROM
    employees 
  WHERE employee_id = 143)
  
#返回公司工资最少的员工的last_name,job_id和salary
SELECT 
  last_name,
  job_id,
  salary 
FROM
  employees 
WHERE salary = 
  (SELECT 
    MIN(salary) 
  FROM
    employees)
2. 将子查询条件放在having后面
#案例：查询最低工资大于50号部门最低工资 的部门id和其最低工资
SELECT 
  department_id,
  MIN(salary) 
FROM
  employees 
GROUP BY department_id 
HAVING MIN(salary) > 
  (SELECT 
    MIN(salary) 
  FROM
    employees 
  WHERE department_id = 50) ;
```

##### 列子查询（多行子查询）

![多行比较操作符](MySQL%E5%9F%BA%E7%A1%80.assets/%E5%A4%9A%E8%A1%8C%E6%AF%94%E8%BE%83%E6%93%8D%E4%BD%9C%E7%AC%A6.png)

any/some 可以用min替换
all可以用max替换

```mysql
1. 使用in()
#案例：返回location_id是1400或1700的部门中的所有员工姓名
SELECT 
  last_name 
FROM
  employees 
WHERE department_id IN 
  (SELECT DISTINCT 
    department_id 
  FROM
    departments 
  WHERE location_id IN (1400, 1700)) 
2. 使用any
#案例：返回其它工种中比job_id为‘IT_PROG’任意一员工的工资低的员工  的员工号、姓名、job_id 以及salary
SELECT 
  employee_id,
  last_name,
  job_id,
  salary 
FROM
  employees 
WHERE salary < ANY 
  (SELECT 
    salary 
  FROM
    employees 
  WHERE job_id = 'IT_PROG') 
  AND job_id <> 'IT_PROG' ;
#或者
SELECT 
  employee_id,
  last_name,
  job_id,
  salary 
FROM
  employees 
WHERE salary < 
  (SELECT 
    MAX(salary) 
  FROM
    employees 
  WHERE job_id = 'IT_PROG') 
  AND job_id <> 'IT_PROG' ;
3.使用all
SELECT 
  employee_id,
  last_name,
  job_id,
  salary 
FROM
  employees 
WHERE salary < 
  (SELECT 
    MIN(salary) 
  FROM
    employees 
  WHERE job_id = 'IT_PROG') 
  AND job_id <> 'IT_PROG' ;
#或者
SELECT 
  employee_id,
  last_name,
  job_id,
  salary 
FROM
  employees 
WHERE salary < ALL
  (SELECT 
    salary
  FROM
    employees 
  WHERE job_id = 'IT_PROG') 
  AND job_id <> 'IT_PROG' ;
```

##### 行子查询

```mysql
#案例：查询员工编号最小而且工资最高
SELECT 
  * 
FROM
  employees 
WHERE (employee_id, salary) = 
  (SELECT 
    MIN(employee_id),
    MAX(salary) 
  FROM
    employees)
```

#### SELECT后面的子查询

```mysql
#案例：查询每个部门的员工个数
SELECT 
  d.*,
  (SELECT 
    COUNT(*) 
  FROM
    employees e 
  WHERE e.department_id = d.department_id) 
FROM
  departments d
#案例：查询员工号=102的部门名
```

#### FROM后面的子查询

将子查询结果充当一张表，要求必须起别名

```mysql
#在from 后面加select，查询的子表需要注意起别名
#案例：查询每个部门的平均工资的工资等级
SELECT 
  ag_dep.*,
  job_grades.`grade_level` 
FROM
  (SELECT 
    AVG(salary) ag,
    department_id 
  FROM
    employees 
  GROUP BY department_id) AS ag_dep 
  INNER JOIN job_grades 
WHERE ag_dep.ag BETWEEN lowest_sal 
  AND highest_sal 

```

#### EXISTS后面（相关子查询）

语法：
exists（完整的查询语句）
结果： 1 / 0

通常，能用exists的查询都能被其他查询代替

该语法可以理解为，将主查询的数据，放到子查询中做条件验证，根据验证结果(（TRUE或FALSE）来决定主查询的数据结果是否得以保留。

Exists执行顺序如下： 
　　1.首先执行一次外部查询 
　　2.对于外部查询中的每一行分别执行一次子查询，而且每次执行子查询时都会引用外部查询中当前行的值。 
　　3.使用子查询的结果来确定外部查询的结果集。（如果外部查询返回100行，SQL  就将执行101次查询，一次执行外部查询，然后为外部查询返回的每一行执行一次子查询。但实际上，SQL的查询 优化器有可能会找到一种更好的方法来执行相关子查询，而不需要实际执行101次查询。）

IN的执行过程如下：

　　1.首先运行子查询，获取子结果集
	    2.主查询再去结果集里去找符合要求的字段列表，.符合要求的输出,反之则不输出。

```mysql
#案例1：查询有员工的部门名
#方法一：
SELECT department_name
FROM departments d
WHERE EXISTS(
	SELECT * 
	FROM employees e
	WHERE e.`department_id` = d.`department_id`
)

#方法二：
SELECT department_name
FROM departments d
WHERE d.`department_id` IN(
	SELECT e.`department_id`
	FROM employees e
	
)

#案例2：查询没有女朋友的男神信息
#方法1，使用in
SELECT b.*
FROM boys b
WHERE b.`id` NOT IN(
	SELECT boyfriend_id
	FROM beauty bu
)
#方法2，使用exists
```

### 进阶8：分页查询

应用场景：

	实际的web项目中，但要显示的数据，一页显示不全
	需要根据用户的需求提交对应的分页查询的sql语句

语法：

```mysql
select 字段|表达式,...
FROM 表
【where 条件】
【group by 分组字段】
【having 条件】
【order by 排序的字段】
limit offset,size

offset: 要显示条目的起始索引（起始索引从0开始）。同时如果offset从0开始，则可以省略不写
size：要显示的条目个数 
```

特点：

- 起始条目索引从0开始
- limit子句放在查询语句的最后，在语句的执行顺序上也排在最后
- 公式：select * FROM  表 limit （page-1）*sizePerPage,sizePerPage
  假如:
  每页显示条目数sizePerPage
  要显示的页数 page

```mysql
案例：查询前五条员工信息
SELECT * FROM employees LIMIT 5; 将offset省略

#案例：查询第11条-第25条
SELECT * FROM employees LIMIT 10,15;

#案例：查询有奖金的员工信息，并且工资较高的前十名显示出来
SELECT 
  * 
FROM
  employees 
WHERE commission_pct IS NOT NULL 
ORDER BY salary DESC 
LIMIT 10 ;
```

### 进阶9：联合查询

引入：
	union 联合、合并，将多条查询语句的结果合成一个结果

应用场景：
	要查询的结果来自于多个表，且多个表没有直接的连接关系，但查询的信息一致时

语法：

```mysql
select 字段|常量|表达式|函数 【FROM 表】 【where 条件】 union 【all】
select 字段|常量|表达式|函数 【FROM 表】 【where 条件】 union 【all】
select 字段|常量|表达式|函数 【FROM 表】 【where 条件】 union  【all】
.....
select 字段|常量|表达式|函数 【FROM 表】 【where 条件】
```

特点：

	1、多条查询语句的查询的列数必须是一致的
	2、多条查询语句的查询的列的类型几乎相同
	3、union代表去重，union all代表不去重

```mysql
#案例：插叙内部么年号>90或邮箱包含a的员工信息
SELECT * FROM employees WHERE email LIKE '%a%' OR department_id > 90;
#可以改写成以下形式

SELECT * FROM employees WHERE email LIKE '%a%'
UNION
SELECT * FROM employees WHERE department_id > 90;
```

## DML语言学习

### 插入

```mysql
语法：
方式一：
insert into 表名(字段名，...)
values(值1，...)

方式二：
insert into 表名
set 列名=值，列名=值, ....;
```

特点：

	1、字段类型和值类型一致或兼容，而且一一对应
	2、可以为空的字段，可以不用插入值，或用null填充
	3、不可以为空的字段，必须插入值
	4、字段个数和值的个数必须一致
	5、字段可以省略，但默认所有字段，并且顺序和表中的存储顺序一致

```mysql
1.插入方式1
#案例：不可以为null的列必须插入值，可以为null的列如何插入值？
方式1：用null填充
INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
VALUES(13, '唐艺昕', '女', '1990-4-23', 1898888888, NULL, 2);

方式2：nullable即可以为空的字段在插入时可以列名和值都不写
INSERT INTO beauty(id,NAME,sex,borndate,phone,boyfriend_id)
VALUES(14, 'Sophie', '女', '1990-4-23', 1898888888, 2);

#案例：字段可以省略，但默认所有字段，并且顺序和表中的存储顺序一致
INSERT INTO beauty
VALUES(18, '张飞', '男', NULL, '119',NULL,NULL)

2.插入方式2
INSERT INTO beauty
SET id=19, NAME='刘涛', phone = '999'`beauty`


```

两种插入方式比较

```mysql
- 方式一支持一次插入多行，语法如下：
insert into 表名【(字段名,..)】 values(值，..),(值，...),...;
INSERT INTO beauty
VALUES(35, '唐艺昕2', '女', '1990-4-23', 1898888888, NULL, 2),
(36, '唐艺昕3', '女', '1990-4-23', 1898888888, NULL, 2);

- 方式一支持子查询，语法如下：
  insert into 表名
  查询语句;
  
INSERT INTO beauty(id, NAME, phone)
SELECT 44,'宋茜','11809866;' #将查询结果插入表中
  
  
```

### 修改

语法：

```mysql
一、修改单表的记录 ★
语法：
update 表名
set 字段=值,字段=值 
【where 筛选条件】;

二、修改多表的记录【补充】
语法：
update 表1 别名 
left|right|inner join 表2 别名 
on 连接条件  
set 字段=值,字段=值 
【where 筛选条件】;
```
```mysql
1.修改单表
#案例：修改beauty表中姓唐的女生的电话为13338291019
UPDATE beauty SET phone = '13338291019'
WHERE NAME LIKE '唐%'

2.修改多表记录
#案例：修改张无忌的女朋友的手机号为‘114’
UPDATE boys bo
INNER JOIN beauty b ON bo.`id` = b.`boyfriend_id`
SET b.`phone` = '114'
WHERE bo.`boyName` = '张无忌'

#案例：修改没有男朋友的女神的男朋友编号都为2
UPDATE beauty b 
LEFT OUTER JOIN boys bo
ON b.`boyfriend_id` = bo.id
SET b.boyfriend_id = 2
WHERE b.`boyfriend_id` IS NULL
```

### 删除

```mysql
1.方式1：delete语句 
单表的删除： ★
	delete FROM 表名 【where 筛选条件】【limit条目数】

多表的删除：
delete 别名1,别名2 from 表1 别名  #delete后面要删除哪个表的内容就加上他的别名
inner|left|right join 表2 别名 
on 连接条件
 【where 筛选条件】
2.方式2：truncate语句  没有筛选条件，清空删除
truncate table 表名
```

```mysql
1. 方式1
#单表删除
#案例：删除手机号以9结尾的女神信息
DELETE FROM beauty
WHERE phone LIKE '%9'

#多表删除
#案例：删除张无忌的女朋友的信息
DELETE b
FROM beauty b
INNER JOIN boys bo
ON b.`boyfriend_id` = bo.`id`
WHERE bo.`boyName` = '张无忌';

2.方式2
#案例：将魅力值>100的男神信息删除
```

两种方式的区别【面试题】


	#1.truncate不能加where条件，而delete可以加where条件
	#2.truncate的效率高一丢丢
	#3.truncate 删除带自增长的列的表后，如果再插入数据，数据从1开始
	#delete 删除带自增长列的表后，如果再插入数据，数据从上一次的断点处开始
	#4.truncate删除不能回滚，delete删除可以回滚
	#5.truncate删除没有返回值，delete删除有返回值

## DDL语句

Data Definition Language

```mysql
#补充通用写法
DROP DATABASE IF EXISTS 旧库名；
CREATE DATABASE 新库名

DROP TABLE IF EXISTS 旧表名
CREATE TABLE表名();
```

### 库的管理：

```mysql
一、创建库
create database 库名
create database if not exists 库名 【character set 字符集名】 #提高创建的容错性
二、删除库
drop database if exists 库名
三、更改库的字符集
ALTER DATABASE 库名 CHARACTER SET 字符集
```
```mysql
#案例：创建库Books
CREATE DATABASE books;
```

### 表的管理：

#### 创建表	

```mysql
语法
create table 【if not exists】 表名(
	字段名 字段类型 【约束】,
	字段名 字段类型 【约束】,
	...
	字段名 字段类型 【约束】 

)
# 创建book表
CREATE TABLE book(
	id INT, #编号
	bName VARCHAR(20), #图书名
	price DOUBLE, #价格
	authorId INT, #作者编号
	publishDate DATETIME #出版日期
)
```

#### 修改与删除表

```mysql
修改表 alter
语法：ALTER TABLE 表名 ADD|MODIFY|DROP|CHANGE COLUMN 字段名 【字段类型 约束】【first|after 字段名】;#用first和after控制位置
删除表 
drop table IF EXISTS 表名

#案例：修改列名publishdate为pubdate
ALTER TABLE book CHANGE COLUMN publishdate pubDate DATETIME;

#案例：修改列的类型或约束
ALTER TABLE book MODIFY COLUMN pubDate TIMESTAMP

#案例：添加新列
ALTER TABLE author ADD COLUMN annual DOUBLE;

#案例：删除列
ALTER TABLE author DROP COLUMN annual

#案例：修改表名
ALTER TABLE author RENAME TO book_author;

#案例：删除表
DROP TABLE IF EXISTS book_author;

```

#### 表的复制

```mysql
#案例讲解
CREATE TABLE IF NOT EXISTS author(
	id INT,
	lastname VARCHAR(20),
	country VARCHAR(20)
)

INSERT INTO author VALUES
(1, '村上春树', '日本'),
(2, '莫言', '中国'),
(3, '冯唐', '中国'),
(4, '金庸', '中国');

#1.仅仅复制表的结构
CREATE TABLE copy LIKE author;

#2.复制表的结构外加数据
CREATE TABLE copy2 
SELECT * FROM author;

#3. 通过选择复制部分表
CREATE TABLE copy3
SELECT id, lastname
FROM author 
WHERE country = '中国';

#4.通过选择仅仅复制表的结构但是不复制内容
CREATE TABLE copy4
SELECT id,lastname
FROM author
WHERE 0 #0就代表false
```

### 常见数据类型

```mysql
一、数值型
1、整型
tinyint、smallint、mediumint、int/integer、bigint
1         2        3          4            8

特点：
①都可以设置无符号和有符号，默认有符号，通过unsigned设置无符号
②如果超出了范围，会报out or range异常，插入临界值
③长度可以不指定，默认会有一个长度
长度代表显示的最大宽度，如果不够则左边用0填充，但需要搭配zerofill，并且默认变为无符号整型
#案例：unsigned
DROP TABLE IF EXISTS tab_int;
CREATE TABLE tab_int(
	t1 INT,
	t2 INT UNSIGNED
);

#案例：zerofill
DROP TABLE IF EXISTS tab_int;
CREATE TABLE tab_int(
	t1 INT ZEROFILL,
	t2 INT ZEROFILL
);

2、浮点型
定点数：decimal(M,D)
浮点数:
	float(M,D)   4
	double(M,D)  8

特点：
①M代表整数部位+小数部位的个数，D代表小数部位
②如果超出范围，则报out or range异常，并且插入临界值
③M和D都可以省略，但对于定点数，M默认为10，D默认为0
④如果精度要求较高，则优先考虑使用定点数

二、字符型
char、varchar、binary、varbinary、enum、set、text、blob(用于保存较大的二进制)

char：固定长度的字符，写法为char(M)，最大长度不能超过M，其中M可以省略，默认为1
varchar：可变长度的字符，写法为varchar(M)，最大长度不能超过M，其中M不可以省略
enum与set不区分大小写
#案例：enum
CREATE TABLE tab_char(
	c1 ENUM('a','b','c')
)
#案例：set
CREATE TABLE tab_char(
	c1 SET('a','b','c')
)

三、日期型，日期型必须用单引号引起来  
year年
date日期
time时间
datetime 日期+时间          8      
timestamp 日期+时间         4   比较容易受时区、语法模式、版本的影响，更能反映当前时区的真实时间

```

### 常见约束

含义：一种限制，用于限制表中的数据，为了保证表中的数据的准确和可靠性

添加约束的时机：创建表时 和 修改表时

#### 约束的添加分类

- 列级约束：六大约束语法上都支持，但外键约束没有效果，且一个字段可以添加多个列级约束。

  位置：列的后面

- 表级约束：除了非空、默认、其他都支持

  位置：所有列的下面

  补充：表级约束可以起约束名而列级约束不可以（主键不起效果）



```mysql
NOT NULL：非空，该字段的值必填
UNIQUE：唯一，该字段的值不可重复，但可以为空
DEFAULT：默认，该字段的值不用手动插入有默认值
CHECK：检查，mysql不支持
PRIMARY KEY：主键，该字段的值不可重复并且非空  unique+not null
FOREIGN KEY：外键，确认该字段的值引用了另外的表的字段。用于限制两个表的关系，保证该字段的值必须来自于主表的关联列的值
```

```mysql
通用写法：
CREATE TABLE IF NOT EXISTS stuino(
	id INT PRIMARY KEY,
	stuname VARCHAR(20) NOT NULL,
	sex CHAR(1),
	age INT DEFAULT NOT NULL 18,#可以添加多个约束
	seat INT UNIQUE,
	majorid INT,
	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)
)

1.创建表时添加约束
语法：直接在字段名和类型后面追加类型约束即可
只支持：默认、非空、主键、唯一

#添加列级约束
CREATE TABLE stuinfo(
	id INT PRIMARY KEY,#主键
	stuName VARCHAR(20) NOT NULL,#非空
	gender CHAR(1) CHECK(gender ='男' OR gender = '女'),#检查
	seat INT UNIQUE,#唯一
	age INT DEFAULT 18,#默认约束
	major INT REFERENCES major(id)#外键约束，但只是语法不报错，并不生效
)

CREATE TABLE major(
	id INT PRIMARY KEY,
	majorName VARCHAR(20)
)

SHOW INDEX FROM stuinfo;用于查看索引。主键、外键、唯一键会自动生成索引

2.添加表级约束
语法：在各个字段的最下面
【constraint 约束名】 约束类型（字段名）
CREATE TABLE stuinfo(
	id INT,
	stuname VARCHAR(20),
	gender CHAR(1),
	seat INT,
	age INT,
	majorid INT,
	
	CONSTRAINT pk PRIMARY KEY(id),#主键
	CONSTRAINT uq UNIQUE(seat),#唯一键
	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)#外键
)

SHOW INDEX FROM stuinfo
```

#### 主键和唯一的对比

```mysql
1、区别：
①、一个表至多有一个主键，但可以有多个唯一
②、主键不允许为空，唯一可以为空
2、相同点
都具有唯一性
都支持组合键，但不推荐
		保证唯一性	是否允许为空	一个表中可以有多少个	是否允许组合 
主键		√			×			至多1个			√，但不推荐
唯一		√			√			可以有多个（null）		   √，但不推荐
```

#### 外键

- 用于限制两个表的关系，从表的字段值引用了主表的某字段值
- 要求在从表上设置外键
- 外键列和主表的被引用列要求类型一致，意义一样，名称无要求
- 主表的被引用列要求是一个key（一般就是主键，也可以是唯一键）
- 插入数据，先插入主表。删除数据，先删除从表

```mysql
可以通过以下两种方式来删除主表的记录
#方式一：级联删除
ALTER TABLE stuinfo ADD CONSTRAINT fk_stu_major FOREIGN KEY(majorid) REFERENCES major(id) ON DELETE CASCADE;

#方式二：级联置空
ALTER TABLE stuinfo ADD CONSTRAINT fk_stu_major FOREIGN KEY(majorid) REFERENCES major(id) ON DELETE SET NULL;
```

#### 修改表时添加约束

```mysql
1.添加列级月红素
alter table 表名 modify column 字段名 字段类型 新约束;

2.添加表级约束
alter table 表名 add 【constraint 约束名】 约束类型(字段名) 【外键的引用】

CREATE TABLE stuinfo(
	id INT,
	stuname VARCHAR(20),
	gender CHAR(1),
	seat INT,
	age INT,
	majorid INT
)
#1.添加非空约束
ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20) NOT NULL;

#2.添加默认约束
ALTER TABLE stuinfo MODIFY COLUMN age INT DEFAULT 18;

#3.添加主键
#①列级约束写法
ALTER TABLE stuinfo MODIFY COLUMN id INT PRIMARY KEY;
#②表级约束的写法
ALTER TABLE stuinfo ADD CONSTRAINTmy_stuinfo_id_pk  PRIMARY KEY(id)# 列级约束的写法支持给主键起名字，让起完名字后还是PRIMARY

#4.添加唯一约束
#①列级约束写法
ALTER TABLE stuinfo MODIFY COLUMN seat INT UNIQUE;
#②表级约束写法
ALTER TABLE stuinfo ADD UNIQUE(seat);

#5.添加外键
ALTER TABLE stuinfo ADD CONSTRAINT fk_stuinfo_majorforeign FOREIGN KEY(majorid) REFERENCES major(id)

```

#### 修改表时删除表

```
#1.删除非空约束
ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20) NULL;
#2.删除主键
ALTER TABLE stuinfo DROP PRIMARY KEY;

#3.删除唯一键
ALTER TABLE stuinfo DROP INDEX seat;

#4.删除外键约束
ALTER TABLE stuinfo DROP FOREIGN KEY fk_stuinfo_majorforeign;
SHOW INDEX FROM stuinfo

```

### 标识列/自增长列

特点：

- 不用手动插入值，可以自动提供序列值，默认从1开始，步长为1
  auto_increment_increment
  如果要更改起始值：手动插入值
  如果要更改步长：更改系统变量
  set auto_increment_increment=值;
- 一个表至多有一个自增长列
- 自增长列只能支持数值型
- 自增长列必须为一个key

```mysql
一、创建表时设置自增长列
create table 表(
	字段名 字段类型 约束 auto_increment
)
二、修改表时设置自增长列
alter table 表 modify column 字段名 字段类型 约束 auto_increment
三、删除自增长列
alter table 表 modify column 字段名 字段类型 约束 



#
CREATE TABLE tab_identity(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20)
)

INSERT INTO tab_identity VALUES(NULL, 'john') #在自增长列用null填充即可
INSERT INTO tab_identity(NAME) VALUES('johb') #或者省略列名

#修改自增长的步长，起始值为1，不可通过变量修改，但可以一开始手动插入修改
SHOW VARIABLES LIKE '%auto_increment%'
SET auto_increment_increment = 3
INSERT INTO tab_identity VALUES(1, 'john')
```



##  TCL语言

### 含义	

Transaction COntrol Language，事物控制语言。

事物：通过一组逻辑操作单元（一组DML——sql语句），将数据从一种状态切换到另外一种状态。一组sql语句要么都执行要么都不执行

### 特点

​	（ACID）

- 原子性：将事务的SQL语句看做一个整体，要么都执行，要么都回滚
- 一致性：保证数据的状态操作前和操作后保持一致
- 隔离性：多个事务同时操作相同数据库的同一个数据时，一个事务的执行不受另外一个事务的干扰
- 持久性：一个事务一旦提交，则数据将持久化到本地，除非其他事务对其进行修改

### 事务的分类

- 隐式事务，没有明显的开启和结束事务的标志。insert、update、delete语句本身就是一个事务

  ```mysql
  SHOW VARIABLES LIKE ‘autocommit’
  ```

  

- 显式事务，具有明显的开启和结束事务的标志

  ```mysql
  ①开启事务：前提是必须先设置自动提交功能为禁用
  set autocommit=0;#只对当前事物有效
  start transaction;#可以省略
  
  ②编写一组逻辑sql语句
  注意：sql语句支持的是select、insert、update、delete
  语句1；
  语句2；
  ....
  
  设置回滚点：
  savepoint 回滚点名;
  
  ③结束事务
  提交：commit;
  回滚：rollback;
  回滚到指定的地方：rollback to 回滚点名;
  ```


- delete和truncate在事务使用时的区别

  - delete在事务中支持回滚
  - truncate在事务使用中不支持回滚



### 使用到的关键字

```mysql
set autocommit=0;
start transaction;
commit;
rollback;

设置回滚点：savepoint  断点
commit to 断点
rollback to 断点

#案例
SET autocommit =0；
START TRANSACTION
DELETE FROM account WHERE id=25;
SAVEPOINT a;#设置保存点
DELETE FROM account WHERE id=28;

ROLLBACK TO a;#回滚到保存点
```

### 事务的隔离级别

事务并发问题如何发生  ？：

- 当多个事务同时操作同一个数据库的相同数据时

事务的并发问题有哪些？

- 脏读：一个事务读取到了另外一个事务未提交的数据
- 不可重复读：同一个事务中，多次读取到的数据不一致
- 幻读：幻读是针对数据**插入（INSERT）**操作来说的。一个事务先读取数据时，另外一个事务进行更新（主要是插入），然后第一个事务进行操作，操作结果发现更新信息和最初读取的数据不一致（比如因为第二个事务插入操作导致更新结果多了一行，像产生了幻觉）

如何避免事务的并发问题？通过设置事务的隔离级别

- READ UNCOMMITTED

- READ COMMITTED 可以避免脏读

- REPEATABLE READ 可以避免脏读、不可重复读和一部分幻读

- SERIALIZABLE可以避免脏读、不可重复读和幻读（对正在操作的事务进行类似于加锁操作，让其他准备进行操作的事务等待）

  ![数据库隔离级别](MySQL%E5%9F%BA%E7%A1%80.assets/%E6%95%B0%E6%8D%AE%E5%BA%93%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB.png)

  ​									     	脏读		不可重复读		幻读

  read uncommitted:读未提交     ×                ×              ×        
  read committed：读已提交      √                ×              ×
  repeatable read：可重复读     √                √              ×
  serializable：串行化         	   √                √              √

	1.设置隔离级别：
	set session|global  transaction isolation level 隔离级别名;
	使用global是设置数据库系统的全局隔离级别，使用session是设置当前连接隔离级别
	2.查看隔离级别：
	select @@tx_isolation;
	3.修改cmd字符集
	set names gbk;


## 视图

含义：理解成一张虚拟的表。视图的行和列的数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的，只保存了sql逻辑，不保存查询结果

视图和表的区别：
	
	     使用方式	  占用物理空间
	
	视图	完全相同	不占用，仅仅保存的是sql逻辑
	
	表	完全相同	占用

视图的好处：


	1、sql语句提高重用性，效率高（相当于封装了一个方法）
	2、和表实现了分离，提高了安全性

### 视图的创建

​	语法：

```
	CREATE VIEW  视图名
	AS
	查询语句;
```

### 视图的增删改查

​	1、查看视图的数据 ★
​	

	SELECT * FROM my_v4;
	SELECT * FROM my_v1 WHERE last_name='Partners';
	
	2、插入视图的数据
	INSERT INTO my_v4(last_name,department_id) VALUES('虚竹',90);
	
	3、修改视图的数据
	
	UPDATE my_v4 SET last_name ='梦姑' WHERE last_name='虚竹';


​	
​	4、删除视图的数据
​	DELETE FROM my_v4;
###某些视图不能更新
​	包含以下关键字的sql语句：分组函数、distinct、group  by、having、union或者union all
​	常量视图
​	Select中包含子查询
​	join
​	FROM一个不能更新的视图
​	where子句的子查询引用了FROM子句中的表
###视图逻辑的更新
​	#方式一：
​	CREATE OR REPLACE VIEW test_v7
​	AS
​	SELECT last_name FROM employees
​	WHERE employee_id>100;
​	
​	#方式二:
​	ALTER VIEW test_v7
​	AS
​	SELECT employee_id FROM employees;
​	
​	SELECT * FROM test_v7;
###视图的删除
​	DROP VIEW test_v1,test_v2,test_v3;
###视图结构的查看	
​	DESC test_v7;
​	SHOW CREATE VIEW test_v7;

##存储过程

含义：一组经过预先编译的sql语句的集合
好处：

	1、提高了sql语句的重用性，减少了开发程序员的压力
	2、提高了效率
	3、减少了传输次数

分类：

	1、无返回无参
	2、仅仅带in类型，无返回有参
	3、仅仅带out类型，有返回无参
	4、既带in又带out，有返回有参
	5、带inout，有返回有参
	注意：in、out、inout都可以在一个存储过程中带多个
###创建存储过程
语法：

	create procedure 存储过程名(in|out|inout 参数名  参数类型,...)
	begin
		存储过程体
	
	end

类似于方法：

	修饰符 返回类型 方法名(参数类型 参数名,...){
	
		方法体;
	}

注意

	1、需要设置新的结束标记
	delimiter 新的结束标记
	示例：
	delimiter $
	
	CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名  参数类型,...)
	BEGIN
		sql语句1;
		sql语句2;
	
	END $
	
	2、存储过程体中可以有多条sql语句，如果仅仅一条sql语句，则可以省略begin end
	
	3、参数前面的符号的意思
	in:该参数只能作为输入 （该参数不能做返回值）
	out：该参数只能作为输出（该参数只能做返回值）
	inout：既能做输入又能做输出


#调用存储过程
	call 存储过程名(实参列表)
##函数


###创建函数

学过的函数：LENGTH、SUBSTR、CONCAT等
语法：

	CREATE FUNCTION 函数名(参数名 参数类型,...) RETURNS 返回类型
	BEGIN
		函数体
	
	END

###调用函数
	SELECT 函数名（实参列表）





###函数和存储过程的区别

			关键字		调用语法	返回值			应用场景
	函数		FUNCTION	SELECT 函数()	只能是一个		一般用于查询结果为一个值并返回时，当有返回值而且仅仅一个
	存储过程	PROCEDURE	CALL 存储过程()	可以有0个或多个		一般用于更新


##流程控制结构

###系统变量
一、全局变量

作用域：针对于所有会话（连接）有效，但不能跨重启

	查看所有全局变量
	SHOW GLOBAL VARIABLES;
	查看满足条件的部分系统变量
	SHOW GLOBAL VARIABLES LIKE '%char%';
	查看指定的系统变量的值
	SELECT @@global.autocommit;
	为某个系统变量赋值
	SET @@global.autocommit=0;
	SET GLOBAL autocommit=0;

二、会话变量

作用域：针对于当前会话（连接）有效

	查看所有会话变量
	SHOW SESSION VARIABLES;
	查看满足条件的部分会话变量
	SHOW SESSION VARIABLES LIKE '%char%';
	查看指定的会话变量的值
	SELECT @@autocommit;
	SELECT @@session.tx_isolation;
	为某个会话变量赋值
	SET @@session.tx_isolation='read-uncommitted';
	SET SESSION tx_isolation='read-committed';

###自定义变量
一、用户变量

声明并初始化：

	SET @变量名=值;
	SET @变量名:=值;
	SELECT @变量名:=值;
赋值：

	方式一：一般用于赋简单的值
	SET 变量名=值;
	SET 变量名:=值;
	SELECT 变量名:=值;


	方式二：一般用于赋表 中的字段值
	SELECT 字段名或表达式 INTO 变量
	FROM 表;

使用：

	select @变量名;

二、局部变量

声明：

	declare 变量名 类型 【default 值】;
赋值：

	方式一：一般用于赋简单的值
	SET 变量名=值;
	SET 变量名:=值;
	SELECT 变量名:=值;


	方式二：一般用于赋表 中的字段值
	SELECT 字段名或表达式 INTO 变量
	FROM 表;

使用：

	select 变量名



二者的区别：

			作用域			定义位置		语法
用户变量	当前会话		会话的任何地方		加@符号，不用指定类型
局部变量	定义它的BEGIN END中 	BEGIN END的第一句话	一般不用加@,需要指定类型

###分支
一、if函数
	语法：if(条件，值1，值2)
	特点：可以用在任何位置

二、case语句

语法：

	情况一：类似于switch
	case 表达式
	when 值1 then 结果1或语句1(如果是语句，需要加分号) 
	when 值2 then 结果2或语句2(如果是语句，需要加分号)
	...
	else 结果n或语句n(如果是语句，需要加分号)
	end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）
	
	情况二：类似于多重if
	case 
	when 条件1 then 结果1或语句1(如果是语句，需要加分号) 
	when 条件2 then 结果2或语句2(如果是语句，需要加分号)
	...
	else 结果n或语句n(如果是语句，需要加分号)
	end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）

特点：
	可以用在任何位置

三、if elseif语句

语法：

	if 情况1 then 语句1;
	elseif 情况2 then 语句2;
	...
	else 语句n;
	end if;

特点：
	只能用在begin end中！！！！！！！！！！！！！！！


三者比较：
			应用场合
	if函数		简单双分支
	case结构	等值判断 的多分支
	if结构		区间判断 的多分支


###循环

语法：


	【标签：】WHILE 循环条件  DO
		循环体
	END WHILE 【标签】;

特点：

	只能放在BEGIN END里面
	
	如果要搭配leave跳转语句，需要使用标签，否则可以不用标签
	
	leave类似于java中的break语句，跳出所在循环！！！





# MySQL高级

## MySQL的架构与存储引擎

### MySQL字![image_1d6dfk4orjaj1ra8536aijb99.png-112.4kB](MySQL%E5%9F%BA%E7%A1%80.assets/16a2f479833d3340)符集设置

| 系统变量                   | 描述                                                         |
| -------------------------- | ------------------------------------------------------------ |
| `character_set_client`     | 服务器解码请求时使用的字符集                                 |
| `character_set_connection` | 服务器处理请求时会把请求字符串从`character_set_client`转为`character_set_connection` |
| `character_set_results`    | 服务器向客户端返回数据时使用的字符集                         |

- 服务器认为客户端发送过来的请求是用`character_set_client`编码的。

  假设你的客户端采用的字符集和 ***character_set_client*** 不一样的话，这就会出现意想不到的情况。比如我的客户端使用的是`utf8`字符集，如果把系统变量`character_set_client`的值设置为`ascii`的话，服务器可能无法理解我们发送的请求，更别谈处理这个请求了。

- 服务器将把得到的结果集使用`character_set_results`编码后发送给客户端。

  假设你的客户端采用的字符集和 ***character_set_results*** 不一样的话，这就可能会出现客户端无法解码结果集的情况，结果就是在你的屏幕上出现乱码。比如我的客户端使用的是`utf8`字符集，如果把系统变量`character_set_results`的值设置为`ascii`的话，可能会产生乱码。

- `character_set_connection`只是服务器在将请求的字节串从`character_set_client`转换为`character_set_connection`时使用，它是什么其实没多重要，但是一定要注意，该字符集包含的字符范围一定涵盖请求中的字符，要不然会导致有的字符无法使用`character_set_connection`代表的字符集进行编码。比如你把`character_set_client`设置为`utf8`，把`character_set_connection`设置成`ascii`，那么此时你如果从客户端发送一个汉字到服务器，那么服务器无法使用`ascii`字符集来编码这个汉字，就会向用户发出一个警告。

### MySQL架构





![img](MySQL%E5%9F%BA%E7%A1%80.assets/v2-f8d848179f49e357c4e348c68b4e62b5_720w-1627227238062.jpg)

- 连接层：最上层是一些客户端和连接服务。主要完成一些类似于连接处理、授权认真、以及相关的安全方案：

- 服务层：第二层架构主要完成大多数的核心服务功能能够，如SQL接口，缓存查新，SQL分析、查询优化等。所有跨存储引擎的功能也在这一层实现。
- 引擎层：MySQL可以使用不同的存储引擎，在实际项目中根据需要进行选取，默认的存储引擎是InnoDB
- 存储层：将数据存储在运行于裸机的文件系统上，完成文件系统与存储引擎的交互

![image_1c8d26fmg1af0ms81cpc7gm8lv39.png-97.9kB](MySQL%E5%9F%BA%E7%A1%80.assets/167f4c7b99f87e1c)

从图中我们可以看出，服务器程序处理来自客户端的查询请求大致需要经过三个部分，分别是`连接管理`、`解析与优化`、`存储引擎`。

- 每当有一个客户端进程连接到服务器进程时，服务器进程都会创建一个线程来专门处理与这个客户端的交互，当该客户端退出时会与服务器断开连接，服务器并不会立即把与该客户端交互的线程销毁掉，而是把它缓存起来，在另一个新的客户端再进行连接时，把这个缓存的线程分配给该新客户端。这样就起到了不频繁创建和销毁线程的效果，从而节省开销。

- 查询缓存：虽然查询缓存有时可以提升系统性能，但也不得不因维护这块缓存而造成一些开销，比如每次都要去查询缓存中检索，查询请求处理完需要更新查询缓存，维护该查询缓存对应的内存区域。从MySQL 5.7.20开始，不推荐使用查询缓存，并在MySQL 8.0中删除。
- 语法解析：如果查询缓存没有命中，接下来就需要进入正式的查询阶段了。因为客户端程序发送过来的请求只是一段文本而已，所以`MySQL`服务器程序首先要对这段文本做分析，判断请求的语法是否正确，然后从文本中将要查询的表、各种查询条件都提取出来放到`MySQL`服务器内部使用的一些数据结构上来。
- 查询优化：因为我们写的`MySQL`语句执行起来效率可能并不是很高，`MySQL`的优化程序会对我们的语句做一些优化。我们可以使用`EXPLAIN`语句来查看某个语句的执行计划
- 存储引擎：为了实现不同的功能，`MySQL`提供了各式各样的`存储引擎`，不同`存储引擎`管理的表具体的存储结构可能不同

### InnoDB的特点



| 对比项 | MyISAM                                                   | InnoDB                                                       |
| ------ | -------------------------------------------------------- | ------------------------------------------------------------ |
| 主外键 | 不支持                                                   | 支持                                                         |
| 事务   | 不支持                                                   | 支持                                                         |
| 行表锁 | 表锁，即使操作一条记录也会锁住整个表，不适合高并发的操作 | 行锁，操作时只锁住某一行，不对其他行有影响。适合高并发操作   |
| 缓存   | 只缓存索引，不缓存真实数据                               | 不仅缓存索引还要缓存真实数据，对内存要求比较高，而且内存大小对性能有决定性影响 |
| 表空间 | 小                                                       | 大                                                           |
| 关注点 | 性能                                                     | 事务                                                         |

### SQL的执行加载顺序

```sql
#程序员编写顺序
SELECT DISTINCT
		<select_list>
FROM
     <left_table> <join_type>
JOIN <right_table> ON <join_condition>
WHERE
		 <where_condition>
GROUP BY
      <group_bylist>
HAVING
 			<having_condition>
ORDER BY
			<order_by_condition>
LIMIT <limit number>

#SQL的的理解顺序从FROM开始
FROM <left_table>
ON <join_condition>
<join_type> JOIN <right_table>
WHERE <where_condition>
GROUP BY <group_by_condition>
HAVING <having_condition>
SELECT
DISTINCT <select_list>
ORDER BY <order_by_condition>
LIMIT <limit_number>

    
```





## InnoDB

`InnoDB`是一个将表中的数据存储到磁盘上的存储引擎，所以即使关机后重启我们的数据还是存在的。而真正处理数据的过程是发生在内存中的，所以需要把磁盘中的数据加载到内存中，如果是处理写入或修改请求的话，还需要把内存中的内容刷新到磁盘上。

为了提高读写效率，`InnoDB`采取的方式是：将数据划分为若干个页，以页作为磁盘和内存之间交互的基本单位，InnoDB中页的大小一般为 ***16*** KB。也就是在一般情况下，一次最少从磁盘中读取16KB的内容到内存中，一次最少把内存中的16KB内容刷新到磁盘中。

`InnoDB`目前定义了4种行格式

- COMPACT行格式
- Redundant行格式
- Dynamic和Compressed行格式

一个页一般是`16KB`，当记录中的数据太多，当前页放不下的时候，会把多余的数据存储到其他页中，这种现象称为`行溢出`。

### InnoDB数据页结构

数据页代表的这块`16KB`大小的存储空间可以被划分为多个部分，不同部分有不同的功能，各个部分如图所示：

![img](MySQL%E5%9F%BA%E7%A1%80.assets/16f13ee1e2dfac7c)

| 名称                 | 中文名             | 占用空间大小 | 简单描述                 |
| -------------------- | ------------------ | ------------ | ------------------------ |
| `File Header`        | 文件头部           | `38`字节     | 页的一些通用信息         |
| `Page Header`        | 页面头部           | `56`字节     | 数据页专有的一些信息     |
| `Infimum + Supremum` | 最小记录和最大记录 | `26`字节     | 两个虚拟的行记录         |
| `User Records`       | 用户记录           | 不确定       | 实际存储的行记录内容     |
| `Free Space`         | 空闲空间           | 不确定       | 页中尚未使用的空间       |
| `Page Directory`     | 页面目录           | 不确定       | 页中的某些记录的相对位置 |
| `File Trailer`       | 文件尾部           | `8`字节      | 校验页是否完整           |

在页的7个组成部分中，我们自己存储的记录会按照我们指定的`行格式`存储到`User Records`部分。但是在一开始生成页的时候，其实并没有`User Records`这个部分，每当我们插入一条记录，都会从`Free Space`部分，也就是尚未使用的存储空间中申请一个记录大小的空间划分到`User Records`部分，当`Free Space`部分的空间全部被`User Records`部分替代掉之后，也就意味着这个页使用完了，如果还有新的记录插入的话，就需要去申请新的页了，这个过程的图示如下：

![image_1cosvi1in9st476cdqfki1n39m.png-133.8kB](MySQL%E5%9F%BA%E7%A1%80.assets/16a95c0fe86555ed)

### 记录的行格式

我们先创建一个表：

```sql
mysql> CREATE TABLE page_demo(
    ->     c1 INT,
    ->     c2 INT,
    ->     c3 VARCHAR(10000),
    ->     PRIMARY KEY (c1)
    -> ) CHARSET=ascii ROW_FORMAT=Compact;
Query OK, 0 rows affected (0.03 sec)
```

这个新创建的`page_demo`表有3个列，其中`c1`和`c2`列是用来存储整数的，`c3`列是用来存储字符串的。需要注意的是，我们把 ***c1*** 列指定为主键，所以在具体的行格式中InnoDB就没必要为我们去创建那个所谓的 ***row_id*** 隐藏列了。而且我们为这个表指定了`ascii`字符集以及`Compact`的行格式。所以这个表中记录的行格式示意图就是这样的：

![image_1c9o2eib2vl11qnf1dfl1d2lco313.png-76.4kB](MySQL%E5%9F%BA%E7%A1%80.assets/16a95c0feca77be3)

## 索引

 索引主要有单值索引和复合索引。

### 什么是索引

索引是帮助MySQL高效获取数据的数据结构，它是“**排好序的快速查找数据结构**”。关键字为**排序**和**快速**

在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法。这种数据结构就是索引

一般来说索引本身也很大，不可能全部存储在内存中，**因此索引往往以索引文件的形式存储在磁盘上。**

所用一般指B树（多路搜索树，不一定还是二叉树）。其中聚集索引，次要索引，复合索引，前缀索引，唯一索引默认都是使用B+树索引，统称为索引。另外其他还有哈希索引。

- 索引会影响where后面的查找
- 索引会影响order by后面的排序

### **索引的优缺点**

优点：

- 提高数据检索的效率，降低数据库的IO成本
- 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗

缺点：

- 实际上索引也是一张表，该表保存了主键和索引字段，并指向实体表的记录，所以索引列也是要占用空间的
- 索引提高查询速度，但会降低表的更新速度，如对表进行INSERT、UPDATE和DELETE。在更新表时，MySQL不仅要保存数据，还要更新索引

索引建立选择

需要建立索引的情况

- 主键自动建立唯一索引
- 频繁作为查询条件的字段应该创建索引
- 查询中与气他表关联的字段，外键关系

### 索引分类：

- 单值索引：即一个索引值包含单个列，一个表可以有多个单值索引
- 唯一索引：索引列的值必须唯一，但允许有空值
- 复合索引：即一个索引包含多个值



### 基本语法

```
创建：
CREATE [UNIQUE] INDEX indexName ON mytable(columnname(length))
ALTER mytable ADD [UNIQUE] INDEX [indexName] ON (columnname(length))

删除:
DROP INDEX [indexName] ON mytable;

查看：
SHOW INDEX FROM table_name


```













