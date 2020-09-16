##目标查找用户列表(可登录的)
 
 	网上查找可用命令为
	cat /etc/passwd | grep -v /sbin/nologin | cut -d : -f 1
	分析：两个管道 三个命令
	查看用户列表文件
	
	而grep为常用文本处理工具之一
	全称为： Global search Regular Expression and Print out the line
	Global search"为全局搜索之意。
	Regular Expression"表示正则表达式。

	man grep
	找到参数-v的意义
	把no match 的标出来
 	
	而查找的 /sbin
	有
	/sbin目录: 主要放置一些系统管理的必备程序
	例如:cfdisk、dhcpcd、dump、e2fsck、fdisk、halt、
	ifconfig、ifup、 ifdown、init、insmod、lilo、
	lsmod、mke2fs、modprobe、quotacheck、reboot、
	rmmod、 runlevel、shutdown等
	如果这是用户和管理员必备的二进制文件，就会放在/bin。如果这是系统管理员必备，但是一般用户根本不会用到的二进制文件，就会放在 /sbin。


	cut 命令从文件的每一行剪切字节、字符和字段并将这些字节、字符和字段写至标准输出。
	如果不指定 File 参数，cut 命令将读取标准输入。必须指定 -b、-c 或 -f 标志之一。

	第一，字节（bytes），用选项-b

	第二，字符（characters），用选项-c

	第三，域（fields），用选项-f