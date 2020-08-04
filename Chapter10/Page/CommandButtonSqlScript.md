# [返回目录](../../README.html)

## [技术参考](../Index.html) - [页面标签](../Page.html) - [命令按钮](CommandButton.html) - 命令按钮前置与后置数据库脚本

&emsp;&emsp;命令按钮（CommandButton）是命令（Command）执行的容器与触发界面元素。命令（Command）是框架提供针对动态数据（Dynamic Data）执行的业务逻辑。一个典型的命令执行场景是“用户选中某条或几条动态数据，然后点击命令按钮，系统针对选中的动态数据逐条执行命令，完成业务逻辑”。框架提供了几十个命令帮助应用程序完成常见的业务逻辑，但这些命令并不能满足所有的业务需求，因此，框架为命令按钮提供了前置数据库脚本（PreSqlScript）与后置数据库脚本（PostSqlScript）。

&emsp;&emsp;前置数据库脚本（PreSqlScript）：命令按钮在触发命令执行之前需要执行的一段数据库 SQL 脚本。

&emsp;&emsp;后置数据库脚本（PostSqlScript）：命令按钮在触发命令成功执行之后需要执行的一段数据库 SQL 脚本。

&emsp;&emsp;通过以下代码可以为命令按钮配置前置、后置数据库脚本：

```xml
<eIvy:COMMANDBUTTON CommandName="FormSave" Text="保存"
    PreSqlScriptName=""            <!--前置数据库脚本的名称-->
    PreSqlScriptParameters=""      <!--前置数据库脚本的传入参数-->
    PostSqlScriptName=""           <!--后置数据库脚本的名称-->
    PostSqlScriptParameters=""/>   <!--后置数据库脚本的传入参数-->
```

### **脚本传入参数**

&emsp;&emsp;这里最关键的是数据库脚本的传入参数，其配置格式为：

```xml
{传入参数计算表达式1.@参数名称1}{传入参数计算表达式2.@参数名称2}...
```

&emsp;&emsp;一个传入参数由一对花括号界定，并由传入参数计算表达式和@参数名称两部分由“.”运算符连接。

&emsp;&emsp;传入参数计算表达式有以下几种形式构成：

* 常量

```xml
<eIvy:COMMANDBUTTON CommandName="Empty" Text="XXX"
    PreSqlScriptName="SomeSqlScript"
    PreSqlScriptParmaters="{1.@ContactID}"/>
```

&emsp;&emsp;说明：传入参数计算表达式为数值常量 1，该数值将作为 @ContactID 传入参数值。

* 动态数据控件属性表达式

```xml
<eIvy:COMMANDBUTTON CommandName="Empty" Text="XXX"
    PreSqlScriptName="SomeSqlScript"
    PreSqlScriptParameters="{(@SecurityContext.LoginUserMapping.EntityID).@EntityID}"/>
```

&emsp;&emsp;说明：如果该命令按钮放置在视图模板中，则可以通过属性表达式 (@SecurityContext.LoginUserMapping.EntityID) 获取当前登录用户对应的实体ID，并传递给参数 @EntityID。

* 页面属性表达式

```xml
<eIvy:COMMANDBUTTON CommandName="Empty" Text="XXX"
    PreSqlScriptName="SomeSqlScript"
    PreSqlScriptParameters="{(@Now.Year).@Year}"/>
```

&emsp;&emsp;说明：如果该命令按钮放置在页面/母版页模板中，则可以通过属性表达式 (@Now.Year) 获取当前时间的年份，并传递给参数 @Year。

* 命令按钮属性表达式

```xml
<eIvy:COMMANDBUTTON CommandName="Empty" Text="XXX"
    PreSqlScriptName="SomeSqlScript"
    PreSqlScriptParameters="{this.SecurityContext.LoginUserMapping.EntityID.@EntityID}"/>
```

&emsp;&emsp;说明：通过命令按钮属性表达式 this.SecurityContext.LoginUserMapping.EntityID （this 关键字可以不用写）可以获取当前登录用户对应实体ID，并传递给参数 @EntityID。

* 字段引用表达式

```xml
<eIvy:COMMANDBUTTON CommandName="Empty" Text="XXX"
    PreSqlScriptName="SomeSqlScript"
    PreSqlScriptParameters="{{@ID}.@EntityID}"/>
```

&emsp;&emsp;说明：如果该命令按钮放置在视图模板的字段引用上下文中（DynamicList 的 <eIvy:ROWREPEATOR> 标签内部；DynamicForm 的任何位置），则通过字段引用表达式 {@ID} 获取当前数据的ID主码值，并传递给参数 @EntityID。

&emsp;&emsp;有关于更多表达式，参见 [模板表达式](../Expression.html)。更多属性参见 [常见属性表达式](../CommonProperty.html)。

### **默认传入参数**

&emsp;&emsp;实际上前置数据库脚本会默认带上两个传入参数：

```xml
{this.DynamicDataControl.SelectedPrimaryKeys.@ID}   <!--当前选中数据行的主码，这是一个集合属性-->
{this.Page.PrincipalJson.@Principal}                <!--当前登录用户的身份信息-->
```

&emsp;&emsp;而后置数据库脚本会默认带上两个传入参数：

```xml
{this.Command.AffectedPrimaryKeys.@ID}              <!--命令执行成功后所影响的数据行主码，这是一个集合属性-->
{this.Page.PrincipalJson.@Principal}                <!--当前登录用户的身份信息-->
```

&emsp;&emsp;由于数据库脚本会默认带上上述传入参数，因此在脚本中可以直接使用这两个参数。

### **脚本执行次数**

&emsp;&emsp;另外，我们需要知道的一个事实，前置与后置数据库脚本会执行多少次的问题。

&emsp;&emsp;如果是前置脚本，执行次数会根据默认参数 {this.DynamicDataControl.SelectedPrimaryKeys.@ID} 的多少来决定，即选中了多少行数据。如果选中了 3 行数据，则脚本会执行 3 次，每次执行传递的 @ID 参数为一个选中的标量值，而不是整个选中的 ID 集合。

&emsp;&emsp;但当传入参数中还配置了另外一个集合属性，例如：{SecurityContext.LoginUser.Roles.RoleName.@Role}，那么是默认参数 {this.DynamicDataControl.SelectedPrimaryKeys.@ID} 的个数乘上 {SecurityContext.LoginUser.Roles.RoleName.@Role} 参数的个数。即在参数表达式中各参数个数的笛卡尔积的秩。

&emsp;&emsp;对于后置脚本也是如同前置脚本一样，只不过需要按照默认参数 {this.Command.AffectedPrimaryKeys.@ID} 来计算。

### **脚本传出参数**

&emsp;&emsp;有的时候当命令前置、后置脚本执行完毕后，希望获知输出参数值，这可以通过命令按钮专有属性获取：

```xml
<eIvy:COMMANDBUTTON ...
    PreSqlScriptOutputParameters=""    <!--前置数据库脚本输出参数-->
    PostSqlScriptOutputParameters=""   <!--后置数据库脚本输出参数-->
/>
```

&emsp;&emsp;这两个参数并非可配置属性，但可以通过属性表达式获取。

&emsp;&emsp;例如：当我们在页面放置一个命令按钮，该按钮需要执行一段前置数据库脚本，在该脚本中会向A表中插入一条数据，新增的这条数据 ID 值会作为 @ID 输出参数传递给命令按钮对象，此时命令按钮会根据新增的这条数据 ID 值构造出页面跳转地址，用于导航到A表该数据的修改页面。

&emsp;&emsp;以下为数据库脚本 SomeSqlScript ：

```sql
INSERT INTO A
(UsedFlag, SourceType, Creator, CreateTime, Editor, EditTime, ...)
VALUES
(1, 1, @Principal, GETDATE(), @Principal, GETDATE(), ...)

SELECT @ID = SCOPE_IDENTITY()
```

&emsp;&emsp;以下为页面的 CommandButton 标签代码 ：

```xml
<eIvy:COMMANDBUTTON CommandName="Empty" CssClass="btn btn-default" Text="XXX"
    DynamicDataControlID="DDC1" 
    PreSqlScriptName="SomeSqlScript"
    NavigateUrl="~/A/EditForm?ID={this.PreSqlScriptOutputParameters[ID].Value.@ID}"
/>
```

&emsp;&emsp;注意到 NavigateUrl 属性中配置了脚本参数表达式 {this.PreSqlScriptOutputParameters[ID].Value.@ID} ，通过这个表达式可以获取来自前置脚本 SomeSqlScript 中传出的 @ID 参数值，该参数值即为新增A表数据的主码值。或者用以下的表达式获得传出参数 @ID 值（2020/07/09之前的框架版本）：

```xml
<eIvy:COMMANDBUTTON CommandName="Empty" CssClass="btn btn-default" Text="XXX"
    DynamicDataControlID="DDC1" 
    PreSqlScriptName="SomeSqlScript"
    NavigateUrl="~/A/EditForm?ID={this.SqlScriptParameterResolver.ScriptParameters[ID].DbParameter.Value.@ID}"
/>
```

---
&emsp; &copy; eIvy Framework 2019.
