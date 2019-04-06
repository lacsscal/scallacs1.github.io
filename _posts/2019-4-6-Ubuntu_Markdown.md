

#前言
---
近期准备把Python的相关学习转移到Linux系统上，由于有一段时间没用Linux操作系统了，为了流畅学习做了一些准备，重装系统后为了保证blog的持续更新以及优质阅览体验，对vim进行了Markdown的相关插件配置。这里进行一些记录。

---

## 正文
 - Vundle 插件
 
 Vundle是 vim bundle 的简称，是vim的插件管理器。安装Vundle即可较为简单的增改想要的插件。

 - Vundle 的安装


    git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim

（提前配置好Git的链接）
 默认安装在/.vim/bundle/vundle目录下。
安装好后即可通过 vim 编辑 ~/.vimrc  文件来配置想要的插件。

![修改配置信息](/img/markdown/vundle.png)
如图配置想要的插件，将其地址写在 vundle#begin和vundle#end之间。
打开一个vim ,命令模式输入:PluhinInstall,即可完成插件的安装。
当然，如果想要删除插件的话，在.vimrc文件的相关行删除该插件，并且打开vim使用命令:BundleClean即可。

 - 配置Markdown的插件
 
 经过目录的更行很容易发现，其实安装的插件都到了 ～/.vim/bundle目录下，所以也可以直接下载删除相关目录增改插件。
 
 把vim配置成一个比较好用的markdown编辑器大概给它配置以下功能：
  - 语法高亮
  - 实时预览

  用直接配置目录的方式配置语法高亮
  

    cd ~/.vim/bundle
    git clone https://github.com/tamlok/vim-markdown.git

  然后将/root/.vim/bundle/vim-markdown文件夹下的plugin目录（存放插件）、syntax目录（存放语法的解析文件）、ftdetect目录（存放插件对哪些后缀的文件生效）至少这三个目录复制到/root/.vim/目录下才会生效的。

  用 vim-instant-markdown 插件实现实时预览


该插件的功能是：用vim打开一个 .md 文件时会自动弹出一个浏览器窗口，实现实时预览。

安装插件之前需要先安装 node.js 和 npm

    sudo add-apt-repository ppa:chris-lea/node.js
    sudo apt-get update
    sudo apt-get install nodejs


安装完node.js之后 安装 instant-markdown-d

    sudo npm -g install instant-markdown-d
    
安装vim-instant-markdown插件

编辑 ～/.vimrc

增加Plugin'suan/vim-instant-markdown'

执行命令：PluginInstall

即可完成实时预览插件的安装。

### 问题来了
完成安装后实际并没有浏览器窗口弹出，貌似有哪里出了问题，一一排查安装步骤后，在检查到 " instant-markdown-d "的时候，发现有一个报错被忽略了，键入 instant-markdown-d,会回显 ![](/img/markdown/instant_markdown.png)

查询相关资料后，发现，原因是node.js的版本过低


    sudo apt-get update
    curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
    sudo apt-get install -y nodejs

更新node.js 版本后，重新安装 instant-markdown-d,成功。`
