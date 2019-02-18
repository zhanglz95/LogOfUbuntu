# 安装Ubuntu的记录

这个页面记录安装Ubuntu的过程，包括一些必要的设置和安装必要软件的过程。
此记录在Ubuntu18.04测试。

## 更换源

为了更快的速度，我们首先修改阿里源为默认源。

### 备份
```markdown
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

### 在sources.list文件前面添加如下条目，即阿里源
```markdown
sudo gedit /etc/apt/sources.list
```
```markdown
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```
### 另附其他源条目
### 中科大
```markdown
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```
### 163
```markdown
deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
```
### 清华
```markdown
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

## 更新软件仓库列表

更新系统内的软件仓库列表，获取最新的软件列表

```markdown
sudo apt-get update
```

然后需要更新本地的软件，下面这行命令会将系统所有软件与列表比对并且更新

```markdown
sudo apt-get upgrade
```

## 设置虚拟显示器
主机可能大部分时间远程控制，这个时候需要设置虚拟显示器来保持远程访问分辨率。
Ubuntu18.04一直没搞定这个问题，最简单的方法去淘宝买一个显卡欺骗器……
解决。

## 安装chrome
直接去官网下载后双击安装[chrome](https://www.google.cn/chrome/)

这里我还需要安装一个SwitchOmega来配合shadowsocks科学上网。
__待更新__

## 安装teamviewer
首先去官网下载相应的安装包[teamviewer](https://www.teamviewer.com/zhcn/download/linux/)，下载完成后，在对应下载目录执行命令
```markdown
sudo dpkg -i [安装包名称]
```
如果出错了，执行命令
```markdown
sudo apt install -f
```
然后再安装

如果要卸载teamviewer
```markdown
apt purge teamviewer
```

## 安装显卡驱动（如果有）
按照这里的教程进行安装[安装显卡教程](https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-18-04-bionic-beaver-linux#h7-automatic-install-using-ppa-repository-to-install-nvidia-beta-drivers)。
### 下载驱动
先查看自己显卡型号
```markdown
lspci |grep VGA
```
NVIDIA官网选择合适的驱动程序[NVIDIA驱动](https://www.nvidia.com/Download/index.aspx)。

### 禁用nouveau
```markdown
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
```
然后检查是否成功。
```markdown
cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf
blacklist nouveau
options nouveau modeset=0
```
刷新再重启
```markdown
sudo update-initramfs -u
sudo reboot
```

### 安装驱动
重启后进入命令行Ctrl+Alt+F4(不同电脑不一样，一般在F1-F6)，进入刚下载驱动的目录。
```markdown
sudo service lightdm stop
#或sudo service gdm stop
sudo sh ./[驱动文件名.run]
```
安装完成后验证安装是否成功。
```markdown
sudo nvidia-smi
```
显示显卡信息则安装成功。

## 安装cuda
这一步[参考教程](https://zhuanlan.zhihu.com/p/47330858)

## 安装anaconda
这一步[参考教程](https://blog.csdn.net/lwplwf/article/details/79162470)
### 下载
地址：https://www.anaconda.com/download/#linux

### 安装
进入安装包所在目录，执行命令
```markdown
bash [安装包名]
```
一路回车，有时候需要输入yes。

问到是否安装VSCode时候可以选择No。

再执行命令让.bashrc中添加的路径生效：
```markdown
source ~/.bashrc
```
搞定

## 安装pytorch
Coming soon...

## 安装tensorflow
Coming soon...

## 安装git+ssh
这里参考ubuntu git的[安装教程](https://segmentfault.com/a/1190000013154540)
### 安装git
安装并配置用户信息
```markdown
sudo apt-get install git
git config --global user.name "Your Name"
git config --global user.email Your_email@example.com
```

### 配置ssh
安装ssh
```markdown
sudo apt-get install ssh
```
创建密钥文件
```markdown
ssh-keygen -t rsa -C "你的github账号邮箱"
#这里会要求输入密钥口令，留空即可
```
密钥存储在~/.ssh文件夹中。将id_rsa.pub文件内容全部复制添加到github即可。
测试：
```markdown
ssh -T git@github.com
#注意，这里需要输入yes后回车，不能留空！
```
如果结果为 “ ...You've successfully authenticated, but GitHub does not provide shell access”，则说明成功。

## 额外记录
### 键鼠没有反应BUG
突然键鼠不能用，网上查到是需要安装一个包：http://forum.ubuntu.org.cn/viewtopic.php?t=487790
```markdown
sudo apt install xserver-xorg-input-all
```
