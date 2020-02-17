1.下载地址

https://www.google.cn/intl/zh-CN/chrome/browser/thankyou.html?standalone=1&system=true&platform=win&installdataindex=defaultbrowser

2.强制允许网站播放flash

[新建文件.reg]

Windows Registry Editor Version 5.00  

[HKEY_CURRENT_USER\SOFTWARE\Policies\Chromium] 

"AllowOutdatedPlugins"=dword:00000001 

"RunAllFlashInAllowMode"=dword:00000001 

"DefaultPluginsSetting"=dword:00000001 

"HardwareAccelerationModeEnabled"=dword:00000001 

[HKEY_CURRENT_USER\SOFTWARE\Policies\Chromium\PluginsAllowedForUrls] 

"1"="https://*" 

"2"="http://*" 

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome] 

"AllowOutdatedPlugins"=dword:00000001 

"RunAllFlashInAllowMode"=dword:00000001 

"DefaultPluginsSetting"=dword:00000001 

"HardwareAccelerationModeEnabled"=dword:00000001 

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\PluginsAllowedForUrls] 

"1"="https://*" 

"2"="http://*"

添加完注册表后从policy可以看到flash已经允许

chrome://policy/

3. 如果flash有声音黑屏, 设置 decode为 disable

chrome://flags/#disable-accelerated-video-decode
