# <div style="height:40px"><div style="float:left">eIvy Framework 使用文档</div> <div style="float:right"><img width="80" height="40" src="Logo.png"></img></div></div>

编写说明：  
（1）需要安装 Visual Studio Code  
（2）为 Visual Studio Code 安装 Markdown PDF，Markdown Theme Kit，markdownlint 扩展插件  
（3）为 Visual Studio Code 首选项配置 setting.json 让 markdown 文件自动转换为 html 文件  
（4）需要安装 Git  
（5）需要具有 GitHub 账号，如果没有需要注册一个账号  
（6）在 Visual Studio Code 中从 https://github.com/lishan1047/eIvyFramework.Document.git 获取 eIvyFramework.Document 库  
注：  
  setting.json 中为 markdown 配置 html 文件自动生成  
  ```json
  {
    "workbench.colorTheme": "Visual Studio Light",
    "git.autofetch": true,
    "markdown-pdf.type": [
        "html"
    ],
    "markdown-pdf.convertOnSave": true,
    "editor.minimap.enabled": false,
    "markdown-pdf.clip.height": 0
  }
  ```

提供 eIvy Framework 的使用手册与教程

## [概述](Chapter01/Index.html)

### &emsp;&emsp;[初衷](Chapter01/Idea.html)

### &emsp;&emsp;[基本思路](Chapter01/BasicSolution.html)

### &emsp;&emsp;[核心概念](Chapter01/CoreConcepts.html)  

## [安装](Chapter02/Index.html)

### &emsp;&emsp;[环境准备](Chapter02/Prepare.html)

### &emsp;&emsp;[不同版本](Chapter02/Version.html)

### &emsp;&emsp;[手动安装](Chapter02/SetupByMannul.html)

### &emsp;&emsp;[安装程序](Chapter02/SetupAuto.html)

## [数据库](Chapter03/Index.html)

### &emsp;&emsp;[数据库基本配置](Chapter03/DatabaseConfig.html)

### &emsp;&emsp;[表格创建](Chapter03/Sec02.html) - 甘

### &emsp;&emsp;[字段创建](Chapter03/Sec03.html)  

### &emsp;&emsp;[数据库脚本创建](Chapter03/Sec04.html) - 朱

### &emsp;&emsp;[存储过程创建](Chapter03/Sec05.html) - 朱

### &emsp;&emsp;[用户自定义函数创建](Chapter03/Sec06.html) - 刘晨宁

### &emsp;&emsp;[消息队列创建](Chapter03/Sec07.html) - 黄强

### &emsp;&emsp;[字段值列表维护](Chapter03/Sec08.html) - 李孟洁

### &emsp;&emsp;[触发器创建](Chapter03/Sec09.html) - 何翀

### &emsp;&emsp;[表格侦听](Chapter03/Sec10.html) - 石智铭

### &emsp;&emsp;[启用版本跟踪](Chapter03/Sec11.html) - 甘

### &emsp;&emsp;[注册验证器](Sec12.html) - 朱

### &emsp;&emsp;[账号激活器](Sec13.html) - 刘晨宁

## [站点](Chapter04/Index.html)

### &emsp;&emsp;[站点基本配置](Chapter04/Sec01.html) - 黄强

### &emsp;&emsp;[母版页创建](Chapter04/Sec02.html) - 李孟洁

### &emsp;&emsp;[数据页面创建](Chapter04/Sec03.html) - 何翀

### &emsp;&emsp;[动作页创建](Chapter04/Sec04.html) - 石智铭

### &emsp;&emsp;[视图创建](Chapter04/Sec05.html) - 甘

### &emsp;&emsp;[页块创建](Chapter04/Sec06.html) - 朱

### &emsp;&emsp;[工具栏创建](Chapter04/Sec07.html) - 刘晨宁

### &emsp;&emsp;[模块创建](Chapter04/Sec08.html) - 黄强

### &emsp;&emsp;[站点脚本](Chapter04/Sec09.html) - 李孟洁

### &emsp;&emsp;[站点主题](Chapter04/Sec10.html) - 何翀

### &emsp;&emsp;[站点搜索](Chapter04/Sec11.html) - 石智铭

## [安全](Chapter05/Index.html)

### &emsp;&emsp;[账号管理](Chapter05/Sec01.html) - 甘

### &emsp;&emsp;[角色管理](Chapter05/Sec02.html) - 朱

### &emsp;&emsp;[功能权限](Chapter05/Sec03.html) - 刘晨宁

### &emsp;&emsp;[密码格式](Chapter05/Sec04.html) - 黄强

### &emsp;&emsp;[CA 登录](Chapter05/Sec05.html)

## [工作流](Chapter05/Index.html)

### &emsp;&emsp;[工作流模型](Chapter06/Sec01.html) - 甘

### &emsp;&emsp;[工作流配置](Chapter06/Sec02.html) - 黄强

### &emsp;&emsp;[工作流设计器](Chapter6/Sec03.html) - 石智铭

## [公开应用](Chapter07/Index.html)

### &emsp;&emsp;[公开应用配置](Chapter07/Sec01.html) - 何翀

### &emsp;&emsp;[公开服务](Chapter07/Sec02.html) - 李孟洁

### &emsp;&emsp;[公开动态数据](Chapter07/Sec03.html) - 石智铭

## [技术参考](Chapter10/Index.html)

### &emsp;&emsp;[字段型](Chapter10/Sec01.html)

### &emsp;&emsp;[视图标签](Chapter10/Sec02.html)

### &emsp;&emsp;[页面标签](Chapter10/Page.html)

### &emsp;&emsp;[筛选子句标签](Chapter10/Sec04.html)

### &emsp;&emsp;[排序子句标签](Chapter10/Sec05.html)

### &emsp;&emsp;[命令参考](Chapter10/Sec06.html)

### &emsp;&emsp;[工具栏配置](Chapter10/Sec07.html)

---
&emsp; &copy; eIvy Framework 2019.
