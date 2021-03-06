##玩转Kali 2.0系列教程-Kali 2.0中安装LOIC离子炮
```
QQ群：346198711    Kali魔鬼训练营
LOIC简介：
LOIC被称之为低轨道离子炮，攻击效果很强。（详细说明请大家访问百度）

目前网上的大部分教程都是讲在windows下的用法，今天咱们来一起学习下如何在kali 2.0下安装配置。

温馨提示：请大家不要用此软件攻击国内的网站，此教程仅供大家学习参考，不要做违法的事情。

aptitude install git-core monodevelop      #安装monodevelop运行环境

mkdir loic && cd loic/

leafpad loic.sh      #写入代码（代码在下面）

chmod +x loic.sh      #赋予权限

./loic.sh update         #更新，可选：如果下面无法安装

./loic.sh install         #安装

apt-get install mono-gmcs        #安装依赖

./loic.sh run          #运行

-------------------------代码如下--------------------------
#!/bin/bash
# Copyfuck ? 2010 q
#
# This script installs, updates and runs LOIC on Linux.
#
# Supported distributions:
#    * Ubuntu
#    * Debian
#    * Fedora
#
# Usage: bash ubuntu_loic.bash <install|update|run>
#


GIT_REPO=http://github.com/NewEraCracker/LOIC.git
GIT_BRANCH=master


DEB_MONO_PKG="monodevelop liblog4net-cil-dev"
FED_MONO_PKG="mono-basic mono-devel monodevelop mono-tools"


lower() {
    tr '[A-Z]' '[a-z]'
}


what_distro() {
    if which lsb_release ; then
        echo lsb_release -si | lower
    elif grep -qi ubuntu /etc/*release ; then
        echo "ubuntu"
    elif [[ -e /etc/fedora-release ]] ; then
        echo "fedora"
    else
        # Assume Debian-based
        echo "debian"
    fi
}


DISTRO=$(what_distro)


ensure_git() {
    if ! which git ; then
        if [[ $DISTRO = 'ubuntu' || $DISTRO = 'debian' ]] ; then
            sudo apt-get install git
        elif [[ $DISTRO = 'fedora' ]] ; then
            sudo yum install git
        fi
    fi
}


is_loic() {
    is_loic_git || { [[ -d LOIC ]] && cd LOIC && is_loic_git; }
}


is_loic_git() {
    [[ -d .git ]] && grep -q LOIC .git/config
}


get_loic() {
    ensure_git
    if ! is_loic ; then
        git clone $GIT_REPO -b $GIT_BRANCH
    fi
}


compile_loic() {
    get_loic
    if ! is_loic ; then
        echo "Error: You are not in a LOIC repository."
        exit 1
    fi
    if [[ $DISTRO = 'ubuntu' || $DISTRO = 'debian' ]] ; then
        sudo apt-get install $DEB_MONO_PKGS
    elif [[ $DISTRO = 'fedora' ]] ; then
        sudo yum install $FED_MONO_PKS
    fi
    mdtool build
}


run_loic() {
    is_loic
    if [[ ! -e bin/Debug/LOIC.exe ]] ; then
        compile_loic
    fi
    if ! which mono ; then
        if [[ $DISTRO = 'ubuntu' || $DISTRO = 'debian' ]] ; then
            sudo apt-get install mono-runtime
        elif [[ $DISTRO = 'fedora' ]] ; then
            sudo yum install mono-runtime
        fi
    fi
    mono bin/Debug/LOIC.exe
}


update_loic() {
    ensure_git
    if is_loic ; then
        git pull --rebase
        compile_loic
    else
        echo "Error: You are not in a LOIC repository."
    fi
}


case $1 in
    install)
        compile_loic
        ;;
    update)
        update_loic
        ;;
    run)
        run_loic
        ;;
    *)
        echo "Usage: $0 <install|update|run>"
        ;;
esac


--------------------------------------------------------------------------


```
##kali安装笔记
```
汉化（这条是废话，我安装时选择中文语言）
安装VMWare Tools（这条也是废话，按照VMWare提示安装就好了）
更换官方源
修改sources.list文件：
leafpad /etc/apt/sources.list
然后选择添加以下较快的源：
#官方源 
deb http://http.kali.org/kali kali main non-free contrib
deb-src http://http.kali.org/kali kali main non-free contrib
deb http://security.kali.org/kali-security kali/updates main contrib non-free
#中科大kali源 
deb http://mirrors.ustc.edu.cn/kali kali main non-free contrib 
deb-src http://mirrors.ustc.edu.cn/kali kali main non-free contrib 
deb http://mirrors.ustc.edu.cn/kali-security kali/updates main contrib non-free 
#新加坡kali源 
deb http://mirror.nus.edu.sg/kali/kali/ kali main non-free contrib 
deb-src http://mirror.nus.edu.sg/kali/kali/ kali main non-free contrib
deb http://security.kali.org/kali-security kali/updates main contrib non-free
deb http://mirror.nus.edu.sg/kali/kali-security kali/updates main contrib non-free
deb-src http://mirror.nus.edu.sg/kali/kali-security kali/updates main contrib non-free
#debian_wheezy国内源
deb http://ftp.sjtu.edu.cn/debian wheezy main non-free contrib
deb-src http://ftp.sjtu.edu.cn/debian wheezy main non-free contrib
deb http://ftp.sjtu.edu.cn/debian wheezy-proposed-updates main non-free contrib
deb-src http://ftp.sjtu.edu.cn/debian wheezy-proposed-updates main non-free contrib
deb http://ftp.sjtu.edu.cn/debian-security wheezy/updates main non-free contrib
deb-src http://ftp.sjtu.edu.cn/debian-security wheezy/updates main non-free contrib
deb http://mirrors.163.com/debian wheezy main non-free contrib
deb-src http://mirrors.163.com/debian wheezy main non-free contrib
deb http://mirrors.163.com/debian wheezy-proposed-updates main non-free contrib
deb-src http://mirrors.163.com/debian wheezy-proposed-updates main non-free contrib
deb-src http://mirrors.163.com/debian-security wheezy/updates main non-free contrib
deb http://mirrors.163.com/debian-security wheezy/updates main non-free contrib 
保存之后运行：apt-get update && apt-get dist-upgrade
安装中文输入法
apt-get install ibus ibus-googlepinyin   
安装目标工具LOIC
kali-linux中安装方法：
    aptitude install git-core monodevelop      #安装monodevelop运行环境
    mkdir loic && cd loic/
    nano loic.sh      #写入代码或直接下载；链接点此查看
    chmod +x loic.sh      #赋予权限
    ./loic.sh update         #更新，可选：如果下面无法安装
    ./loic.sh install         #安装
    apt-get install mono-gmcs        #安装依赖
    ./loic.sh run          #运行


安装完试了一下，完全就是DoS工具嘛，没达到测试自己网站的目的，果断卸载掉。
cd ../
rm -f -r loic
apt-get remove mono-gmcs
apt-get remove monodevelop


安装flash
换linux也得娱乐不是，装个flash看个youku也不错啊，走起～
apt-get install flashplugin-nonfree
update-flashplugin-nonfree --install
```
##sshd服务开机启动
```


```
##smbus base address uninitialized upgrade BIOS or use force_addr = 0xaddr
```
Check module is being loaded
lsmod | grep i2c_piix4
If so, blacklist it in the file /etc/modprobe.d/blacklist.conf, by adding the following to the end of the file:
blacklist i2c_piix4
Update the initramfs
sudo update-initramfs -u -k all
```
##change postgres password
```
1.删除PostgreSQL用户密码
$ sudo passwd -d postgres
2.设置PostgreSQL用户密码
$ sudo -u postgres passwd
```
##linux是否安装mysql
```
$ ps -aux|grep mysql
```
##kali 忘记root密码
1.启动你的Kali Linux，等出现GRUB引导菜单时，
按向下方向键选择“恢复模式”，按E键进入编辑模式。
2.进入编辑模式后，参照下图进行修改（将ro改为rw，在后面添加init=/bin/bash）：
Kali 2.0忘记登录密码时重置密码的方法
3. 修改完成后，按F10键或Ctrl+X键继续启动。
（注意要选下面那个recoverMode启动）启动完成后，出现命令行界面。
4. 这时输入passwd root，回车就可以直接设置新密码
（修改其他用户，把root改为其他用户名即可）
5.回车以后，输入新密码，并再次输入确认，系统提示密码更新成功！