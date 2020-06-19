## 关于安装以及一些问题
1. JAVA 的JDBC(Java Database Connectivity)编程
2. MySQL数据库使用

		JDBC使用不同的驱动(实现方式)+统一的API对多种数据库操作
		跨平台 跨数据库
		
		JDBC四种驱动
		1. JDBC-ODBC桥(微软通用) 映射
		2. 直接映射JDBC的API到数据库特定客户端(用C写)API
		3. 三层结构的JDBC （Applet阶段 现在少）
		4. 纯JAVA 目前最流行(jar包)

本来使用安装版本的MySQL但是需要17版本的visual Studio与本机的VS19版本冲突，选择使用免安装版本的MySQL自行配置

		MySQL常用指令
		①安装服务：mysqld --install
		②初始化：　mysqld --initialize --console
 		③开启服务：net start mysql
 		④关闭服务：net stop mysql
 		⑤登录mysql：mysql -u root -p
 				Enter PassWord：(密码)
 		⑥修改密码：alter user 'root'@'localhost' identified by 'root';(by 接着的是密码)
 		⑦标记删除mysql服务：sc delete mysql
 [安装MySQL参见](https://www.cnblogs.com/winton-nfs/p/11524007.html)           

		关于卸载
		1、关闭Mysql服务管理员身份运行命令行（cmd）: net stop mysql

   		2、删除Mysql的注册表
      		1. Win+R打开运行界面，在输入框中输入 regedit 进入系统注册表窗口
      		2. 分别在以下目录中找到 MySQL 的注册表，
			   鼠标右键直接删除MySQL目录中的 EventMessageFile 和 TypesSupported 两个文件
	           就好了,如果对应的目录中没有,就不用删除了，
			   也可以搜索注册表: 在系统注册表窗口选择「编辑」 — 选择「查找」 — 输入 「MySQL」进行查找,
			   将找到的MySQL目录中的 EventMessageFile 和 TypesSupported 两个文件进行删除
	
			   HKEY_LOCAL_MACHINE/SYSTEM/ControlSet001/Services/Eventlog/Application/MySQL
			   HKEY_LOCAL_MACHINE/SYSTEM/ControlSet002/Services/Eventlog/Application/MySQL
			   HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Services/Eventlog/Application/MySQL
 
		3、移除Mysql服务
	     （1）以管理员身份使用命令行(cmd)进入MySQL的 bin 目录下
	     （2）执行移除 MySQL服务的命令 : mysqld -remove
	     （3）当看到有Service successfully removed时，则表示移除Mysql服务成功
 
		4、删除Mysql文件  将Mysql安装目录下的文件全部删除即可     


[注]关于SQLyog连接MySQL报错，错误代码1251
	
	1251 client does not support authentication protocol requested by server;consider upgrading Mysql client
	ERROR 1396 (HY000): Operation ALTER USER failed for 'root'@'localhost'  

问题原因:

	主要是由于mysql8以前的加密规则与mysql8以后的存在差异。

解决办法:

	ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;  ##修改加密规则
	
	ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'; ##更新一下用户的密码 password 为自己想要重新设置的密码
	
	FLUSH PRIVILEGES; ##刷新权限         

[注2]MySQL8.0.16版本在SQLYog8.14内执行查询均报错1064的解决

	执行任何一个语句，均报错，但大多能在报错后返回结果

	查看日志，发现sqlyog给所有的查询都同时执行了explain extended     
	
	在mysql 5.7或更早的版本内，这么做可以得到详细的执行扩展信息（SQLYog不仅仅会执行查询，还会向你展示性能等扩展情况），
	但mysql 8里，经过查阅文档发现，已经改为了直接使用explain，不含extended的语句
	
	所以：在sqlYog里找到 工具-选项-增强工具，取消explain扩展信息的勾选
      
##SQL语句基础

sql:structured query language 结构化查询语言         
；是操作和检索 关系数据库的标准语言             

分类

1. 查询：主要由select完成
2. DML(Data Manipulation Language,数据库操作语言)主要 insert update delete
3. DDL(Data Defination Language,数据库定义语言)主要 create alter drop truncate
4. DCL(Data Control Language,数据库控制语言)主要 grant revoke
5. 事务控制语句  commit rollback savepoint

SQL关键字不区分大小写

DDL操作数据库对象 DML操作数据表里的数据：增删改查

DML由insert into ；update； delete from；三个命令组成






















##DBMS
严格来讲 数据库只是存放数据的地方，
而在用户访问操作这些数据的时候 需要用到数据库管理系统Database Management System(DBMS)
DBMS 负责管理数据的存储，安全，一致性，并发，恢复和访问等

现今使用的数据库基本是关系型数据库
以数据表为基本数据存储单元
数据表作为存储数据的逻辑单元(想像为行列表格[行作为记录， 列作为字段])
是以数据库建表时必须指明字段数(列数)
并且为每张数据表指明主键列(用以唯一标识此行的记录)















##SQL与JDBC
1. JAVA提供待实现的JDBC对数据库的访问API
2. 数据库厂商提供数据库的相关驱动
3. 由JDBC驱动转换，使得相同的JDBC API操作不同的数据库
4. 由JDBC完成以下操作
	1. 建立与数据库的连接
	2. 执行SQL语句
	3. 获得SQL语句的执行结构

	
[注]ODBC Open DataBase Connectivity:开放数据库连接 JDBC仿ODBC | JDBC API 只是执行SQL语句的工具

