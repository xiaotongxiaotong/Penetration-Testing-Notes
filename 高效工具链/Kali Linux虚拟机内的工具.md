可使用不同的单词列表（common.txt, big.txt, directory-list-2.3-medium.txt）
1.gobuster:一个用 Go 语言编写的快速目录/文件、DNS 和 VHost 扫描工具。它常用于渗透测试中发现隐藏的 Web 路径。
![[Pasted image 20251114221532.png]]
2.dirb:一个基于字典的 Web 目录扫描工具，使用预定义的单词列表来暴力破解目录和文件。它通常预装在 Kali Linux 中。
![[Pasted image 20251114221543.png]]3.扫描结果注意
高危风险文件/目录
=================

1. 配置文件目录
   - 路径示例: /config, /configuration, /settings
   - 可疑内容:
     * 数据库连接信息（用户名、密码、主机）
     * API密钥和访问令牌
     * 加密密钥和盐值
     * 系统路径信息
   - 风险等级: 🔴 极高
   - 检查方法: 直接访问目录查看文件列表

2. 数据库相关目录
   - 路径示例: /database, /db, /sql, /backup
   - 可疑内容:
     * 数据库备份文件（.sql, .bak, .dump）
     * 数据库导出文件
     * 数据库连接配置文件
   - 风险等级: 🔴 极高
   - 文件扩展名: .sql, .bak, .dump, .mdb, .db

3. 备份文件
   - 常见模式:
     * filename.bak, filename.old, filename.backup
     * filename.php.bak, config.php.old
   - 可疑内容:
     * 源代码泄露
     * 配置文件包含敏感信息
     * 数据库连接字符串
   - 风险等级: 🔴 极高

🟡 中危风险文件/目录
=================

4. PHP信息文件
   - 文件名: phpinfo.php, info.php, test.php
   - 泄露信息:
     * PHP版本和配置
     * 服务器路径结构
     * 环境变量
     * 已安装的扩展
   - 风险等级: 🟡 中危
   - 利用价值: 信息收集，帮助规划进一步攻击

5. 安装/设置文件
   - 文件名: setup.php, install.php, admin.php
   - 风险:
     * 可能允许重装系统
     * 可能重置管理员密码
     * 可能暴露系统配置
   - 风险等级: 🟡 中危

6. 日志文件
   - 路径示例: /logs, /var/log, /tmp/logs
   - 可疑内容:
     * 访问日志可能包含敏感URL
     * 错误日志可能泄露系统信息
     * 可能包含调试信息
   - 风险等级: 🟡 中危

🟠 信息收集价值文件
=================

7. robots.txt
   - 路径: /robots.txt
   - 价值:
     * 列出管理员不想被搜索引擎索引的目录
     * 可能指向管理后台、敏感目录
   - 状态码: 200
   - 检查内容: 查看Disallow指令

8. 版本信息文件
   - 文件名: readme.txt, version.txt, changelog.txt
   - 价值:
     * 泄露软件版本信息
     * 帮助寻找已知漏洞
   - 风险等级: 🟠 低危

9. git/SVN目录
   - 路径: /.git/, /.svn/, /.hg/
   - 风险:
     * 可能下载整个源代码
     * 包含提交历史、配置文件
   - 风险等级: 🔴 极高（如果可访问）