# <div style="height:40px"><div style="float:left">eIvy Framework 开发者博客</div> <div style="float:right"></div></div>

# SQL 游标
&emsp;&emsp;游标是处理结果集的一种机制，它可以定位到结果集中的某一行、多行数据进行读写，也可以移动到所需要的行进行数据操作。它的主要用处有：  
>定位到结果集中的某一行。  
>对当前位置的数据进行读写。  
>可以对结果集中的数据进行单独操作。  

游标的优点：  
&emsp;&emsp;游标提供了一种对表中检索的数据灵活操作的一种手段,游标提供了对结果集的每一行进行不同的操作的方法，而不是一次对整个结果集进行同一操作。游标也提供增、删、改的功能。  

游标的缺点：  
&emsp;&emsp;游标的速度很慢，在使用过程中一定要谨慎，游标的滥用可能会导致数据库发生异常错误。  

游标的生命周期：  
&emsp;&emsp;游标的生命周期包含五个阶段：**声明游标、打开游标、读取游标数据、关闭游标、释放游标**。  
1. 声明游标：  
   ```SQL
   DECLARE cursor_name cursor  for select column_name from table_name
   ```  
2. 打开游标：  
   ```SQL
   open cursor_name
   ```
3. 读取游标数据：  
   ```SQL
   fetch
     [next|prior|first|last|absoute n|relative n]
   from cursor_name into @variable_name
   ```  
   参数说明：  
   * next：当前位置的下一行  
   * prior：当前位置的上一行  
   * first：结果集的第一行  
   * last：最后一行  
   * absoute n：从游标的第一行开始数，第n行  
   * relative n：从当前位置开始数，第n行  
   例如：  
   ```SQL
   fetch next from cursor_name
   fetch prior from cursor_name
   fetch first from cursor_name
   fetch last from cursor_name
   fetch absoute 4 from cursor_name
   fetch relative 3 from cursor_name
   ```
   通过检测全局变量@@FETCH_Status的值，获得提取状态信息，该状态用于判断Fetch语句返回数据的有效性。当执行一条Fetch语句之后，@@Fetch_Status可能出现3种情况：  
   * 0，Fetch语句执行成功
   * -1，Fetch语句失败或行不在结果集中。
   * -2，提取的行不存在。  

    例如：  
    ```SQL
    declare @OrderID int
    fetch absoute 4 from cursor_name into @OrderID
    while @@Fetch_Status=0 --提取成功，进行下一行的操作
      begin
        select @OrderID as id
        fetch next from cursor_name into @OrderID  --移动游标
      end
    ```
   **利用游标更新、删除数据**  
   ```SQL
   --1.声明游标
   declare cur1 cursor for 
   select ContactID,CardID from Contacts
   --2.打开游标
   open cur1
   --3.声明游标提取数据所要存放的变量
   declare @ContactID int
   declare @CardID int
   --4.定位游标定位到哪一行
   fetch first from cur1 into @ContactID,@CardID --into的变量数量必须与游标查询结果集的列数目相同
   while @@fetch_status=0
   begin
     if(@ContactID=1001)
     begin
     update Contacts set Name="测试" where current of cur1 --更新当前行
     end

     if(@ContactID=2001)
     begin
     delete Contacts where current of cur1 --删除当前行
     end
     fetch next from cur1 into @ContactID,@CardID --移动游标至下一行
   end
   ```
4. 关闭游标  
   &emsp;&emsp;游标打开后，服务器会专门为游标分配一定的内从空间存放游标操作数据的结果集。所以游标一旦用过，应及时关闭，避免服务器资源浪费。  
   `close cursor_name`
5. 释放游标  
   删除游标，释放资源  
   `deallocate cursor_name`

---
&emsp; &copy; eIvy Framework 2020.