# Ubuntu深度学习环境入坑记

这个页面记录安装Ubuntu配置深度学习环境的过程，包括一些必要的设置和安装必要软件的过程。
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

## 科学上网
安装shadowsocks
```markdown
sudo apt install libsodium-dev 
sudo pip install https://github.com/shadowsocks/shadowsocks/archive/master.zip -U
```

任意地方新建一个配置文件shadowsocks.config:
```markdown
{ "server":"*****", "server_port":*****, "local_port":1080, "password":"*****", "timeout":600, "method":"chacha20-ietf-poly1305" }
```

启动
```markdown
sslocal -c /etc/shadowsocks.json -d start
```

终端内使用，需安裝 proxychains
```markdown
sudo apt-get install proxychains
```
编辑 /etc/proxychains.conf，只修改最后一行。
```markdown
socks5 127.0.0.1 1080
```
接着我们就可以直接 用 proxychains + 命令的方式使用代理，例如
```markdown
proxychains curl xxxx
proxychains wget xxxx
sudo proxychains apt-get xxxx
```

这里我还需要安装一个SwitchOmega来配合shadowsocks科学上网。
[SwitchOmega教程](https://portal.shadowsocks.ch/index.php?rp=/knowledgebase/50)
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

### teamviewer检测到商业用途
Ubuntu18.04.1：

teamview就是通过id来绑定终端的，id是通过网卡mac计算出来的，不改id的话，即使卸载重装，重启等操作，打开teamviewer可以看到ID是不会变的。

1.删除计算机上原有的teamviewer：

sudo apt --purge remove teamviewer
rm -rf /home/xxx/.local/share/teamviewer
rm -rf /home/xxx/.local/share/teamviewer14
rm -rf /home/xxx/.config/teamviewer

2.永久修改mac地址：

sudo gedit /etc/init.d/rc.local

在里面添加下面几行

sudo /sbin/ifconfig eth0 down

sudo /sbin/ifconfig eth0 hw ether 00:21:11:70:DD:EE（填新的MAC地址）

sudo /sbin/ifconfig eht0 up

3.点击右上角网络连接->wire connected->wired settings ->Identity ->Cloned Address -> 填上上面的新的MAC地址

或者在设置 ->Network ->wired 小齿轮 ->Identity ->Cloned Address -> 填上上面的新的MAC地址

4.下载安装teamviewer：

 sudo dpkg -i teamviewer_14.1.3399_amd64.deb

sudo apt install -f

``测试未通过``

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
这一步记录部分步骤，若有出错看原教程。

### 下载
首先去下载符合自己要求的CUDA：[CUDA Toolkit](https://developer.nvidia.com/cuda-downloads),安装方式选择runfile。

### 安装
__这里默认插上了支持Cuda的GPU、禁用了nouveau驱动等操作__

重启电脑，进入登录界面的时候，不要登录进入桌面(否则可能会失败，若不小心进入，请重启电脑)，直接按Ctrl+Alt+F4进入文本模式（命令行界面，不同电脑输入不同），然后登录账户。登录后输入：
```markdown
sudo service lightdm stop 
#sudo service gdm stop 
```
然后切换到cuda安装文件的路径：例如我的cuda在下载文件夹里所以我要输入：
```markdown
cd Downloads
sudo sh [安装包名称]
```
开始安装，按提示一步步操作，按住回车看完声明。按照提示输入相应字符，例如有的需要输入accept，有的需要输入yes。

遇到提示是否安装openGL ，选择no（如果你的电脑跟我一样是双显，且主显是非NVIDIA的GPU在工作，需要选择no），其他都选择yes或者默认即可。（如果您的电脑是双显卡且在这一步选择了yes，那么你极有可能安装完CUDA之后，重启图形化界面后遇到登录界面循环问题：输入密码后又跳回密码输入界面。这是因为你的电脑是双显，而且用来显示的那块GPU不是NVIDIA，则OpenGL Libraries就不应该安装，否则你正在使用的那块GPU（非NVIDIA的GPU）的OpenGL Libraries会被覆盖，然后GUI就无法工作了。）

安装成功后，会显示installed，否则会显示failed。

然后重新启动图形化界面，输入：
```markdown
sudo service lightdm start 
#sudo service gdm start 
```
这时会进入图形界面，否则需要手工进入。

然后重启。

执行：
```markdown
ls /dev/nvidia*
```
若显示：
```markdown
/dev/nvidia0      /dev/nvidiactl      /dev/nvidia-uvm
```
则成功，否则看原教程找解决方案。

### 设置环境变量
终端输入：
```markdown
sudo gedit /etc/profile
```
文件末尾复制添加（64位）：
```markdown
export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64\
${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
保存文件，并重启。

### 验证环境变量
a.验证驱动版本
```markdown
cat /proc/driver/nvidia/version
```
输出类似：
```markdown
NVRM version: NVIDIA UNIX x86_64 Kernel Module  410.48  Thu Sep  6 06:36:33 CDT 2018
GCC version:  gcc version 7.3.0 (Ubuntu 7.3.0-27ubuntu1~18.04) 
```

b.验证CUDA Toolkit
```markdown
nvcc -V
```
输入类似：
```markdown
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sat_Aug_25_21:08:01_CDT_2018
Cuda compilation tools, release 10.0, V10.0.130
```
全部安装成功

### 尝试编译cuda提供的例子
打开终端输入：
```markdown
cd ~/NVIDIA_CUDA-10.0_Samples
make
```
这一步会耗费一点时间进行编译，最后如果编译成功，最后会显示Finished building CUDA samples。

然后测试cuda设备连接情况
```markdown
cd ~/NVIDIA_CUDA-10.0_Samples/bin/x86_64/linux/release/
sudo ./bandwidthTest
```

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

## 添加国内源
### 设置清华源镜像
```markdown
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
 ```
### 设置pytorch镜像
```markdown
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/peterjc123/
```
## 安装pytorch
[pytorch官网](https://pytorch.org/)

## 安装tensorflow
[tensorflow官网](https://www.tensorflow.org/)

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

## BUG记录
### 键鼠没有反应BUG
突然键鼠不能用，网上查到是需要安装一个包：http://forum.ubuntu.org.cn/viewtopic.php?t=487790
```markdown
sudo apt install xserver-xorg-input-all
```
### /dev/sda2 recovering journal 现象的解决办法
参考[博客](https://www.cnblogs.com/hutao722/p/9441674.html)

1. 在卡住不动的界面我们需要切换到命令行模式，Ctrl+Alt+F3（跟机器有关，大概是在F1~F6之间）

2. 检查显卡驱动是否已经损坏 cat /proc/driver/nvidia/version，若不能看到，则说明有问题

3. 卸载已安装显卡驱动：

```markdown
sudo apt purge nvidia-*
sudo apt autoremove
```

4. 在官网下载对应的显卡驱动程序，官网地址为：https://www.nvidia.com/Download/index.aspx，选择好对应的显卡驱动后，下载后的文件名为NVIDIA-Linux-x86_64-390.77.run

5. 安装显卡驱动：sudo bash NVIDIA-Linux-x86_64-390.77.run

6. 一些询问信息：

   1）Accept License

   2）The distribution-provided pre-install script failed! Are you sure you want to continue? -> CONTINUE INSTALLATION

   3）Install NVIDIA's 32-bit compatibility libraries? ->YES

   4) An incomplete installation of libglvnd was found. Do you want to install a full copy of libglvnd? This will overwrite any existing libglvnd libraries.-> Install and overwrite existin

   4) Would you like to run the nvidia-xconfig utility? -> YES

7. 安装完成
