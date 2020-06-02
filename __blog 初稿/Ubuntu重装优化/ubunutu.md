#记一次ubuntu系统重装+优化的方方面面

##起 
卸载掉双系统的Ubuntu之后，需要保留Linux的学习环境

再次选择搭建VMWare的虚拟机（可以使用快照，易于管理）

	磁盘分区配置方案
	4G  RAM 
	40G ROM
		/      15G   ext4
		swap   4G    swap
		/boot  200M  ext4
		/home  20G   ext4
	或者配置方案选择不分区 

系统安装成功后，回忆此前为了方便使用所作的准备工作，有几步必要环节罗列如下

1. 换源（把apt-get 所用的镜像更新为国内的源，提高速度）
2. 安装中文输入法
3. 安装VIM并做一些对应的配置
4. 安装Chrome
5. 卸载系统自带的一些不必要的软件
6. 安装解决依赖的工具aptitude
7. 为了美观做字体更改 桌面更改 终端优化

##承&结
具体步骤实现如下

###换源：
##

	需要更改的目标文件是
	/etc/apt/sources.list
	[注]更改前先备份(原文件可能会用到)


	本次使用清华源 感觉速度还行
	#deb cdrom:[Ubuntu 16.04 LTS _Xenial Xerus_ - Release amd64 (20160420.1)]/ xenial main restricted
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial universe
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates universe
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial multiverse
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates multiverse
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security universe
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security multiverse


	完成后使用以下语句做更新
    sudo apt-get update && sudo apt-get upgrade

###输入法 
安装搜狗输入法 （网上经验贴）
安装输入法竟然会导致系统内核崩溃(没找到原因)
最后采用替代方案 安装Google输入法 

##
	sudo apt-get install fcitx-googlepinyin
	在language中设置fcitx
	在Text entry中添加输入法 
	更改快捷键


###安装vim 并做配置

之前有篇blog是使用插件做VIM配置 本次采用直接对VIM的配置文件vimrc做更改达到目的

##
	安装
	sudo apt-get install vim 
	配置目标文件
	sudo vim /etc/vim/vimrc
	配置
	[注]{本段代码摘自 https://blog.csdn.net/qq_35208390/article/details/78441013}
	" 显示相关  配置时把以下代码粘贴到目标文件底部
	""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
	set shortmess=atI   " 启动的时候不显示那个援助乌干达儿童的提示  
	winpos 5 5         " 设定窗口位置  
	set lines=30 columns=85    " 设定窗口大小  
	set nu              " 显示行号  
	set go=             " 不要图形按钮  
	"color asmanian2     " 设置背景主题  
	set guifont=Courier_New:h10:cANSI   " 设置字体  
	syntax on           " 语法高亮  
	autocmd InsertLeave * se nocul  " 用浅色高亮当前行  
	autocmd InsertEnter * se cul    " 用浅色高亮当前行  
	set ruler           " 显示标尺  
	set showcmd         " 输入的命令显示出来，看的清楚些  
	set cmdheight=1     " 命令行（在状态行下）的高度，设置为1  
	"set whichwrap+=<,>,h,l   " 允许backspace和光标键跨越行边界(不建议)  
	set scrolloff=3     " 光标移动到buffer的顶部和底部时保持3行距离  
	set novisualbell    " 不要闪烁(不明白)  
	set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}   "状态行显示的内容  
	set laststatus=1    " 启动显示状态行(1),总是显示状态行(2)  
	set foldenable      " 允许折叠  
	set foldmethod=manual   " 手动折叠  
	set background=dark "背景使用黑色 
	set nocompatible  "去掉讨厌的有关vi一致性模式，避免以前版本的一些bug和局限  
	" 显示中文帮助
	if version >= 603
	    set helplang=cn
	    set encoding=utf-8
	endif
	" 设置配色方案
	"colorscheme murphy
	"字体 
	"if (has("gui_running")) 
	"   set guifont=Bitstream\ Vera\ Sans\ Mono\ 10 
	"endif 
	
	
	set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936
	set termencoding=utf-8
	set encoding=utf-8
	set fileencodings=ucs-bom,utf-8,cp936
	set fileencoding=utf-8
	
	"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
	"""""新文件标题""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
	"新建.c,.h,.sh,.java文件，自动插入文件头 
	autocmd BufNewFile *.cpp,*.[ch],*.sh,*.java exec ":call SetTitle()" 
	""定义函数SetTitle，自动插入文件头 
	func SetTitle() 
    "如果文件类型为.sh文件 
    if &filetype == 'sh' 
        call setline(1,"\#########################################################################") 
        call append(line("."), "\# File Name     : ".expand("%")) 
        call append(line(".")+1, "\# Author        : enjoy5512") 
        call append(line(".")+2, "\# mail          : enjoy5512@163.com") 
        call append(line(".")+3, "\# Created Time  : ".strftime("%c")) 
        call append(line(".")+4, "\#########################################################################") 
        call append(line(".")+5, "") 
        call append(line(".")+6, "\#!/bin/bash") 
    call append(line(".")+7, "")
    call append(line(".")+8, "")
    else 
        call setline(1, "/*************************************************************************") 
        call append(line("."), "    > File Name       : ".expand("%")) 
        call append(line(".")+1, "    > Author          : enjoy5512") 
        call append(line(".")+2, "    > Mail            : enjoy5512@163.com ") 
        call append(line(".")+3, "    > Created Time    : ".strftime("%c")) 
        call append(line(".")+4, " ************************************************************************/") 
        call append(line(".")+5, "")
    endif
    if &filetype == 'cpp'
        call append(line(".")+6, "#include<iostream>")
    call append(line(".")+7, "")
        call append(line(".")+8, "using namespace std;")
        call append(line(".")+9, "")
        call append(line(".")+10, "int main(int argc,char *argv[])")
        call append(line(".")+11, "{")
        call append(line(".")+12, "     ")
        call append(line(".")+13, "    return 0;")
        call append(line(".")+14, "}")
    endif
    if &filetype == 'c'
        call append(line(".")+6, "#include<stdio.h>")
        call append(line(".")+7, "")
        call append(line(".")+8, "int main(int argc,char *argv[])")
        call append(line(".")+9, "{")
        call append(line(".")+10, "     ")
        call append(line(".")+11, "    return 0;")
        call append(line(".")+12, "}")
    autocmd BufNewFile * 12 j
    endif
	endfunc
	""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
	"键盘命令
	""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
	"C，C++ 按F5编译运行
	map <F5> :call CompileRunGcc()<CR>
	func! CompileRunGcc()
	    exec "w"
	    if &filetype == 'c'
	        exec "!gcc % -o %<"
	        exec "! ./%<"
	    elseif &filetype == 'cpp'
	        exec "!g++ % -o %<"
	        exec "! ./%<"
	    elseif &filetype == 'sh'
	        :!./%
	    endif
	endfunc
	"C,C++的调试
	map <C-F5> :call Rungdb()<CR>
	func! Rungdb()
	    exec "w"
	    if &filetype == 'c'
	        exec "!gcc % -g -o %<"
	        exec "!gdb -tui ./%<"
	    elseif &filetype == 'cpp'
	        exec "!g++ % -g -o %<"
	        exec "!gdb -tui ./%<"
	    endif
	endfunc
	""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
	""实用设置
	"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
	" 设置当文件被改动时自动载入
	set autoread
	" quickfix模式
	autocmd FileType c,cpp map <buffer> <leader><space> :w<cr>:make<cr>
	"代码补全 
	set completeopt=preview,menu 
	"允许插件  
	filetype plugin on
	"共享剪贴板  
	set clipboard+=unnamed 
	"从不备份  
	set nobackup
	"自动保存
	set autowrite
	set ruler                   " 打开状态栏标尺
	set cursorline              " 突出显示当前行
	set magic                   " 设置魔术
	set guioptions-=T           " 隐藏工具栏
	set guioptions-=m           " 隐藏菜单栏
	set foldcolumn=0
	set foldmethod=indent 
	set foldlevel=3 
	set foldenable              " 开始折叠
	" 不要使用vi的键盘模式，而是vim自己的
	set nocompatible
	" 语法高亮
	set syntax=on
	" 去掉输入错误的提示声音
	set noeb
	" 在处理未保存或只读文件的时候，弹出确认
	set confirm
	" 自动缩进
	set autoindent
	set cindent
	" Tab键的宽度
	set tabstop=4
	" 统一缩进为4
	set softtabstop=4
	set shiftwidth=4
	"禁止生成临时文件
	set nobackup
	set noswapfile
	"搜索忽略大小写
	set ignorecase
	"搜索逐字符高亮
	set hlsearch
	set incsearch
	"行内替换
	set gdefault
	"编码设置
	set enc=utf-8
	set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936
	"语言设置
	set langmenu=zh_CN.UTF-8
	set helplang=cn
	" 我的状态行显示的内容（包括文件类型和解码）
	set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}
	"set statusline=[%F]%y%r%m%*%=[Line:%l/%L,Column:%c][%p%%]
	"set statusline=\ %<%F[%1*%M%*%n%R%H]%=\ %y\ %0(%{&fileformat}\ %{&encoding}\ %c:%l/%L%)\
	" 总是显示状态行
	set laststatus=2
	" 命令行（在状态行下）的高度，默认为1，这里是2
	set cmdheight=2
	" 侦测文件类型
	filetype on
	" 载入文件类型插件
	filetype plugin on
	" 为特定文件类型载入相关缩进文件
	filetype indent on
	" 保存全局变量
	set viminfo+=!
	" 在被分割的窗口间显示空白，便于阅读
	set fillchars=vert:\ ,stl:\ ,stlnc:\
	" 高亮显示匹配的括号
	set showmatch
	" 匹配括号高亮的时间（单位是十分之一秒）
	set matchtime=1
	" 光标移动到buffer的顶部和底部时保持3行距离
	set scrolloff=3
	" 为C程序提供自动缩进
	set smartindent
	" 高亮显示普通txt文件（需要txt.vim脚本）
	au BufRead,BufNewFile *  setfiletype txt
	"自动补全
	":inoremap ( ()<ESC>i
	":inoremap ) <c-r>=ClosePair(')')<CR>
	:inoremap { {<CR>}<ESC>O
	:inoremap } <c-r>=ClosePair('}')<CR>
	":inoremap [ []<ESC>i
	":inoremap ] <c-r>=ClosePair(']')<CR>
	":inoremap " ""<ESC>i
	":inoremap ' ''<ESC>i
	function! ClosePair(char)
	    if getline('.')[col('.') - 1] == a:char
	        return "\<Right>"
	    else
	        return a:char
	    endif
	endfunction
	filetype plugin indent on 
	"打开文件类型检测, 加了这句才可以用智能补全
	set completeopt=longest,menu

###安装Chrome等软件以及卸载部分软件
##
	没什么特殊的地方
	注意.deb文件的安装
	移动到 /opt 文件夹之后
	dpkg -i **.deb
	sudo apt-get install -f
	就可以了

###系统做美化处理(方便自己)

折腾zsh
##
	测试当前shell
	echo $SHELL

	查看系统安装了哪些shell
	cat /etc/shells

	安装zsh
	sudo apt install zsh

	切换默认shell为zsh
	chsh -s /bin/zsh
	reboot
	
	安装oh-my-zsh
	wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh

	安装incr自动补全插件
	wget http://mimosa-pudica.net/src/incr-0.2.zsh
	
	移动插件到oh-my-zsh目录的插件库下
	sudo mkdir ~/.oh-my-zsh/plugins/incr/	
	sudo mv incr-0.2.zsh ~/.oh-my-zsh/plugins/incr/incr-0.2.zsh

	使插件生效进入vim编辑
	sudo vim ~/.zshrc
	在vim界面按i进入输入模式在末尾加上如下代码
	source ~/.oh-my-zsh/plugins/incr/incr*.zsh
	
	之后可自行改变主题 参见wiki样图

	
##坑

1. 安装某些软件如果当时没有成功需要使用

	apt-get remove *** 卸载(后期依赖多了难以找到)
2. 相比以前 本次安装  搜狗输入法时总是导致系统内核崩溃(且没找到错误原因)
	其中第一次崩溃时还没有制作快照(只能重装系统)
    最后只得选择替代方案Google输入法

3. 卸载某些软件时(或其他原因)造成ubuntu-desktop崩溃
   某次重启时发现(为时已晚 无法回溯)
##
	解决方案：重装桌面
	sudo apt-get install --reinstall ubuntu-desktop，之后apt-get upgrade一下
	但是可能因为更新过源，重装时报错
	(这时可以采用之前备份的原始 cn.ubuntu源)
	cd /etc/apt/
	mv sources.list.bak sourcecs.list
	update之后重试
	------------------------------------------
	以上方法总是存在问题
	换做cn.ubuntu源很慢且不知是否中途因为网络中断 无法定位到软件包
	再次研究报错信息，发现在试图执行update时有一条
	0 upgraded, 0 newly installed, 0 to remove and 468 not upgraded
	没有upgrade升级的多达四百多条
	直接执行 apt-get upgrade 不能成功
	使用apt-get dist-upgrade 成功，发现系统内核版本升级为18且桌面问题解决。
	------------------------------------------

##杂项
一些知识点

1. 使用apt-get -f install修复依赖关系，修复过程中破化了包括Gnome在内的大量环境
2. 定期保存系统快照，在执行**autoremove等可能出现未知破坏操作**情况下一定要至少保留卸载列表。
3. fcitx 与Google拼音的输入法切换 ctrl+space 存在快捷键抢占
4. 列出用户 sudo vi(cat) /etc/passwd
5. 配置插件以及更新插件 配置 rc文件 source更新
6. 0 upgraded, 0 newly installed, 0 to remove and 6 not upgraded解决方法 sudo apt-get dist-upgrade 
7.  关于 update upgrade dist-upgrade
## 
	update：当执行apt-get update时，update重点更新的是来自软件源的软件包的索引记录（即index files）。

	upgrade：当执行apt-get upgrade时，upgrade是根据update更新的索引记录来下载并更新软件包。

	dist-upgrade:当执行apt-get dist-upgrade时，除了拥有upgrade的全部功能外，dist-upgrade会比upgrade更智能地处理需要更新的软件包的依赖关系。
	



