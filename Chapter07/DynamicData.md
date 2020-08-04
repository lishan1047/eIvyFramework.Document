# [返回目录](../README.html)

## [公开应用](Index.html) - 公开动态数据

&emsp;&emsp;公开动态数据是将数据库表格中的数据提供给第三方应用程序。其调用方式和返回数据格式是我们需要关心的核心内容。  

### **概述**  

&emsp;&emsp;第三方应用对数据库表格中的数据请求仍然是 CRUD 的过程，即对数据的新增、读取、修改、删除操作。在这个过程中还需要关注数据访问的安全性问题，即哪些用户可以访问哪些数据的问题。

### **数据查询**  

&emsp;&emsp;数据查询的步骤：

* 为需要访问数据的表格单独配置一个视图，并对视图进行筛选与选定可访问的字段，例如：Contacts 表的 OpenAccess

* 将该视图设置为“提供为 API”

* 为该视图配置功能权限，有关如何配置功能权限，参见 [功能权限](../Chapter05/FunctionRight.html)

* 获取动态数据请求的访问令牌，有关如何获得访问令牌，参见 [公开应用配置](Config.html)

* 确定按照以下 WebAPI 访问：

```xml
http(s)://域名/api/Contacts/OpenAccess?OpenID=访问令牌
```

* 可以采用如下代码获得数据：

```javascript
$(document).ready(function(){
    $.getJSON('http(s)://域名/api/Contacts/OpenAccess?OpenID=访问令牌',
        function(data) {
            if(data.ErrorCode) {
                alert(data.ErrorMessage);
            } else {
                console.log(data.TableName);
                console.log(data.ViewName);
                console.log(data.PagingSetting);
                console.log(data.Rows);
            }
        });
})
```

&emsp;&emsp;数据查询分两种方式：

* 多记录查询

&emsp;&emsp;一次查询出若干条数据，采用的 WebAPI 如上例所示，但还可以有如下参数可选：

```html
PageIndex:  检索页码
PageSize:   检索页大小
GetValueMode: 检索值方式
```

&emsp;&emsp;GetValueMode 有如下取值，默认为 ValueAndText：

```C#
        /// <summary>
        /// 包括数据存储值与显示文本。
        /// </summary>
        ValueAndText,
        /// <summary>
        /// 只包括数据存储值。
        /// </summary>
        OnlyValue,
        /// <summary>
        /// 只包括数据显示文本。
        /// </summary>
        OnlyText,
        /// <summary>
        /// 包括数据存储值与 NULL 显示文本。
        /// </summary>
        ValueAndNullText,
        /// <summary>
        /// 包括 NULL 数据存储值与显示文本。
        /// </summary>
        NullValueAndText,
        /// <summary>
        /// 当值与文本不同时包括值与文本，否则只包括值。
        /// </summary>
        ValueAndTextWhenNotEqual,
```

&emsp;&emsp;上述参数只需要在 WebAPI 的 URL 后跟上地址栏参数方式，例如：

```html
http(s)://域名/api/Contacts/OpenAccess?OpenID=访问令牌&PageIndex=3&GetValueMode=ValueAndTextWhenNotEqual
```

&emsp;&emsp;多记录查询获得的数据格式类似如下：

<img src="Image/2020072901.png"></img>

图 1 多记录查询获得数据格式

&emsp;&emsp;其中除了 ID 字段外，其他字段由一个两元素数组构成：[存储值,显示文本]。如果给定参数 GetValueMode 为 ValueAndTextWhenNotEqual，则字段数据只有在存储值与显示文本不一致的情况下，才既给出存储值，又给出显示文本，否则只给出存储值。GetValueMode 采用多种取值方式，目的在于节约网络传输的数据量，因为大多数第三方应用都是通过网络跨域访问系统数据表格中的数据的。

* 单记录查询

&emsp;&emsp;如果一次查询一条数据，则按照如下的 WebAPI 来获取：

```xml
http(s)://域名/api/Contacts/OpenAccess(3)?OpenID=访问令牌
```

&emsp;&emsp;上述 WebAPI 相比较于多记录访问的，在访问地址后面用小括号界定出要查询的单条数据 ID 值。对于单记录查询，第三方应用必需明确知道查询记录的主码 ID 值。

&emsp;&emsp;单记录查询获得的数据类似下图所示：

<img src="Image/2020072902.png"></img>

图 2 单记录查询获得数据格式

---
&emsp; &copy; eIvy Framework 2019.
