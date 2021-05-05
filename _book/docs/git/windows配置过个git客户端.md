# 生成秘钥

ssh-keygen -t rsa -C "wjw_595@126.com"

把公钥配置到 相应服务器 比如 gitee.com

把私钥拷贝到 C:/Users/你的用户名/.ssh

 git config --global user.name "wjw_595"

   git config --global user.email "wjw_595@126.com"



配置config

ssh -T git@github.com