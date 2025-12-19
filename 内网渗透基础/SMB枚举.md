1.内网是公司大楼的话，SMB就是大楼里的一个公共图书馆（共享文件服务器)。枚举SMB就是列出目标电脑上的所有的共享书架（SMB共享）和有借书卡的人（用户列表）
2.用到的工具是：`smbclient`：可以查看和连接共享
            smbclient -L //<target_ip>
            `enum4linux`：会尝试收集用户、组、密码策略等各种信息
            enum4linux -a <target_ip>
3.过程：
    3.1匿名访问smbclient -L（共享） //<target_ip> -**N**（匿名）这里是匿名访问单个文件
    ![[Pasted image 20251206230100.png]]
    使用enum4linux进行全面枚举
    enum4linux -a 192.168.202.183 > enum_results.txt（内容保存到这个文件看
    在输出中发现用户名和密码，尝试用发现的用户名连接共享
    # 例如使用 msfadmin 用户连接 tmp 共享
     smbclient //192.168.202.183/tmp -U msfadmin
     尝试使用空密码：smbclient //192.168.202.183/tmp -U ""
     然后再探索敏感文件
    3.2弱口令测试
