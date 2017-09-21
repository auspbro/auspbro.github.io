---
title: Windows 测试 master 部署
date: 2017-09-21 21:27:38
tags:
---

## 1. USB Dongle driver

## 2. install Dell webcam app

## 3. disable UAC (system & regedit) 
        { 1. Access the registry remotely from another machine, and set the registry entryt EnableLua to 0.

         HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
         EnableLua=0 (dword)

         2. Logon as the built-in administrator account, which does not process UAC.
       }

## 4. skip Win8 Metro UI

## 5. set Win8 test mode

## 6. turn off Hibernate (admin command: powercfg -h off)

## 7. run media player firstly

## 8. Power management set to "do nothing"

## 9. Turn off system protection->local disk on -> configure-> Turn off system protection-> OK

## 10. BSOD- disable automatically restart

## 11. Clear d:\testlog\.

## 12. Change time zone to UTC +8

## 13. Dell WBDD memory confirgue: control panel>all control panel items>administrative tools>local palicy>user rights assignment>lock pages in memory>every one

