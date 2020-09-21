# <div style="height:40px"><div style="float:left">eIvy Framework 开发者博客</div> <div style="float:right"><img width="80" height="40" src="../../Logo.png"></img></div></div>

触发器中的inserted表与deleted表
=============================
触发器语句中使用了两种特殊的表：deleted 表和 inserted 表。Microsoft? SQL Server 2000 自动创建和管理这些表。可以使用这两个临时的驻留内存的表测试某些数据修改的效果及设置触发器操作的条件；然而，不能直接对表中的数据进行更改。 
## inserted 和 deleted 表主要用于触发器中
 * 扩展表间引用完整性  
 * 在以视图为基础的基表中插入或更新数据   
 * 检查错误并基于错误采取行动  
 * 找到数据修改前后表状态的差异，并基于此差异采取行动    
 * Inserted表用于存储INSERT和UPDATE语句所影响的行的副本。在一个插入或更新事务处理中，新建行被同时添加到inserted表和触发器表中。Inserted表中的行是触发器表中新行的副本。  
 * 更新事务类似于在删除之后执行插入；首先旧行被复制到 deleted 表中，然后新行被复制到触发器表和 inserted 表中。   
 * 在设置触发器条件时，应当为引发触发器的操作恰当使用 inserted 和 deleted 表。虽然在测试 INSERT 时引用 deleted 表或在测试 DELETE 时引用 inserted 表不会引起任何错误，但是在这种情形下这些触发器测试表中不会包含任何行。  
> 说明：如果触发器操作取决于一个数据修改所影响的行数，应该为多行数据修改（基于 SELECT 语句的 INSERT、DELETE 或 UPDATE）使用测试（如检查 @@ROWCOUNT），然后采取相应的对策。  
SQL Server? 2000 不允许 AFTER 触发器引用 inserted 和 deleted 表中的 text、ntext 或 image 列；然而，允许 INSTEAD OF 触发器引用这些列。有关更多信息，请参见 CREATE TRIGGER。   
## 在 INSTEAD OF 触发器中使用 inserted 和 deleted 表   
* 传递到在表上定义的 INSTEAD OF 触发器的 inserted 和 deleted 表遵从与传递到 AFTER 触发器的 inserted 和 deleted 表相同的规则。inserted 和 deleted 表的格式与在其上定义 INSTEAD OF 触发器的表的格式相同。inserted 和 deleted 表中的每一列都直接映射到基表中的列。  
* 有关引用带 INSTEAD OF 触发器的表的 INSERT 或 UPDATE 语句何时必须提供列值的规则与表没有 INSTEAD OF 触发器时相同：  
 1. 不能为计算列或具有 timestamp 数据类型的列指定值。  
 2. 不能为具有 IDENTITY 属性的列指定值，除非该列的 IDENTITY_INSERT 为 ON。当 IDENTITY_INSERT 为 ON 时，INSERT 语句必须提供一个值。  
* INSERT 语句必须为所有无 DEFAULT 约束的 NOT NULL 列提供值。   
* 对于除计算列、标识列或 timestamp 列以外的任何列，任何允许空值的列或具有 DEFAULT 定义的 NOT NULL 列的值都是可选的。   
* 当INSERT、UPDATE 或 DELETE 语句引用具有 INSTEAD OF 触发器的视图时，数据库引擎将调用该触发器，而不是对任何表采取任何直接操作。即使为视图生成的 inserted 和 deleted 表中的信息格式与基表中的数据格式不同，该触发器在生成执行基表中的请求操作所需的任何语句时，仍必须使用 inserted 和 deleted 表中的信息。   
* 传递到在视图上定义的 INSTEAD OF 触发器的 inserted 和 deleted 表格式与为该视图定义的 SELECT 语句的选择列表相匹配。例如：  
CREATE VIEW EmployeeNames (EmployeeID, LName, FName)   
AS  
SELECT EmployeeID, LastName, FirstName   
FROM Northwind.dbo.Employees   
* 视图的结果集有三列：一个 int 列和两个 nvarchar 列。传递到在视图上定义的 INSTEAD OF 触发器的 inserted 和 deleted 表也具有名为 EmployeeID 的 int 列、名为 LName 的 nvarchar 列和名为 FName 的 nvarchar 列。   
> 视图的选择列表还包含不直接映射到单个基表列的表达式,一些视图表达式（如常量调用或函数调用）可能不引用任何列，这类表达式会被忽略。复杂的表达式会引用多列，但在 inserted 和 deleted 表中，每个插入的行仅有一个值。如果视图中的简单表达式引用具有复杂表达式的计算列，则这些简单表达式也有同样的问题。视图上的 INSTEAD OF 触发器必须处理这些类型的表达式。有关更多信息，请参见视图上 INSTEAD OF 触发器中的表达式和计算列。  
顺便说一下，当对某张表建立触发器后，分3种情况讨论   
#### 1.插入操作（Insert）  
Inserted表有数据，Deleted表无数据   
#### 2.删除操作（Delete）   
Inserted表无数据，Deleted表有数据   
#### 3.更新操作（Update）   
Inserted表有数据（新数据），Deleted表有数据（旧数据)


 










<img src="../Photo/Logo.png"/>

---
&emsp; &copy; eIvy Framework 2019.