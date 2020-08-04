# [返回目录](../../README.html)

## [技术参考](../Index.html) - [页面标签](../Page.html) - SQLSCRIPTOR

&emsp;&emsp;页面 SQL 执行器（Sql Scriptor）：页面加载并执行 SQL 脚本，而无需通过命令按钮去执行。  

### 标签语法

```xml
<eIvy:SQLSCRIPTOR/>
```

&emsp;&emsp;有的时候我们并不希望通过点击命令按钮的方式去执行一段 SQL 脚本，而是在页面加载的时候就能够执行，这就需要通过该标签来执行数据库脚本。以下是一段非常简单的数据库脚本 Test，只是抛出了一个异常：

```sql
RAISERROR('这是一个错误。', 16, 1)
```

&emsp;&emsp;我们可以通过以下代码，使得上述 Test 数据库脚本在页面加载的时候执行。

```xml
<eIvy:DYNAMICLIST ID="DDC1" .../>
<eIvy:SQLSCRIPTOR DynamicDataControlID="DDC1"
    ScriptName="Test"
/>
```

&emsp;&emsp;这个标签还有一些有价值的属性，可以帮助我们更好的完成任务。

&emsp;&emsp;默认情况下，数据库脚本只是在页面首次加载的时候执行，但当我们需要页面每次回发的时候都能执行，则需要通过将 ExecutePagePostManner 属性设置为 Always。

```xml
<eIvy:DYNAMICLIST ID="DDC1" .../>
<eIvy:SQLSCRIPTOR DynamicDataControlID="DDC1"
    ScriptName="Test"
    ExecutePagePostManner="Always"
/>
```

&emsp;&emsp;这个属性有三个取值，代表的意思参见下方说明：

```xml
Initial : 页面首次加载执行
Post: 页面回发的时候执行
Always: 无论何时，总是执行
```

&emsp;&emsp;另外，当我们需要将数据库脚本推迟到大多数页面内容加载完成后执行，则需要设置属性 ExecutePageEvent 为 PreRender。该属性有两个取值，参见下方说明：

```xml
Load: 加载页面且按钮命令执行之前，执行数据库脚本
PreRender: 按钮命令执行之后，执行数据库脚本
```

&emsp;&emsp;如果数据库脚本抛出了异常，这个标签会将页面导向到系统错误页，但有的时候，我们只是希望将错误信息显示在当前页面，则可以通过将属性 ErrorThrownManner 设置为 Alert。该属性有两个取值，参见以下说明：

```xml
Error: 导向到系统错误页，显示数据库脚本抛出的异常
Alert: 在当前页显示数据库脚本抛出的异常消息。
```

&emsp;&emsp;当然，我们也可以在数据库脚本中，将消息按照 JSON 格式配置为警告：

```sql
RAISERROR('{Type:"Warning",Message:"TEST",ErrorCode:0}', 16, 1)
```

&emsp;&emsp;如果数据库脚本带有传入参数，则通过属性 SqlScriptParameters 进行配置，其配置方法同 [命令按钮前置与后置数据库脚本](CommandButtonSqlScript.html) 一文中介绍的脚本传入参数一样，而且是等效于后置脚本参数的配置。

---
&emsp; &copy; eIvy Framework 2019.
