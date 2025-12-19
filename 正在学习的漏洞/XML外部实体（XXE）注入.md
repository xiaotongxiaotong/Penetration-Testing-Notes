1.XML 外部实体注入（也称为 XXE）是一种 Web 安全漏洞，允许攻击者干扰应用程序对 XML 数据的处理。它通常允许攻击者查看应用程序服务器文件系统上的文件，并与应用程序本身可以访问的任何后端或外部系统进行交互
2.在某些情况下，攻击者可以利用 XXE 漏洞执行服务器端请求伪造 （SSRF） 攻击，从而升级 XXE 攻击以破坏底层服务器或其他后端基础设施
3.XXE攻击类型
    (1）利用XXE检索文件，其中定义了一个包含d的外部实体，并在应用程序的响应中返回
    （2）利用XXE进行SSRF攻击，其中外部实体是根据后端系统的URL定义的
      （3）利用盲XXE带外数据泄露，其中敏感数据从应用程序服务器传输到攻击者控制的系统。
      （4）[利用盲 XXE 通过错误消息检索数据](https://portswigger.net/web-security/xxe/blind#exploiting-blind-xxe-to-retrieve-data-via-error-messages) ，攻击者可以触发包含敏感数据的解析错误消息。
4.DTD：文档类型定义，是一种用于定义XML文档结构和合法元素的方式。你可以把它看作是XML文档的’蓝图‘或模式。它规定了文档中有哪些元素，这些元素的属性，他们的嵌套关系以及可以使用的实体
5.实验：使用外部实体检索实验
在XML声明和stockcheck之间插入外部实体
`!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>`
再将productid编号替换为对外部实体引用：&xxe(显示调取信息的地方)
