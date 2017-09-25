---
title: Windows 常用软件的静默安装参数
date: 2017-09-25 13:26:12
tags:
---

## 一、如何得到软件的静默安装参数

* 软件如果已经安装，到注册表中查询其安装/卸载参数：

看InstallSource（如果有）和UninstallString的参数内容信息
* 第三方软件查询静默安装参数
* 手工测试：
反正拿到一个安装程序，用/?查询下。
如果不支持/?参数，还可以用各静默安装参数试试就知道了（ [/S] [/silent [/noreboot]] [/verysilent [/sp-] [/norestart]] [/q] [/qn] [/qb] [REBOOT=SUPPRESS] [/s /v/qn] [/q:a /r:n] [/u /n /z] [/quiet[/SilentInstallNoSponsor] [/SilentInstall] [/s /qn] [/s /qd] [-s] [-q] 等）
这步比较枯燥，但通常都比较有效。
* 试完上面的参数，表面上看好像软件不支持静默安装，此时，可以考虑解压安装包：
优先测试软件自带方法解压而支持静默安装：如：
office 2003用/a参数解压；Office 2007 Service Pack补丁包用/extract解压；ACDSee10 /a解压
不支持自带参数解压的用WinRAR或7-ZIP解压安装包，直接提取安装文件，执行静默安装
* 还可以改软件配置文件，执行静默安装
如：Total Commander：用winrar解压出来，修改install.inf中auto=1即可自动安装
* 如果还不行，想办法获取到该软件支持静默安装的版本，或重新打包版本，或用Au3的键盘鼠标自动点击安装吧

## 二、常用软件静默参数
个人感觉 InstallShield 封装的执行静默安装不太稳定，有时会莫名的安装失败，也比较占用资源。其它格式的都还可以。如：Google SketchUp 6 用InstallShield 封装的，静默部署失败率让我很头疼。
       1. .msi 格式
       msiexec.exe /i ActiveSync.msi /quiet /norestart 

       2. .msp 格式
       msiexec.exe /p hotfix.msp /quiet /norestart

       3. 系统运行库
               ○ Visual C++ 2008 (VC8)解压出来得到msi文件 (官方下载的是exe文件，通过.exe /?查询也是/Q，但部署/Q和/q均报错，所以用解压部署的方法)
               ○ Visual C++ 2009(VC9).exe /q
               ○ Visual C++ 2010 (VC10).exe /q
               ○ Microsoft .NET Framework 1.1解压出来得到msi文件 (官方下载的是exe文件，通过.exe /?查询也是/Q，但部署/Q和/q均报错，所以用解压部署的方法)
               ○ Microsoft .NET Framework 2.0.exe /q
               ○ Microsoft .NET Framework 3.0.exe /q
               ○ Microsoft .NET Framework 3.5.exe /q
               ○ Microsoft .NET Framework 4.0.exe /q

       4. 系统常用
               ○ Windows补丁，通常都是/quiet /norestart即可。如WindowsXP-KB915865-v11-x86-CHS.exe
               ○ 
               ○ WinRAR.exe /S
               ○ 7-ZIP.exe /S
               ○ Google拼音.exe /S
               ○ 搜狗拼音.exe /S
               ○ 驱动精灵2012.exe /S
               ○ CPU-Z v1.60 官方中文安装版.exe /S
               ○ Google Picasa v3.9.135.93 官方简体中文版.exe /S
               ○ 迅雷看看播放器 v4.8.8.966.exe /S
               ○ 网易有道桌面词典 5.0 正式版.exe /S
               ○ 微软雅黑字体for XP.exe /Q
               ○ VMware-workstation-full-8.0.2-591240.exe /silent /noSilentReboot

       5. 即时通讯
               ○ 阿里旺旺淘宝买家版 2011 正式版 SP1.exe /S
               ○ 飞信 2012 贺岁版 v4.7.0.exe /S
               ○ 腾讯 QQ 2011 正式版.exe /S
               ○ 腾讯 TM 2009 Beta v3.3.exe /S
               ○ 腾讯通 RTX 2011 正式版.exe /S

       6. 文字办公
               ○ Office 2003 Professional
       方法1：
       对office2003原始安装光盘进行重新封装（也称：管理员安装方式），使其支持网络安装。运行原始安装光盘下的Setup.exe /a，然后输入序列号，选择一个文件夹作为office安装点。（如在硬盘上的d:\office）

       运行msiexec.exe /i Pro11.msi /quiet /norestart即可
       方法2：
       Step3：制作定制安装的MST文件：运行office 2003 resource kit工具的Custom Installation Wizard，创建一个对应的MST文件，存放在Office 2003目录下。（在setup和pro11.msi文件同一目录）
       msiexec.exe /a pro11.msi /p setup.msp
               ○ Office 2003 兼容包
       FileFormatConverters.exe /quiet /norestart

       ○ Office System 2007 /Office System 2010  (包括Project/Visio)
       运行setup.exe /admin，用Office 2007自定义工具生成.msp文件（包含同意许可协议、序列号、注册信息、显示级别等内容），msp文件名字可随意取。(下图为：Auto_Office_2007.msp，其它的msp是Office 2007 Service Pack 3运行/extract后解压出来的安装文件)

       将定制后的msp放到Office 2007/2010的Updates目录后，运行根目录下的setup.exe 即可实现全自动安装（office 2007/2010的Service Pack补丁包同样适用，放入Updates目录即可）
       另外要集成Office 2007或2010的Service Pack，只需要运行Service Pack的exe安装程序后加/extract参数到Office 的Updates目录即可。

               ○ Adobe PDF Reader v9.1.exe /sAll

               ○ Adobe Acrobat X Pro v10.1.0
       解压出来，得到msi文件，msiexec.exe /i AcroPro.msi /quiet /norestart
               ○ Foxit PDF Reader v5.1.0.1117.exe /S
               之前的版本是 -i

               ○ Foxmail.exe 6.0.exe /sp- /verysilent /norestart
               ○ Google金山词霸.exe  /S

       7. 网络与安全
               ○ 微软TMG防火墙客户端
       msiexec.exe /i TMGClient.msi  ENABLE_AUTO_DETECT=1 REFRESH_WEB_PROXY=1 /quiet /norestart
               ○ Adobe Flash Player ActiveX for IE v11.3
       10之前的版本好像是/S，之后的版本通过查询注册表得知静默安装是/install，卸载是-maintain activex
               ○  IE 8
       IE8-WindowsXP-x86-CHS.exe /quiet /update-no /promptrestart

               ○ Microsoft Security Essential (MSE)
       mseinstall_xp_v2.1.1116.exe /s /q /o /runwgacheck
               ○ 360安全卫士.exe  /S
               ○ 超级兔子.exe  /S
               ○ PPLive.exe /S
               ○ 迅雷.exe  /S

       8. 手机相关
               ○ 91手机助手
       需要Microsoft .NET Framework 2.0，msiexec.exe /i netfs.msi  /quiet /norestart
       91mobilesetup_v3.1.8.583.exe /sp- /verysilent /norestart
               ○  iTunes
       解压出来均是msi格式文件，只需要注意参考下面的安装顺序即可
       (1) AppleApplicationSupport.msi
       (2) AppleMobileDeviceSupport.msi"
       (3) AppleSoftwareUpdate.msi"
       (4) Bonjour.msi"
       (5) iTunes.msi"
               ○ Nokia 套件
       同理解压出来均是msi文件，顺序：
       (1) Packages\CCD\Setup\Nokia_Connectivity_Cable_Driver.msi
       (2) Packages\PCCS\Setup\PCCS.msi
       (3) Packages\VC80_x86\Setup\VC80_x86_v2.msi  或 Packages\VC80_x64\Setup\VC80_x64_v2.msi
       (4) Packages\Nokia_Suite\Setup\Nokia_Suite.msi

       9. 设计类
               ○ AutoCAD 2004/AuctoCAD 2007
       找到AutoCAD\ACAD.msi文件,参数为
       msiexec.exe /i  acad.msi ACADSERIALPREFIX=<序列号前缀:111> ACADSERIALNUMBER=<序列号:20111111>  ACADFIRSTNAME=<注册者姓> ACADLASTNAME=<注册者名> ACADORGANIZATION=<注册单位>  ACADDEALER=<经销商>  ACADDEALERPHONE=<经销商电话12345>  /quiet

       ○ Google SketchUp v8.0.11752 中文免费版
       解压得到msi
       ○ CorelDraw 较老版本：
       Setup32.exe /silent（很少界面的静默安装）；新版本是MSI