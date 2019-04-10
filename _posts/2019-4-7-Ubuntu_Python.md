## 前言
昨天完成了Linux系统上的Markdown环境的搭建，今天配置一下Vim的Python环境，就完成了基本所有的环经搭建部分，可以进入正题了。

---
# 正文
有了Markdown环境的搭建经验，给vim配置Python编译器的功能其实大同小异。

同样修改vim的配置文件 ～/.vimrc ,在正式配置一些插件功能之前，补充一下此前忘记提到的一些基础配置，
附代码如下：

      1 "基础配置
      2 set nocompatible  "去除VI一致性
      3 set number "显示行号       
      4 set showmatch "显示匹配的括号
      5 set scrolloff=3 "距离顶部和底部3行
      6 set encoding=utf-8 "编码   
      7 set fenc=utf-8 "编码       
      8 set hlsearch "搜索高亮     
      9 syntax on "语法高亮  

接下来开始补充相关的插件

首先，添加一个代码补全的插件  **YouComPleteMe**
这个插件安装后还需要手动编译，然后再在  ~/.vimrc中配置

[官方地址](http://valloric.github.io/YouCompleteMe/)     |      [github地址](https://github.com/Valloric/YouCompleteMe)

准备一些工具：

      suod apt-get install build-essential cmake
      sudo apt-get install python-dev python3-dev

这次没使用 Vundle 来安装此插件，改用手动安装

      #在主文件目录执行
      git clone https://github.com/Valloric/YouCompleteMe.git
      #手动下载完后检查仓库的完整性，切换到 YouCompleteMe 目录下，输入如下命令：
      #进入到YCM文件目录
      cd .vim/bundle/YoucomPleteMe 
      #执行命令
      git submodule update --init --recursive

接下来就要开始编译了
目录跳转到插件所在位置，执行：

      ./install.py --clang-completer（后面的参数是为了支持C的编译环境）

**这时候出了点问题**，如图：![](./img/python_env/complete_error.png)

说是 Could Not Find PythonLibs 找到了python2.7.6的库环境，按照它的提示，我以为是Python的版本问题，此后，再次执行了
  
      sudo apt-get install python3-dev

但显示已经是最新版本了，最后发现是由于Cmake的版本问题

使用 **Cmake --version**显示当前版本为2.8
接下来执行以下操作更新Cmake的版本：

      sudo apt-get autoremove cmake  "卸载过去的版本
      cd ~
      wget https://cmake.org/files/v3.5/cmake-3.5.2.tar.gz
      tar xvf cmake-3.5.2.tar.gz
      cd cmake-3.5.2
      ./bootstrap --prefix=/usr
      make
      sudo make install
      cmake --version "检查当前版本


更新Cmake后再次安装 YouCompleteMe 即成功

      110 set runtimepath+=~/.vim/bundle/YouCompleteMe
      111 let g:ycm_collect_identifiers_from_tags_files = 1           " 开启 YCM基于标签引擎  
      112 let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释与字符串中的内容也用于补全  
      113 let g:syntastic_ignore_files=[".*\.py$"]
      114 let g:ycm_seed_identifiers_with_syntax = 1                  " 语法关键字补全  
      115 let g:ycm_complete_in_comments = 1
      116 let g:ycm_confirm_extra_conf = 0                 "如果为1的话，会总是提示是否加载.ycm_extra_conf.py文件  
      117 let g:ycm_key_list_select_completion = ['', '']  " 映射按键,没有这个会拦截掉tab, 导致其他插件的tab不能用.  
      118 let g:ycm_key_list_previous_completion = ['', '']
      119 let g:ycm_complete_in_comments = 1                          " 在注释输入中也能补全  
      120 let g:ycm_complete_in_strings = 1                           " 在字符串输入中也能补全  
      121 let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释和字符串中的文字也会被收入补全  
      122 let g:ycm_global_ycm_extra_conf ='~/.vim/bundle/YouCompleteMe/.ycm_extra_conf.py'  "制定YCM目录
      123 let g:ycm_show_diagnostics_ui = 0                           " 禁用语法检查  
      124 inoremap  pumvisible() ? "\" : "\" |                        "回车即选中当前项
      125 let g:ycm_min_num_of_chars_for_completion = 2  "开始补全的字符数"
      126 
      127 let g:ycm_python_binary_path = 'python'  "jedi模块所在python解释器路径"
      128 let g:ycm_seed_identifiers_with_syntax = 1  "开启使用语言的一些关键字查询"
      129 let g:ycm_autoclose_preview_window_after_completion=1 "补全后自动关闭预览窗口"" 回车即选中当前项
      130 nnoremap  :YcmCompleter GoToDefinitionElseDeclaration|     " 跳转到定义处
      131 let g:ycm_min_num_of_chars_for_completion=2                 "从第2个键入字符就开始罗列匹配项 
      132 let g:ycm_global_ycm_extra_conf ='~/.vim/bundle/YouCompleteMe/.ycm_extra_conf.py'


修改YouCompleteMe的配置文件 .ycm_exyra_conf.py并把它复制到主目录 ～/


其他插件

自动缩进插件

自动缩进可以使用indentpython.vim插件：

      Plugin 'vim-scripts/indentpython.vim'

安装syntastic插件，每次保存文件时Vim都会检查代码的语法：


     Plugin 'vim-syntastic/syntastic'

[缩进指示线](https://github.com/Yggdroot/indentLine)

      Plugin 'Yggdroot/indentLine'

开关：

      :IndentLinesToggle
