# <div style="height:40px"><div style="float:left">eIvy Framework 开发者博客</div> <div style="float:right"><img width="80" height="40" src="../../Logo.png"></img></div></div>

<h3 align="right">作者：曹怡楠</h3>

我们这次将要讲述的是框架中参数的获取方法，主要有以下三种：  
1. 从地址栏中获取  
2. 从框架中获取登录ID@EntityID,登录角色@LoginRole
3. 从字段中获取

* 1. 地址栏
    > 1. 筛选视图获取参数  
    ```
    <eIvy:Parameters>
    <eIvy:QueryStringParameter xmlns:eIvy="http://www.eIvy.com" Name="(变量名)" DataType="(变量类型)" QueryStringName="(变量名)" />
    </eIvy:Parameters>
    ```
    > 2. 脚本获取参数  
    ```
    PreSqlScriptParameters="{Page.Request.QueryString[@(地址栏中变量名)].@(脚本中变量名)}"
    (后置脚本同上)
    ```
* 2. 框架
    > 1. 筛选视图获取@EntityID,@LoginRole
    ```
    <eIvy:Parameters>
    <eIvy:ControlPropertyParameter xmlns:eIvy="http://www.eIvy.com" Name="EntityID" DataType="Int" ControlPropertyName="SecurityContext.LoginUserMapping.EntityID" />
    <eIvy:ControlPropertyParameter xmlns:eIvy="http://www.eIvy.com" Name="LoginRole" DataType="String" ControlPropertyName="SecurityContext.LoginRole" />
    </eIvy:Parameters>
    ```
    > 2. 脚本获取@EntityID,@LoginRole
    ```
    PreSqlScriptParameters="{Page.SecurityContext.LoginUserMapping.EntityID.@EntityID}{Page.SecurityContext.LoginRole.@LoginRole}"
    (后置脚本同上)
    ```
* 3. 字段
    > 脚本获取参数
    ```
    PreSqlScriptParameters="{{@(字段名)}.@(参数名)}
    (后置脚本同上)
    ```


<img src="../Photo/Logo.png" position="center" />

---
&emsp; &copy; eIvy Framework 2019.
