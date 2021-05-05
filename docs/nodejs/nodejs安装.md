先安装gcc

方法一：nvm安装

nvm install --lts  ##安装最新稳定版
node –v
npm -v

备注：

nvm命令：
1、nvm list-remote ：列出所有可安装版本
2、nvm install {版本号} ：安装指定版本
3、nvm ls ：查看已经安装的版本
4、nvm use {版本号} ：使指定版本生效
5、nvm alias default {版本号} ：设置默认版本

方法二：压缩包安装

1、下载 
wget https://nodejs.org/dist/v8.5.0/node-v8.5.0-linux-x64.tar.gz
2、tar zxvf node-v8.5.0-linux-x64.tar.gz
3、配置环境变量
vim /etc/profile 编辑环境变量，在末尾添加

export NODE_HOME="/usr/local/node-v8.5.0-linux-x64"
export PATH=$PATH:$NODE_HOME/bin

4、刷新 source /etc/profile

5、验证
node -v

. ~/.nvm/nvm.sh
#. ~/.profile //备注，如果其他命令可能会需要
#. ~/.bashrc //同上

﻿tar xvJf node-v12.16.1-linux-x64.tar.xz -C /usr/local