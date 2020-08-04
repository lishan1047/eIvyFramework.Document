# [返回目录](../README.html)

## [数据库](Index.html) - 页面脚本调用数据库脚本  

&emsp;&emsp;框架提供了 SQL 脚本调用的一般处理程序，使得开发者可以通过页面 JS 脚本调用后台 SQL 脚本，并获得输出值。例如有这样的一个使用场景：

&emsp;&emsp;某张表达软件开发需求的表格有两个字段：优先级（PriorLevel）、状态（Status），当优先级为“高”的时候，状态的取值一定要为“待开发”。希望将这一业务需求反映到用户在页面操作的时候，当其选择优先级的下拉列表，应该立刻执行这一逻辑，并将得到的状态值更新到页面。这一过程对于用户来说，既不希望在保存这张表格数据的时候执行，也不希望引起页面的回发执行。为此，需要进行页面的异步执行。

<img src="Image/2018051301.png"></img>

图 1 表格字段示意图

通过以下步骤可以完成这一过程。

（1）首先编写SQL脚本代码：RequirementsTest

```sql
IF(@PriorLevel = 1)
BEGIN
  SET @Status = 1
END
```

（2）通过视图模板中的字段属性设置，将优先级、状态两个字段的 CssClass 属性分别设置为“form-control _priorLevel”、“form-control _status”。这一设置的目的是为了方便在页面查找到唯一的字段元素，form-control 是为了保留原有的样式。

（3）在页面模板中写入以下代码。

```javascript
<script>
  function invokeRequirementsTest() {
    $('._priorLevel').change(function() {
      eIvy.ajax(eIvy.RequestMethod.POST, 
                '~/HttpHandlers/SqlScriptInvoker.ashx' 
                    + '?name=RequirementsTest' 
                    + '&amp;params=' + JSON.stringify({PriorLevel:$('._priorLevel').val(),Status:-1}), 
                function(result, status){
                    var r = JSON.parse(result);
                    if(r.error) {
                        alert(r.error);
                    } else {
                        if(r.Status &gt; 0) {
                          $('._status').val(r.Status);
                        }
                    }
                },
                function(status){
                    alert(status);
                });
    });
  }
</script>
<eIvy:SCRIPTLOADER PageStartupScript="invokeRequirementsTest();"></eIvy:SCRIPTLOADER>
```

说明：

（1）上述代码的关键在于通过 eIvy.ajax 这个框架带的异步调用函数，向SQL脚本处理程序 SqlScriptInvoker.ashx 发起远程调用。而该远程异步调用是由“优先级”下拉列表的值发生变化的时候触发的，即 $('._priorLevel').change(...)

（2）在SQL脚本处理程序调用的时候，将 name（SQL 脚本名称）、params（脚本参数，以 JSON 格式表示）两个参数作为地址栏参数传递给 SQL 脚本处理程序。

（3）函数 function(result,status) 是当异步调用成功之后需要执行的逻辑，function(status) 是当异步调用失败之后需要执行的逻辑。

（4）JSON.parse(result) 将处理程序调用返回的 Json 格式字符串转换成对象 r，通过判断 r.error 属性，检测处理程序是否执行异常，如果成功执行，则将返回结果 r.Status 替换现在的“状态”值，即 $('._status').val(r.Status)。

SQL脚本处理程序 SqlScriptInvoker.ashx 的参数：

```xml
name：SQL 脚本名称
params：SQL 脚本传入与传出参数，以 JSON 格式表示，注意初始取值的逻辑
```

&emsp;&emsp;另外要注意，处于安全目的，用于SQL脚本处理程序 SqlScriptInvoker.ashx 调用执行的数据库脚本需要配置功能，否则执行的时候会出现异常。

---
&emsp; &copy; eIvy Framework 2019.