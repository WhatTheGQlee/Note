# 存储引擎

## MyISAM 和 InnoDB 的区别

```
1.是否支持行级锁
MyISAM 只有表级锁(table-level locking)，而 InnoDB 支持行级锁(row-level locking)和表级锁,默认为行级锁。

2.是否支持事务
MyISAM 不提供事务支持，而InnoDB提供事务支持，实现了四个隔离级别，具有提交和回滚事务的能力。并且，InnoDB 默认使用的 （可重读）隔离级别是可以解决幻读问题发生的（基于 MVCC 和 Next-Key Lock）。可以扯到spring事务

3.是否支持外键  MyISAM 不支持，而 InnoDB 支持。

4.是否支持数据库异常崩溃后的安全恢复
MyISAM 不支持，而 InnoDB 支持。使用 InnoDB 的数据库在异常崩溃后，数据库重新启动的时候会保证数据库恢复到崩溃前的状态。这个恢复的过程依赖于 redo log 。

5.索引实现不一样。
虽然 MyISAM 引擎和 InnoDB 引擎都是使用 B+Tree 作为索引结构，但是两者的实现方式不太一样。
如果使用的是 MyISAM 存储引擎，B+ 树索引的叶子节点保存数据的物理地址，即用户数据的指针。
如果使用的是 InnoDB 存储引擎， B+ 树索引的叶子节点保存完整的数据本身。
```



## MySQL 基础架构

```
连接器： 身份认证和权限相关(登录 MySQL 的时候)。
查询缓存： 执行查询语句的时候，会先查询缓存（MySQL 8.0 版本后移除，因为这个功能不太实用）。
分析器： 没有命中缓存的话，SQL 语句就会经过分析器，分析器说白了就是要先看你的 SQL 语句要干嘛，再检查你的 SQL 语句语法是否正确。
优化器： 按照 MySQL 认为最优的方案去执行。
执行器： 执行语句，然后从存储引擎返回数据。 执行语句之前会先判断是否有权限，如果没有权限的话，就会报错。
插件式存储引擎 ： 主要负责数据的存储和读取，提供读写接口。采用的是插件式架构，支持 InnoDB、MyISAM、Memory 等多种存储引擎。
```

<img src="https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/javaguide/13526879-3037b144ed09eb88.png" alt="img" style="zoom: 80%;" />

## InnoDB 是如何存储数据的

**数据库表的空间文件的结构**

**表空间由段（segment）、区（extent）、页（page）、行（row）组成**，InnoDB存储引擎的逻辑存储结构大致如下图：

<img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/row_format/%E8%A1%A8%E7%A9%BA%E9%97%B4%E7%BB%93%E6%9E%84.drawio.png" alt="img" style="zoom: 80%;" />

**InnoDB 行的格式**

行格式，就是一条记录的存储结构。InnoDB 提供了 4 种行格式，分别是 Redundant、Compact、Dynamic和 Compressed 行格式。COMPACT 行格式：

![img](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/row_format/COMPACT.drawio.png)

```
「变长字段长度列表」只出现在数据表有变长字段的时候。

表中的某些列可能会存储 NULL 值，如果把这些 NULL 值都放到记录的真实数据中会比较浪费空间，所以 Compact 行格式把这些值为 NULL 的列存储到 NULL值列表中。

每个数据库表的行格式都有「NULL 值列表」吗？
不一定，当数据表的字段都定义成 NOT NULL 的时候，这时候表里的行格式就不会有 NULL 值列表了。

row_id:如果我们建表的时候指定了主键或者唯一约束列，那么就没有 row_id 隐藏字段了。如果既没有指定主键，又没有唯一约束，那么 InnoDB 就会为记录添加 row_id 隐藏字段。row_id不是必需的，占用 6 个字节。

trx_id:事务id，表示这个数据是由哪个事务生成的。 trx_id是必需的，占用 6 个字节。

roll_pointer:这条记录上一个版本的指针。roll_pointer 是必需的，占用 7 个字节。

varchar(n) 中 n 最大取值为多少？
varchar(n) 字段类型的 n 代表的是最多存储的字符数量，并不是字节大小哦。如果数据库用的是ascii，1个字符占用1字节，而UTF-8字符集下，一个字符最多需要三个字节。
MySQL 规定除了 TEXT、BLOBs 这种大对象类型之外，其他所有的列（不包括隐藏列和记录头信息）占用的字节长度加起来不能超过 65535 个字节。因此，在数据库表只有一个 varchar(n) 字段且字符集是 ascii 的情况下，varchar(n) 中 n 最大值 = 65535 - 2 - 1 = 65532。 1个变长字段长度占用2字节，null值列表最低1字节（字段允许为null，才会有null值列表）。

行溢出后，MySQL 是怎么处理的？
InnoDB 的数据是按「数据页」为单位来读写的，数据交换的基本单位是页，一个页大小一般是16kb（16384字节），而一个 varchar(n) 类型的列最多可以存储 65532字节。如果发生行溢出，多出的数据就会存到另外的「溢出页」中。如下所示：
```

![img](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/row_format/%E8%A1%8C%E6%BA%A2%E5%87%BA.png)

```
InnoDB 的数据是按「数据页」为单位来读写的，也就是说，当需要读一条记录的时候，并不是将这个记录本身从磁盘读出来，而是以页为单位，将其整体读入内存。数据库的 I/O 操作的最小单位是页，InnoDB 数据页的默认大小是 16KB。
数据页包括七个部分，结构如下图：
```

<img src="https://mmbiz.qpic.cn/mmbiz_png/J0g14CUwaZdgV8QoMTlAsFLK9yrPWQiapuicajhnJ54u7PN9OibJUHzcpdt98priaBMGgmCzEFvic6neD7bQl1bAZjQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1" alt="图片" style="zoom:50%;" />

<img src="https://mmbiz.qpic.cn/mmbiz_png/J0g14CUwaZdgV8QoMTlAsFLK9yrPWQiapMdhMrCXVW0Dlhyib6Uicmd0agZ2M0fkrggx2cvYS7fjic9yjkS0qpkcjA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1" alt="图片" style="zoom:80%;" />

Mysql的io次数 等于b+树的层高

![在这里插入图片描述](https://img-blog.csdnimg.cn/79fe6379ea8b435f918f38326cbd90b8.png#pic_center)

# 执行一条select语句，期间发生了什么

![查询语句执行流程](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/sql%E6%89%A7%E8%A1%8C%E8%BF%87%E7%A8%8B/mysql%E6%9F%A5%E8%AF%A2%E6%B5%81%E7%A8%8B.png)

1.连接器

2.查询缓存

MySQL 服务收到 SQL 语句后，就会解析出 SQL 语句的第一个字段，看看是什么类型的语句。如果 SQL 是查询语句（select 语句），MySQL 就会先去查询缓存（ Query Cache ）里查找缓存数据

3.解析SQL

进行词法分析，语法分析。判断你输入的这个 SQL 语句是否满足 MySQL 语法，没有问题就构建语法树。

4.执行 SQL
4.1预处理器 -- 检查 SQL 查询语句中的表或者字段是否存在；将 `select *` 中的 `*` 符号，扩展为表上的所有列；
4.2优化器---**优化器主要负责将 SQL 查询语句的执行方案确定下来**，确定执行哪个索引
4.3执行器

# 索引篇



索引分类

- 按「数据结构」分类：**B+tree索引、Hash索引、Full-text索引**。
- 按「物理存储」分类：**聚簇索引（主键索引）、非聚簇索引（需要回表）**。
- 按「字段特性」分类：**主键索引、唯一索引、普通索引、前缀索引**。
- 按「字段个数」分类：**单列索引、联合索引**。

在创建表时，InnoDB 存储引擎会根据不同的场景选择不同的列作为索引：

- 如果有主键，默认会使用主键作为聚簇索引的索引键（key）；
- 如果没有主键，就选择第一个不包含 NULL 值的唯一列作为聚簇索引的索引键（key）；
- 在上面两个都没有的情况下，InnoDB 将自动生成一个隐式自增 id 列作为聚簇索引的索引键（key）；
- **创建的主键索引和二级索引默认使用的是 B+Tree 索引**。

B+Tree 是myq一种多叉树，叶子节点才存放数据，非叶子节点只存放索引，而且每个节点里的数据是按主键顺序存放的，每一层父节点的索引值都会出现在下层子节点的索引值中。因此叶子节点包括了所有索引值信息，并且每个叶子节点都有两个指针，指向上一个叶子节点和下一个叶子节点，形成双向链表。**查询效率很高**，即使数据量很大，查询一个数据的磁盘I/O维持在3-4次。

**主键索引**：B+Tree 的叶子节点存放的是实际数据，所有完整的用户记录都存放在主键索引的 B+Tree 的叶子节点里；
主键索引的 B+Tree 如图所示：
![主键索引 B+Tree](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/%E7%B4%A2%E5%BC%95/btree.drawio.png)

**二级索引的B+ Tree 的叶子节点存放的是主键值，而不是实际数据。**如下图所示：

![二级索引 B+Tree](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/%E7%B4%A2%E5%BC%95/%E4%BA%8C%E7%BA%A7%E7%B4%A2%E5%BC%95btree.drawio.png)



**唯一索引：**建立在 UNIQUE 字段上的索引，一张表可以有多个唯一索引，索引列的值必须唯一，但是允许有空值。
**普通索引**：建立在普通字段上的索引，既不要求字段为主键，也不要求字段为 UNIQUE。
**前缀索引：**指对字符类型字段的前几个字符建立的索引，而不是在整个字段上建立的索引，前缀索引可以建立在字段类型为 char、 varchar、binary、varbinary 的列上。为了减少索引占用的存储空间。

**单列索引：**建立在单列上的索引称为单列索引，比如主键索引；
**联合索引：**建立在多列上的索引称为联合索引，存在**最左匹配原则**，也就是按照最左优先的方式进行索引的匹配。如下列为先按照编号进行索引匹配，编号相同的情况下再按照名称排序。
**联合索引的最左匹配原则，在遇到范围查询（>、<）的时候，就会停止匹配，也就是范围列可以用到联合索引，但是范围列后面的列无法用到联合索引**。**注意，对于 >=、<=、BETWEEN、like 前缀匹配的范围查询，并不会停止匹配**

```
select * from t_table where a >= 1 and b = 2，联合索引（a, b）哪一个字段用到了联合索引的 B+Tree？
由于联合索引（二级索引）是先按照 a 字段的值排序的，所以符合 >= 1 条件的二级索引记录肯定是相邻，于是在进行索引扫描的时候，可以定位到符合 >= 1 条件的第一条记录，然后沿着记录所在的链表向后扫描，直到某条记录不符合 a>= 1 条件位置。所以 a 字段可以在联合索引的 B+Tree 中进行索引查询。
虽然在符合 a>= 1 条件的二级索引记录的范围里，b 字段的值是「无序」的，但是对于符合 a = 1 的二级索引记录的范围里，b 字段的值是「有序」的（因为对于联合索引，是先按照 a 字段的值排序，然后在 a 字段的值相同的情况下，再按照 b 字段的值进行排序）
于是，在确定需要扫描的二级索引的范围时，当二级索引记录的 a 字段值为 1 时，可以通过 b = 2 条件减少需要扫描的二级索引记录范围

你对(a,b,c,d)建立索引,where后条件为
a = 1 and b = 2 and c > 3 and d = 4
那么，a,b,c三个字段能用到索引，而d就匹配不到。因为遇到了范围查询

SELECT * FROM `table` WHERE a = 1 ORDER BY b;
如何建立索引？ 一看就是对(a,b)建索引，当a = 1的时候，b相对有序，可以避免再次排序！ 

SELECT * FROM `table` WHERE a > 1 ORDER BY b;
如何建立索引？ 对(a)建立索引，因为a的值是一个范围，这个范围内b值是无序的，没有必要对(a,b)建立索引。
```

![联合索引](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/%E7%B4%A2%E5%BC%95/%E8%81%94%E5%90%88%E7%B4%A2%E5%BC%95.drawio.png)



**回表**：检索两次才能获得结果。先检二级索引的索引值，找到叶子节点，然后获取主键值，再检主键索引B+tree,查到叶子节点，才获取到数据。如：select * from product where product_no = '0002';

**覆盖索引**：在二级索引的B+ Tree的叶子节点里就能查询到数据。select id from product where product_no = '0002';

## MySQL InnoDB 选择 B+tree 作为索引的原因

```
1.B+ 树可以比 B 树更「矮胖」。B+ 树的非叶子节点不存放实际的记录数据，仅存放索引，因此数据量相同的情况下，相比存储即存索引又存记录的 B 树，B+树的非叶子节点可以存放更多的索引，因此 B+ 树可以比 B 树更「矮胖」，树的高度决定于磁盘 I/O 操作的次数，查询底层节点的磁盘 I/O次数会更少。

2.B+ 树有大量的冗余节点（所有非叶子节点都是冗余索引），这些冗余索引让 B+ 树在插入、删除的效率都更高，比如删除根节点的时候，不会像 B 树那样会发生复杂的树的变化；

3.B+ 树所有叶子节点之间用链表连接了起来，有利于范围查询，而 B 树要实现范围查询，因此只能通过树的遍历来完成范围查询，这会涉及多个节点的磁盘 I/O 操作，范围查询效率不如 B+ 树。
```



## 优化索引的方法

```
1.前缀索引优化
好处：使用前缀索引是为了减小索引字段大小，可以增加一个索引页中存储的索引值，有效提高索引的查询速度。在一些大字符串的字段作为索引时，使用前缀索引可以帮助我们减小索引项的大小。
缺点：order by 就无法使用前缀索引；无法把前缀索引用作覆盖索引；

2.覆盖索引优化
覆盖索引是指 SQL 中 query 的所有字段，在索引 B+Tree 的叶子节点上都能找得到的那些索引，从二级索引中查询得到记录，而不需要通过聚簇索引查询获得，可以避免回表的操作。如查询商品的名称、价格，可以建立联合索引（商品ID、名称、价格），如果索引中存在这些数据，查询将不会再次检索主键索引，从而避免回表。

3.主键索引最好是自增的
如果我们使用自增主键，那么每次插入的新数据就会按顺序添加到当前索引节点的位置，不需要移动已有的数据，当页面写满，就自动开辟一个新页面。因为每次插入一条新记录，都是追加操作，不需要重新移动数据，因此这种插入数据的方法效率非常高
如果我们使用非自增主键，由于每次插入主键的索引值都是随机的，因此每次插入新的数据时，就可能会插入到现有数据页中间的某个位置，这将不得不移动其它数据来满足新数据的插入，甚至需要从一个页面复制数据到另外一个页面，我们通常将这种情况称为页分裂。页分裂还有可能会造成大量的内存碎片，导致索引结构不紧凑，从而影响查询效率。

4.索引最好设置为 NOT NULL
第一原因：索引列存在 NULL 就会导致优化器在做索引选择的时候更加复杂，更加难以优化，因为可为 NULL 的列会使索引、索引统计和值比较都更复杂，比如进行索引统计时，count 会省略值为NULL 的行。
第二个原因：NULL 值是一个没意义的值，但是它会占用物理空间。因为 InnoDB 存储记录的时候，如果表中存在允许为 NULL 的字段，那么行格式中至少会用 1 字节空间存储 NULL 值列表。

5防止索引失效
发生索引失效的情况：
a.当我们使用左或者左右模糊匹配的时候，也就是 like %xx 或者 like %xx%这两种方式都会造成索引失效；
b.当我们在查询条件中对索引列做了计算、函数、类型转换操作，这些情况下都会造成索引失效；
c.联合索引要能正确使用需要遵循最左匹配原则，也就是按照最左优先的方式进行索引的匹配，否则就会导致索引失效。
d.在 WHERE 子句中，如果在 OR 前的条件列是索引列，而在 OR 后的条件列不是索引列，那么索引会失效。
```

<img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/%E7%B4%A2%E5%BC%95/%E7%B4%A2%E5%BC%95%E6%80%BB%E7%BB%93.drawio.png" alt="img"  />





## B+树是如何进行查询的

InnoDB 里的 B+ 树中的**每个节点都是一个数据页**，结构示意图如下：

![图片](https://img-blog.csdnimg.cn/img_convert/7c635d682bd3cdc421bb9eea33a5a413.png)

通过上图，我们看出 B+ 树的特点：

- 只有叶子节点（最底层的节点）才存放了数据，非叶子节点（其他上层节）仅用来存放页目录作为索引。页目录是由多个槽组成，槽相当于分组记录的索引。因为数据页中的数据是按照主键值从小到大排序，所有我们通过槽查找记录时，可以使用二分法快速定位数据在那个槽里，再遍历槽内的记录。
- 非叶子节点分为不同层次，通过分层来降低每一层的搜索量；
- 所有节点按照索引键大小排序，构成一个双向链表，便于范围查询；

我们再看看 B+ 树如何实现快速查找主键为 6 的记录，以上图为例子：

- 从根节点开始，通过二分法快速定位到符合页内范围包含查询值的页，因为查询的主键值为 6，在[1, 7)范围之间，所以到页 30 中查找更详细的目录项；

- 在非叶子节点（页30）中，继续通过二分法快速定位到符合页内范围包含查询值的页，主键值大于 5，所以就到叶子节点（页16）查找记录；
- 接着，在叶子节点（页16）中，通过槽查找记录时，使用二分法快速定位要查询的记录在哪个槽（哪个记录分组），定位到槽后，再遍历槽内的所有记录，找到主键为 6 的记录。

可以看到，在定位记录所在哪一个页时，也是通过二分法快速定位到包含该记录的页。定位到该页后，又会在该页内进行二分法快速定位记录所在的分组（槽号），最后在分组内进行遍历查找。



## 索引失效

```
1.对索引使用左或者左右模糊匹配，也就是 like %xx 或者 like %xx% 这两种方式都会造成索引失效。
为什么会索引失效？因为索引B+树是按照索引值有序排列存储的，只能根据前缀进行比较。如果第一个是%模糊匹配，那就不知道从哪个索引值开始比较，只能通过全表扫描的方式来检索。

2.对索引使用函数，因为索引保存的是索引字段的原始值，而不是经过函数计算后的值。
	select * from t_user where length(name)=6;
3.对索引进行表达式计算，因为索引保存的是索引字段的原始值，而不是表达式计算后的值。
   	select * from t_user where id + 1 = 10; 索引失效
   	select * from t_user where id  = 10-1； 索引存在
4.对索引隐式类型转换，如：索引字段是字符串类型，但是条件查询输入的参数是整型，索引失效。
MySql的数据类型转换：MySQL 在遇到字符串和数字比较的时候，会自动把字符串转为数字，然后再进行比较。

5.联合索引非最左匹配。
原因是，在联合索引的情况下，数据是按照索引第一列排序，第一列数据相同时才会按照第二列排序。

6.在 WHERE 子句中，如果在 OR 前的条件列是索引列，而在 OR 后的条件列不是索引列，那么索引会失效。
原因：OR的含义是只要一个能满足条件即可，因此只有一个是索引列是没有意义。只要有条件列不是索引列，就会进行全表扫描。
```

## 索引下推

```
MySQL服务层负责SQL语法解析、生成执行计划等，并调用存储引擎层去执行数据的存储和检索。
索引下推的下推其实就是指将部分上层（服务层）负责的事情，交给了下层（引擎层）去处理。由于在引擎层进行了条件判断，过滤出符合条件的数据才返回给Server层。因此减少了回表次数。
```

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/52288fa925f84cef937bb0b46d27c60a~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="MySQL大概架构" style="zoom: 67%;" />

我们来具体看一下，在没有使用索引下推的情况下，MySQL的查询：

- 存储引擎读取索引记录；
- 根据索引中的主键值，定位并读取完整的行记录；
- 存储引擎把记录交给`Server`层去检测该记录是否满足`WHERE`条件。

使用ICP的情况下，查询过程：

- 存储引擎读取索引记录（不是完整的行记录）；
- 如果索引条件满足，会使用索引中的主键去回表读取完整的行记录
- 存储引擎层把记录交给服务层，服务层检查该记录是否满足条件的其他部分。

例如：select * from tuser where name like '张%' and age=10;

![img](https://pics0.baidu.com/feed/b21c8701a18b87d63beae2f7080746311e30fd42.jpeg@f_auto?token=7dd4ac72ff0ce5937ce6ce64350cd44b)

没有使用索引下推
存储引擎根据通过联合索引找到`name like '张%'` 的主键id（1、4），逐一进行回表扫描，去聚簇索引找到完整的行记录，server层再对数据根据`age=10进行筛选`。

![img](https://pics5.baidu.com/feed/b151f8198618367a7d07a2dd3e7ce5ddb31ce539.jpeg@f_auto?token=28b954d84c29cab120da924853b4af64)

使用了索引下推
存储引擎根据（name，age）联合索引，找到，由于联合索引中包含列，所以存储引擎直接再联合索引里按照`age=10`过滤。按照过滤后的数据再一一进行回表扫描。

![img](https://pics5.baidu.com/feed/472309f79052982248438993c6c515c20a46d402.jpeg@f_auto?token=2b6e49951f7cc5e810dc350d609ec496)





## count(*) 和 count(1) 的 区别

<img src="https://img-blog.csdnimg.cn/img_convert/af711033aa3423330d3a4bc6baeb9532.png" alt="图片" style="zoom: 67%;" />

count()该函数作用时统计符合查询条件的记录中，指定的参数不为null的记录数。而count(1)代表是 统计表中1这个表达式不为null的记录有多少，而1永远不等于null，所有实际在统计表中的所有记录数。
原因：count 函数的参数是 1，不是字段，所以不需要读取记录中的字段值。比count(主键字段)少一个读取表中的字段值的并比较是否为null的操作。count(*) ，MySQL会将参数 \* 转化为参数0，count(\*)和count(1)的执行过程一样。

 

## 如何优化count(*)

```
1.近似值，执行explain 进行估算
2.额外表保存计数值
```



# 事务篇

## 事务的特性- ACID

```
1.原子性(Atomicity)：一个事务中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节，而且事务在执行过程中发生错误，会被回滚到事务开始前的状态。
2.一致性(Consistency)：执行事务前后，数据保持一致，例如转账业务中，无论事务是否成功，转账者和收款人的总额应该是不变的。(目的)
3.隔离性(Isolation)： 并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据操作是独立的；
4.持久性(Durability)：一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不有任何影响。

原子性是通过 undo log（回滚日志） 来保证的
隔离性是通过 MVCC（多版本并发控制） 或锁机制来保证的；
持久性是通过 redo log （重做日志）来保证的；
一致性则是通过持久性+原子性+隔离性来保证；
```



## 并发事务的问题

```
1.脏读：如果一个事务「读到」了另一个「未提交事务修改过的数据」，就意味着发生了「脏读」现象。
2.不可重复读：在一个事务内多次读取同一个数据，出现前后两次读到的数据不一样的情况。重点在内容修改
3.幻读：在一个事务内多次查询某个符合查询条件的「记录数量」，出现前后两次查询到的记录数量不一样。重点在记录数量
```



## 事务的隔离级别

![image-20220827225933650](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220827225933650.png)

![图片](https://img-blog.csdnimg.cn/img_convert/4e98ea2e60923b969790898565b4d643.png)

**MYSQL可重复读隔离级别，完全解决了幻读问题吗？**

```Java
没有完全解决幻读问题，只是避免！！！
针对快照读（普通的select语句），是通过MVCC方式解决幻读。因为可重复读隔离级别下，事务启动时就会生成一个快照（read view)，然后整个事务期间都在用这个快照，即使中途有其他事务插入了新的数据，是不影响当前这个快照。

针对当前读（如select for update），是通过next-key lock(记录锁+间隙锁)方式解决了幻读。当前读意味着要查询最新版本的数据，所以当执行select for update语句时候，会加上next-key lock，此时如果有其他事务想要在锁范围内插入记录，会被阻塞。
```

![image-20220828164111595](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220828164111595.png)



## MVCC

MVCC 是指**多版本并发控制**,用于实现**事务的并发控制**。是**指维护一条记录的多个版本，使得读写操作没有冲突**。

InnoDB 对于 MVCC 的实现，我主要从下面几个点来讲：

1. 当前读和快照读的概念

   **当前读**：就是读取的记录总是最新的，实现的原理就是对正在读的记录加锁，使得读写互斥，这样保证每次读取的都是数据库中最新的记录

   **快照读**：每次读取的时候不一定是最新的数据，而是这条记录的快照版本，这样可以保证读写不互斥，能够并发执行

2. MySQL 数据的隐藏字段

   MySQL 中每条记录是有两个隐藏的字段，分别是：

   - **最近修改事务 ID**：记录着上一次修改该条记录的日志 id

   - **回滚指针**：指向上一个版本的记录的地址

     

3. undo 日志中的版本链

   当事务对数据的修改时，都会记录下修改这条记录的的事务id让回滚指针指向上一个版本，从而形成版本链。

4. readView（读视图）、快照

   快照记录了当前系统活跃的事务ID，为快照读时MVCC提供数据的依据。其主要有以下几个属性：

   1.记录当前活跃事务的ID，

   2.最小活跃事务ID，

   3.预分配事务ID，就是最大活跃事务ID+1

   4.创建快照的事务ID

   

5. MVCC实现流程

1.如果数据表中的一条记录的trx_id值小于Read View中的最小的事务ID，表示这个版本的记录是在创建Read View前已提交的事务生成的，所以该版本的记录对当前事务可见。
2.如果记录的trx_id值大于等于Read View中的预分配事务ID，表示这个版本的记录是在创建Read View后才启动的事务生成的，所以该版本的记录对当前事务不可见。
3.如果记录的trx_id在 最小事务ID 和 最大事务ID 之间，需要判断trx_id是否存在该快照的活跃事务ID列表中：
	a.trx_id存在活跃事务ID列表中，表示生成该版本记录的活跃事务依然活跃着（还没提交事务），所以该版本的记录对当前事务不可见。
	b.trx_id不存在活跃事务ID列表中，表示生成该版本记录的活跃事务已经被提交，所以该版本的记录对当前事务可见。



## Read View 在 MVCC 里如何工作的？

这种通过「版本链」来控制并发事务访问同一个记录时的行为就叫 MVCC（多版本并发控制）

读视图的格式：

![img](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost4@main/mysql/%E4%BA%8B%E5%8A%A1%E9%9A%94%E7%A6%BB/readview%E7%BB%93%E6%9E%84.drawio.png)


![image-20220828170456940](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220828170456940.png)

主键索引记录中的两个隐藏列的作用：

<img src="https://img-blog.csdnimg.cn/img_convert/f595d13450878acd04affa82731f76c5.png" alt="图片" style="zoom:67%;" />

![image-20220828170124238](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220828170124238.png)

在创建 Read View 后，我们可以将记录中的 trx_id 划分这三种情况：

<img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost4@main/mysql/%E4%BA%8B%E5%8A%A1%E9%9A%94%E7%A6%BB/ReadView.drawio.png" alt="img" style="zoom:67%;" />

```
1.如果记录的trx_id值小于Read View中的最小的事务ID，表示这个版本的记录是在创建Read View前已提交的事务生成的，所以该版本的记录对当前事务可见。
2.如果记录的trx_id值大于等于Read View中的预分配事务ID，表示这个版本的记录是在创建Read View后才启动的事务生成的，所以该版本的记录对当前事务不可见。
3.如果记录的trx_id在 最小事务ID 和 最大事务ID 之间，需要判断trx_id是否存在活跃事务ID列表中：
	a.trx_id存在活跃事务ID列表中，表示生成该版本记录的活跃事务依然活跃着（还没提交事务），所以该版本的记录对当前事务不可见。
	b.trx_id不存在活跃事务ID列表中，表示生成该版本记录的活跃事务已经被提交，所以该版本的记录对当前事务可见。
```



## 可重复读是如何工作的？

**可重复读隔离级别是启动事务时生成一个 Read View，然后整个事务期间都在用这个 Read View**，

快照读：事务B只会读取版本比creator_trx_id 值小的值，来达到可重复读。同时为了解决幻读问题，innodb采用mvcc的方式解决了快照读的幻读问题，
当前读：此外，**Innodb 引擎为了解决「可重复读」隔离级别使用「当前读」而造成的幻读问题，就引出了 next-key 锁**，就是记录锁和间隙锁的组合，防止其他事务在这个记录之间插入新的记录。

![img](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/%E9%94%81/gap%E9%94%81.drawio.png)

## 读已提交是如何工作的？

**读已提交隔离级别是在每次读取数据时，都会生成一个新的 Read View**。所以在同一个事务期间，多次读取数据可能会出现不一致。如事务A (id=10)，事务A启动之后，事务B(id=11)也启动，读取余额值为0。事务A把余额从0改成了100，并提交了。那么事务B在查询余额的时候，发现修改的事务版本为10，比自己的快照min_trx_id=52低 。认为已提交了，可以读取



# 锁篇

<img src="https://img-blog.csdnimg.cn/1e37f6994ef44714aba03b8046b1ace2.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0ODI3Njc0,size_16,color_FFFFFF,t_70" alt="img" style="zoom: 80%;" />

## MySQL锁的种类

### 全局锁

锁住整个数据库，整个数据库处于只读状态。应用场景：全库逻辑备份。可以使用**可重复读的隔离级别**来避免全局锁，做法：在备份数据库之前先开启事务，会先创建 Read View，然后整个事务执行期间都在用这个 Read View，而且由于 MVCC 的支持，备份期间业务依然可以对数据进行更新操作。因为事务的四大特性中的隔离性。

###  表级锁

```
1.表锁
表锁除了会限制别的线程的读写外，也会限制本线程接下来的读写操作。如果本线程对表加了读锁，也会阻塞本线程对表的写操作

2.元数据锁（MDL）
我们不需要显示的使用 MDL，因为当我们对数据库表进行操作时，会自动给这个表加上 MDL，直到事务提交后才释放：
MDL是为了保证当用户对表执行CRUD操作时，防止其他线程读i这个表结构做变更。
对一张表进行 CRUD 操作时，加的是 MDL 读锁； 对一张表做结构变更操作的时候，加的是 MDL 写锁；

3.意向锁
当执行插入、更新、删除操作，需要先对表加上「意向独占锁」，然后对该记录加独占锁。
意向共享锁和意向独占锁是表级锁，不会和行级的共享锁和独占锁发生冲突，而且意向锁之间也不会发生冲突，只会和共享表锁（read）和独占表锁（write）发生冲突。
作用：如果没有表级别的意向锁，那么加行级别的独占锁（写锁）时，需要遍历表里所有的记录查看是否存在行锁。引入表级别的意向锁，可以通过查看表是否有意向锁而快速判断表中是否有记录被加锁。

4.AUTO-INC 锁
AUTO-INC 锁是特殊的表锁机制，锁不是在一个事务提交后才释放，而是执行完插入语句后就会立即释放。这个锁主要用在主键自增。大数据插入影响性能。
在MySQL5.1.22版本开始，InnoDB存储引擎提供了一种轻量级的锁来实现自增。原理：给被AUTO_INCREMENT 修饰的字段加上轻量级锁，然后给该字段赋值一个自增的值，就把这个轻量级锁释放了，而不需要等待整个插入语句执行完后才释放锁。
```

### 行级锁

```
InnoDB 引擎是支持行级锁的，而 MyISAM 引擎并不支持行级锁！！！
行级锁的种类：
1.Record Lock，记录锁，也就是仅仅把一条记录锁上；
2.Gap Lock，间隙锁，锁定一个范围，但是不包含记录本身；间隙锁之间是兼容的，不是互斥！！。  左开右开区间，解决幻读
3.Next-Key Lock：Record Lock + Gap Lock （临键锁） 的组合，锁定一个范围，并且锁定记录本身。左开右闭区间。
注意：在能使用记录锁或者间隙锁就能避免幻读的情况下，next-key lock就会退化成记录锁或者间隙锁。

4.插入意向锁：一个事务在插入一条记录的时候，插入位置被加了间隙锁，则该事务的插入操作发生阻塞，并生成一个插入意向锁，表明想插入新数据，但是被阻塞等待。是一种特殊的间隙锁，是互斥的。
```



## MySQL加锁规则

```
什么SQL语句会加行级锁？
普通的select语句是不会对记录加锁的，因为它属于快照读，是通过MVCC实现的。
update和delete操作都会加行级锁，且锁的类型都是独占锁，读写互斥，谢谢互斥。加锁的基本单位是next-key lock
注意：在能使用记录锁或者间隙锁就能避免幻读的情况下，next-key lock就会退化成记录锁或者间隙锁。

1.唯一索引等值查询 = > select * from user where id = 2;
当查询的记录是存在的，在用「唯一索引进行等值查询」时，next-key lock 会退化成「记录锁」。
为什么会退化？
原因就是在唯一索引等值查询并且查询记录存在的情况下，仅靠记录锁也能避免幻读。
由于主键具有唯一性，所有其他事务插入同样id，会因为主键冲突，导致无法加插入记录。
由于对id=1加了记录锁，其他事务也无法删除该记录。

当查询的记录是不存在的，在索引树找到第一条大于该查询记录的记录后，next-key lock 会退化成「间隙锁」。
例如：在索引值有1，2，5，查找id=3的，会退化成间隙锁（2，5）

2.唯一索引范围查询 ：
情况一：针对大于等于的范围查询，因为存在等值查询的条件，如果找到了就会退化成记录锁。
情况二：针对小于 或者 小于等于 的范围查询，要看条件值的记录是否存在表中：
	1.当条件值的记录不在表中，next-key lock 会退化成间隙锁。
	2.当条件值记录在表中，如果是小于条件范围查询，会退化成间隙锁。如果是小于等于，不会退化成间隙锁。

3.非唯一索引等值查询
当我们用非唯一索引进行等值查询的时候，因为存在两个索引，一个是主键索引，一个是非唯一索引（二级索引），所以在加锁时，同时会对这两个索引都加锁，但是对主键索引加锁的时候，只有满足查询条件的记录才会对它们的主键索引加锁。

当查询的记录「存在」时，由于不是唯一索引，所以肯定存在索引值相同的记录，于是非唯一索引等值查询的过程是一个扫描的过程，直到扫描到第一个不符合条件的二级索引记录就停止扫描，然后在扫描的过程中，对扫描到的二级索引记录加的是 next-key 锁，而对于第一个不符合条件的二级索引记录，该二级索引的 next-key 锁会退化成间隙锁。同时，在符合查询条件的记录的主键索引上加记录锁。

当查询的记录「不存在」时，扫描到第一条不符合条件的二级索引记录，该二级索引的 next-key 锁会退化成间隙锁。因为不存在满足查询条件的记录，所以不会对主键索引加锁。

4.非唯一索引范围查询
next-key lock 不会退化为间隙锁和记录锁。

5没有加索引的查询
没有加索引的查询时全表扫描，因此是每条记录都加上next-key 锁，相当于锁住全表。
```

## 死锁

### 为什么间隙锁与间隙锁之间是兼容的

**间隙锁的意义只在于阻止区间被插入**，因此是可以共存的。**一个事务获取的间隙锁不会阻止另一个事务获取同一个间隙范围的间隙锁**，共享和排他的间隙锁是没有区别的，他们相互不冲突，且功能相同，即两个事务可以同时持有包含共同间隙的间隙锁。



### 插入意向锁是什么

插入意向锁虽然名字有意向锁，但它是一种特殊的间隙锁。但不同于间隙锁的是，该锁只用于并发插入操作。插入意向锁与间隙锁的另一个非常重要的差别是：尽管「插入意向锁」也属于间隙锁，但两个事务却不能在同一时间内，一个拥有间隙锁，另一个拥有该间隙区间内的插入意向锁，**插入意向锁和间隙锁之间是冲突的**。

插入意向锁的生成时机：

- 每插入一条新记录，都需要看一下待插入记录的下一条记录上是否已经被加了间隙锁，如果已加间隙锁，此时会生成一个插入意向锁，然后锁的状态设置为等待状态



### Insert 语句是怎么加行级锁的

```
Insert 语句在正常执行时是不会生成锁结构的，它是靠聚簇索引记录自带的 trx_id 隐藏列来作为隐式锁来保护记录的。
隐式锁:当事务需要加锁时，如果这个锁不可能发生冲突，InnoDB会跳过加锁环节，这种机制称为隐式锁。隐式锁是 InnoDB 实现的一种延迟加锁机制，其特点是只有在可能发生冲突时才加锁，从而减少了锁的数量，提高了系统整体性能。冲突场景：
1.如果记录之间加有间隙锁，为了避免幻读，此时是不能插入记录的；
2.如果 Insert 的记录和已有记录存在唯一键冲突，此时也不能插入记录。
```

### 死锁发生

如果两个事务分别向对方持有的间隙锁范围内插入一条记录，而插入操作为了获取到插入意向锁，都在等待对方事务的间隙锁释放，于是就造成了循环等待，满足了死锁的四个条件：**互斥、占有且等待、不可强占用、循环等待**，因此发生了死锁。

在数据库层面，有两种策略通过「打破循环等待条件」来解除死锁状态：
1.设置事务等待锁的超时时间。innodb_lock_wait_timeout = 50。
2.开启主动死锁检测：innodb_deadlock_detect = on，检测到死锁后，会主动回滚死锁中的某一个事务。

**对应的死锁的sql语句**

![img](https://cdn.xiaolincoding.com//mysql/other/90c1e01d0345de639e3426cea0390e80.png)

```
select id from t_order where order_no = 1007 for update; 加的是记录锁+间隙锁，范围为 [1006, +∞]
select id from t_order where order_no = 1008 for update; 加的是间隙锁，范围为(1006, +∞]

由于间隙锁是不会发生互斥的，可以共存，一个事务获取的间隙锁不会阻止另一个事务获取同一个间隙范围的间隙锁。
当要插入的时候，发现另一个事务有间隙锁，所有插入阻塞。
```



# 日志篇

三种日志
![image-20220829164530429](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220829164530429.png)

## 回滚日志(undo log)

```
每当InnoDB引擎对一条记录进行操作（修改、删除、新增）时，要把回滚时需要的信息都记录到 undo log 里，比如：
在插入一条记录时，要把这条记录的主键值记下来，这样之后回滚时只需要把这个主键值对应的记录删掉就好了；

一条记录的每一次更新操作产生的 undo log 格式都有一个 roll_pointer 指针和一个 trx_id 事务id：
1.通过 trx_id 可以知道该记录是被哪个事务修改的；
2.通过 roll_pointer 指针可以将这些 undo log 串成一个链表，这个链表就被称为版本链；

因此，undo log 还有一个作用，通过 ReadView + undo log 实现 MVCC（多版本并发控制）。

undo log 两大作用：
1.实现事务回滚，保障事务的原子性。
2.通过ReadView + undo log实现MVCC。undo log 为每条记录保存历史记录，然后MYSQL在执行快照读，会根据快照的版本号沿着原纪录的版本链找到能见的记录。
```

<img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/how_update/%E7%89%88%E6%9C%AC%E9%93%BE.png?image_process=watermark,text_5YWs5LyX5Y-377ya5bCP5p6XY29kaW5n,type_ZnpsdHpoaw,x_10,y_10,g_se,size_20,color_0000CD,t_70,fill_0" alt="版本链" style="zoom:80%;" />

## Buffer Pool

```
innodb存储引擎设计了缓冲池，来提高数据库的读写性能
当读取数据时，如果数据存在于 Buffer Pool 中，客户端就会直接读取 Buffer Pool 中的数据，否则再去磁盘中读取。
当修改数据时，如果数据存在于 Buffer Pool 中，那直接修改 Buffer Pool 中数据所在的页，然后将其页设置为脏页（该页的内存数据和磁盘上的数据已经不一致），为了减少磁盘I/O，不会立即将脏页写入磁盘，后续由后台线程选择一个合适的时机将脏页写入到磁盘。因此，存在断电崩溃问题。
Buffer Pool 除了缓存「索引页」和「数据页」，还包括了 Undo 页，插入缓存、自适应哈希索引、锁信息。
undo页记录的是undo log。
```

<img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost4@main/mysql/innodb/bufferpool%E5%86%85%E5%AE%B9.drawio.png?image_process=watermark,text_5YWs5LyX5Y-377ya5bCP5p6XY29kaW5n,type_ZnpsdHpoaw,x_10,y_10,g_se,size_20,color_0000CD,t_70,fill_0" alt="img" style="zoom:80%;" />



## 重做日志(redo log)

```
redo log 是物理日志，记录了某个数据页做了什么修改。
这是因为Buffer Pool里面的数据不是实时写入数据库，因此有断电导致数据丢失的问题。
为了防止断电导致数据丢失的问题，当有一条记录需要更新的时候，InnoDB 引擎就会先把记录写到 redo log 里面，并更新内存，这个时候更新就算完成了。同时，InnoDB 引擎会在适当的时候，由后台线程将缓存在 Buffer Pool 的脏页刷新到磁盘里，这就是 WAL （Write-Ahead Logging）技术，指的是 MySQL 的写操作并不是立刻更新到磁盘上，而是先记录在日志上，然后在合适的时间再更新到磁盘上。
当系统崩溃时，虽然脏页数据没有持久化，但是redo log 已经持久化，就能恢复数据内容。
```

<img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/how_update/wal.png?image_process=watermark,text_5YWs5LyX5Y-377ya5bCP5p6XY29kaW5n,type_ZnpsdHpoaw,x_10,y_10,g_se,size_20,color_0000CD,t_70,fill_0" alt="img" style="zoom:80%;" />

### redo log 和 undo log 的联系与区别

```
undo log 记录了此次事务「开始前」的数据状态，记录的是更新之前的值；
redo log 记录了此次事务「完成后」的数据状态，记录的是更新之后的值；
流程：开启事务后，InnoDB层更新记录前，首先要记录相应的undo log, 然后把undo log插入到Buffer Pool中的undo页面，最后记录修改undo页面的redo log。
事务提交之前发生了崩溃，会通过 undo log 回滚事务；事务提交之后发生了崩溃，重启后会通过 redo log 恢复事务
```

<img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/how_update/%E4%BA%8B%E5%8A%A1%E6%81%A2%E5%A4%8D.png?image_process=watermark,text_5YWs5LyX5Y-377ya5bCP5p6XY29kaW5n,type_ZnpsdHpoaw,x_10,y_10,g_se,size_20,color_0000CD,t_70,fill_0" alt="事务恢复" style="zoom: 80%;" />



### redo log 要写到磁盘，数据也要写磁盘，为什么要多此一举

```
写入redo log的方式使用了追加操作， 所以磁盘操作是顺序写，而写入数据需要先找到写入位置，然后才写到磁盘，所以磁盘操作是随机写。顺序写效率>随机写效率。因此redo log写入磁盘开销很小。
```

### 产生的 redo log 是直接写入磁盘的吗

```
不是，redo log 也有自己的缓存—— redo log buffer，每当产生一条 redo log 时，会先写入到 redo log buffer，后续再持久化到磁盘。默认大小 16 MB。
```

<img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/how_update/redologbuf.webp" alt="事务恢复" style="zoom: 33%;" />

### 在redo log buffer的redo log什么时候写入磁盘

```
主要有下面几个时机：
1.MySQL正常关闭时；
2.当redo log buffer 中记录的写入量大于redo log buffer内存空间的一半时，会触发落盘；
3.InnoDB的后台线程每隔 1 秒，将redo log buffer 持久化到磁盘。
4.每次事务提交时都将缓存在redo log buffer里的redo log直接持久化到磁盘(这个策略由innodb flush log at trx commit参数控制)
	参数设置为0，表示每次事务提交时，不触发写入磁盘操作
	参数设置为1，表示每次事务提交时，都将缓存在 redo log buffer 里的 redo log 直接持久化到磁盘
	参数设置为2，表示每次事务提交时，都只是缓存在 redo log buffer 里的 redo log 写到 redo log 文件。注意写	  入到「 redo log 文件」并不意味着写入到了磁盘。而是意味着写到文件缓存。
数据安全性：参数 1 > 参数 2 > 参数 0
写入性能：参数 0 > 参数 2 > 参数 1
```

![image-20220829202325998](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220829202325998.png)

<img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/how_update/innodb_flush_log_at_trx_commit2.drawio.png?image_process=watermark,text_5YWs5LyX5Y-377ya5bCP5p6XY29kaW5n,type_ZnpsdHpoaw,x_10,y_10,g_se,size_20,color_0000CD,t_70,fill_0" alt="img" style="zoom:80%;" />



### redo log 文件写满了怎么办

```
InnoDB存储引擎有1个重做日志文件组( redo log Group），「重做日志文件组」由有2个redo log文件组成，这两个redo日志的文件名叫 ：ib_logfile0 和 ib_logfile1。重做日志文件组是以循环写的方式工作的。先写file0,写满就切换file1。

redo log 是循环写的方式，相当于一个环形，InnoDB 用 write pos 表示 redo log 当前记录写到的位置，用 checkpoint 表示当前要擦除的位置。如下图：
write pos 和 checkpoint 的移动都是顺时针方向；
write pos ～ checkpoint 之间的部分（图中的红色部分），用来记录新的更新操作；
check point ～ write pos 之间的部分（图中蓝色部分）：待落盘的脏数据页记录；

当redo log 文件满了。此时MySQL不再执行新的更新操作。而是停下来将Buffer Pool中的脏页刷新到磁盘中，然后标记redo log哪些记录可以被擦除，并擦除记录，腾出空间，然后MySQL恢复正常运行。因为redo log文件的目的是存储数据页更改了，但是没有刷新到磁盘的记录，当我们把脏页数据刷新到磁盘，意味着这些redo log 记录就没有作用了，可以覆盖掉。
```

<img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/how_update/checkpoint.png" alt="img" style="zoom: 67%;" />





## 归档日志(binlog)

### redo log 和 binlog 有什么区别

```
binlog 文件是记录了所有数据库表结构变更和表数据修改的日志，不会记录查询类的操作
1、适用对象不同：
binlog是MySQL的Server层实现的日志，所有存储引擎都可以使用；
redo log 是Innodb存储引擎实现的日志；

2.文件格式不同：
binlog 有 3 种格式类型，分别是 STATEMENT（默认格式）、ROW、 MIXED，区别如下：
        STATEMENT：每一条修改数据的 SQL 都会被记录到 binlog 中,合适主从复制中 slave 端再根据 SQL 语句重现。
        ROW：记录行数据最终被修改成什么样了
        MIXED：包含了 STATEMENT 和 ROW 模式，它会根据不同的情况自动使用 ROW 模式和 STATEMENT 模式；
redo log 是物理日志，记录的是在某个数据页做了什么修改，比如对XXX表空间中的YYY数据页ZZZ偏移量的地方做了AAA 更新；

3.写入方式不同：
binlog 是追加写，写满一个文件，就创建一个新的文件继续写，不会覆盖以前的日志，保存的是全量的日志。
redo log 是循环写，日志空间大小是固定，全部写满就从头开始，仅保存未被刷入磁盘的脏页日志。

4.用途不同：
binlog 用于备份恢复、主从复制；
redo log 用于掉电等故障恢复。

redo log 可以在事务没提交之前持久化到磁盘，但是 binlog 必须在事务提交之后，才可以持久化到磁盘。
```



### 主从复制

MySQL的主从复制依赖与binlog，记录MySQL所有的变化。复制的过程就是将binlog中的数据从主库传输到从库，一般是异步，也就是主库上执行事务操作不会等待复制binlog的线程同步。

![MySQL 主从复制过程](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/how_update/%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6%E8%BF%87%E7%A8%8B.drawio.png?image_process=watermark,text_5YWs5LyX5Y-377ya5bCP5p6XY29kaW5n,type_ZnpsdHpoaw,x_10,y_10,g_se,size_20,color_0000CD,t_70,fill_0)

![image-20220830161607439](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220830161607439.png)

### 从库是不是越多越好

```
不是。因为从库数量增加，从库连接上来的 I/O 线程也比较多，主库也要创建同样多的 log dump 线程来处理复制的请求，对主库资源消耗比较高，同时还受限于主库的网络带宽。
```

### MySQL 主从复制还有哪些模型？

![image-20220830170040729](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220830170040729.png)

### binlog 什么时候刷盘

一个事务的binlog 是不能被拆开的，因为被拆开的时候，再备库执行就会被当作多个事务分段执行，破坏原子性。因此，MySQL给每个线程都分配了binlog cache。·

事务执行过程中，先把日志写到 binlog cache（Server 层的 cache）
事务提交的时候，再把 binlog cache 写到 binlog 文件中，并清空binlog cache

![binlog cach](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/how_update/binlogcache.drawio.png)

![image-20220830171051555](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220830171051555.png)

![image-20220830172320950](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220830172320950.png)



## 两阶段提交

```
事务提交后，redo log 和 binlog 都要持久化到磁盘，但是这两个是独立的逻辑，可能出现半成功的状态，这样就造成两份日志之间的逻辑不一致。redo log影响主库的数据， binlog影响从库的数据，如：
1.如果在将 redo log 刷入到磁盘之后， MySQL 突然宕机了，而 binlog 还没有来得及写入。
2.如果在将 binlog 刷入到磁盘之后， MySQL 突然宕机了，而 redo log 还没有来得及写入。

MySQL 为了避免出现两份日志之间的逻辑不一致的问题，使用了「两阶段提交」来解决。两阶段提交把单个事务的提交拆分成了 2 个阶段，分别是分别是「准备阶段」和「提交阶段」。
```

### 两阶段提交的过程

![两阶段提交](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/how_update/%E4%B8%A4%E9%98%B6%E6%AE%B5%E6%8F%90%E4%BA%A4.drawio.png?image_process=watermark,text_5YWs5LyX5Y-377ya5bCP5p6XY29kaW5n,type_ZnpsdHpoaw,x_10,y_10,g_se,size_20,color_0000CD,t_70,fill_0)

![image-20220830173539521](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220830173539521.png)



### 两阶段如何处理异常

![时刻 A 与时刻 B](https://cdn.xiaolincoding.com/gh/xiaolincoder/mysql/how_update/%E4%B8%A4%E9%98%B6%E6%AE%B5%E6%8F%90%E4%BA%A4%E5%B4%A9%E6%BA%83%E7%82%B9.drawio.png?image_process=watermark,text_5YWs5LyX5Y-377ya5bCP5p6XY29kaW5n,type_ZnpsdHpoaw,x_10,y_10,g_se,size_20,color_0000CD,t_70,fill_0)

![image-20230218230647650](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230218230647650.png)

### 两阶段提交有什么问题

```
两阶段提交虽然保证了两个日志文件的数据一致性，但是性能很差，主要有两个方面的影响：
1.磁盘I/O次数高：对于“双1”配置，每个事务提交都会进行两次fsync（刷盘），一次是redo log刷盘，另一次是binlog刷盘。
a.当 sync_binlog = 1 的时候，表示每次提交事务都会将 binlog cache 里的 binlog 直接持久到磁盘；
b.当 innodb_flush_log_at_trx_commit = 1 时，表示每次事务提交时，都将缓存在 redo log buffer 里的 redo log 直接持久化到磁盘；

2.锁竞争激烈：两阶段提交虽然能够保证「单事务」两个日志的内容一致，但在「多事务」的情况下，却不能保证两者的提交顺序一致，因此，在两阶段提交的流程基础上，还需要加一个锁来保证提交的原子性，从而保证多事务的情况下，两个日志的提交顺序一致。
在早期的 MySQL 版本中，通过使用 prepare_commit_mutex 锁来保证事务提交的顺序，在一个事务获取到锁时才能进入 prepare 阶段，一直到 commit 阶段结束才能释放锁，下个事务才可以继续进行 prepare 操作。
```

### binlog组提交

![image-20220830215355566](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220830215355566.png)

此时锁只针对每个队列进行保护，不再锁住提交事务的整个过程。锁粒度减少了，使得多个阶段可以并发进行。

### 有 binlog 组提交，那有 redo log 组提交吗？

```
MySQL 5.7 才有 redo log 组提交。原理：在 prepare 阶段不再让事务各自执行 redo log 刷盘操作，而是推迟到组提交的 flush 阶段，也就是说 prepare 阶段融合在了 flush 阶段。过程如下：
1.flush阶段：第一个事务会成为 flush 阶段的 Leader，此时后面到来的事务都是Follower。然后一次将同组事务的 redolog 刷盘(prepare),之后将这组事务执行过程中产生的binlog写入到binlog文件(仅缓存在文件系统，不刷盘)。
2.sync阶段：等待一段时间，组合到更多事务的binlog，再一起刷盘。
3.commit阶段：调用引擎的提交事务接口，将 redo log 状态设置为 commit。
```

[MySQL 日志：undo log、redo log、binlog 有什么用？ | 小林coding (xiaolincoding.com)](https://xiaolincoding.com/mysql/log/how_update.html#组提交)

## MySQL 磁盘 I/O 很高，有什么优化的方法

![image-20220830222124562](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220830222124562.png)



# 内存篇

## Buffer Pool

### 如何管理空闲页？

```
为了更好的管理这些在 Buffer Pool 中的缓存页，InnoDB 为每一个缓存页都创建了一个控制块，控制块信息包括「缓存页的表空间、页号、缓存页地址、链表节点」等等。
为了能够快速找到空闲的缓存页，可以使用链表结构，将空闲缓存页的「控制块」作为链表的节点，这个链表称为 Free 链表（空闲链表）。
```

![img](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost4@main/mysql/innodb/freelist.drawio.png)

###  如何管理脏页？

```
设计 Buffer Pool 除了能提高读性能，还能提高写性能，也就是更新数据的时候，不需要每次都要写入磁盘，而是将 Buffer Pool 对应的缓存页标记为脏页，然后再由后台线程将脏页写入到磁盘。
那为了能快速知道哪些缓存页是脏的，于是就设计出 Flush 链表。跟 Free 链表类似的。区别在于 Flush 链表的元素都是脏页
```

### 如何提高缓存命中率？

简单的 LRU 算法并没有被 MySQL 使用，因为简单的 LRU 算法无法避免下面这两个问题：

- 预读失效；
- Buffer Pool 污染；

```
预读失效
MySQL 在加载数据页时，会提前把它相邻的数据页一并加载进来，目的是为了减少磁盘 IO。但是可能这些被提前加载进来的数据页，并没有被访问，相当于这个预读是白做了，反而存在热点数据页给淘汰出去的可能。

Buffer Pool 污染
当某一个 SQL 语句扫描了大量的数据时，在 Buffer Pool 空间比较有限的情况下，可能会将 Buffer Pool 里的所有页都替换出去，导致大量热数据被淘汰了，等这些热数据又被再次访问的时候，由于缓存未命中，就会产生大量的磁盘 IO，MySQL 性能就会急剧下降，这个过程被称为 Buffer Pool 污染。
```

MySQL做法：
1.它改进了 LRU 算法，将 LRU 划分了 2 个区域：**old 区域 和 young 区域**。预读的页只需要加入到 old 区域的头部，当页被真正访问的时候，才将页插入 young 区域的头部。youg : old = 63:37
2.进入到 young 区域头部条件不仅需要被访问，而且还需要**在 old 区域停留时间超过 1 秒**（默认）。

![img](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost4@main/mysql/innodb/young%2Bold.png)

另外，MySQL 针对 young 区域其实做了一个优化，为了防止 young 区域节点频繁移动到头部。young 区域前面 1/4 被访问不会移动到链表头部，只有后面的 3/4被访问了才会。



### 脏页什么时候会被刷入磁盘？

```
下面几种情况会触发脏页的刷新：

当 redo log 日志满了的情况下，会主动触发脏页刷新到磁盘；
Buffer Pool 空间不足时，需要将一部分数据页淘汰掉，如果淘汰的是脏页，需要先将脏页同步到磁盘；
MySQL 认为空闲时，后台线程回定期将适量的脏页刷入到磁盘；
MySQL 正常关闭之前，会把所有的脏页刷入到磁盘；
```



# 面试篇

## 慢查询

```
1.慢查询是什么？
慢查询，执行很慢的查询。有多慢？超过 long_query_time 参数设定的时间阈值（默认10s），就被认为是慢的，是需要优化的。慢查询被记录在慢查询日志里。

2.怎么查看/分析慢查询日志记录？
默认不开启慢查询日志，需要手动打开SET GLOBAL slow_query_log = ‘ON’，通过慢查询日志定位出查询效率较低的SQL，可以使用explain查看SQL的执行计划。

3.怎么优化慢查询
● 多数慢SQL都跟索引有关，比如数据表不加索引，sql语句导致索引不生效等。可以优化索引、sql语句，确保中索引。
● 还可以优化SQL语句，比如多次连表关联查询，查询了多余的行并且抛弃掉，加载了许多结果并不需要的列。
● 如果单表数据量过大导致慢查询，可以考虑分库分表
```



## delete、truncate、drop

1.DELETE属于数据库DML操作语言，只删除数据不删除表的结构，会走事务，执行时会触发trigger；

2、在 InnoDB 中，DELETE其实并不会真的把数据删除，mysql 实际上只是给删除的数据打了个标记为已删除，因此 delete 删除表中的数据时，表文件在磁盘上所占空间不会变小，存储空间不会被释放，只是把删除的数据行设置为不可见。 虽然未释放磁盘空间，但是下次插入数据的时候，仍然可以重用这部分空间（重用 → 覆盖）。

以下都是不走事务，没有回滚机制，立即执行。

3、**drop**：删除内容和定义，释放空间。（表结构和数据一同删除）

4、**truncate**：删除内容，释放空间，但不删除定义。（表结构还在，数据删除）

**三者的执行速度，一般来说：drop > truncate > delete**

# SQL语句篇



## 基本语法

SQL查询五子句：

mysql> select */字段列表 from 数据表名称 where 子句 group by 子句 having 子句 order by 子句 limit 子句;
①.where 子句
②.group by 子句
③.having 子句
④.order by 子句
⑤.limit子句
特别注意：五子句的顺序是固定的，不能颠倒。

三，当一个查询语句同时出现了where,group by,having,order by的时候，执行顺序和编写顺序是：

1.执行where xx对全表数据做筛选，返回第1个结果集。

2.针对第1个结果集使用group by分组，返回第2个结果集。

3.针对第2个结果集中的每1组数据执行select xx，有几组就执行几次，返回第3个结果集。

4.针对第3个结集执行having xx进行筛选，返回第4个结果集。

5.针对第4个结果集排序。



**①.where 子句：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210518083218936.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTQ0NjQx,size_16,color_FFFFFF,t_70)

过滤空值要 is not null

```sql
select device_id, gender, age,university from user_profile where age is not null
```



**②.group by 子句（难重点）：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210518094612892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTQ0NjQx,size_16,color_FFFFFF,t_70)

**③.having 子句：**

- having与where类似，根据条件对数据进行过滤筛选
- where针对表中的列发挥作用，查询数据
- having针对查询结果集发挥作用，筛选数据！！！

```sql
求每个学科中，学科人数大于3人的学科信息
select subject,count(*) from table group by subject having count(*)>3 
```



## 连接查询

1.内连接查询：把两个表甚至多个表进行连接，然后拿表1中的每条记录与表2进行匹配，如果有对应，则显示。反之忽略。也就是没有空值的情况的。

2.左外连接查询：把左表中的每一条数据都保留，右表匹配到结果就显示，匹配不到就显示为null

3.右外连接查询：把右表中的每一条数据都保留，左表匹配到结果就显示，匹配不到就显示为null

```sql
select tableA.字段列表, tableB.字段列表 from tableA inner join tableB on tableA.id = tableB.id

select * from person as p inner join card as c on p.card_id = c.id;
```

## 面试常见题目

查询排名第二的数据

```sql
select Max(salary) from Employee where salary <(select max(salary) from employee)
```

查询各科目成绩排名第二名的学生

```sql
with rankScores as {
	select  subject, student, score,
	dense_rank() over (partition by subject order by score desc) as ranking
	from scores
}
select subject, student, score
from rankScores
where ranking = 2;


WITH 子句：查询以一个 WITH 子句开始，这用于创建一个临时的结果集，其中包含每个科目的学生分数以及排名。这个子句的目的是为了在后续查询中重复使用这个结果集。
DENSE_RANK() 函数：在 SELECT 语句中，我们使用 DENSE_RANK() 窗口函数。这个函数根据每个科目中的分数（score）降序排名，而且根据 subject 列分组（PARTITION BY subject）。这意味着它会为每个科目内的学生排名，并将排名结果放入 ranking 列中。
```

