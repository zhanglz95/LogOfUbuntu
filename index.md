# 安装Ubuntu之后的步骤

这个页面记录安装Ubuntu的过程，包括一些必要的设置和安装必要软件的过程。
若无提及，此记录在Ubuntu18.04测试。

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
Coming soon...

## 安装chrome
直接去官网下载后双击安装[chrome](https://www.google.cn/chrome/)

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
这里按照这里的教程进行安装[安装显卡教程](https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-18-04-bionic-beaver-linux#h7-automatic-install-using-ppa-repository-to-install-nvidia-beta-drivers)。
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
Coming soon...

## 安装anaconda
Coming soon...

## 安装pytorch
Coming soon...

## 安装tensorflow
Coming soon...

## 安装git+ssh
Coming soon...
