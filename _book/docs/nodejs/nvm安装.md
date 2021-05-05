nvm安装成功后，但命令不可用(command not found)

第一种情况：未安装nvm
第二种情况：安装成功nvm，但输入命令提醒“ command not found”
第一种解决办法：
centos/redhat系统直接下载安装nvm

curl https://raw.github.com/creationix/nvm/v0.33.11/install.sh | sh
第二种解决办法：
1.进入执行者的家目录下的.nvm隐藏文件夹

cd  ~/.nvm
2.查看目录下是否有.bash_profile文件（这是环境变量隐藏文件）

[root@Blog .nvm]# ls -a
.                Dockerfile      .github     Makefile      README.md
..               .dockerignore   .gitignore  .npmrc        ROADMAP.md
bash_completion  .editorconfig   install.sh  nvm-exec      test
.bash_profile    .git            LICENSE.md  nvm.sh        .travis.yml
CONTRIBUTING.md  .gitattributes  .mailmap    package.json  update_test_mocks.sh
[root@Blog .nvm]# 
3.如果没有就创建该文件，然后保存文件

[root@Blog .nvm]# vim .bash_profile 
export NVM_DIR="/root/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  
4.执行环境变量

[root@Blog .nvm]# souce .bash_profile 
5.输入nvm命令，是否还提醒命令不存在？如果有，命令安装成功

[root@Blog ~]# nvm --version 
0.33.11