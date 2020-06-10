##解决VSCode终端输出乱码问题

已知VSCode的终端为PowerShell
默认中文输出是GBK国标码
需要转换为UTF-8

手动修改
每次启动后输入：chcp 65001
比较麻烦

一次性解决：
命令行输入：
	
	notepad $PROFILE

之后会自动创建一个文件，一般为：

	<库>\<文档>\WindowsPowerShell\Microsoft.PowerShell_profile.ps1

在其中添加：

	chcp 65001


----

[Q]： 关于git-bash为什么不能执行交互式的程序(输入参数)
	
	使用 winpty	不能解决