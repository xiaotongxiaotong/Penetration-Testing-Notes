DVWA靶场渗透测试报告

| 项目   | 详情                                                              |
| ---- | --------------------------------------------------------------- |
| 测试目标 | DVWA (Damn Vulnerable Web Application) - Security Level: Medium |
| 测试时间 | 25.11.18                                                        |
| 测试人员 |                                                                 |
| 测试范围 | web应用安全测试                                                       |
| 漏洞统计 |                                                                 |
1.信息收集阶段
1.1端口扫描（nmap)
命令：nmap -sV 192.168.100.101
![[Pasted image 20251118213240.png]]
1.2目录扫描
命令：gobuster dir -u http://192.168.100.101/ -w /usr/share/wordlists/dirb/common.txt -x php,html,txt
![[Pasted image 20251118215005.png]]/php.ini公开访问这会泄露服务器配置
![[Pasted image 20251118220149.png]]
allow_url_fopen = on      # 允许远程文件操作
allow_url_include = on    # 允许远程文件包含 - 极度危险！
`allow_url_include = on` + ✅ 已知应用类型(DVWA) = 完美攻击条件
/config和/database可能包含敏感数据，但该网站禁止访问
2.漏洞测试（靶场中级）
2.1漏洞类型：SQL注入
位置：http://192.168.100.101/vulnerabilities/sqli/
风险等级：
复现步骤：
![[Pasted image 20251119132502.png]]
' or '1'='1 SQL注入漏洞存在，版本名和数据库名如上图所示
有两张表
id=1 and (select count(table_name) from information_schema.tables where table_schema=database())=2 --
**影响**：攻击者可窃取用户cookie、重定向用户、执行恶意操作
**修复建议**：对用户输入进行HTML编码，实施内容安全策略
2.2漏洞类型：xss跨站点脚本
位置：http://192.168.100.101/vulnerabilities/xss_r/
风险等级：
复现步骤：
xss (reflect):双写绕过
![[Pasted image 20251119224656.png]]
影响：
建议：
XSS（store）
不太会，先空着
2.3漏洞类型：file upload
位置：http://192.168.100.101/vulnerabilities/upload/
风险等级：
复现步骤：
![[Pasted image 20251121211310.png]]
在文档中创建php文件上传
![[Pasted image 20251121211408.png]]
中级
上传的必须是image/jpeg或png文件
![[Pasted image 20251121214056.png]]
上传php文件
将content-Type内容改成image/png
j将exploit.PHP后缀改成.png
影响：
建议：
