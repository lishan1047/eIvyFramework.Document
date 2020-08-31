# [返回目录](../../README.html)

## [技术参考](../Index.html) - [页面标签](../Page.html) - DYNAMICCROSSTABLE

&emsp;&emsp;动态交叉表（Dynamic Cross Table）：继承了 DYNAMICLIST 所有特性，但是以交叉表的方式呈现数据，而且与 DYNAMICPIVOT 相比，具有数据编辑的功能。  

### 标签语法

```xml
<eIvy:DYNAMICCROSSTABLE/>
```

### **交叉表呈现方式**

&emsp;&emsp;交叉表是将数据库中二维表的数据转置为多维表的形式，例如：我们存在一张数据库表格，具备字段 School 学院、Major 专业、Class 班级、StudentCount 学生人数  

|School|Major|Class|Gender|StudentCount|
|---|---|---|---|---|
|经济管理|会计学|1班|男|11|
|经济管理|会计学|1班|女|21|
|经济管理|会计学|2班|男|15|
|经济管理|会计学|2班|女|16|
|经济管理|人力资源|1班|男|14|
|经济管理|人力资源|1班|女|17|
|经济管理|人力资源|2班|男|13|
|经济管理|人力资源|2班|女|18|
|经济管理|人力资源|3班|男|16|
|经济管理|人力资源|3班|女|16|
|软件|软件工程|1班|男|20|
|软件|软件工程|1班|女|11|
|软件|软件工程|2班|男|21|
|软件|软件工程|2班|女|9|
|软件|软件测试|1班|男|16|
|软件|软件测试|1班|女|17|

&emsp;&emsp;我们希望以下面的交叉表方式呈现：  

<img src="../Image/2020072202.png"></img>

图 1 交叉表呈现方式

&emsp;&emsp;由于交叉表与列表的呈现格式完全不同，因而视图模板在交叉表中会被忽略，因此当你尝试去编辑视图模板获得不一样的界面效果，这一操作是无效的。  

### **基本配置**

&emsp;&emsp;通过 eIvy:DYNAMICCROSSTABLE 标签可以实现将数据库中原始二维表数据以这种交叉表方式呈现。这里有三个关键概念：行字段（Row Fields）、列字段（Column Fields）、单元格字段（Cell Fields），行字段就是上述交叉表中的 School、Major、Class，列字段则是 Gender，单元格字段是 StudentCount。我们可以通过以下标签来配置：  

```xml
<eIvy:DYNAMICCROSSTABLE
    RowFieldNames="School,Major,Class"
    ColumnFieldNames="Gender"
    CellFieldNames="StudentCount"
/>
```

### **数据单元格的上下文菜单命令**

&emsp;&emsp;我们还可以通过属性 CellCommandButtonSetting 为单元格中的数据项目提供操作命令，例如下面代码提供了删除命令按钮，图2 显示了配置命令后，当我们鼠标移入到单元格数据项目上，会出现 ... 符号，点击之后会出现命令操作菜单。

```xml
<eIvy:DYNAMICCROSSTABLE
    RowFieldNames="School,Major,Class"
    ColumnFieldNames="Gender"
    CellFieldNames="StudentCount"
    CellCommandButtonSetting="[{CommandName:'ListDelete',Text:'删除',CssClass:'fa fa-close',Prompt:'删除数据后无法恢复，是否执行删除？'}]"
/>
```

&emsp;&emsp;属性 CellCommandButtonSetting 采用 JSON 数组格式，每个{}表达的对象为一个命令按钮配置属性（有关命令按钮的属性参见[命令按钮](../Index.html)），而且又因为交叉表继承自 DynamicList，因此适用于 DynamicList 的所有命令都可以用在单元格的数据项上。

&emsp;&emsp;另外，一些小技巧，可以帮大忙。例如：我们期望单元格内容的数据可以导航到另外的页面，通过为其配置命令菜单，可以实现，代码如下：  

```xml
<eIvy:DYNAMICCROSSTABLE
    ...
    CellCommandButtonSetting="[{CommandName:'ListDelete',Text:'删除',CssClass:'fa fa-close',Prompt:'删除数据后无法恢复，是否执行删除？'},{CommandName:'Empty',Text:' 查看详情',CssClass:'fa fa-info',NavigateUrl:'~/Students/DispForm?ID={@ID}'}]"
    ...
/>
```

&emsp;&emsp;上述代码中通过在属性 CellCommandButtonSetting 中增加了一个命令的方式，为单元格数据项添加了一个导航到详情页的操作菜单。这种配置其实如同是在视图模板中为命令按钮配置属性一样，大家也注意到了 {@ID} 这种字段引用占位符的用法和视图模板中完全一致。

### **保持数据编辑状态**

&emsp;&emsp;实际上由于交叉表是继承自 DynamicList，因而 DynamicList 的很多功能和配置属性对于交叉表来说都是有效的，例如：AlwaysEditMode 属性可以让交叉表的单元格数据项处于编辑状态，由此可以通过交叉表的形式来编辑数据。下面的代码使得 StudentCount 字段在交叉表单元格中处于编辑状态，并配合一个保存命令按钮用于保存编辑的数据。

```xml
<eIvy:DYNAMICCROSSTABLE ID="DCT1"
    RowFieldNames="School,Major,Class"
    ColumnFieldNames="Gender"
    CellFieldNames="StudentCount"
    AlwaysEditMode="True"
/>
<eIvy:COMMANDBUTTON DynamicDataControlID="DCT1" CommandName="ListEditSave" Text="保存" CssClass="btn btn-primary"/>
```

<img src="../Image/2020072301.png"></img>

图 2 交叉表显示效果  

&emsp;&emsp;我们注意到学院、专业、班级、性别这些行列字段并没有因为 AlwaysEditMode 属性被设置为 True，而处于编辑状态，即使在视图中将这些字段设置为可修改和新增，仍然如此。因此，要意识到一旦字段被当作行列字段，就失去了编辑功能。 

### **数据分页处理**

&emsp;&emsp;对于交叉表同样可以采用分页控件进行数据分页呈现，但合计数据只会根据当前页的数据进行汇总。因此，一般情况下我们为了能够显示并合计完整的数据，需要将 DefaultPageSize 属性设置到足够大，以便于交叉表提取到完整的数据。  

### **更多外观设置**

&emsp;&emsp;交叉表还提供了一组高度定制化的样式与外观属性，通过下图大家可以了解这些属性所设置交叉表的对应关系。  

<img src="../Image/2020072302.png"></img>

图 3 交叉表的样式与外观属性  

&emsp;&emsp;通过上图大家还观察到了单元格中放置了两个输入框，其实这是在 CellFieldNames 属性设置了两个字段：StudentCount 学生人数、MajorTransCount 转专业人数。这样交叉表会自动将这两个字段放置在单元格中，并且按照对应位置汇总相关数据。同时还可以通过设置 CellItemCssClassSetting 属性分别为这两个字段设置不同的样式，以区分输入框中填写的内容是属于哪个字段的，不过该属性设置要遵循 JSON 数组格式，以 {FieldName:'xx',CssClass:'yy'} 作为一个字段的配置。另外，我们还可以用 CellItemSeparator 属性和 SumItemSeparator 属性分别为单元格内多个字段，以及汇总单元格内的多个字段添加分隔符，在外观上加以区分。  

&emsp;&emsp;完整的代码如下：  

```xml
  <style>
  .MajorTrans input {
    background-color: #f3e8cb;
  }
  .ColSumCell {
    background-color: #a6e2d4;
  }
  .RowSumCell {
    background-color: #d4c1c1;
  }
  .ColHeaderCell {
    background-color: red;
  }
  .RowHeaderCell {
    background-color: green;
  }
  .RowColSumCell {
    background-color: yellow;
  }
  .CrossHeader {
    background-color: blue;
  }
  .Cell {
    background-color: gray;
  }
  </style>
  <eIvy:DYNAMICCROSSTABLE
    ID="DDC1" CssClass="SimpleListStyle form-inline table table-striped table-bordered table-hover"
    RowFieldNames="School,Major,Class"
    ColumnFieldNames="Gender"
    CellFieldNames="StudentCount,MajorTransCount"
    AlwaysEditMode="True"
    DefaultPageSize="100"
    CellCommandButtonSetting="[{CommandName:'ListDelete',Text:'删除',CssClass:'fa fa-close',Prompt:'删除数据后无法恢复，是否执行删除？'}]"
    CrossHeaderText="学院专业班级/性别"
    ColumnSumHeaderText="汇总"
    RowSumHeaderText="汇总"
    ColumnSumCssClass="ColSumCell"
    RowSumCssClass="RowSumCell"
    RowColSumCssClass="RowColSumCell"
    ColumnHeaderCssClass="ColHeaderCell"
    RowHeaderCssClass="RowHeaderCell"
    CrossHeaderCssClass="CrossHeader"
    CellCssClass="Cell"
    CellItemCssClassSetting="[{FieldName:'MajorTransCount',CssClass:'MajorTrans'}]"
    CellItemSeparator="／"
    SumItemSeparator="／"
    EnableSummary="True"
/>
<eIvy:COMMANDBUTTON DynamicDataControlID="DCT1" CommandName="ListEditSave" Text="保存" CssClass="btn btn-primary"/>
```

&emsp;&emsp;其实还有一些外观属性配置，可以帮助我们提供更多定制化需求，例如：EmptyText 当没有数据的时候，显示的文本；EmptyDataCssClass 当没有数据的时候表格的样式。  

### **交叉表的汇总**

&emsp;&emsp;大家也注意到了 EnableSummary 属性，通过将这个属性设置为 False 可以关闭汇总功能。  

&emsp;&emsp;该标签还提供了汇总客户端回调方法，即通过配置 SumClientCallbak 属性可以实现交叉表在生成或者重新计算汇总值的过程中执行一段自己 JS 代码的逻辑。例如：我们想检验一下自己输入的数据汇总结果是否超出某个范围，通过下面的代码可以实现：

```xml
<eIvy:DYNAMICCROSSTABLE
    ID="DDC1" CssClass="SimpleListStyle form-inline table table-striped table-bordered table-hover"
    RowFieldNames="School,Major,Class"
    ColumnFieldNames="Gender"
    CellFieldNames="StudentCount"
    AlwaysEditMode="True"
    DefaultPageSize="100"
    CellCommandButtonSetting="[{CommandName:'ListDelete',Text:'删除',CssClass:'fa fa-close',Prompt:'删除数据后无法恢复，是否执行删除？'}]"
    CrossHeaderText="学院专业班级/性别"
    SumClientCallback="CheckStudentCount"
/>
<script>
function CheckStudentCount(param) {
  if(param.fname == "StudentCount" && param.dir == "cross") {
    if(param.val <= 0) {
      alert("输入学生总人数不能小于等于 0！");
    }
  }
}
</script>
```

&emsp;&emsp;关键在于回调方法参数为一个 JSON 对象：

```josn
{
    fname: 'xx'                 // 汇总字段名
    dir: 'column'|'row'|'cross' // 汇总方向：column 纵列方向、row 横行方向、cross 交叉汇总
    val: 80                     // 汇总值
}
```

&emsp;&emsp;由于交叉表在每生成一个汇总数据的时候就会调用一次由 SumClientCallback 指定的 JS 方法，所以我们要搞清楚是针对什么汇总数据进行检验的，上述例子是针对 StudentCount 字段的交叉汇总值进行检验，所以通过 param.fname == "StudentCount" && param.dir == "cross" 这个条件式来判断的。

&emsp;&emsp;当我们单元格字段处于编辑状态，一旦输入值发生变化之后，也会引起重新计算该汇总字段的汇总数据，从而也就引发回调方法。因此，该回调方法也能够响应汇总数据的重新计算过程。

&emsp;&emsp;另外，SumClientCallback 这个属性设置在 EnableSummary 属性设置为 True 的时候才会有效。

### **数据行的选择**

&emsp;&emsp;因为交叉表是继承自 DynamicList，因此我们可以利用 SelectMode 属性改变数据行的选择行为。如：

```xml
<eIvy:DYNAMICCROSSTABLE ID="DDC1"
  SelectMode="Multiple"
  ...
/>
  <eIvy:SELECTOR SelectableControlID="DDC1"></eIvy:SELECTOR>
```

&emsp;&emsp;这样可以将每条数据的选择框呈现到界面上，但是由于交叉行列重新布局了原有的数据行，因此一个单元格内可能存在多行数据的情况。SelectMode 属性在交叉表中的默认值被重置为 None，因此需要显示地加上该属性的配置。

### **数据新增功能**

&emsp;&emsp;同样的，由于交叉表继承自 DynamicList，因此可以启用交叉表的新增数据功能。例如：通过配置

---
&emsp; &copy; eIvy Framework 2019.
