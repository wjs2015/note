# MySQL必知必会笔记



## 第一章  了解SQL

数据库（database）：一个以某种有组织的方式存储的数据集合。（保存有组织的数据的容器）

数据库软件：DBMS（数据库管理系统），数据库是通过DBMS创建和操纵的容器。

表（table）：某种特定类型数据的结构化清单。表名应是唯一的。

模式（schema）：关于数据库和表的布局及特性的信息。

列（column）：表中的一个字段。每个列都有相应的数据类型（datatype）。

行（row、record）：表中的一个记录。

主键（primary key）：一列（或一组列），其值能够唯一区分表中的每行。

- 应该总是定义主键，保证记录的唯一性。
- 任意两行都不具有相同的主键值。
- 每个行都必须具有一个主键值（主键列不允许为NULL）。
- 可以使用多个列作为主键（所有列值得组合唯一）。

应遵守习惯：

- 不更新主键列中的值。
- 不重用主键列的值。
- 不在主键列中使用可能会更改的值。

SQL（Structured Query Language）：结构化查询语言。几乎所有DBMS都支持。



## 第二章  MySQL简介

MySQL：

- 一种DBMS，开源，快，可信赖，简单。
- 一种基于客户机-服务器的数据库。与数据文件打交道的只有服务器软件，关于数据、数据更删改查的请求都由服务器软件完成，这些请求来自客户机软件，结果返回给客户机。
- 服务器软件为MySQL DBMS，客户机可以是MySQL提供的工具、脚本语言、程序设计语言等。
- 

## 第三章  使用MySQL

为了连接到MySQL，需提供的信息：

- 主机名
- 端口
- 一个合法用户名
- 用户口令

选择数据库：**USE**关键字：

~~~mysql
use crashcourse;
~~~

必须先使用use打开数据库，才能读取其中的数据。

可用**SHOW**命令显示与数据库有关信息：

~~~mysql
show databases；#返回可用数据库的一个列表

show tables；#返回数据库内的列表

show columns from customers；#显示指定表的列的详细信息（名称、数据类型、是否允许为null、键信息、默认值等）

show status;#显示广泛的服务器状态信息

show create databases; show create table;#分别用于显示创建特定数据库或表的MySQL语句

show grants;#用于显示授予用户的安全权限

show errors; show warnings;#显示服务器错误或警告信息
~~~

自动增量：在每行添加到表中时，自动为每行分配一个可用编号。

**DESCRIBE**语句：describe customers 与 show columns from customers等价。



## 第四章  检索数据

SELECT语句：须给出想选择什么以及从什么地方选择两个信息，如：

~~~mysql
select prod_name from products;#从products表中选择prod_name列，顺序没有特殊意义

select prod_id, prod_name, prod_price from products;#可同时选择多列，以逗号分隔

select * from products;#检索所有列
~~~

多条SQL语句必须以分号间隔，单条可不加（最好加，没坏处）。

SQL不区分大小写。

检索不同的行：使用**DISTINCT**关键字，仅返回不同的值：

~~~mysql
select distinct vend_id from products;#仅显示vend_id列中不同的值
~~~

distinct关键字应用于所有列而不仅是前置它的列，除非指定的两个列都相同，否则所有的行都将被检索出来，例如：

~~~mysql
select distinct vend_id, prod_price from products;#两列都相同的才选其一输出
~~~

限制结果：为了返回第一行或前几行，可使用**LIMIT**子句：

~~~mysql
select prod_name from products limit 5;#返回不多于5行

select prod_name from products limit 5,5;#指定开始行和行数（第一行为行0，limit 1,1检索出的是第二行）

select prod_name from products limit 5 offset 5;#更易理解，可消除阅读上的歧义
~~~

完全限定的表名：使用完全限定的名字来引用表、列：

~~~mysql
select products.prod_name from crashcourse.products;
~~~



## 第五章  排序检索数据

**关系数据库设计理论认为，如果不明确排序顺序，这不应该假定检索出的数据的顺序有意义。**

**ORDER BY**子句可明确地排序用select语句检索出的数据：

~~~mysql
select prod_name from products order by prod_name;#对prod_name列以字母顺序排序数据
~~~

使用非检索的列排序数据是完全合法的。

按多个列排序：排序完全按所规定的的顺序进行，即代码中列的出现顺序：

~~~mysql
select prod_id， prod_price, prod_name from products order by prod_price, prod_name;#先按照		prod_price排序，再按照prod_name排序
~~~

指定排序方向：DESC、ASC关键字，只应用到直接位于其前面的列名（ASC没多大用处，因为升序是默认的）：

~~~mysql
select prod_id, prod_price, prod_name from products order by prod_price desc, prod_name;
~~~

对文本进行排序时的大小写问题：MySQL中默认A与a相同，如需区分，需寻求数据库管理员帮助（将表的collate改成case sensitive或使用case sensitive的collate进行order by：select * from test_collate order by text collate utf8_bin;）

将ORDER BY和LIMIT组合使用，可找出最高或最低值：

~~~mysql
select prod_price from products order by prod_price desc limit 1;
~~~

子句顺序：FROM->ORDER BY->LIMIT



## 第六章  过滤数据

**WHERE**子句指定搜索条件（过滤条件），在表明之后给出：

~~~mysql
select prod_name, prod_price from products where prod_price = 2.50;
~~~

子句顺序：WHERE->ORDER BY

WHERE子句操作符：

| =       | 等于               |
| ------- | ------------------ |
| <>      | 不等于             |
| !=      | 不等于             |
| <       | 小于               |
| <=      | 小于等于           |
| >       | 大于               |
| >=      | 大于等于           |
| BETWEEN | 在指定的两个值之间 |

**用单引号限定字符串**

范围值检查：BETWEEN

~~~mysql
select prod_name, prod_price from products where prod_price between 5 and 10;#包括开始值和结束值
~~~

空值检查：**IS NULL**子句：

~~~mysql
select prod_name from products where prod_price IS NULL;
~~~

**NULL与不匹配：在通过选择出不具有特定值的行时，可能希望返回具有NULL值得行，但是，不行！因为未知具有特殊含义，数据库不知道他们是否匹配，所以在匹配过滤或不匹配过滤时不返回他们！**



## 第七章  数据过滤

组合WHERE子句：**AND**、**OR**逻辑操作符：

~~~mysql
select prod_id, prod_price, prod_name from products where vend_id = 1003 and prod_price <= 10;

select prod_name, prod_price from products where vend_id = 1002 or vend_id = 1003;
~~~

**SQL在处理OR操作符之前，优先处理AND操作符。**应使用圆括号明确地分组相应的操作符：

~~~mysql
select prod_name, prod_price from products where (vend_id = 1002 or vend_id = 1003) and pro_price >= 10;
~~~

圆括号优先级高于AND、OR操作符。（使用圆括号没有坏处，能够消除歧义）

**IN**操作符：用来指定条件范围

~~~mysql
select prod_name, prod_price from products where vend_id in ( 1002, 1003) order by prod_name;
~~~

IN操作符与OR操作符的作用相同，但是IN操作符：

- 清楚直观
- 计算次序更容易管理（因为使用的操作符更少）
- **IN操作符一般比OR操作符执行更快**
- **IN的最大优点是可以包含其他SELECT语句，使得能够更动态地建立WHERE子句。**

**NOT**操作符：否定它之后所跟的任何条件。

~~~mysql
select prod_name, prod_price from products where vend_id not in ( 1002, 1003) order by 			prod_name;#除1002,1003之外的所有供应商制造的产品
~~~

**MySQL支持使用NOT对IN、BETWEEN和EXISTS子句取反。**



## 第八章  用通配符进行过滤

通配符：用来匹配值得一部分的特殊字符。

搜索模式：由字面值、通配符或两者组合构成的搜索条件。

未在搜索子句中使用通配符，必须使用**LIKE**操作符。LIKE操作符指示MySQL，后跟的搜索模式利用通配符匹配而不是直接相等匹配进行比较。

百分号（%）通配符：**%**表示任何字符出现任意次数：

~~~mysql
select prod_id, prod_name from products where prod_name like 'jet%';#找出所有以词jet起头的产品
~~~

可以使用多个通配符：

~~~mysql
select prod_id, prod_name from products where prod_name like '%anvil%';#匹配任何位置包含文本anvil的值
~~~

通配符可以出现在搜索模式中间：

~~~mysql
select prod_name from products where prod_name like 's%e';#以s开头，以e结尾的所有产品
~~~

**%还能匹配0个字符。**

**%不能匹配NULL。**

下划线（_）通配符：用途与%一样，但下划线只匹配单个字符，**不能多也不能少！**

通配符搜索的处理一般要比前面讨论的其他搜索花更长时间！因此：

- 不要过度使用通配符。
- 在确实需要使用通配符时，除非绝对必要，否则不要把他们放在搜索模式的开始处。把通配符置于搜索模式的开始处，搜索起来是最慢的。（？）
- 仔细注意通配符的位置。



## 第九章  用正则表达式进行搜索

正则表达式：用来匹配文本的特殊的串（字符集合）。

~~~mysql
select prod_name from products where prod_name regexp '1000' order by prod_name;#与文字正文1000匹配的一个正则表达式
~~~

**'.'** 是正则表达式语言中一个特殊的字符，表示匹配任意一个字符。

~~~mysql
select prod_name from products where prod_name regexp '.000' order by prod_name;#1000和2000都匹配返回
~~~

LIKE与REGEXP的区别：

LIKE匹配整个列的全名（因此需使用通配符），REGEXP在列名内进行匹配，即列值的一部分（不需使用通配符）。详见MySQL必知必会P53.

MySQL中的正则表达式匹配不区分大小写。为了区分大小写，使用**BINARY**关键字，如：

~~~mysql
where prod_name regexp binary 'Jetack .000';
~~~

为搜索两个串之一，使用'|'：

~~~mysql
select prod_name from products where prod_name regexp '1000|2000' order by prod_name;
~~~

匹配特定字符：用'[ ]' ：

~~~mysql
select prod_name from products where prod_name regexp '[123] Ton' order by prod_name;
~~~

[123]等同于[1|2|3]

如果不使用[ ]，|将作用于整个串，如：'1|2|3 Ton' 表示1或2或3 Ton

字符集合也可以被否定：使用**^**：如[ ^ 123]

可以使用**'-'**来定义一个范围，如[0-9]等同于[0123456789]，范围可以是字母字符，如 [a-z]。

匹配特殊字符：需匹配 '.' , '[]' , '|' , '-' 等字符时：使用**' \ \ '**进行转义：

~~~mysql
select vend_name from vendors where vend_name regexp '\\.' order by vend_name;
~~~

' \ \ '也用来引用元字符：

| 元字符 | 说明     |
| ------ | -------- |
| \ \ f  | 换页     |
| \ \ n  | 换行     |
| \ \ r  | 回车     |
| \ \ t  | 制表     |
| \ \ v  | 纵向制表 |

**为了匹配反斜杠' \ '本身，使用' \ \ \ '**

匹配字符类：为更方便工作，可以使用预定义字符集，称为字符类。见MySQL必知必会P58.

匹配多个实例：有时需要对匹配的数目进行更强的控制：使用正则表达式重复元字符来完成（放在匹配的字符后面）：

| 元字符 | 说明                         |
| ------ | ---------------------------- |
| *      | 0个或多个匹配                |
| +      | 1个或多个匹配                |
| ？     | 0个或1个匹配                 |
| {n}    | 指定数目匹配                 |
| {n, }  | 不少于指定数目的匹配         |
| {n, m} | 匹配数目的范围（m不超过255） |

如：

~~~mysql
select prod_name from products where prod_name regexp '\\([0-9] sticks?\\)' order by 			prod_name;#\\(匹配(,\\)匹配),[0-9]匹配任意数字，sticks?匹配stick和stickes（0个s或1个s）

select prod_name from products where prod_name regexp '[[:digit:]]{4}' order by 				prod_name;#[:digit:]匹配任意数字（详见P58.），{4}确切的要求它前面的字符出现4次，所以此句表示匹配任意连		在一起的4位数字
~~~

定位符：为了匹配特定位置的文本，需使用下列定位符：

| 元字符  | 说明       |
| ------- | ---------- |
| ^       | 文本的开始 |
| $       | 文本的结尾 |
| [[:<:]] | 词的开始   |
| [[:>:]] | 词的结尾   |

~~~mysql
select prod_name from products where prod_name regexp '^[0-9\\.]' order by prod_name;#找出以一个数开始的所有产品，如果使用'[0-9\\.]'匹配的是任意位置
~~~

可在不使用数据库表的情况下用SELECT来检查正则表达式，REGEXP检查总是返回0（没有匹配）或1（匹配），如：

~~~mysql
select 'hello' regexp '[0-9]';#返回0
~~~



## 第十章  创建计算字段

我们需要直接从数据库中检索出转换、计算或格式化过的数据，而不是检索出数据，再有客户机应用程序重新格式化。（这样做会更快）

计算字段是在运行时在SELECT语句内创建的。

字段：与列意思相同，术语字段通常用在计算字段的连接上。

只有数据库知道哪些是实际的表列，哪些列是计算字段。从客户机角度而言，计算字段与与其他实际列的数据以相同的方式返回。

拼接字段：**Concat()**函数（其他DBMS可能使用+或||）：

~~~mysql
select Concat(vend_name, '(', vend_country, ')') from vendors order by vend_name;#返回的列值为			vend_name(vend_country)
~~~

删除数据右侧多余的空格：RTrim()函数（左侧：LTrim() ，左右两侧Trim() ）：

~~~mysql
select Concat(RTrim(vend_name), '(', RTrim(vend_country), ')') from vendors order by 			vend_name;#返回的列值为vend_name(vend_country)
~~~

使用别名：一个未命名的列不能用于客户机，一次计算出的新列需要有一个名字，使用**AS**关键字赋予别名：

~~~mysql
select Concat(RTrim(vend_name), '(', RTrim(vend_country), ')') as vend_title from vendors 		order by vend_name;
~~~

任何客户机应用都可以按名引用这个列，就像它是一个实际的表列一样。（别名也可用于引用实际列）别名有时也称导出列。

执行算数计算：

~~~mysql
select prod_id, quantity, item_price, quantity*item_price as expanded_price from orderitems 	where order_num = 20005;
~~~

| 操作符 | 说明 |
| ------ | ---- |
| +      | 加   |
| -      | 减   |
| *      | 乘   |
| /      | 除   |



## 第十一章  使用数据处理函数

函数可移植性不那么强。

文本处理函数：

- RTrim() 去除列值右边的空格。
- Upper() 将文本转换为大写
- Left() 返回串左边的字符
- Length() 返回串的长度
- Locate() 找出串的一个子串
- Lower() 将串转换为小写
- LTrim() 去掉串左边的空格
- Right() 返回串右边的字符
- RTrim() 去掉串右边的空格
- Soundex() 返回串的SOUNDEX值（将任何一个文本串转换为描述其语音表示的字母数字模式的算法）
- SubString() 返回子串的字符

SOUNDEX考虑了类似的发音字符和音节，是得能够对串进行发音比较而不是字母比较。如使用where Soundex(cust_contact) = Soundex('Y. Lie') 可以匹配到 'Y Lee'

日期和时间处理函数：

详见P71.

MySQL使用的日期格式：yyyy-mm-dd，应该总是使用4位数字年份

~~~mysql
select cust_id, order_num from orders where order_date = '2005-09-01';
~~~

如果存储的是2005-09-01 11:30:05，则上述语句无法匹配，解决办法：Date()函数：

~~~mysql
select cust_id, order_num from orders where Date(order_date) = '2005-09-01';
~~~

数值处理函数：

Abs()、Cos()等，详见P74.



## 第十二章  汇总数据

聚集函数：用于将数据汇总，运行在行组上，计算和返回单个值的函数：

- 确定表中的行数
- 获得表中行组的和
- 找出表列的最大值、最小值、平均值等

| 函数    | 说明             |
| ------- | ---------------- |
| AVG()   | 返回某列的平均值 |
| COUNT() | 返回某列的行数   |
| MAX()   | 返回某列的最大值 |
| MIN()   | 返回某列的最小值 |
| SUM()   | 返回某列值之和   |

AVG()函数：可用来返回所有列的平均值，也可用于返回特定列或行的均值。

~~~mysql
select avg(prod_price) as avg_price from products;#返回prod_price列的均值

select avg(prod_price) as avg_price from products where vend_id = 1003;#返回特定行或列的均值
~~~

为了求多个列的均值，必须使用多个AVG()函数，AVG()函数忽略值为NULL的行。

COUNT()函数：确定表中行的数目或符合特定条件的行的数目。

~~~mysql
count(*)#统计表的行数，不管表列中包含NULL或不包含

select count(*) as num_cust from customers;

select count(cust_email) as num_cust from customers;#如果使用了列名（而不是*），则忽略NULL的行
~~~

MAX()/MIN()函数：需指定列名

~~~mysql
select max(prod_price) as max_price from products;
~~~

MAX()/MIN()忽略NULL行。

SUM()函数：需指定列名，忽略NULL行

聚集不同值：**DISTINCT**关键字

对于以上5个聚集函数，都可：

- 对所有行执行计算，指定ALL参数或不给参数（ALL是默认行为）
- 只包含不同值，指定DISTINCT参数

~~~mysql
select avg(distinct prod_price) as avg_price from products where vend_id = 1003;#只考虑各个不同	的价格
~~~

**DISTINCT**不能用于COUNT(*)，DISTINCT必须指定列名，不能用于计算或表达式。

组合聚集函数：

~~~mysql
select count(*) as num_items, min(prod_price) as price_min, max(prod_price) as price_max, 		avg(prod_price) as price_avg from products; 
~~~



## 第十三章  分组数据

**GROUP BY**子句和**HAVING**子句。

创建分组：在SELECT语句的GROUP BY子句中建立。

~~~mysql
select vend_id, count(*) as num_prods from products group by vend_id;#按vend_id排序并分组数据，因此count(*)对每个vend_id都计算一次
~~~

GROUP BY子句只是MySQL分组数据，然后对每个组而不是整个结果进行聚集。

- GROUP BY子句可以包含任意数目的列，这使得能对分组进行嵌套，为数据分组提供更细致的控制。
- 如果在GROUP BY子句中嵌套了分组，数据将在最后规定的分组进行汇总。
- GROUP BY子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。如果在SELECT中使用表达式，则必须在GROUP BY字句中指定相同的表达式。不能使用别名。
- 除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子句中给出。
- 如果分组列中具有NULL值，则将NULL作为一个分组返回。
- GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。

使用WITH ROLLUP关键字，可以得到每个分组以及每个分组汇总级别的值。

过滤分组：

where过滤指定的是行而不是分组。where没有分组概念。应该使用**HAVING**子句，HAVING子句可以过滤分组。

where子句完成的功能having都能完成。

~~~mysql
select cust_id, count(*) as orders from orders group by cust_id having count(*) >= 2;
~~~

**可以这样理解：where在分组之前过滤，而having在分组之后过滤。**

分组和排序：

| ORDER BY         | GROUP BY                                                 |
| ---------------- | -------------------------------------------------------- |
| 排序产生的输出   | 分组行。但输出可能不是分组的顺序                         |
| 任意列都可以使用 | 只可能使用选择列或表达式列，而且必须使用每个选择列表达式 |
| 不一定需要       | 如果与聚集寒湿一起使用列（或表达式），则必须使用         |

**不要忘记ORDER BY：一般使用GROUP BY子句时，应该也给出ORDER BY子句，这是保证数据正确排序的唯一方法。**

~~~mysql
select order_num, sum(quantity*item_price) as ordertotal from orderitems group by order_num having sum(quantity*item_price) >= 50 order by ordertotal;
~~~

SELECT子句顺序：SELECT->FROM->WHERE->GROUP BY->HAVING->ORDER BY->LIMIT



## 第十四章 使用子查询

子查询：嵌套在其他查询中的查询。SELECT语句中，子查询总是从内向外处理。

orders表：订单信息：订单号、客户ID、订单日期

orderitems表：各订单的物品

customers表：客户信息

检索订购了物品TNT2的客户：

~~~mysql
select cust_name, cust_contact 
from customers 
where cust_id in (select cust_id 
                  from orders 
                  where order_num in (select order_num 
                                      from order_items 
                                      where prod_id = 'TNT2'));
~~~

对于能嵌套的子查询的数目没有限制，不过在实际使用时出于性能考虑，不能嵌套太多子查询。

作为计算字段使用子查询：

每个客户的订单总数：

~~~mysql
select cust_name, cust_state,(select count(*) 
                              from orders 
                              where orders.cust_id = customers.cust_id) as orders 
from customers order by cust_name;
~~~

上例中子查询对检索出的每个客户执行一次（自动分组？）

相关子查询：涉及外部查询的子查询。任何时候只要列名可能有多义性，就必须使用完全限定名。

**使用子查询时，最好逐渐增加，先建立和测试内层查询，然后用硬编码数据建立和测试外层查询，确认正常后，嵌套，再测试。**





## 第十五章  联结表

外键：外键为某个表中的一列，它包含另一个表的主键值，定义了两个表之间的关系。

联结：一种用来在一条select语句中关联表的机制。

联结不是物理实体，即在实际的数据库表中不存在，存在于查询的执行当中。

仅允许在关系列中出现合法值，这就是维护引用完整性，它是通过在表的定义中指定主键和外键来实现的。

创建联结：

~~~mysql
select vend_name, prod_name, prod_price
from vendors, products
where vendors.vend_id = products.vend_id
order by vend_name, prod_name;
~~~

在联结两个表时，实际上是将第一个表中的每一行与第二个表中的每一行配对（笛卡尔积、叉联结），where子句作为过滤条件，因此where子句很重要。

内部联结：上例称为等值联结，又称内部联结，也可写为：

~~~mysql
select vend_name, prod_name, prod_price
from vendors inner join products
on vendors.vend_id = products.vend_id;
~~~

**应该首选INNER JOIN ON语法。**

联结多个表：SQL对一条SELECT语句中可以联结的表的数目没有限制。

~~~mysql
select prod_name, vend_name, prod_price, quantity
from orderitems, products, vendors
where products.vend_id = vendors.vend_id
	and orderitems.prod_id = products.prod_id
	and order_num = 20005;
~~~

第十四章用的例子：返回订购产品TNT2的客户列表：

~~~mysql
select cust_name, cust_contact
from customers, orders, orderitems
where customers.cust_id = orders.cust_id
	and orderitems.order_num = orders.order_num
	and prod_id = 'TNT2';
~~~



## 第十六章  创建高级联结

SQL还允许给表名起别名：

- 缩短SQL语句
- 允许在单条SELECT语句中多次使用相同的表。

~~~mysql
select cust_name, cust_contact
from customer as c, orders as o, orderitems as oi
where c.cust_id = o.cust_id
	and oi.order_num = o.order_num
	and prod_id = 'TNT2';
~~~

**与列别名不一样，表别名不返回到客户机。**

不同类型的联结：

自联结：自联结通常作为外部语句用来代替从相同的表中检索数据时使用的子查询，结果一样，但使用联结快得多。

~~~mysql
select p1.prod_id, p1.prod_name
from products as p1, products as p2
where p1.vend_id = p2.vend_id
	and p2.prod_id = 'DTNTR';#联结两张相同的表
~~~

作用等同于：

~~~mysql
select prod_id, prod_name
from products
where vend_id = (select vend_id
                from products
                where prod_id = 'DTNTR')
~~~

自然联结：标准的联结（内部联结）返回所有数据，甚至相同的列多次出现，自然联结排除多次出现，每个列只返回一次。之前提到的多有内部联结都是自然联结（可能永远都不会用到不是自然联结的内部联结）。

外部联结：许多联结将一个表中的行与另一个表中的行相关联。但有时候会需要包含没有关联的行。联结包含了那些在相关表中没有关联的行，这种联结称为外部联结。（**OUTER  JOIN  ON**)

例如：

~~~mysql
select customers.cust_id, orders.order_num
from customer inner join orders
on customers.cust_id = orders.cust_id#内部联结，检索所有客户及其订单（不包括没有订单的客户）
~~~

~~~mysql
select customers.cust_id, orders.order_num
from customers left outer join orders
on customers.cust_id = orders.cust_id#外部联结，检索所有客户及其订单（包括没有订单的客户）
~~~

**在使用OUTER JOIN语法时，必须使用RIGHT或LEFT关键字指定包括其所有行的表。**即两种基本的外联结形式：左外部联结和右外部联结。



使用带聚集函数的联结：

~~~mysql
select customers.cust_name, customers.cust_id, count(orders.orders_num) as num_ord
from customers inner join orders
on customers.cust_id = orders.cust_id
group by customers.custa_id;#检索所有客户及每个客户所下的订单数。
~~~

使用联结的要点：

- 主义所使用的联结类型。
- 保证使用正确的联结条件。
- 应该总是提供联结条件，否则会出现笛卡尔积。
- 在一个表中可以包含多个表，甚至对于每个联结可以采用不同的联结类型，但应该在一起测试它们之前，分布测试每个联结。



## 第十七章  组合查询

利用**UNION**操作符将多条SELECT语句组合成一个结果集。

MySQL允许执行多个查询（多条SELECT语句），并将结果作为单个查询结果集返回。这些组合查询成为**并**（union）或复合查询。

任何具有多个where子句的select语句都可以作为一个组合查询给出。

~~~mysql
select vend_id, prod_id, prod_price
from products
where prod_price <= 5
union
select vend_id, prod_id, prod_price
from products
where vend_id in (1001, 1002)
~~~

需要注意的规则：

- union必须由两条或两条以上select语句组成，语句之间用关键字union分隔。
- union中的每个查询必须包含相同的列、表达式或聚集函数（不过各个列不需要以相同的次序出现）。
- 列数据类型必须兼容：类型不必完全相同，但是必须是DBMS可以隐含的转换的类型。

**UNION从查询结果集中自动去除了重复的行。**如果不需要去除重复行，则使用**UNION ALL**。（UNION ALL的作用是WHERE无法取代的）。

在使用UNION组合查询是，只能使用一条ORDER BY子句，它必须出现在最后一条SELECT语句之后。



## 第十八章  全文本检索

并非所有引擎都支持全文本搜索，MyISAM支持，InnoDB不支持。

- 通配符和正则表达式匹配通常要求MySQL尝试匹配表中所有行（而这些搜索极少使用表索引）。因此，由于被搜索行数不断增加，这些搜索可能非常耗时。
- 使用通配符和正则表达式匹配，很难明确地控制匹配什么和不匹配什么。
- 虽然基于通配符和正则表达式的搜索提供了非常灵活的搜索，但它们都不能提供一种智能化的选择结果的方法。

这些都可以使用全文本搜索来解决。在使用全文本搜索时，MySQL不需要分别查看每个行，不需要分别分析和处理没个词。



全文本搜索：

为了进行安文本搜索，必须索引被搜索的列，而且要随着数据的改变不断地重新索引。在对表列进行适当设计后，MySQL会自动进行所有的索引和重新索引。

在索引之后，select可以与Match()和Against()一起使用以实际执行搜索。



启用全文本搜索支持：一般在创建表时启用。CREATE TABLE语句接受FULLTEXT子句，它给出被索引的一个逗号分隔的列表。

~~~mysql
create table productnotes(
	note_id int not null auto_increment,
    prod_id char(10) not null,
    note_date datetime not null,
    note_text text null,
    primary key(note_id),
    fulltext(note_text)
)engine=MyISAM#对note_text进行索引，也可索引多个列
~~~

在定义之后，MySQL自动维护该索引。在增加、更新、删除行时，索引随之自动更新。

**不要在导入数据是使用fulltext**，更新索引要花时间。如果正在导入数据到一个新的表，此时不应该启用fulltext，应该首先导入所有数据，然后再修改表，定义fulltext，这样有助于更快地导入数据（而且使索引数据的总时间小于在导入每行是分别进行索引所需的总时间）。

进行全文本搜索：在索引之后，使用两个函数Match()、Against()执行全文本搜索，Match()指定被搜索的列，Against()指定要使用的搜索表达式。

~~~mysql
select note_text
from productnotes
where Match(note_text) Against('rabbit');
~~~

**传递给Match()的值必须与fulltext()定义中的相同。如果指定多个列，则必须列出它们，并且次序正确。**

搜索不区分大小写，除非使用binary方式。

**全文本搜索返回以文本匹配的良好程度排序的数据。**具有较高等级的行先返回。（如第3个词出现的等级高于第20个词出现的等级）。

~~~mysql
select note_text, match(note_text) against('rabbit') as rank
from products;#第二列为计算出的等级值
~~~

等级由MySQL根据行中词的数目、唯一词的数目、整个索引中词的总数以及包含盖茨的行的数目计算出来。

**全文本搜索相当快！**

查询扩展：用来设法放宽所返回的全文本搜索结果的范围。

在使用查询扩展时，MySQL对数据和索引进行两边扫描来完成搜索：

- 首先，进行一个基本的全文本搜索，找出与搜索条件匹配的所有行。
- 其次，MySQL检查这些匹配行并选择所有有用的词
- 再其次，MySQL再次进行全文本搜索，这次不仅使用原来的条件，而且还是用所有有用的词。

~~~mysql
select note_text
from productnotes
where Match(note_text) Against('anvils' with query expansion);
~~~

布尔文本搜索：以布尔方式，可以提供关于如下内容的细节：

- 要匹配的词
- 要排斥的词
- 排列提示
- 表达式分组
- 另外一些内容

布尔方式即使没有定义fulltext索引，也可以使用，但这是一种非常缓慢的操作。

**IN BOOLEAN MODE**：

~~~mysql
select note_text
from productnotes
where Match(note_text) Against('heavy' in boolean mode);#没有指定布尔操作，其结果与没有使用布尔操作相同
~~~

~~~mysql
select note_text
from productnotes
where Match(note_text) Against('heavy -rope*' in boolean mode);#匹配包含heavy但不包含任意以repo开始的词的行
~~~

布尔操作符：

| 布尔操作符 | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| +          | 包含，词必须存在                                             |
| -          | 排除，词必须不出现                                           |
| >          | 包含，且增加等级值                                           |
| <          | 包含，且减小等级值                                           |
| ()         | 把词组成子表达式（允许这些子表达式作为一个组被包含、排除、排列等 |
| ~          | 取消一个词的排序值                                           |
| *          | 词尾的通配符                                                 |
| " "        | 定义一个短语                                                 |

~~~mysql
select note_text
from productnotes
where Match(note_text) Against('rabbit bait' in boolean mode);#没有指定操作符，这个搜索匹配包含rabbit和bait中的**至少一个词**的行
~~~

~~~mysql
select note_text
from productnotes
where Match(note_text) Against('"rabbit bait"' in boolean mode);#匹配短语rabbit bait
~~~

在布尔方式中，**不**按等级值降序排序返回的行。

全文本搜索使用说明：

- 索引全文本数据时，短词被忽略且从索引中排除。（3个或3个以下字符的词）
- MySQL内建一个非用词列表，这些词在索引全文本数据时被忽略，如果需要，可以覆盖这个列表。
- 许多次出现频率很高，搜索他们没有用处，MySQL规定了一条50%规则，如果一个词出现在50%以上的行中，则将其作为一个非用词忽略。50%规则不适用于IN BOOLEAN MODE
- 如果表行中的行数少于3，则全文本搜索不返回结果（因为每个词或者不出现，或者至少出现在50%的行中）。
- 忽略词中的单引号。例如don't索引为dont
- 不具有词分隔符的语言不能恰当地返回全文本搜索结果（汉语就是！）



## 第十九章  插入数据

**INSERT  INTO**：用来插入或添加行到数据库表。

- 插入完整的行
- 插入行的一部分
- 插入多行
- 插入某些查询的结果

~~~Mysql
insert into customers
values(null,
      'Pep',
      '100 Main Street',
      'Los Angeles',
      'CA',
      '90046',
      'USA',
      null,
      null);
~~~

insert语句一般没有输出。

**值的次序很重要！**

但上述方式并不安全，不能保证获得次序的信息，次序的信息也有可能改变。

更安全的方式：

~~~mysql
insert into customers(cust_name,
                     cust_adress,
                     cust_city,
                     cust_state,
                     cust_zip,
                     cust_country,
                     cust_contact,
                     cust_email)
values(
      'Pep',
      '100 Main Street',
      'Los Angeles',
      'CA',
      '90046',
      'USA',
      null,
      null);#这种方式就不一定按照实际表列的次序了。且自增的列也不必写null了
~~~

多个用户并发访问时，如果数据检索是最重要的，则可在INSERT和INTO之间添加**LOW_PRIORITY**关键字，以降低插入数据的优先级，这也适用于UPDATE和DELETE.

插入多个行：

~~~mysql
insert into customers(cust_name,
                     cust_adress,
                     cust_city,
                     cust_state,
                     cust_zip,
                     cust_country)
values(
      'Pep',
      '100 Main Street',
      'Los Angeles',
      'CA',
      '90046',
      'USA');
insert into customers(cust_name,
                     cust_adress,
                     cust_city,
                     cust_state,
                     cust_zip,
                     cust_country)
values(
      'Martian',
      '42 Street',
      'New York',
      'NY',
      '11213',
      'USA');
~~~

或者：

~~~mysql
insert into customers(cust_name,
                     cust_adress,
                     cust_city,
                     cust_state,
                     cust_zip,
                     cust_country)
values(
      'Pep',
      '100 Main Street',
      'Los Angeles',
      'CA',
      '90046',
      'USA')，
	(
      'Martian',
      '42 Street',
      'New York',
      'NY',
      '11213',
      'USA');
~~~

单条INSERT语句处理多个插入比使用多条INSERT语句快！

插入检索出的数据：**INSERT SELECT**

~~~mysql
insert into customers(cust_id,
                     cust_contact,
                      ...)
             select cust_id,
             		cust_contact,
             		...
             from custnew;#列名不一定要匹配，MySQL不关心select返回的列名，而是使用列的位置
~~~

insert select的select语句还可包含where子句。



## 第二十章  更新和删除数据

**UPDATE和DELETE**

使用时，一定不要忽略WHERE子句，不然一不小心就更新或删除了所有行。

更新：

~~~mysql
update customers
set cust_name = 'fudds',
	cust_email = '1111@kkk.com'
where cust_id = 10005;
~~~

更新时如遇错误，默认会恢复到更新之前的状态，若要忽略错误继续更新，使用UPDATE IGNORE customers...

删除行：

~~~mysql
delete from customers
where cust_id = 10006;
~~~

删除所有行：使用**TRUNCATE TABLE**语句（删除原来的表，创建一张新表），比DELETE快。

在对UPDATE或DELETE使用WHERE之前，先用SELECT进行测试。

使用强制实施引用完整性的数据库，这样MySQL将不允许删除具有与其他表相关联的数据的行。



## 第二十一章  创建和操纵表

创建表：**CREATE TABLE**

~~~mysql
create table customers
(
    cust_id int not null auto_increment,
    cust_name char(50） not null,
    ...
    primary key(cust_id)
)engine=InnoDB;
~~~

如果仅想在一个表不存在时创建它，可在表名后给出**IF NOT EXISTS**.

如果不指定not null，则默认指定的是null。

主键值必须唯一。（多个列构成的主键组合值必须唯一），主键只能使用not null。

每个表允许一个AUTO_INCREMENT列，而且它必须被索引（如，通过使它成为主键）。

可以在insert时指定一个值，用以覆盖auto_increment生成的值，只要提供的值是唯一的即可。后续的增量将开始使用该手工插入的值。

可使用last_insert_id()函数获得上一个生成的自增量值：select last_insert_id()

指定默认值：**DEFAULT**关键字：

~~~mysql
create table orderitems
(
	...
    quantity int not null default 1,
    ...
)engine=InnoDB;
~~~

MySQL仅支持常量默认值，不支持函数。

引擎类型：不同引擎具有各自不同的功能和特性，为不同人物选择正确的引擎能获得良好的功能和灵活性。

- InnoDB：一个可靠的事务处理引擎，不支持全文本搜索。

- MEMORY：功能上等同于MyISAM，但由于数据存储在内存中，速度很快

- MyISAM：一个性能极高的引擎，它支持全文本搜索，不支持事务处理。

引擎类型可以混用，但外键不能跨引擎，即使用一个引擎的表不能引用具有使用不同引擎的表的外键（？）



更新表：更新表的**定义**：**ALTER TABLE**

理想状态下，当表中存储了数据后，该表应尽量避免更新。

~~~mysql
alter table vendors
add vend_phone char(20);#添加列

alter table vendors
drop column vend_phone;#删除列

alter table orderitems
add constraint fk_orderitems_orders
foreign key (order_num) references orders (order_num);#添加外键


~~~

使用ALTER TABLE应非常小心，使用之前应完整备份。



删除表：**DROP TABLE** customers2;

重命名表：RENAME ... TO ... , ... TO ...;



## 第二十二章  使用视图

视图：虚拟的表。不包含表中应该有的任何列或者数据，只包含使用时动态检索数据的查询（只是一个查询）。

视图作用：

- 重用SQL语句。
- 简化复杂的SQL操作。在编写查询后，可以方便的重用它而不必知道它的基本查询细节。
- 使用表的组成部分而不是整个表。
- 保护数据，给用户授予表的特定部分的访问权限而不是整个表的访问权限。
- 更改数据格式和表示。

在视图创建之后，可以用于表基本相同的方式利用它们。（添加和更新数据存在某些限制）

**视图本身不包含数据**，因此它们返回的数据是从其他表中检索出来的。在添加和更改这些表中的数据时，视图将返回改变过的数据。

性能问题：因为视图不包含数据，所以每次使用视图时，都必须处理查询执行时所需的任一个检索。如果用多个联结或过滤创建了复杂的视图或者嵌套视图，可能会发现性能下降得厉害。在部署使用了大量视图的应用前，应该进行测试。

视图的规则和限制：

- 与表一样，视图必须唯一命名。
- 对于可以创建的额视图数目没有限制。
- 为了创建视图，必须具有足够的访问权限，这些限制通常由数据库管理人员授予。
- 视图可以嵌套。
- ORDER BY可以用在视图中，但如果从该视图检索数据的SELECT语句也包含ORDER BY，那么该视图中的ORDER BY将会被覆盖。
- 视图不能索引，也不能有关联的触发器或默认值。
- 视图可以和表一起使用。

创建视图：**CREATE VIEW**

查看创建视图的语句：**SHOW CREATE VIEW viewname**

删除视图：**DROP VIEW viewname**

更新视图：可先用DROP再用CREATE，也可直接使用**CREATE OR REPLACE VIEW**

~~~mysql
create view productcustomers as
select cust_name, cust_contact, prod_id
from customers, orders, orderitems
where customers.cust_id = orders.cust_id
	and orderitems.order_num = orders.order_num;
	
select cust_name, cust_contact
from productcustomers
where prod_id = 'TNT2';
~~~

利用视图，可以一次性编写好基础的SQL，然后根据需要多次使用。

如果从视图检索数据时使用了一条where子句，则两组子句（一组在视图中，另一组是传递给视图的）将自动结合。

通常，视图时可更新的（INSERT、UPDATE、DELETE），更新一个视图即更新其基表。

但并非所有视图都可更新，如果MySQL不能正确的确定被更新的基数据，则不允许更新（包括插入和删除），如视图定义中有以下操作，则不允许更新：

- 分组（使用GROUP BY和HAVING）
- 联结
- 子查询
- 并
- 聚集函数
- DISTINCT
- 导出（计算）列

**最好只将视图应用于检索。**



## 第二十三章  使用存储过程

存储过程：为以后的使用而保存的一条或多条MySQL语句的集合。简单、安全、高性能。

使用存储过程比使用单独的SQL语句要快！

执行（调用）存储过程：**CALL**

CALL接受存储过程的名字及需要传递给它的参数：

~~~mysql
call productpricing(@pricelow,
                    @pricehigh,
                    @priceaverage);#计算并返回产品的最低、最高和平均价格
~~~

创建存储过程：

~~~mysql
create procedure productpricing()
begin
	select avg(prod_price) as priceaverage
	from products;
end  #计算并返回产品的平均价格
~~~

临时更改语句分隔符：除\外，任何字符都可以用做语句分隔符。

~~~mysql
delimiter //
...
delimiter ;
~~~

删除存储过程：DROP PROCEDURE 或 DROP PROCEDURE IF EXISTS

使用参数：IN（传递给存储过程）、OUT（从存储过程传出）、INOUT（传入和传出）

一般，存储过程并不显示结果，而是把结果返回给指定的变量：

~~~mysql
create procedure productpricing(
	OUT pl DECIMAL(8,2)
	OUT ph DECIMAL(8,2)
	OUT pa DECIMAL(8,2)
)
begin
	select min(prod_price)
	into pl
	from products;
	
	select max(prod_price)
	into ph
	from products;
	
	select avg(prod_price)
	into pa
	from products;
end;

call productpricing(@pricelow,
                    @pricehigh,
                    @priceaverage);
                    
select @priceaverage;
~~~

**不能通过一个参数返回多个行和列。**MySQL变量都必须以@开始。

~~~mysql
create procedure ordertotal(
	in onumber int,
	out ototal decimal(8, 2)
)
begin
	select sum(item_price*quantity)
	from orderitems
	where order_num = onumber
	into ototal;
end;

call ordertotal(20005, @total);
~~~

可用**DECLARE**语句定义局部变量。还有IF...THEN...ELSEIF...THEN...END IF

**COMMENT**关键字：如果给出，将在SHOW PROCEDURE STATUS的结果中显示。

非零值都为真，只有零值为假。



## 第二十四章  使用游标

有时需要在检索出来的行中前进或后退一行或多行，这就是使用游标的原因。

游标（cursor）：游标是存储在MySQL服务器上的数据库查询，它不是一条select语句，而是被该语句检索出来的结果集。主要应用于交互式应用。MySQL游标只能用于存储过程。

- 在能够使用游标之前，必须声明它。这个过程没有检索数据，只是定义要使用的select语句。
- 一旦声明之后，必须打开游标以供使用，这个过程用前面定义的select语句把数据实际检索出来。
- 对于填有数据的游标，根据需要取出各行。
- 在结束游标使用时，必须关闭游标。

创建游标：**DECLARE**

~~~mysql
create procedure processorders()
begin
	declare ordernumbers cursor
	for
	select order_num from orders;
end;
~~~

打开和关闭游标：**OPEN**、**CLOSE**

~~~mysql
open ordernumbers;

close ordernumbers;
~~~

在处理open语句时执行查询。

close释放游标使用的所有内存和资源，因此每个游标不再需要时都应该关闭。

需要再次使用时，open就行，不必重新声明。

MySQL会在到达end语句时自动关闭游标。

使用游标数据：**FETCH**

使用FETCH分别访问游标的每一行

~~~mysql
create procedure processorders()
begin

	declare o int;
	declare ordernumbers cursor
	for
	select order_num from orders;
	
	open ordernumbers;

	fetch ordernumbers into o;

	close ordernumbers;
end;
~~~

用declare语句定义的局部变量必须在定义游标或句柄之前定义，而句柄必须在游标之后定义。



~~~mysql
create procedure processorders()
begin
	declare done boolean default 0;
	declare o int;
	declare t decimal(8, 2);
	
	declare ordernumbers cursor
	for
	select order_num from orders;
	
	declare continue handler for sqlstate '02000' set done = 1;
	
	create table if not exists ordertotals(order_num int, total decimal(8, 2));
	
	open ordernumbers;
	
	repeat
		fetch ordernumbers into o;
		call ordertotal(o, 1, t);
		
		insert into ordertotals(order_num, total)
		values(o, t);
		
	until done end repeat;
	
	close ordernumbers;
end;
~~~



## 第二十五章  使用触发器

让某条语句在事件发生时自动执行。

触发器是MySQL响应以下任意语句而自动执行的一条MySQL语句：

- DELETE
- INSERT
- UPDATE

其他MySQL语句不支持触发器。



创建触发器：**CREATE TRIGGER**

- 唯一的触发器名。
- 触发器关联的表。
- 触发器应该响应的活动。
- 触发器何时执行。

触发器必须在每个表中唯一，但不是在每个数据库中唯一。即同一个数据库中的两个表可具有相同名字的触发器。但以后可能会限制，因此最好在数据库范围内使用唯一的触发器名。

~~~mysql
create trigger newproduct after insert on products
for each row select 'Product added';#每插入一行，显示一次Product added
~~~

**只有表支持触发器，视图不支持，临时表也不支持。**

触发器按每个表每个事件每次地定义，每个表每个事件每次只允许一个触发器。因此每个表最多支持6个触发器（每条INSERT、UPDATE、DELETE之前和之后）。单一触发器不能与多个事件或多个表关联。

如果before触发器失败，则不执行请求的操作。如果before触发器或语句本身失败，将不执行after触发器。

删除触发器：**DROP TRIGGER**

~~~mysql
drop trigger newproduct;
~~~

触发器不能更新或覆盖，为了修改一个触发器，必须先删除它，然后再重新创建。



INSERT触发器：

- insert触发器代码内，可饮用一个名为NEW的虚拟表，访问被插入的行。
- 在before insert触发器中，new中的值也可以被更新（允许更改被插入的值）。
- 对于auto_increment列，new在insert执行之前包含0，在insert执行之后包含新的自动生成值。

~~~mysql
create trigger neworder after insert on orders
for each row select new.order_num;
~~~

通常将before用于数据验证和净化。



DELETE触发器：

- 在delete触发器代码内，可以引用一个名为OLD的虚拟表，访问被删除的行。
- OLD中的值全都是只读的，不能更新。

~~~mysql
create trigger deleteorder before delete on orders
for each row
begin
	insert into archive_orders(order_num, order_date, cust_id)
	values(old.order_num, old.order_date, old.cust_id);
end;
~~~

使用before delete触发器的优点（相对于after delete）：如果由于某种原因，订单不能存档，delete本身将被放弃。



UPDATE触发器：

- 在update触发器代码中，可以引用一个名为OLD的虚拟表，访问以前的值，引用一个名为NEW的虚拟表访问新更新的值。
- 在before update触发器中，new中的值可能也被更新（允许更改将要用于update语句中的值）。
- old中的值全都是只读的，不能更新。

~~~mysql
create trigger updatevendor before update on vendors
for each row set new.vend_state = Upper(new.vend_state);#确保插入的值都是大写（净化数据）
~~~

- 创建触发器可能需要特殊的安全访问权限，但是，触发器的执行时自动的。
- 应该用触发器来保证数据的一致性。
- 触发器的一种非常有意义的使用是创建审计跟踪。使用触发器，把更改记录到另一个表非常容易。
- MySQL触发器不支持CALL语句，即不能从触发器调用存储过程。



## 第二十六章  管理事务处理

并非所有引擎都支持事务处理。MyISAM和InnoDB，前者不支持，后者支持。

事务处理可以用来维护数据库的完整性，它保证成批的MySQL操作要么完全执行，要么完全不执行。以保证数据库不包含不完整的操作结果。

- 事务：一组SQL语句。

- 回退：撤销指定SQL语句的过程。

- 提交：将为存储的SQL语句结果写入数据库表。

- 保留点：指事务处理中设置的临时占位符，可以对它发布回退。（与回退整个事务处理不同）

控制事务处理：

关键在于将SQL语句组分解为逻辑块，并明确规定数据何时应该回退，何时不应该回退。

用**START TRANSACTION**标识事务开始。

用**ROLLBACK**命令来回退。只能在一个事务处理中使用。

不能回退CREATE或DROP操作。

提交操作是自动进行的，为了进行明确的提交：用**COMMIT**语句。当commit或rollback语句执行后，事务会自动关闭（隐含事务关闭）。

保留点：处理部分提交或回退，**SAVEPOINT**语句。

~~~mysql
savepoint delete1；

...

rollback to delete1;
~~~

**保留点越多越好。**

释放保留点：事务处理完成自动释放，也可用**RELEASE SAVEPOINT**明确地释放。

更改默认的提交行为：

为指示MySQL不自动提交更改：set autocommit = 0；

autocommit标志是针对每个连接而不是服务器的。



## 第二十七章  全球化和本地化

不同的语言和字符集需要以不同的方式存储和检索数据。MySQL需要适应不同的排序和检索数据的方法。

字符集：字母和符号的集合

编码：某个字符集成员的内部表示。

校对：规定字符如何比较的指令。

使用何种字符集和校对的决定在服务器、数据库和表级进行。

查看支持的字符集完整列表：**SHOW CHARACTER SET**

查看所支持校对的完整列表：**SHOW COLLATION**

确定字符集和校对：

**SHOW VARIABLES LIKE ‘character%';**

**SHOW VARIABLES LIKE ‘collation%';**

给表指定的字符集和校对：

~~~mysql
create table mytable(
	column1 int,
    column2 varchar(10)
)default character set hebrew
 collate hebrew_general_ci;
~~~

- 如果指定了两者，则使用这些值。
- 如果只指定了CHARATER SET，则使用此字符集及其默认校对。
- 如果都没指定，则使用数据库默认。

对每个列设置他们：

~~~mysql
create table mytable(
	column1 int,
    column2 varchar(10)
    column3 varchar(10) charater set latin1 collate latin1_general_ci
)default character set hebrew
 collation hebrew_general_ci;
~~~

如果需要用与创建表时不同的校对顺序排序特定的select语句，可以在select语句中进行：

~~~mysql
select * from customers
order by lastname, firstname collate latin_general_cs;
~~~

collate还可用于group by， having，聚集函数，别名等。

串可以在字符集之间转换：使用Cast()或Convert()函数。



## 第二十八章  安全管理

用户应具有特定的访问权限，不能多也不能少。

管理访问控制需要创建和管理用户账号。

不要在日常的MySQL操作中使用root

MySQL用户账号和信息存储在名为mysql的数据库中。其中有一个名为user的表，包含所有用户，user表中有一个user列，存储用户登录名。

创建用户账号：**CREATE USER**

~~~mysql
create user ben identified by 'password';
~~~

口令不是必要的。

identified by指定的口令为纯文本，MySQL在将其保存到user表之前对其进行了加密。为了作为散列值指定口令，使用identified by password

重命名：

~~~mysql
rename user ben to bforta；
~~~

删除用户及其权限：

~~~mysql
drop user bforta;
~~~

设置访问权限：

新创建用户没有访问权限。

查看用户权限：

~~~mysql
show grants for bforta;
~~~

设置权限：**GRANT**

~~~mysql
grant select on crashcourse.* to bforta;#允许用户在crashcourse数据的所有表上使用select
~~~

grant的反操作为**REVOKE**，用来撤销特定的权限：

~~~mysql
revoke select on crashcourse.* from bforta;
~~~

grant和revoke可以在几个层次上控制访问权限：

- 整个服务器：使用grant all和revoke all
- 整个数据库：使用on database.*
- 特定的表：on database.table
- 特定的列
- 特定的存储过程

权限列表：详见P202.

可以对不存在的数据库或表设置权限，但这样做的副作用是当某个数据库或表被删除时，相关的访问权限仍然存在，而且，如果未来重新创建这些数据库或表，相关权限仍然起作用。

~~~mysql
grant select, insert on crashcourse.* to bforta;#一次设置多个权限
~~~

更改口令：

~~~mysql
set password for bforta = Password('...');
~~~

Password()函数用于加密。

上例如不指定用户名，则是更改当前登录用户的口令。



## 第二十九章  数据库维护

数据备份：详见P205.

数据库维护：

检查表键是否正确：**ANALYZE TABLE**

**CHECK TABLE**用来检查表的许多问题，在MyISAM表上还对索引进行检查。

**CHANGED**检查最后一次检查以来改动过的表。

**EXTENDED**执行最彻底的检查

**FAST**只检查未正常关闭的表

**MEDIUM**检查所有被删除的链接并进行键检验

**QUICK**只进行快速扫描



如果MyISAM表访问产生不一致和不正确的结果，可能需要用**REPAIR TABLE**来修复，这条语句不应经常使用，如需经常使用，则意味着可能会有更大的问题要解决。

如果从一个表中删除大量数据，应该使用**OPTIMIZE TABLE**来回收所有空间，从而优化性能。

在排除系统启动问题时，应尽量手动启动服务器。MySQL服务器自身通过在命令行上执行mysqld启动。



## 第三十章  改善性能

P209.