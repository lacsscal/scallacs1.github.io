1. 主活动MainActivity 在配置文件 AndroidManifest.xml中注册
2. 主活动即是手机图标启动的活动
3. 活动是Android应用程序的门面，应用中的东西都在活动中
4. Activity即Android活动的基类,包含用户界面，主要用来与用户交互
5. Android设计讲究 逻辑与视图分离 所以不在活动中编写界面
	1. 常用做法布局文件(layout?)中编写界面,在活动中引入？
6. res/layout(布局文件)
7. res为资源文件夹
	1. drawable：图片
	2. mipmap：应用图标(兼容各种设备所以有多个)
	3. values：字符串,样式。颜色...
	4. layout：布局文件
8. 开发日志工具Log(android.util.Log)
9. 