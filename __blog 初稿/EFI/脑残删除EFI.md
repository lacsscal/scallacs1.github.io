#起
感觉双系统用起来不太灵活，准备把Linux系统删除后装成虚拟机
**采用直接格式化对应硬盘，之后删除引导项的方式写在Linux，中途手抖误删(格式化)EFI引导区**

*注：EFI是可扩展固件接口（Extensible Firmware Interface）*

在Windows系统中，除了平时可以看到的自己分的磁盘区还有系统保留的EFI和MSR分区，一般不被显示
（配图4）

	esp就是efi系统分区，用于保存系统引导文件;
	msr是微软保留分区， GPT格式磁盘用于安装Win7/8系统都会自动创建该分区。

#承&结
手贱删除后，系统没有引导项
虽然系统正确但是开不了机，需要自行配置EFI分区
在网上查阅相关资料，得出解决方案步骤大致如下

1. 配置对应的U盘启动盘(此前有相关经验使用UltraISO+win10镜像)
2. 在BIOS界面选择开机引导方式为U盘启动
3. 在PE盘中不使用重装系统，用修复系统打开命令行
4. 使用diskpart程序
5. 常见有两套方案解决EFI分区的生问题，这里采用简单的一段命令如下
##
	1. diskpart
	2. list 
	3. select disk *
	4. list partition
	5. creat partition efi size=256
	6. format quick
	7. exit
	cmd主界面上再用
	bcdboot C:\Windows

解决
#坑

配图（0,1,2,3）
开机成功，但是发现 EFI分区位置不再C盘前，位置很怪，开机很慢
而且导致其后的分区无法合并
解决：再次删除EFI区，先对此时空闲的分区进行合并
在后再次进行此前操作 解决。