---
abbrlink: 0
---

# windows系统下的使用技巧

> 参考文档: [官方文档](https://support.microsoft.com/zh-cn/office/%e6%96%87%e4%bb%b6%e4%b8%8e%e5%ad%98%e5%82%a8-9bf80d1c-fc20-43bb-a76a-13c4316ea442?ui=zh-CN&rs=zh-CN&ad=CN)

## windows系统下的注册表
> 使用 **windows键+R键**打开运行窗口,输入**regedit**进入注册表编辑界面。
> 注册表是windows操作系统、硬件设备以及客户应用程序得以正常运行和保存设置的核心“数据库”，也可以说是一个非常巨大的树状分层结构的数据库系统。
> 参考资料：[注册表](https://www.cnblogs.com/sepmaple/articles/9401215.html)


### HKEY_CLASSES_ROOT
> 说明：该根键包括启动应用程序所需的全部信息，包括扩展名，应用程序与文档之间的关系，驱动程序名，DDE和OLE信息，类ID编号和应用程序与文档的图标等

### HKEY_CURRENT_USER
> 说明：该根键包括本地计算机的系统信息，包括硬件和操作系统信息，安全数据和计算机专用的各类软件设置信息

### HKEY_LOCAL_MACHINE
> 说明：该根键包括本地计算机的系统信息，包括硬件和操作系统信息，安全数据和计算机专用的各类软件设置信息

### HKEY_USERS
> 说明：该根键包括计算机的所有用户使用的配置数据，这些数据只有在用户登录系统时才能访问。这些信息告诉系统当前用户使用的图标，激活的程序组，开始菜单的内容以及颜色，字体

###  HKEY_CURRENT_CONFIG
> 说明：该根键包括当前硬件的配置信息，其中的信息是从HKEY_LOCAL_MACHINE中映射出来的。

## windows家庭版开启管理员权限
> windows系统一般默认关闭管理员账户,因此有些文件删除无法获得管理员权限。

### 以管理员权限打开Powershell

* net user administrator /active:yes 开启超级管理员账户
* net user administrator /active:no 关闭超级管理员账户
* net user guest /active:yes 开启来宾账户
* net user guest /active:no 关闭来宾账户

### 在本地组策略编辑器中启动管理员权限账号

> windows10家庭版没有本地组策略编辑器，因此需要安装。

* 在运行环境中输入** gpedit.msc** ，进入本地组策略编辑器.

## windows之注册表操作
> 在操作注册表之前一定首先得备份

### 修复误删的windows组件

### **sfc**命令

* 在命令行输入**sfc /scannow** (管理员权限)

### **dism**命令


> 功能：扫描全部系统文件和系统映像文件是否与官方版本一致，时间较长，耐心等待。
* 在命令行输入**Dism /Online /Cleanup-Image /ScanHealth**

> 功能：检测文件损坏程度
* Dism /Online /Cleanup-Image /CheckHealth

> 功能：进行修复

*  DISM /Online /Cleanup-image /RestoreHealth

> 功能：检查映像版本
* Dism /online /Get-CurrentEdition

> 功能：检查系统是否可升级
* Dism /online /Get-TargetEditions

> 功能：指定DISM修复源
> 下载官方原生 Windwos 10 ISO，并提取出其中的 install.wim
* DISM /Online /Cleanup-Image /RestoreHealth /Source:文件路径install.wim

