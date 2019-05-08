#数据可视化（二）
---
### 概述：
通过可视化数据来探索数据，与**数据挖掘**紧密相关，数据挖掘用代码来探索数据集的规律和关联。

------
## 1.：使用Pygal模拟掷骰子
用Python可视化包Pygal来生成可缩放的图形文件。这里对掷骰子的结果进行分析。

[Pygal画廊](http://www.pygal.org/en/stable/)：其中可以看到各种图标的代码实现

在安装Pygal库时一直报这样一个错
![](/img/data_visual/pygal_error.png)

升级了pip的版本后依然不变，最后发现应该是网络通信的问题，给pip换成国内源后果然解决。
  
      [global]
      index-url = https://pypi.tuna.tsinghua.edu.cn/simple

导入pygal库对实验生成的数据进行可视化操作

      21 #对结果进行数据可视化
      22 hist = pygal.Bar()
      23
      24 hist.title = "Request of rolling one D6 1000 times"
      25 hist.x_labels = ['1','2','3','4','5','6']
      26 hist.x_title = "Result"
      27 hist.y_title = "Frequency of Result"
      28
      29 hist.add('D6', frequency)
      30 hist.render_to_file('die_visual.svg')

生成的图表被渲染成 .svg文件，可以用Web浏览器打开


---
## 2.：下载数据进行可视化

目标：访问并可视化两种常见格式存储的数据：**CSV 和 JSON**

- (1) CSV文件格式：将数据写作一系列以逗号分隔的值。
（CSV文件格式对人来说稍显麻烦，但程序可以轻松处理）
- (2) Json

---
### 处理CSV格式的数据

1. 分析CSV文件头

csv模块包含在Python标准库中，可用于分析CSV文件的数据行，以快速提取感兴趣的值。

      1 import csv
      2 filename = 'aaa.csv'
      3 with open(filename) as f:
      4     reader = csv.reader(f)#建立一个与该文件关联的阅读器对象（reader）
      5     header_row = next(reader)
      6     print(header_row)         

以上代码可以得到目标文件 aaa.csv 的首行

根据取出的首行信息，即可判断文件中的打大致信息情况，由此进行后续操作。

取出数据后，结合模块matplotlib进行数据可视化。

      # 根据数据绘制图形
      fig = plt.figure(dpi=128, figsize=(10, 6))
      plt.plot(highs, c='red')

      # 设置图形的格式
      plt.title("XXXXXXXXXXXXXXX", fontsize=24)
      plt.xlabel('', fontsize=16)
      plt.ylabel("Temperature (F)", fontsize=16)
      plt.tick_params(axis='both', which='major', labelsize=16)
      plt.show()



---
### 处理Json格式的数据
下载到的 json 文件 其实是一个很长的 Python列表，每个列表元素又是一个有着四个键的字典

大致浏览json 文件的内容，能根据需求取出想要的数据信息。
以一个世界人口数据信息的文件为例。有json格式的文件***population_data.json***
用Python打开后，大致了解这是一个全世界数年的人口信息调查的结果，我们可以按照
年份这个“键”名取出信息。以此类推，我们把所取出的信息进行数据可视化。

对于这样的一个世界人口数据文件，使用Pygal 数据可视化生成一张地图。还需要解决
数据特定格式问题。

---
为了把json数据绘制成地图文件完成可视化，先给pygal模块增加一个插件模块**pygal_maps_world**,这里遇到了一个问题：下载完成该模块后，加载时一直报错：“找不到该模块”
这一类问题通常涉及包/模块 的 安装位置问题
    执行 import sys  ; print(sys.path)查看Python搜索路径，确保自己的模块在Python的搜索路径中。
    Python搜索路径其实是一个列表，当导入某模块时，Python会自动搜索这个列表当中的路径，如果路径中存在要导入的模块文件则导入成功。

通常作为没有管理员权限的用户，在服务器上用Pip装包需要装到自己Home底下。

有时候装了之后找不到这个包，import不进来，会报no module named xxx， 错误。为什么呢？因为虽然装到了一个路径底下，但是Python找不到这个路径，这个时候需要改环境变量。

----
到这里为止，基本的数据可视化处理所遇见的问题就差不多了，剩下的大多是熟悉模块的功能，增改一些细节或是增加一些高级功能。

