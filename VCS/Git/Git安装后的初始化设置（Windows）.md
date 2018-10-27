## Git安装后的初始化设置（Windows）

### 1：设置user.name 和 user.email，git提交会使用这些配置信息。

git config --global user.name "whyles"

git config --global user.email "whyles520@foxmail.com"

### 2：生成ssh key，使用ssh方式获取远程库的时候需要配置ssh key

windows上需要打开git bash 执行以下命令。

ssh-keygen -t rsa -C "comment"

注意： ssh-keygen 中间没有空格;-t 加密方式，默认rsa;-C,C是大写，注释内容，一般填为邮箱。