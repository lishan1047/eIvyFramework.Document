# 传参  
## 1、筛选获取参数  
  <eIvy:Parameters>  
    <eIvy:QueryStringParameter xmlns:eIvy="http://www.eIvy.com" Name="ID（对这个参数的命名）" DataType="Int" QueryStringName="TID（上个页面传过来的参数名）" />--上一个页面传过来的参数  
    <eIvy:ControlPropertyParameter xmlns:eIvy="http://www.eIvy.com" Name="EntityID（名字任取，一般为这个）" DataType="Int"   ControlPropertyName="SecurityContext.LoginUserMapping.EntityID" />--获取@EntityID  
    <eIvy:ControlPropertyParameter xmlns:eIvy="http://www.eIvy.com" Name="LoginRole（名字任取，一般为这个）" DataType="String"   ControlPropertyName="SecurityContext.LoginRole" />--获取当前登录角色@LoginRole  
  </eIvy:Parameters>  
  
## 2、工具栏传参  
 <eIvy:TOOLBAR ToolBarItemParameterSettings="SouvenirIssued-PreSqlScriptParameters={SecurityContext.LoginUserMapping.EntityID.@EntityID}  {PeriodicalThesis.@TableName}" />  

其中SouvenirIssued为工具栏名，PreSqlScriptParameters表示前置脚本，若是后置脚本用PostSqlScriptParameters  

## 3、命令按钮传参  
<eIvy:COMMANDBUTTON xmlns:eIvy="http://www.eIvy.com" ID="BTN01" Text="设置主题智库" CommandName="FormSave"（脚本执行的命令，此处为保存）   CssClass="BTN1 btn btn-large btn-default"（按钮的样式） PostSqlScriptName="selectTheme" （后置脚本名称，若为前置脚本则为PreSqlScriptName）  PostSqlScriptParameters="{this.SecurityContext.LoginUserMapping.EntityID.@EntityID}（传入当前ID）{Page.SecurityContext.LoginRole.@Role}(传入当前登录者的角色){@ID}" DynamicDataControlID="DFC1" SuccessMessage="设置成功"（脚本执行后的弹出框）></eIvy:COMMANDBUTTON>  

## 4、点开某个页面自动执行一段JS脚本    
<script type="text/javascript">  
function invokeRequirementsTest() {  
 var tableStyle = document.getElementById("table");  
var thStyle = tableStyle.getElementsByTagName("th");  
 thStyle[3].style.width = "10px";  
   }  
</script>（所要执行的JS脚本）  
  <eIvy:SCRIPTLOADER PageStartupScript="invokeRequirementsTest();"></eIvy:SCRIPTLOADER>（页面调用）  
  
## 5、筛选框和命令按钮放一行  
<div style="height:32px;">  
    <div style="position:absolute;top:50px;left:10px;">  
      <eIvy:COMMANDBUTTON xmlns:eIvy="http://www.eIvy.com" ID="BTN01" Text="保存" CommandName="ListEditSave" CssClass="BTN1 btn btn-large   btn-default" PostSqlScriptName="updateThinktankAdminID" PostSqlScriptParameters="{this.SecurityContext.LoginUserMapping.EntityID.@EntityID}  {@ID}" DynamicDataControlID="DDC1" SuccessMessage="保存成功！"></eIvy:COMMANDBUTTON>  
    </div>  
    <div style="position:absolute;top:50px;left:85px;">  
      <eIvy:SIMPLEFILTER xmlns:eIvy="http://www.eIvy.com" ID="F1" FilterableControlID="DDC1" Visible="true" />  
    </div>  
  </div>  

### 为添加的按钮设置立体样式  
  <style>.BTN1{  
box-shadow: 2px 2px  12px  #000000;（左右距离、上下距离、阴影过度、阴影颜色）  
}</style>  
![按钮示例](C:\Users\yf\Desktop\1.PNG)