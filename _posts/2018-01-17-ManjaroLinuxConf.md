---
layout: post
title: Manjaro Linux 配置
tags: 操作系统
---

![manjaro linux]({{site.baseurl}}/images/posts/manjaro.jpeg)

### 原稿修改version 7版
- 声明：个人配置，参考更新
- 用途：记录安装 manjaro 之后的初始化配置
- 硬件：小米笔记本（13.3）
- 系统：manjaro linux 17  

---

### 小米笔记本无线网（wifi）链接问题

    sudo nano /etc/modprobe.d/blacklist.conf

写入 `blacklist acer-wmi` 保存退出
重启电脑后无线网连接正常

---
### 更换hosts文件

类似VPN，若能连接外网，可略过
[点击进入LaoD博客](https://laod.cn/hosts/2017-google-hosts.html)  

---
### 如何更换和添加源

```
sudo nano /etc/pacman.d/mirrorlist
```

建议把清华的源镜像放在第一位，更新列表和系统的时候速度会快  

---

    sudo nano /etc/pacman-mirrors.conf

修改 `OnlyCountry = China` （注意把前面的注释 # 删掉）

保存退出  

---

    sudo nano /etc/pacman.conf

添加archlinuxcn

```
## 清华大学 (ipv4, ipv6, http, https)
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

---
    sudo pacman -Syy

更新一下源列表，此处若出现错误，请按照终端提示，删除一个文件
（本机是类似于/var/lib***的一个db文件）  

重新执行 `sudo pacman -Syy` 就没有问题了  

---

    sudo pacman -S archlinuxcn-keyring

此步很关键，是安装`archlinuxcn`的GPG keys  

---

滚动升级一下系统和软件(不建议频繁滚动升级，稳定为主)  

    sudo pacman -Syyu

开始更新系统了  

---

### 搜狗输入法的安装

```
sudo pacman -S fcitx-sogoupinyin
```
```
sudo pacman -S fcitx-im
```
```
sudo pacman -S fcitx-configtool
```
```
nano ~/.xprofile
```

文末添加
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```
保存退出，重启生效

---
### emacs 无法输入中文  
```
sudo nano ~/.bashrc
alias myemacs ='LC_CTYPE="zh_CN.utf8" emacs'
source ~/.bashrc
```

---

### alias 设置别名在修改了 .bashrc 之后重启依然失效

- 你如果用的是 bash ，修改 .bashrc
- 如果你用的是 `oh-my-zsh`，请去修改 `.zshrc`

---

### 键位的设置

- 交换 ctrl 和　caps
```
sudo nano /etc/profile
```

- 添加 `setxkbmap -option ctrl:swapcaps` 保存退出

**or**

- 只将　caps　替换为　ctrl (建议后者)

```
sudo vim /etc/default/keyboard
```

- 修改`XKBOPTIONS="ctrl:nocaps"`

---

### oh-my-zsh 的配置

查看本地有哪几种shell  
```
cat /etc/shells
```
manjaro 默认已经安装了 zsh  

---
切换到zsh，输入密码，连续回车确认 
```
chsh -s /bin/zsh
```

安装 oh-my-zsh的配置文件  


via curl
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
或者
via wget
```
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

当你连接不到github时，你可以采用手工配置  

- 百度网盘oh-my-zsh镜像：（从github上克隆下来的）
[点击下载](https://pan.baidu.com/s/1eSGJMb4)

- 解压到家目录，重命名为 `.oh-my-zsh`
- 创建配置文件
```
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```
- 关闭重启终端

---

### [Grub壁纸替换](https://wiki.archlinux.org/index.php/GRUB_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))


```
sudo nano /etc/default/grub
```

- GRUB_BACKGROUND="/usr/share/grub/background.png"
修改为
```
GRUB_BACKGROUND="/home/zane/Pictures/bgpic.jpg"
```

- 将GRUB_BACKGROUND="图片地址的绝对路径".

```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
