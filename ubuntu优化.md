```
远程软件需要用unbuntu on xorg登陆
echo $XDG_SESSION_TYPE

ubuntu snap商店国内加速
sudo apt-get install snapd
sudo snap install snap-store
sudo snap install snap-store-proxy
sudo snap install snap-store-proxy-client

微信, 企业微信(deepin),（qq上腾讯官网）
微信
https://www.ubuntukylin.com/applications/
企业微信
!!!!  https://deepin-wine.i-m.dev/  !!!!
git clone https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu.git
http://packages.deepin.com/deepin/pool/non-free/d/deepin.com.weixin.work/
####  企业微信需要注释掉 /opt/apps/com.qq.weixin.work.deepin/files/run.sh 下面的两行代码, 不然会无法启动 ####
#### 
   #export WINEDLLPATH=/opt/$APPRUN_CMD/lib:/opt/$APPRUN_CMD/lib64
   #export WINEPREDLL="$ARCHIVE_FILE_DIR/dlls"
#### 
企业微信头像问题
sudo apt install libjpeg62:i386

winetricks 32架构问题
rm -rf ~/.wine/
WINEARCH=win32 WINEPREFIX=~/.wine winecfg
winetricks riched20


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
gimp(图像编辑),vlc(视频播放),kdenlive/shotcut(视频编辑),snap-store,nap-store-proxy,snap-store-proxy-client(应用商店)

字体下载
https://fontsdata.com/find.php?q=mt+extra
```
