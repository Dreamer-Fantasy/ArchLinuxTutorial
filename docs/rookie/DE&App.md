# 桌面环境与常用应用

官方文档: [安装后的工作](<https://wiki.archlinux.org/index.php/General_recommendations_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>)  
本文只介绍最基本的，能使系统真正意义上可用所需的组件  
相关视频链接： 2020ArchLinux 安装桌面环境和常用软件 视频文字结合效果更好

#### 1.确保系统为最新

```bash
pacman -Syyu    #升级系统中全部包
```

#### 2.准备非 root 用户

添加用户，比如新增加的用户叫 wallen

```bash
useradd -m -g users -G wheel -s /bin/bash wallen  #wheel附加组可sudo进行提权
```

设置新用户 wallen 的密码

```bash
passwd wallen
```

```bash
visudo
```

找到这样的一行,把前面的注释符号#去掉:

```bash
#%wheel ALL=(ALL) ALL
```

这里稍微解释一下
%wheel 代表是 wheel 组，百分号是前缀  
ALL= 代表在所有主机上都生效(如果把同样的`sudoers`文件下发到了多个主机上)  
(ALL) 代表可以成为任意目标用户  
ALL 代表可以执行任意命令
一个更详细的例子:

```bash
%mailadmin   snow,rain=(root) /usr/sbin/postfix, /usr/sbin/postsuper, /usr/bin/doveadm
nobody       ALL=(root) NOPASSWD: /usr/sbin/rndc reload
```

组 mailadmin 可以作为 root 用户，执行一些邮件服务器控制命令。可以在 "snow" 和 "rain"这两台主机上执行

用户 nobody 可以以 root 用户执行`rndc reload`命令。可以在所有主机上执行。同时可以不输入密码。(正常来说 sudo 都是要求输入调用方的密码的)

:wq 保存并退出即可。

#### 3.安装 KDE Plasma 桌面环境

```bash
pacman -S plasma-meta #安装plasma-meta元软件包
```

#### 4.安装 greeter sddm

```
pacman -S sddm
```

重启电脑，即可看到欢迎界面，输入新用户的密码即可登录桌面

#### 5.开启 32 位支持库与 ArchLinuxCN 支持库

进入桌面后，搜索 terminal，可以找到 konsole。它是 KDE 桌面环境默认的命令行终端。

```bash
sudo vim /etc/pacman.conf
```

去掉[multilib]一节中两行的注释，来开启 32 位库支持。  
在文档结尾处加入下面的文字，来开启 ArchLinuxCN 源。

```bash
[archlinuxcn]
SigLevel = Optional TrustAll
Server = http://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

上面服务器的地址是清华的，也可用下面中科大的

```bash
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

最后:wq 保存退出，刷新 pacman 数据库

```bash
sudo pacman -Syyu
```

#### 6.安装基础功能包

<!-- 3：安装自动补全工具  待确认这个包是否在kde
    pacman -S bash-completion
     -->

接下来我们安装一些基础功能包

```bash
sudo pacman -S ntfs-3g                          #识别NTFS格式的硬盘
sudo pacman -S adobe-source-han-serif-cn-fonts  #安装一个开源中文字体
sudo pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra  #安装谷歌开源字体
sudo pacman -S firefox chromium                 #安装常用的火狐、谷歌浏览器
sudo pacman -S yay                              #yay命令可以让用户安装AUR中的软件
```

[AUR](https://aur.archlinux.org/)

#### 7.设置系统为中文

如果想要系统换为中文，需要重新设置 locale

编辑 /etc/locale.gen，去掉 zh_CN.UTF-8 的注释符号（#）。

然后使用 locale-gen 生成 locale。

    locale-gen

编辑

    /etc/locale.conf

    echo 'LANG=zh_CN.UTF-8'  >> /etc/locale.conf

#### 8.安装输入法

对比目前的几种输入法方案，百度、搜狗、中州韵等，此处推荐选用 fcitx5 的默认输入法。  
[Fcitx5](https://wiki.archlinux.org/index.php/Fcitx5)

#### 9.显卡驱动

现在是 2020 年，显卡驱动的安装在 Arch Linux 上已经变得非常容易。

- 英特尔核心显卡

```bash
sudo pacman -S xf86-video-intel #英特尔核显
```

- Nvidia 显卡

  - 若为台式机，拥有独立的显卡，直接安装如下两个包即可。

  ```bash
  sudo pacman -S nvidia nvidia-settings
  ```

  [台式机显卡官方文档](https://wiki.archlinux.org/index.php/NVIDIA)

  - 若为笔记本，除上述两个包，推荐安装 optimus-manager。可以在核心显卡和独立显卡间轻松切换。

  ```
  sudo yay -S optimus-manager
  ```

  [笔记本双显卡官方文档](https://wiki.archlinux.org/index.php/NVIDIA_Optimus)

- AMD 显卡  
  群主目前没有 AMD 显卡或者笔记本，无法提供最佳实践

```bash
sudo pacman -S xf86-video-amdgpu    #amd显卡
```

#### 后续

如果作为一个普通使用者，到这里你的系统已经配置完毕了。不会命令行也没太大关系，你可以慢慢探索 KDE 这个桌面环境，记住每天用如下命令更新系统即可。

```bash
sudo pacman -Syyu
```

接下来你可以查阅娱乐、办公、多媒体等章节了解更多使用软件的安装与使用。如果你需要成为一名较为专业的人员，那么请阅读进阶、高级、以及编程章节。