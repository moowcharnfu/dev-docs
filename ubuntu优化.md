```
远程软件需要用unbuntu on xorg登陆
echo $XDG_SESSION_TYPE

ubuntu snap商店国内加速
sudo apt-get install snapd
sudo snap install snap-store
sudo snap install snap-store-proxy
sudo snap install snap-store-proxy-client

微信, 企业微信, qq
用ubuntu安装虚拟机virtualbox和virtualbox-guest-additions-iso扩展插件,
然后下载一个小于4G的windows系统(ed2k://|file|cn_windows_10_business_editions_version_1909_updated_jan_2020_x86_dvd_e861101e.iso|3802454016|5B909C955D8A65CA2002D8594D137A63|/)+共享目录就可以无缝使用腾讯产品了


windows远程桌面
remmina(windows remote desktop)

7zip
sudo apt install p7zip-full
7z x origin.7z
7z a -t7z target.7z folder

删除应用
sudo apt-get --purge remove -y appname
# snap
sudo snap list
sudo snap remove chromium firefox
# 清空
sudo apt autoremove
sudo apt autopurge
sudo apt autoclean

# 删除应用图标
sudo nautilus /usr/share/applications/
sudo nautilus ~/.local/share/applications/

#　禁用休眠
systemctl status sleep.target
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target

常用软件
gimp(图像编辑),vlc(视频播放),kdenlive/shotcut(视频编辑),snap-store,snap-store-proxy,snap-store-proxy-client(应用商店)
vlc地址
http://devimages.apple.com.edgekey.net/streaming/examples/bipbop_4x3/gear2/prog_index.m3u8
rtmp://mobliestream.c3tv.com:554/live/goodtv.sdp
rtmp://liteavapp.qcloud.com/live/liteavdemoplayerstreamid

字体下载
https://fontsdata.com/find.php?q=mt+extra

###### dpkg错误修复 #####
# backup
sudo mv /var/lib/dpkg/info /var/lib/dpkg/info_old
# new
sudo mkdir /var/lib/dpkg/info
# update
sudo apt-get update
sudo apt-get -f install
# copy new to old
sudo mv /var/lib/dpkg/info/* /var/lib/dpkg/info_old/
# del new
sudo rm -rf /var/lib/dpkg/info
# recovert
sudo mv /var/lib/dpkg/info_old /var/lib/dpkg/info

---
dpkg --get-selections | grep linux-
sudo dpkg --configure -a

# 输入法卡顿重启大法
pkill -f ibus-daemon
ibus-daemon -r -d -x
sleep 0.5
ibus restart
sleep 0.5

# apt优化
sudo apt autopurge
sudo apt autoremove
sudo apt autoclean
sudo apt update
sudo apt upgrade

```
