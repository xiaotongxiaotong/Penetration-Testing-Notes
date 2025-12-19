1.检查网络连通性：ping [主机]
### 扫描功能
###### 主机发现
nmap -sP 192.168.81.0/24
###### 端口发现
nmap ip
namp ip -p 80
nmap ip -p 80,3306
nmap ip -p 1-65535
###### 隐藏扫描
nmap -sS [靶机IP]` （TCP SYN扫描）  
###### 服务发现
nmap -sV [靶机IP]` （版本侦测）  
###### 操作系统版本发现
nmap -O [靶机IP]` （操作系统识别）【虚拟机的话在前面加上sudo】
###### 爆破
nmap --script ssh-brute ip -p端口号（使用 的是默认的密码本）
创建文件 nmap --script ssh-brute --script-args '账号的路径，带有密码文件的路径' ip -p端口号
###### 漏洞扫描（弱）
nmap --script=vuln ip(依靠nmap自带的漏洞)
2.在虚拟机上打开要扫描的网站
eg：
###### 针对Web端口的详细扫描
sudo nmap -p 80 -A 192.168.100.102
###### 或者扫描所有端口
sudo nmap -p- 192.168.100.102
###### 检查HTTP方法
nmap -p 80 --script http-methods 192.168.100.102
###### 检查安全头信息
 nmap -p 80 --script http-security-headers 192.168.100.102
###### 枚举Web目录
nmap -p 80 --script http-enum 192.168.100.102
###### 使用nmap的漏洞扫描脚本
nmap -p 80 --script vuln 192.168.100.102
###### 检查常见Web漏洞
 nmap -p 80 --script http-sql-injection,http-xss-detector 192.168.100.102
 ### **nmap - 网络扫描器**

- **主要功能**：网络发现、端口扫描、服务识别、操作系统检测
    
- **工作层次**：网络层和传输层（TCP/IP层面）
    
- **典型用途**：找出网络上哪些主机在线、开放了哪些端口、运行什么服务
    

### **Burp Suite - Web应用渗透测试工具**

- **主要功能**：拦截、修改和分析HTTP/HTTPS流量，Web漏洞扫描
    
- **工作层次**：应用层（HTTP/HTTPS层面）
    
- **典型用途**：SQL注入、XSS、CSRF等Web漏洞测试