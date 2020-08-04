# [返回目录](../README.html)

## [技术参考](Index.html) - [模板表达式](Expression.html) - 常见属性表达式

&emsp;&emsp;下表列出了常见属性表达式。

|属性|动态数据控件属性表达式|页面属性表达式|命令按钮属性表达式|
|---|---|---|---|
|SecurityContext.LoginUserMapping.EntityID<br/>获得当前登录用户的对应实体ID|(@SecurityContext.LoginUserMapping.EntityID)|(@SecurityContext.LoginUserMapping.EntityID)|SecurityContext.LoginUserMapping.EntityID|
|SecurityContext.LoginUser.Identity.Name<br/>获得当前登录用户名|(@SecurityContext.LoginUser.Identity.Name)|(@SecurityContext.LoginUser.Identity.Name)|SecurityContext.LoginUser.Identity.Name|
|SecurityContext.LoginUserDisplayName<br/>获得当前登录用户的显示名称|(@SecurityContext.LoginUserDisplayName)|(@SecurityContext.LoginUserDisplayName)|SecurityContext.LoginUserDisplayName|
|SecurityContext.LoginRole<br/>获得当前登录用户的角色（一次装载一个角色）|(@SecurityContext.LoginRole)|(@SecurityContext.LoginRole)|SecurityContext.LoginRole|
|Now<br/>获得当前服务器时间|(@Now)|(@Now)|Now|
|Now.Year<br/>获得当前服务器时间的年份|(@Now.Year)|(@Now.Year)|Now.Year|
|Now.Month<br/>获得当前服务器时间的月份|(@Now.Month)|(@Now.Month)|Now.Month|
|Now.Day<br/>获得当前服务器时间的日期|(@Now.Day)|(@Now.Day)|Now.Day|
|Now.Date<br/>获得当前服务器时间的日期部分|(@Now.Date)|(@Now.Date)|Now.Date|
|CssClass<br/>获得类样式|(@CssClass)|(@CssClass)|CssClass|
|Style<br/>内联样式|(@Style)|(@Style)|Style|
|Visible<br/>可见性|(@Visible)|(@Visible)|Visible|
|UserProfile.LastLoginRole<br/>当前登录用户最后一次登录的角色|(@UserProfile.LastLoginRole)|(@UserProfile.LastLoginRole)|UserProfile.LastLoginRole|
|ForceChangingPassword<br/>当前登录用户是否要强制修改密码|(@ForceChangingPassword)|(@ForceChangingPassword)|ForceChangingPassword|
|EncodedRawUrl<br/>当前页面经过编码的Url|(@Page.EncodedRawUrl)|(@EncodedRawUrl)|Page.EncodedRawUrl|
|Request[XX]<br/>当前页面地址栏参数值|(@Request[XX])|(@Request[XX])|Request[XX]|
|PreSqlScriptOutputParameters<br/>前置脚本输出参数|N/A|N/A|PreSqlScriptOutputParameters[XX].Value|
|PostSqlScriptOutputParameters<br/>后置脚本输出参数|N/A|N/A|PostSqlScriptOutputParameters[XX].Value|

---
&emsp; &copy; eIvy Framework 2019.
