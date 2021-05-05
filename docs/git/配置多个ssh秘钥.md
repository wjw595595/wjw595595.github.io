# 生成秘钥

ssh-keygen -t rsa -C wjw_595@126.com

取不同的名称：

如： id_rsa_github,id_rsa_gitee

秘钥拷贝到 .ssh 文件下

## 添加公钥

公钥添加到相应的服务器

添加 config文件(**核心**)

```
Host gitlab.com
    HostName gitlab.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_gitlab
# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_github

# gitee
Host gitee.com
    HostName gitee.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
```

测试

 ssh -T git@gitee.com

 ssh -T git@github.com

 ssh -T git@gitlab.com