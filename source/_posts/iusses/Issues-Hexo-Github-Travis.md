---
title: Hexo 通过 Travis 自动部署到 Github
date: 2016-12-15 22:29:21
tags: [Hexo, Travis, Github]
---
### 配置 Travis 用于自动生成

Travis 的 构建周期 分为两步：

install 用于安装构建所需要的一些依赖
script 运行构建脚本
我们可以自定义这两个步骤，如在运行之前做一些配置，如果成功做一些动作，失败做一些动作等。具体支持的步骤如下：

before_install
install
before_script
script
after_success or after_failure
before_deploy ，可选
deploy ，可选
after_deploy ，可选
after_script
所以我们的配置如下：

.travis.yml
language: node_js

node_js:
- '0.12'

branches:
  only:
  - source                # 只监测 source 分支上的 commit

before_install:
- npm install -g hexo-cli # 安装 hexo

install:
- npm install             # 安装额外的插件

script:
- git submodule init      # 用于更新主题
- git submodule update
- hexo generate
上面的例子中 npm install 安装 hexo 需要的插件，这要求 package.json 已经设置好。例如，我们要使用 hexo-deployer-git 插件来部署，所以我们需要事先运行下面命令：

npm install --save hexo-deployer-git
上述命令的作用之一是在 package.json 中添加相应的项。

使用 Travis 自动部署

首先，我们需要对 _config.yml 进行配置，以执行 hexo deploy 进行部署：

_config.yml
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/lotabout/lotabout.github.io
  branch: master
然后我们可以在 .travis.yml 添加生成成功后的动作：

.travis.yml
after_success:
- git config --global user.name "Your Name"
- git config --global user.email "Your Email"
- hexo deploy
然而在 hexo deploy 时，我们需要输入 Github 的用户名和密码，但这又要如何自动化呢？

### 1. Github OAuth

Github OAuth 支持一种特殊的 URL 来执行 push/pull 等等操作，而不需要输入用户名密码。但这需要事先在 Github 上创建一个 token：

打开 Personal Access Tokens
点击 Create new token
token 的权限保持默认即可
有了这个 token 后，原先用

https://github.com/username/repo.git
进行访问，现在换成：

https://<token>@github.com/owner/repo.git
即可。切记，这个 token 的权限很大，不要把原文提交到 Github 上。

### 2. Travis 加密 token

上面我们说了，要保护好你的 github token。所以我们在写入 travis 配置时要先对这个 token 进行加密。

首先安装 travis 命令行工具：

gem install travis
travis login
之后通过如下命令在 .travis.yml 添加额外的配置：

travis encrypt 'GH_TOKEN=<TOKEN>' --add
上面命令会在 .travis.yml 添加如下内容：

.travis.yml
env:
  global:
    secure: QAH+/EIDC/Jg...
上面的一长串字符串就是加密后的环境变量。之后，在 Travis 执行脚本时，我们就可能访问环境变量 GH_TOKEN 来获取 github token 了。

最后，我们用 sed 命令动态地修改 github 的 URL，加入 token 信息：

.travis.yml
after_success:
- git config --global user.name "Mark Wallace"
- git config --global user.email "lotabout@gmail.com"
- sed -i'' "/^ *repo/s~github\.com~${GH_TOKEN}@github.com~" _config.yml
- hexo deploy
