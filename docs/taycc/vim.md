# vim

#### mac系统在`终端`使用MacVim
1.先下载MacVim的安装包，然后使用`图形界面`进行安装

2.在`~/.bash_profile`中插入以下代码

```
# macvim的快捷方式
alias gvim='/Applications/MacVim.app/Contents/MacOS/Vim -g'
```

3.MacVim的配置文件是`~/.gvimrc`，不要搞错了`~/.vimrc`是命令行版vim的配置文件


####为vim安装vundle插件管理器

1.从github上clone项目内容

```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

2.在~/.vimrc中插入以下内容

```
set nocompatible              " be iMproved, required
filetype off                  " required



" 启用vundle来管理vim插件
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" 安装插件写在这之后
" =======================================================



" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" 安装markdown文档实时预览
Plugin 'godlygeek/tabular'
Plugin 'plasticboy/vim-markdown'
Plugin 'suan/vim-instant-markdown'



" =======================================================
" 安装插件写在这之前
call vundle#end()               " required
filetype plugin on              " required



" 常用命令
" :PluginList       - 查看已经安装的插件
" :PluginInstall    - 安装插件
" :PluginUpdate     - 更新插件
" :PluginSearch     - 搜索插件，例如 :PluginSearch xml就能搜到xml相关的插件
" :PluginClean      - 删除插件，把安装插件对应行删除，然后执行这个命令即可
" h: vundle         - 获取帮助
" vundle的配置到此结束，下面是你自己的配置
```



<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>