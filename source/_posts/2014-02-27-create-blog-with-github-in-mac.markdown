---
layout: post
title: "在mac下使用github自建博客"
date: 2014-02-27 21:42:50 +0800
comments: true
categories: Archives
---
 参考： http://octopress.org/docs/setup/rvm/

一、 安装Ruby环境

过程：
安装rvm    


   执行 curl -L https://get.rvm.io | bash -s stable —ruby
   报错：checking for Tcl configuration... configure: error: Can't find
        Tcl configuration definitions   Requirements installation failed    
        with status: 1.

      解决方案：xcode-select —install  执行后，再执行1.

2. 安装 ruby
    
    rvm install 1.9.3
    rvm use 1.9.3
    rvm rubygems latest

3.   创建github项目
      注意此项： git@github.com:googlegis/googlegis.github.com.git


4. 在本机创建ssh

cd ~/.ssh
ssh-keygen -t rsa -C 你注册github时的email
弹出Enter file in which to save the key (/Users/twer/.ssh/id_rsa):直接按空格

弹出Enter passphrase (empty for no passphrase):输入你github账号的密码。Enter same passphrase again:再次输入你的密码。

打开~/.ssh下的id_rsa.pub文件复制里面的全部内容。
登陆github，选择Account Settings-->SSH Public Keys 添加ssh，把剪切板的内容复制到key输入框内直接保存。

测试shh:

ssh git@github.com
输出

PTY allocation request failed on channel 0
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
代表成功


5.  安装octopress

      git clone  git://github.com/imathis/octopress.git octopress
       
      cd octopress    # If you use RVM, You'll be asked if you trust the .rvmrc file (say yes).

      ruby --version  # Should report Ruby 1.9.3
   
      安装依赖项
   
       gem install bundler
    
        rbenv rehash 
  
        bundle install
 
       安装默认主题
   
        rake install



6. 部署博客

利用octopress的一个配置rake任务来自动配置上面创建的仓库：可以让我们方便的部署GitHub page。在终端输入如下命令：

rake setup_github_pages
弹出之后输入git@github.com:your_username/your_username.github.com.git不要用提示的io，我的是git@github.com/sgxiang/sgxiang.github.com.git

输入以下命令部署博客

rake generate
rake deploy
如果无法push到仓库的master分支，尝试在项目目录的.git/config中添加

[branch "master"]
 remote = origin
 merge = refs/heads/master


7.写博客

通过命令

rake new_post["myTitle"]
文章生成在目录下的source/_posts目录下。文章是markdown格式的。可以通过 Mou 软件来编辑保存。

关于markdown的格式可以参考这篇文章:http://wowubuntu.com/markdown/

写完后就可以部署更新文章到github上了

rake generate
git add .
git commit -am "Some comment here." 
git push origin source
rake deploy


     8. 更换主题

Install

$ cd octopress  #octopress directory
$ git clone git@github.com:shashankmehta/greyshade.git .themes/greyshade
$ echo "\$greyshade: color;" >> sass/custom/_colors.scss //Substitue 'color' with your highlight color
$ rake "install[greyshade]"
$ rake generate


修改主题：
 修改头像。   mygravatar id
