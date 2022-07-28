---
title: 'npm waring solution'
author: l4ru3
avatar: https://cdn.jsdelivr.net/gh/L4rue/PicGoCDN@master/img/DTB银.jpg
authorLink: larue.top
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
date: 2022-07-28 20:28:46
tags:
keywords: npm
description: 关于npm出现npm WARN config global --global, --local are deprecated的解决方案
photos: https://cdn.jsdelivr.net/gh/L4rue/PicGoCDN@master/img/photos/1.jpg
---
# 关于npm出现"npm WARN config global --global, --local are deprecated"的解决方案
> 在更换新电脑之后， 重新安装node的过程中，遇到了一些问题，特此记录。该问题由npm的特定版本引起，由于之前使用的node版本较低，捆绑的npm版本较低，并没有出现该问题，而这次重装的时候，直接安装的最新的稳定版，恰巧这个版本的npm有问题.....

## 首先要说明的是，该问题由npm官方的deprecated标记导致，除非实在没有别的办法，请一定不要手动更改nodejs文件夹下的各类文件。

---

1. 解决方案1(推荐)  
卸载当前电脑上的nodejs，安装nvm，通过nvm进行版本控制，此方案的优势在于，可以随时切换npm版本，并且在windows上，可以避免npm安装位置的错误导致自行升级npm版本时出现双版本的问题。

2. 解决方案2  
自行升级npm版本
    + linux,MacOs  
        在升级之前，先查看一下当前的npm版本  
        ```
        npm -v
        ```  
        使用下面的命令将npm升级到最新版  
        ```
        npm install -g npm
        ```  
        如果要指定版本运行下面的命令  
        ```
        npm install -g npm@版本号
        ```  

    + Windows  
        在Windows上，由于在安装node时，捆绑安装的npm位置有问题，如果直接使用上述命令升级，会导致在global路径下重新安装一个新的npm，应当使用`npm-windows-upgrade`进行npm版本的管理  

        一般来说，Windows的策略组是不允许直接执行脚本的，所以在使用`npm-windows-upgrade`前，我们先更改策略  
        出于安全考虑，我建议将策略更改限定为当前PowShell，运行命令如下：  
         
        ```
        Set-ExecutionPolicy Unrestricted -Scope Process
        ```  
        （详细内容请参考[微软文档](https://docs.microsoft.com/de-de/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7.2#example-6-set-the-execution-policy-for-the-current-powershell-session)）  

        安装`npm-windows-upgrade`  
        ```
        npm install --global --production npm-windows-upgrade
        ```  
        更新npm  
        ```
        npm-windows-upgrade --npm-version latest
        ```  
        如果在最后一步更新的时候，找不定指定的命令（如下），
        ```
        npm-windows-upgrade : The term 'npm-windows-upgrade' is not recognized as the name of a cmdlet, function, script file,
        or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and
        try again.
        At line:1 char:1
        + npm-windows-upgrade ....
        ```
        是因为缺少path路径，请将global路径追加到系统path变量中，  
        或者使用`dir $(npm -g bin)`  切换到`npm-windows-upgrade`的安装路径下再执行

---
> 参考内容：  
> https://stackoverflow.com/questions/72401421/message-npm-warn-config-global-global-local-are-deprecated-use-loc  
> https://github.com/felixrieseberg/npm-windows-upgrade/issues/134  
> https://www.npmjs.com/package/npm-windows-upgrade