cross-site request forgery
1.一种web安全漏洞，允许攻击者诱导用户执行他们不打算执行的操作。它允许攻击者部分规避同源策略，该策略旨在防止不同网站相互干扰。
2.满足此攻击的三个条件：
	相关操作：应用程序中存在攻击者有理由诱发的操作。这可能是特权操作（例如修改其他用户的权限）或对特定于用户的数据的任何操作（例如更改用户自己的密码）。
	 基于 Cookie 的会话处理： 执行该操作涉及发出一个或多个 HTTP 请求，并且应用程序仅依靠会话 cookie 来识别发出请求的用户。没有其他机制来跟踪会话或验证用户请求。
	 没有不可预测的请求参数：执行该操作的请求不包含攻击者无法确定或猜测其值的任何参数。例如，当导致用户更改其密码时，如果攻击者需要知道现有密码的值，则该函数不会受到攻击。
3.如何构建CSRF攻击
构建 CSRF 漏洞的最简单方法是使用 Burp Suite Professional 内置的 [CSRF PoC 生成器](https://portswigger.net/burp/documentation/desktop/tools/engagement-tools/generate-csrf-poc)

 