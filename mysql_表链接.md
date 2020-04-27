题目】
下面是某公司每天的营业额，表名为“日销”。“日期”这一列的数据类型是日期类型（date）。

![1.png](https://pic.leetcode-cn.com/6b3a7e929e0c6a3cc1d424c48ff47b5d85b7b1aab0199e548ae19c7ab6bad5a6-1.png)

请找出所有比前一天（昨天）营业额更高的数据。（前一天的意思，如果“当天”是1月，“昨天”（前一天）就是1号）

例如需要返回一下结果：

![1.png](https://pic.leetcode-cn.com/f69aff7a115ab250af7782600bea8721cbd5e425ef7637d893314ec57383d8f2-1.png)

【解题思路】

1.交叉联结

首先我们来复习一下之前课程《从零学会sql》里讲过的交叉联结（corss join）的概念。

使用交叉联结会将两个表中所有的数据两两组合。如下图，是对表“text”自身进行交叉联结的结果：

![1.jpg](https://pic.leetcode-cn.com/c6426f81fe0cc6ad436d93bbe04243d27642392e51de57ba46c1ddf6e9e346c9-1.jpg)


直接使用交叉联结的业务需求比较少见，往往需要结合具体条件，对数据进行有目的的提取，本题需要结合的条件就是“前一天”。

2.本题的日销表交叉联结的结果（部分）如下。这个交叉联结的结果表，可以看作左边三列是表a，右边三列是表b。

![1.jpg](https://pic.leetcode-cn.com/46ad928053925cf8a61d5b2037fbe442054290341a9cff8ab3bb897b3a60f041-1.jpg)


红色框中的每一行数据，左边是“当天”数据，右边是“前一天”的数据。比如第一个红色框中左边是“当天”数据（2号），右边是“前一天”的数据（1号）。

题目要求，销售额条件是：“当天” > “昨天”（前一天）。所以，对于上面的表，我们只需要找到表a中销售额（当天）大于b中销售额（昨天）的数据。

3.另一个需要着重去考虑的，就是如何找到 “昨天”（前一天），这里为大家介绍两个时间计算的函数：
daffdate(日期1, 日期2)：
得到的结果是日期1与日期2相差的天数。
如果日期1比日期2大，结果为正；如果日期1比日期2小，结果为负。

例如：日期1（2019-01-02），日期2（2019-01-01），两个日期在函数里互换位置，就是下面的结果

![1.png](https://pic.leetcode-cn.com/97d3e95d343fe80c2c3a934021893a3e7518d0ff17b3a84cadb3e9f9ed53de6c-1.png)

另一个关于时间计算的函数是：
timestampdiff(时间类型, 日期1, 日期2)
这个函数和上面diffdate的正、负号规则刚好相反。
日期1大于日期2，结果为负，日期1小于日期2，结果为正。

在“时间类型”的参数位置，通过添加“day”, “hour”, “second”等关键词，来规定计算天数差、小时数差、还是分钟数差。示例如下图：

![1.png](https://pic.leetcode-cn.com/006f72189f8a62549e64a2236cc9dc03d484e914e49dfa4d7a061f0e758983e4-1.png)

【解题步骤】
1.将日销表进行交叉联结

![1.jpg](https://pic.leetcode-cn.com/6b69e6cbde87d280749bcb6435ea62d0d5d6c4a1bc2c94bee8924e53dfbf8d68-1.jpg)

2.选出上图红框中的“a.日期比b.日期大一天”

可以使用“diffdate(a.日期, b.日期) = 1”或者“timestampdiff(day, a.日期, b.日期) = -1”，以此为基准，提取表中的数据，这里先用diffdate进行操作。

代码部分：

```mysql
select *
from 日销 as a cross join 日销 as b 
     on datediff(a.日期, b.日期) = 1;
```

得到结果：

![1.jpg](https://pic.leetcode-cn.com/7a26da3ad8f99df55ed2e1114fcac01abdf70b8dd143216ddfbdd268c21137de-1.jpg)

3.找出a中销售额大于b中销售额的数据

where a.销售额（万元） > b.销售额（万元）

得到结果：

![1.jpg](https://pic.leetcode-cn.com/91e27d2cf78b1b935c79f1d5ad885bd9cb062c35519e91b7bd20726e9dbcec40-1.jpg)

4.删掉多余数据

题目只需要找销售额大于前一天的ID、日期、销售额，不需要上表那么多数据。所以只需要提取中上表的ID、日期、销售额（万元）列。
结合一开始提到的两个处理时间的方法，最终答案及结果如下：

```mysql
select a.ID, a.日期, a.销售额（万元）
from 日销 as a cross join 日销 as b 
     on datediff(a.日期, b.日期) = 1
where a.销售额（万元） > b.销售额（万元）;
```


或者

```mysql
select a.ID, a.日期, a.销售额（万元）
from 日销 as a cross join 日销 as b 
     on timestampdiff(day, a.日期, b.日期) = -1
where a.销售额（万元） > b.销售额（万元）;
```

![1.jpg](https://pic.leetcode-cn.com/b0849d33f09efa06bf707613211824c9e0b749cd6f31af0e27a5ea5cd87ec199-1.jpg)

【本题考点】
1）考察逻辑思维能力，可以使用课程《分析方法》中的逻辑树分析方法将复杂问题拆解成一个一个可以解决的子问题
2）考察多表联结
3）针对时间的处理语句是在业务中经常用到的，需要熟练掌握。
4） 尤其考察对不同sql数据格式处理的掌握程度，

【举一反三】
下面是气温表，名为weather，date列的数据格式为date，请找出比前一天温度更高的ID和日期

![1.png](https://pic.leetcode-cn.com/8b2368de8e9ffcbff38695d6fd4a84cec47c3de18f9e70a9cdc3d235c81028dd-1.png)


参考答案：

```mysql
select a.ID, a.date
from weather as a cross join weather as b 
     on datediff(a.date, b.date) = 1
where a.temp > b.temp;
```


或者

```mysql
select a.ID, a.date
from weather as a cross join weather as b 
     on timestampdiff(day, a.date, b.date) = -1
where a.temp > b.temp;
```


得到结果

![1.png](https://pic.leetcode-cn.com/a68b6d32cb82e823aa68981eaa64d7eaec42919ecf0e2275778e924aa712c075-1.png)


我是猴子，微信公众号（猴子聊人物），知乎：猴子

----

【题目】

下面是学生的名单，表名为“学生表”；近视学生的名单，表名为“近视学生表”。请问不是近视眼的学生都有谁？
（“学生表”表中的学号与“近视学生”表中的学生学号一一对应）

![12.png](https://pic.leetcode-cn.com/a542821a1ee2cf586eb0f25c2bf69e2809479b4bdbd79e10e1e1624927a4c817-12.png)

【解题思路】

1.我们先来拆解问题：不是近视眼的学生都有谁？

1）“不是近视眼”的学生，近视信息在“近视学生”表里
2） “学生都有谁？”，要求的是“学生姓名”，所以我们的输出答案应该是“学生姓名”，这在“学生”表里。

涉及2张以上表的查询时，我们需要用到多表联结。

2.使用哪种联结呢？
在《从零学会SQL：多表查询》这个课里我讲过各个联结的情况：

![6.jpg](https://pic.leetcode-cn.com/2f5ff13e22b0494e19d327562d970016bbaa88569c0590fa86f9c7dde947bc71-6.jpg)

其中上图黑色框里的sql解决的问题是：不在表里的数据，也就是在表A里的数据，但是不在表B里的数据。

对于这个题目“不是近视眼的学生都有谁？”，就是在“学生表”里的数据，但是不在“近视学生”表里的数据。我们选择下图黑色框里的左联结sql语句。

![7.jpg](https://pic.leetcode-cn.com/1da2b9501f3dbe050ea0b238b89ef5e0ae959041ee254545fae9d966967a5c39-7.jpg)

```mysql
select ...
from 表1 as a
left join 表2 as b
on a.列名=b.列名
where b.列名 is null;
```


3.多表如何联结？
题目已给出，联结两表的关键依据分别为“学号”和“学生学号”。示意图如下：

![13.png](https://pic.leetcode-cn.com/7279386e00e90bd47ddb19326b32c29d7a4ef6df6bffdde8f7d092d53ff047d2-13.png)

【解题步骤】

使用分析思路里的sql语句联结两表

```mysql
select a.姓名 as 不近视的学生名单
from 学生表 as a
left join 近视学生表 as b
on a.学号=b.学生学号
where b.序号 is null;
```

我们来理解下这个sql的运行过程，方便你更深入的理解。

1）在不加where字句的情况下，两表联结得到下图的表

![14.png](https://pic.leetcode-cn.com/162da5c6913ddfa772725cc9324f3aa98e9c27755678990b46ab6d7344b6718c-14.png)

2）假设where字句（where b.序号 is null;）就会把b.序号这一列里为空值（NULL）的行选出来，就是题目要求的不近视的学生。（下图绿色框里的行）

![9.jpg](https://pic.leetcode-cn.com/0221bdc75711a900cfa18d263a070be0c1ad836ce1ccc45f3bde40096cb1f256-9.jpg)
