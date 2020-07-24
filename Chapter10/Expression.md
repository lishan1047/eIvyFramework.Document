# [返回目录](../README.html)

## [技术参考](Index.html) - 模板表达式

&emsp;&emsp;模板表达式：在页面、视图、页块、母版页的模板中，可以用来进行特定计算的表达式。主要有：属性表达式、字段引用表达式。而属性表达式又可分为页面属性表达式、动态数据控件属性表达式、命令按钮控件属性表达式、数据库脚本参数表达式。  

### **页面属性表达式**  

&emsp;&emsp;页面属性表达式可以写在任何模板中，其表达式语法格式如下：

```xml
(@属性路径)
```

* 一对圆括号是表达式界定符。  

* 属性路径是指从页面对象出发不断用属性访问符号 . 递推出来的一串属性，例如：@User.Identity.Name，由于 User 是页面对象的属性，Identity 又是 User 对象的属性，Name 则是 Identity 对象的属性，因而通过这种属性路径最终得到的属性是 Name，即登录用户的名称。常见的页面属性路径参见 [常见属性表达式](CommonProperty.html)。

&emsp;&emsp;下面的例子向我们展示了从页面属性 Now 中获得当前年份。

```xml
<p>
Copyright (@Now.Year)
</p>
```

&emsp;&emsp;如果属性表达式写在了 XML 标签属性中，且标签属性就是整个属性表达式，则表达式界定符-圆括号可以省略不写。下面例子通过页面的 Title 属性将命令按钮的文本设置成了当前页面的标题。

```xml
<eIvy:COMMANDBUTTON Text="@Title" ...>
```

&emsp;&emsp;如果属性是集合类型，或者是索引器，那么可以用一对中括号 + 索引号，或者中括号 + 索引键获取特定属性值。下面的例子就是利用页面索引属性 Request 将当前页面地址栏参数 ClassID 作为跳转页面的地址栏参数。

```xml
<a href="~/Students/List?ClassID=(@Request[ClassID])">班级名单</a>
```

### **动态数据控件属性表达式**  

&emsp;&emsp;这个和页面属性表达式语法完全一致，只是：用在视图模板中；且属性路径是从动态数据控件出发的。下面例子展示了在视图模板中将动态数据控件的 CssClass 属性作为表格的 class。

```xml
<table class="@CssClass">
...
</table>
```

### **命令按钮控件属性表达式**  

&emsp;&emsp;其语法格式如下：

```xml
{this.属性路径.@参数名}
```

* 属性路径是从命令按钮控件出发的。

* this 代表了命令按钮，可以省略。

* 只能写在命令按钮的属性配置中。

* 参数名是数据库脚本的传入参数，如果不是配置在前置与后置数据库脚本参数属性（PreSqlScriptParameters、PostSqlScriptParameters），则该参数名被忽略，但不能省略。

&emsp;&emsp;下面的例子向我们展示了如何利用命令按钮控件属性 this.SecurityContext.LoginUserMapping.EntityID 为命令按钮的前置数据库脚本参数 @EntityID 传递了登录用户的映射实体ID（关于映射实体参见 [账号管理](../Chapter05/Account.html)）。

```xml
<eIvy:COMMANDBUTTON
    ...
    PreSqlScriptParameters="{this.SecurityContext.LoginUserMapping.EntityID.@EntityID}"
/>
```

&emsp;&emsp;下面的例子则通过命令按钮控件属性 PreSqlScriptOutputParameters 获得前置数据库脚本执行后传出参数 @ID 的值，并作为页面跳转的地址栏参数。

```xml
<eIvy:COMMANDBUTTON
    ...
    NavigateUrl="~/Students/DispForm?ID={this.PreSqlScriptOutputParameters[ID].@ID}"
/>
```

### **字段引用表达式**  

&emsp;&emsp;字段引用表达式用于为字段值在模板中提供占位符。其语法如下：

```xml
{@字段名,选项}
```

* 一对花括号为表达式界定符。

* 只能用在 List 型动态数据控件视图模板的 eIvy:ROWREPEATOR 标签之内，Form 型视图模板则可以是任何位置，不能用于页面、页块、母版页模板中。

* 选项一般情况下可以省略，但可以写 TEXT 表示输出字段的显示文本。

&emsp;&emsp;下面的例子展示了利用字段 ID 值作为导航地址栏参数。

```xml
...
<eIvy:ROWREPEATOR>
    ...
    <a href="~/Students/DispForm?ID={@ID}">详情</a>
    ...
</eIvy:ROWREPEATOR>
...
```

---
&emsp; &copy; eIvy Framework 2019.
