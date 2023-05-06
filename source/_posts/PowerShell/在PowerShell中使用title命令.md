---
title: 在PowerShell中使用title命令
date: 2022-10-31 02:10:22
tags: PowerShell
categories: 网络工程（杂项）
typora-root-url: ..
---

#### 在Powershell中无法像在cmd内一样愉快的使用title命令来更改标题而是使用如下命令
```powershell
$host.ui.RawUI.WindowTitle="自定义标题"
```
#### 使用这条命令来更改属实是有点麻烦，很难记，所以我们使用alias来让title命令可以在Powershell内使用
```powershell
test-path $profile
```
#### 如果返回值为false则执行如下命令
```powershell
New-Item -path $profile -type file -force
```
#### 如果返回值为true则执行如下命令

注：在执行此命令前需要电脑内安装了Visual Studio Code

```powershell
code $profile 
```
#### 运行完之后会打开一个profile文件，在该文件内输入以下代码
```powershell
function Set-WindowTitle {
    $host.UI.RawUI.WindowTitle = [string]::Join(" ", $args)
}
Set-Alias -name "title" -value Set-WindowTitle
```
#### 重启Powershell可以发现我们在PowerShell内可以使用title命令改更改窗口标题了
```powershell
title A Minecraft Server
```