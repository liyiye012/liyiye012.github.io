title: 数据库学习笔记VI
date: 2019-11-5 11:54:12
categories: 2019年11月
tags: [Database，MySQL]

---

MySQL优化及常用函数CONCAT,IFNULL,
case when then else end用法,数据排序 asc、desc,
Where与Having的区别

<!-- more -->
# SQL CONCAT 函数
CONCAT 函数用于将两个字符串连接为一个字符串，试一下下面这个例子：

    SQL> SELECT CONCAT('FIRST ', 'SECOND');
    +----------------------------+
    | CONCAT('FIRST ', 'SECOND') |
    +----------------------------+
    | FIRST SECOND               |
    +----------------------------+
    1 row in set (0.00 sec)

# case when then else end用法

    -简单case函数
    case sex
      when '1' then '男'
      when '2' then '女’
      else '其他' end
    --case搜索函数
    case when sex = '1' then '男'
         when sex = '2' then '女'
         else '其他' end  
需要注重的问题，case函数只返回第一个符合条件的值，剩下的case部分将会被自动忽略。

参考练习题目：https://leetcode.com/problems/swap-salary/submissions/
# GROUP BY 语句
GROUP BY 语句用于结合合计函数，根据一个或多个列对结果集进行分组。

实例
我们拥有下面这个 "Orders" 表：

    O_Id	OrderDate	OrderPrice	Customer
    1	2008/12/29	1000	Bush
    2	2008/11/23	1600	Carter
    3	2008/10/05	700	Bush
    4	2008/09/28	300	Bush
    5	2008/08/06	2000	Adams
    6	2008/07/21	100	Carter
现在，我们希望查找每个客户的总金额（总订单）。

我们想要使用 GROUP BY 语句对客户进行组合。

我们使用下列 SQL 语句：

    SELECT Customer,SUM(OrderPrice) FROM Orders
    GROUP BY Customer
结果集类似这样：

    Customer	SUM(OrderPrice)
    Bush	2000
    Carter	1700
    Adams	2000
# IFNULL函数
MySQL控制流函数之一，它接受两个参数，如果不是NULL，则返回第一个参数。 否则，IFNULL函数返回第二个参数。
两个参数可以是文字值或表达式。
以下说明了IFNULL函数的语法：

    IFNULL(expression_1,expression_2);

如果expression_1不为NULL，则IFNULL函数返回expression_1; 否则返回expression_2的结果。
IFNULL函数根据使用的上下文返回字符串或数字。
如果要返回基于TRUE或FALSE条件的值，而不是NULL，则应使用IF函数。

    SELECT IFNULL(NULL,'IFNULL function'); -- returns IFNULL function

参考练习题目：https://leetcode.com/problems/second-highest-salary/

# 数据排序 asc、desc
1、单一字段排序order by 字段名称
作用： 通过哪个或哪些字段进行排序
含义： 排序采用 order by 子句，order by 后面跟上排序字段，排序字段可以放多个，多个采用逗号间隔，order by默认采用升序（asc），如果存在 where 子句，那么 order by 必须放到where 语句后面。
（1）、按照薪水由小到大排序（系统默认由小到大）
例如： select ename,sal from emp order by sal;
（2）、取得job 为 MANAGER 的员工，按照薪水由小到大排序（系统默
认由小到大）
例如： select ename,job,sal from emp where job = ”MANAGER”order by sal;
如果包含 where 语句 order by 必须放到 where 后面，如果没有 where 语句 order by 放到表的后面；

2、手动指定字段排序
（1）、手动指定按照薪水由小到大排序（升序关键字 asc）
例如： select ename,sal from emp order by sal asc;
（2）、手动指定按照薪水由大到小排序（降序关键字desc）
例如： select ename,sal from emp order by sal desc;
3、多个字段排序
（1）、按照 job 和薪水倒序排序
例如： select ename,job,ename from emp order by job desc,sal desc;
注意： 如果采用多个字段排序，如果根据第一个字段排序重复了，会根据第二个字段排序；
4、使用字段位置排序
（1）、按照薪水升序排序（不建议采用此方法，采用数字含义不明确，可读性不强，程序不健壮）
select * from emp order by 6;
参考练习题目：https://leetcode.com/problems/not-boring-movies/

# Where与Having的区别
## 概述
“Where” 是一个约束声明，使用Where来约束来之数据库的数据，Where是在结果返回之前起作用的，且Where中不能使用聚合函数。

“Having”是一个过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在Having中可以使用聚合函数。

## 区别
在说区别之前，得先介绍GROUP BY这个子句，而在说GROUP子句前，又得先说说“聚合函数”——SQL语言中一种特殊的函数。例如SUM, COUNT, MAX, AVG等。这些函数和其它函数的根本区别就是它们一般作用在多条记录上。

如：
    SELECT SUM(population) FROM vv_t_bbc ;

这里的SUM作用在所有返回记录的population字段上，结果就是该查询只返回一个结果，即所有国家的总人口数。

　 　而通过使用GROUP BY 子句，可以让SUM 和 COUNT 这些函数对属于一组的数据起作用。当你指定 GROUP BY region 时，只有属于同一个region（地区）的一组数据才将返回一行值，也就是说，表中所有除region（地区）外的字段，只能通过 SUM, COUNT等聚合函数运算后返回一个值。

   下面再说说“HAVING”和“WHERE”：
　HAVING子句可以让我们筛选成组后的各组数据，WHERE子句在聚合前先筛选记录．也就是说作用在GROUP BY 子句和HAVING子句前；而 HAVING子句在聚合后对组记录进行筛选。

　　让我们还是通过具体的实例来理解GROUP BY 和 HAVING 子句：

### SQL实例
一、显示每个地区的总人口数和总面积：

    SELECT region, SUM(population), SUM(area)
    FROM bbc
    GROUP BY region

先以region把返回记录分成多个组，这就是GROUP BY的字面含义。分完组后，然后用聚合函数对每组中的不同字段（一或多条记录）作运算。

二、显示每个地区的总人口数和总面积．仅显示那些人口数量超过1000000的地区。

    SELECT region, SUM(population), SUM(area)
    FROM bbc
    GROUP BY region
    HAVING SUM(population)>1000000

[注]在这里，我们不能用where来筛选超过1000000的地区，因为表中不存在这样一条记录。

　　相反，HAVING子句可以让我们筛选成组后的各组数据．

ps:如果想根据sum后的字段进行排序可以在后面加上：order by sum(population) desc/asc



## 结论
1.当分组筛选的时候 用having

2.其它情况用where
-----------------------------------------------------
用having就一定要和group by连用，
用group by不一有having （它只是一个筛选条件用的）

只要条件里面的字段, 不是表里面原先有的字段就需要用having。

SQL在查询表的时候先把查询的字段放到了内存里，而where查询的时候是从表里面查的，其余需要用having。

参考练习题：https://leetcode.com/problems/classes-more-than-5-students

# 行转列(PIVOT)与列转行(UNPIVOT)

在做数据统计的时候，行转列，列转行是经常碰到的问题。case when方式太麻烦了，而且可扩展性不强，可以使用 PIVOT，UNPIVOT比较快速实现行转列，列转行，而且可扩展性强。

某表正常情况下的查询结果是这样：

    星期一           1000
    星期二           2000
    星期三           3000
    星期四           4000
    星期五           5000
    星期六           6000
    星期日           7000

希望在一行中显示周一至周日的收入
行转列后是这样：

星期一   星期二   星期三   星期四   星期五   星期六   星期日
1000    2000    3000    4000    5000    6000    7000

也就是说，行转列后，原来的某个列的值变做了列名，在这里就是原来WEEK列的值“星期一”,"星期二"..."星期日"边做了列名，而我们需要做的另一个工作就是计算这些列的值（这里的“计算”其实就是PIVOT里面的聚合函数(sum,avg等)）

    SELECT [星期一],[星期二],[星期三],[星期四],[星期五],[星期六],[星期日]--这里是PIVOT第三步（选择行转列后的结果集的列）这里可以用“*”表示选择所有列，也可以只选择某些列(也就是某些天)
    FROM Table_Name --这里是PIVOT第二步骤(准备原始的查询结果，因为PIVOT是对一个原始的查询结果集进行转换操作，所以先查询一个结果集出来)这里可以是一个select子查询，但为子查询时候要指定别名，否则语法错误
    PIVOT
    (
        SUM(INCOME) for [week] in([星期一],[星期二],[星期三],[星期四],[星期五],[星期六],[星期日])--这里是PIVOT第一步骤，也是核心的地方，进行行转列操作。聚合函数SUM表示你需要怎样处理转换后的列的值，是总和(sum)，还是平均(avg)还是min,max等等。例如如果week_income表中有两条数据并且其week都是“星期一”，其中一条的income是1000,另一条income是500，那么在这里使用sum，行转列后“星期一”这个列的值当然是1500了。后面的for [week] in([星期一],[星期二]...)中 for [week]就是说将week列的值分别转换成一个个列，也就是“以值变列”。但是需要转换成列的值有可能有很多，我们只想取其中几个值转换成列，那么怎样取呢？就是在in里面了，比如我此刻只想看工作日的收入，在in里面就只写“星期一”至“星期五”（注意，in里面是原来week列的值,"以值变列"）。总的来说，SUM(INCOME) for [week] in([星期一],[星期二],[星期三],[星期四],[星期五],[星期六],[星期日])这句的意思如果直译出来，就是说：将列[week]值为"星期一","星期二","星期三","星期四","星期五","星期六","星期日"分别转换成列，这些列的值取income的总和。
    )TBL--别名一定要写

用法示例参见笔记：《1179. Reformat Department Table》

练习题目：https://leetcode.com/problems/reformat-department-table/discuss/382960/MSSQL-Multiple-joins-GroupBy-and-Pivot-table-solutions

#参考资料
【1】 https://tech.meituan.com/2014/06/30/mysql-index.html
【2】 https://blog.csdn.net/qq_37923253/article/details/79688313
