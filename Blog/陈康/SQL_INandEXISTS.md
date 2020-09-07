# <div style="height:40px"><div style="float:left">eIvy Framework 开发者博客</div> <div style="float:right"><img width="80" height="40" src="../../Logo.png"></img></div></div>

# SQL_IN与EXISTS

![数据库展示图1](..\Photo\Datebase1_ck.png)

![数据库展示图2](..\Photo\Datebase2_ck.png)

本文主要讨论SQL中'IN'操作符与'EXISTS'操作符的原理，用法，两者间的比较以及各自的适应情况。
本文中提到的所有数据表基于王珊《数据库系统概论（第4版）》。

IN 与EXISTS大多数情况下都是用于SELECT查询语句的条件判断中，因此我想在正文之前先简要介绍一下SELECT的大致过程：

分析器会先看语句的第一个词，当它发现第一个词是SELECT关键字的时候，它会跳到FROM关键字，然后通过FROM关键字找到表名并把表装入内存。接着是找WHERE关键字，如果找不到则返回到SELECT找字段解析，如果找到WHERE，则分析其中的条件，完成后再回到SELECT分析字段。最后形成一张我们要的虚表。

WHERE关键字后面的是条件表达式。条件表达式计算完成后，会有一个返回值，即非0或0，非0即为真(true)，0即为假(false)。同理WHERE后面的条件也有一个返回值，真或假，来确定接下来执不执行SELECT。

分析器先找到关键字SELECT，然后跳到FROM关键字将目标表导入内存，并通过指针找到第一条记录，接着找到WHERE关键字计算它的条件表达式，如果为真那么把这条记录装到一个虚表当中，指针再指向下一条记录。如果为假那么指针直接指向下一条记录，而不进行其它操作。一直检索完整个表，并把检索出来的虚拟表返回给用户。

下面让我们开始正文。

**IN**

IN 操作符原理：  
当执行下面查询语句：  
```
select * from A where id in (select id from B)
```
IN()只执行一次，它查出B表中的所有id字段并缓存起来。之后，检查A表的id是否与B表中的id相等，如果相等则将A表的记录加入结果集中，直到遍历完A表的所有记录。  
它的查询过程类似于以下过程：  
```
List resultSet={};Array A=(select * from A);Array B=(select id from B);
for(int i=0;i<A.length;i++) {
  for(int j=0;j<B.length;j++) {
      if(A[i].id==B[j].id) {
        resultSet.add(A[i]);
        break;
      }
  }}return resultSet;
  ```

IN 语法： 
``` 
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...)
```
IN 用法  
例：  
```
SELECT Sno
FROM SC
WHERE Cno IN
(SELECT Cno
FROM Course
WHERE CNAME = ‘数据结构’)
```
首先通过子查询得到课名为 “数据结构” 的课程的课号，然后遍历 SC （选课）表中的每一条选课记录，若当前这条记录的课号为 “数据结构” 这门课的课号，则将这条记录的 Sno 列的值放到结果集里面去。最终我们可以得到所有选修了 ”数据结构“ 这门课的学生的学号。  
**EXISTS**  
EXISTS 操作符原理：  
当执行下面查询语句： 
``` 
select * from A where exists (select 1 from B where A.id=B.id)
```
exists()会执行A.length次，它并不缓存exists()结果集，因为exists()结果集的内容并不重要，重要的是其内查询语句的结果集空或者非空，空则返回false，非空则返回true。  
它的查询过程类似于以下过程：  
```
List resultSet={};Array A=(select * from A);
for(int i=0;i<A.length;i++) {
   if(exists(A[i].id) {  //执行select 1 from B where B.id=A.id是否有记录返回
       resultSet.add(A[i]);
   }}return resultSet;
   ```

EXISTS语法：  
语法： EXISTS subquery  
参数： subquery 是一个受限的 SELECT 语句 (不允许有 COMPUTE 子句和 INTO 关键字)。  
结果类型： Boolean 如果子查询包含行，则返回 TRUE ，否则返回 FLASE 。  
EXISTS用法：  
例：  
```
SELECT Sname
FROM Student
WHERE EXISTS
(SELECT *
FROM SC
WHERE Sno = Student.Sno
AND Cno = ‘1’)  
```
在这个查询中，首先会取出 Student 表中的第一条记录，得到其 Sno 列（因为在子查询中用到了）的值（如 95001），然后将该值代入到子查询中。若能找到这样的一条记录，那么说明学号为 95001 的学生选修了 1 号课程。因为能找到这样的一条记录，所以子查询的结果不为空集，那么 EXISTS 会返回 True，从而使 Student 表中的第一条记录中的 Sname 列的值被放入结果集中去。以此类推，遍历 Student 表中的所有记录后，就能得到所有选修了 1 号课程的学生的姓名。  
 	值得注意的是，上面EXISTS的例子与之前IN用到的例子是不一样的，前者是相关子查询，而后者是不相关子查询。判断是否是 “相关子查询” 也很简单，只要子查询不能脱离父查询单独执行，那么就是 “相关子查询”。  
而关于NOT IN 和NOT EXISTS，其实就是对IN 和EXISTS的否定，这里就不再细讲。  
另外，SQL本身是没有除法功能的，但是可以通过NOT EXISTS来实现这一目的。比如若想查询选修了全部课程的学生的姓名。可以通过以下步骤的思路来实现：  
    STEP1：先取 Student 表中的第一个元组，得到其 Sno 列的值。  
	STEP2：再取 Course 表中的第一个元组，得到其 Cno 列的值。  
	STEP3：根据 Sno 与 Cno 的值，遍历 SC 表中的所有记录（也就是选课记录）。若对于某个 Sno 和 Cno 的值来说，在 SC 表中找不到相应的记录，则说明该 Sno 对应的学生没有选修该 Cno 对应的课程。  
	STEP4：对于某个学生来说，若在遍历 Course 表中所有记录（也就是所有课程）后，仍找不到任何一门他/她没有选修的课程，就说明此学生选修了全部的课程。  
	STEP5：将此学生放入结果元组集合中。  
	STEP6：回到 STEP1，取 Student 中的下一个元组。  
	STEP7：将所有结果元组集合显示。  
根据以上思路，可以写出 SQL 语句：  
```
SELECT Sname
FROM Student
WHERE NOT EXISTS
(SELECT *
FROM Course
WHERE NOT EXISTS
(SELECT * FROM SC
WHERE Sno = Student.Sno
AND Cno = Course.Cno))  
```
其中第一个 NOT EXISTS 对应 STEP4，第二个 NOT EXISTS 对应 STEP3。  
另外，在往目标表里插入数据的时候，往往也会使用EXISTS进行查重判断，如下所示：  
INSERT INTO Student(Sno, Sname)
SELECT Sno, Sname FROM TableIn
WHERE NOT EXISTS
(SELECT * FROM Student
WHERE Student.Sno = TableIn.Sno
AND Student.Sname = TableIn.Sname)  
上述方法在目标表数据较大，而导入表数据较小判断是否存在相同项有着更高的效率。  
**IN与EXISTS的区别及应用场景**  
通过上面对IN操作符与EXISTS操作符的探究，我们可以将两者的运行过程粗略的进行下面的描述：IN 这种类型的查询是先执行子查询，得到一个集合（或值），然后将这个集合（或值）作为一个常量带入到父查询的 WHERE 子句中去，回到父查询后依然需要按照正常的流程对父查询中的每一行进行遍历判断；EXISTS(包括 NOT EXISTS )子句的返回值是一个BOOL值。 EXISTS内部有一个子查询语句(SELECT ... FROM...)， 我将其称为EXIST的内查询语句。其内查询语句返回一个结果集。 EXISTS子句根据其内查询语句的结果集空或者非空，返回一个布尔值，一种通俗的可以理解为：将外查询表的每一行，代入内查询作为检验，如果内查询返回的结果取非空值，则EXISTS子句返回TRUE，这一行行可作为外查询的结果行，否则不能作为结果。  
而两者应用场景的比较，通过两者运行过程的，可以粗略的概括为：  
IN适合于外表大而内表小的情况；  
EXISTS适合于外表小而内表大的情况。  

以下是撰写本文时参考到的一些博客，如有侵权，请通知删除。  
[SQL中EXISTS的用法](https://www.cnblogs.com/netserver/archive/2008/12/25/1362615.html)  
[SQL 中的 EXISTS 到底做了什么？](https://zhuanlan.zhihu.com/p/20005249)  
[Sql语句中IN和exists的区别及应用](https://www.cnblogs.com/liyasong/p/sql_in_exists.html)  
[SQL查询中in和exists的区别分析](https://www.jianshu.com/p/f212527d76ff)  


---
&emsp; &copy; eIvy Framework 2019.
