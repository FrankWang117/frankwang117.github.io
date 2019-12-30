
# 在 ubuntu 上 安装 git

# 安装 RVM 
Install GPG keys:
``` terminal 
    gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

``` 
如果出现 `command 'gpg2' not found` 之类的，则需要安装 `gpg2`
``` terminal
    sudo apt install gnupg2
``` 

Install RVM:
``` terminal
    \curl -sSL https://get.rvm.io | bash -s stable
``` 
``` terminal
    rvm -v
```
如遇网络问题，重试几次即可。
# 安装 ruby
查看当前是否安装或安装版本
``` terminal
    ruby -v
```
执行下面命令进行安装：  
``` terminal
    rvm requirements
    rvm install 2.4.6
```