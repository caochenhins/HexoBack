---
title: win10 Linux子系统 完美搭建
tags:
  - Linux
categories:
  - 部署
toc: true
abbrlink: '5'
date: 2019-03-20 20:53:18
---

# 1.Xshell完美连接win10 Linux子系统

## 配置SSH服务
sudo apt-get remove --purge openssh-server   ## 先删ssh
sudo apt-get install openssh-server          ## 在安装ssh  

sudo rm /etc/ssh/ssh_config                  ## 删配置文件，让ssh服务自己想办法链接
sudo service ssh --full-restart
<!-- more -->
 上面命令执行完之后，在xshell中输入用户名和ip就可以通过xshell登录自己电脑的Linux。

## 配置永久解决方案
通过上面的方法，我们可以通过xshell登录自己电脑的Linux。
### 1.修改配置文件
sudo nano /etc/sudoers
加上这句:  
hins ALL=NOPASSWD: ALL
ctrl+o 保存
ctrl+x 退出

### 2.新建bat命令:
bash -c 'sudo service ssh --full-restart'

### 3.Win10设置开启启动
shell:startup
将新建的bat命令文件丢入开始文件夹

# 2.修改root密码:
passwd root

# 3.linux下开启SSH，允许root用户远程登录
## 1. 允许root用户远程登录
修改ssh服务配置文件
sudo vi /etc/ssh/sshd_config
调整PermitRootLogin参数值为yes，如下图：
![1.png](/images/2019/03/20/b8f17cd0-4b0e-11e9-bf42-b5cff98e2bd6.png)
## 2. 允许无密码登录
同上，修改ssh服务配置文件，两种情况：
1） 将PermitEmptyPasswords yes前面的#号去掉
2） 将PermitEmptyPasswords 参数值修改为yes，如下图：
![2.png](/images/2019/03/20/bc469b40-4b0e-11e9-bf42-b5cff98e2bd6.png)

无论哪种，最后PermitEmptyPasswords参数值为yes
以上两种配置，均需要重启ssh服务
service sshd restart  # 或者
/etc/initd.d/sshd restart

# 4.win10 Linux子系统安装rz命令
执行以下命令安装:
 sudo add-apt-repository main
 sudo add-apt-repository universe
 sudo add-apt-repository restricted
 sudo add-apt-repository multiverse

 sudo apt-get update

 sudo apt-get install lrzsz


# 5.安装Jdk
如果Java当前未安装，你将看到以下输出：
Command 'java' not found, but can be installed with:

apt install default-jre
apt install openjdk-11-jre-headless
apt install openjdk-8-jre-headless
apt install openjdk-9-jre-headless

执行以下命令来安装OpenJDK：
sudo apt install default-jre

该命令将安装Java运行时环境（JRE）。这将允许你运行几乎所有的Java软件。
验证安装：
java -version