---
title: Hello-ZSH
date: 2016-12-17 20:46:19
tags:
---

# Mac using ZSH

## 将bash切换为zsh
``` bash
chsh -s /bin/zsh
```
如果要切换回去：
``` bash
chsh -s /bin/bash
```
## 下载oh-my-zsh
1) 直接用git从github上面下载包：
``` bash
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh 
```
2) 备份已有的zshrc(一般不需要)
``` bash
cp ~/.zshrc ~/.zshrc.orig
```
3) 替换zshrc
``` bash
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

（也可以把你的bash的配置文件(~/.bash_prorile或者~/.profile等)给拷贝到zsh的配置文件~/.zshrc里，因为zsh兼容bash）