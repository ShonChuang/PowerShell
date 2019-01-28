---
description: 在 PowerShell 上，使用 WebAdministratorion Module 來操作管理 IIS。
---

# IIS PowerShell

### 載入模組

```text
PS C:\>Import-Module WebAdministration //載入 IIS 管理模組
```

模組會建立虛擬磁碟名稱為 IIS，在磁碟中會有兩個虛擬資料夾 AppPools、Sites。

```bash
// 查看 Drives
PS C:\> Get-PSDrive | select Name

Name        
----        
......
IIS                                  
......

// 進入 IIS 虛擬磁碟，查看目錄

PS C:\> cd IIS:
PS C:\> dir

Name
----
AppPools
Sites
```

## 讀取

### [Get-website](https://technet.microsoft.com/zh-tw/library/hh867835%28v=wps.630%29.aspx) 查詢 Server 網站資料

```text
查詢所有網站
get-website / get-iissite

查詢 default web site
get-website -name "default web site"
```

### [Get-webItemState](https://technet.microsoft.com/zh-tw/library/hh867872%28v=wps.630%29.aspx) 取得網站啟用狀態

```text
查詢所有網站啟用狀態
Get-WebItemState "IIS:\sites\*" 
Value
-----
Stopped
Stopped
Started

查詢 default 網站啟用狀態
Get-webitemstate -psPath "IIS:\sites\Default Web Site"
Value
-----
Stopped
```

### [Get-WebBinding](https://technet.microsoft.com/zh-tw/library/hh867866%28v=wps.630%29.aspx) 取得網站繫節

```text
Get-WebBinding

protocol bindingInformation sslFlags
-------- ------------------ --------
http     *:80:                     0
http     *:80:                     0
http     *:80:                     0
```

### [Get-WebAppPoolState](https://technet.microsoft.com/zh-tw/library/hh867896%28v=wps.630%29.aspx) 取得 AppPoolState 狀態

```text
Get-WebAppPoolState -name "Default*"

Value
-----
Started
```

### Get-IISAppPool 

```text
Get-IISAppPool
Name                 Status       CLR Ver  Pipeline Mode  Start Mode
----                 ------       -------  -------------  ----------
DefaultAppPool       Started      v4.0     Integrated     OnDemand
Classic .NET AppPool Started      v2.0     Classic        OnDemand
.NET v2.0 Classic    Started      v2.0     Classic        OnDemand
.NET v2.0            Started      v2.0     Integrated     OnDemand
.NET v4.5 Classic    Started      v4.0     Classic        OnDemand
.NET v4.5            Started      v4.0     Integrated     OnDemand
```

### [Get-WebConfigFile](https://technet.microsoft.com/zh-tw/library/hh867847%28v=wps.630%29.aspx) 取得web.config

```text
Get-WebConfigFile -PSPath "iis:\sites\Default Web Site"

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----      2018/10/3  上午 11:37          76609 applicationHost.config
```

### [Get-WebConfigurationProperty](https://technet.microsoft.com/zh-tw/library/hh867890%28v=wps.630%29.aspx) 讀取 IIS 設定檔

```text
讀取 IIS 預設文件
Get-WebConfigurationProperty -Filter //defaultDocument/files/add -PSPath 'IIS:\Sites\Teconsole' -Name value | select
value
// -Filter 必填 -name 必填
Value
-----
Default.htm
Default.asp
index.htm
index.html
iisstart.htm
default.aspx
```

### [Get-WebURL](https://technet.microsoft.com/zh-tw/library/hh867874%28v=wps.630%29.aspx) 取得繫結網域名稱

```text
Get-WebURL -PSPath 'IIS:\Sites\Default Web Site'

ResponseUri       Status Description
-----------       ------ -----------
http://localhost/ OK     OK
```

### [Get-WebVirtualDirectory](https://technet.microsoft.com/zh-tw/library/hh867894%28v=wps.630%29.aspx) 讀取虛擬目錄

```text
Get-WebVirtualDirectory -Site 'Default Web Site'

Name             Physical Path
----             -------------
test             C:\Project\test
```

## 新增

### [New-WebApplication](https://technet.microsoft.com/zh-tw/library/hh867877%28v=wps.630%29.aspx) 新增網站

-Name：網站名稱  
-Sist：網站建立節點，若有則為子網站  
-PhysicalPath：真實路徑  
-ApplicationPool：設定應用程式集區

```text
建立子網站
New-WebApplication -name testapp -site 'Default Web Site' -PhysicalPath c:\test -ApplicationPool defaultapppool

Name             Application pool   Protocols    Physical Path
----             ----------------   ---------    -------------
testapp          defaultapppool     http         c:\test

查詢
Get-WebApplication -Site "Default Web Site"
Name             Application pool   Protocols    Physical Path
----             ----------------   ---------    -------------
testapp          defaultapppool     http         c:\test
```

### [New-WebAppPool](https://technet.microsoft.com/zh-tw/library/hh867889%28v=wps.630%29.aspx) 新增應用程式集區

-Name：集區名稱

```text
New-WebAppPool -Name "NewAppPool"

Name                     State        Applications
----                     -----        ------------
NewAppPool               Started

Get-IISAppPool
Name                 Status       CLR Ver  Pipeline Mode  Start Mode
----                 ------       -------  -------------  ----------
DefaultAppPool       Started      v4.0     Integrated     OnDemand
Classic .NET AppPool Started      v2.0     Classic        OnDemand
.NET v2.0 Classic    Started      v2.0     Classic        OnDemand
.NET v2.0            Started      v2.0     Integrated     OnDemand
.NET v4.5 Classic    Started      v4.0     Classic        OnDemand
.NET v4.5            Started      v4.0     Integrated     OnDemand
NewAppPool           Started      v4.0     Integrated     OnDemand
```

### [New-WebBinding](https://technet.microsoft.com/zh-tw/library/hh867854%28v=wps.630%29.aspx) 建立繫結資訊

-name：目標Website  
-IPAddress：IP 位置  
-Port：連結阜  
-HostHeader：主機名稱  
-Protocol：類型\( HTTP, HTTPS,  FTP.\)  
-SslFlags：SSL認證類型  
-- 0: Regular certificate in Windows certificate storage.  
-- 1: SNI certificate.  
-- 2: Central certificate store.  
-- 3: SNI certificate in central certificate store.

```text
New-WebBinding -Name "Default Web Site" -IPAddress "*" -Port 8098 -HostHeader "TestSite"
```

![](.gitbook/assets/tu-pian%20%2831%29.png)

### [New-WebVirtualDirectory](https://technet.microsoft.com/zh-tw/library/hh867858%28v=wps.630%29.aspx) 建立虛擬目錄

-name：虛擬目錄名稱  
-site：掛載網站節點  
-PhysicalPath：真實路徑  
-application：掛載節點如 -site 樣

```text
建立虛擬目錄
New-WebVirtualDirectory -Site "Default Web Site" -Name "ContosoVDir" -PhysicalPath "c:\test"

查詢 defalut web site 中的虛擬目錄
Get-WebVirtualDirectory -Site "Default web site"
 Name             Physical Path
----             -------------
ContosoVDir      c:\test
```

## 修改

### [Set-WebBinding](https://technet.microsoft.com/zh-tw/library/hh867878%28v=wps.630%29.aspx) 設定修改繫節內容

```text
設定修改
Set-WebBinding -Name 'Default Web Site' -BindingInformation "*:80:" -PropertyName "Port" -Value "1234"

查詢
Get-WebBinding -Name "default web site"

protocol bindingInformation sslFlags
-------- ------------------ --------
http     *:1234:                   0
http     *:8098:TestSite           0
```

### [set-ItemProperty](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/set-itemproperty?view=powershell-6) 設定修改參數

```text
讀取訊息
Get-ItemProperty "IIS:\AppPools\DefaultAppPool" | select *

name                        : DefaultAppPool
queueLength                 : 1000
autoStart                   : True
enable32BitAppOnWin64       : False
managedRuntimeVersion       : v4.0
managedRuntimeLoader        : webengine4.dll
enableConfigurationOverride : True
managedPipelineMode         : Integrated

修改 enable32BitAppOnWin64 設定
Set-ItemProperty -Path IIS:\AppPools\DefaultAppPool -name enable32BitAppOnWin64 -value true

查看
name                        : DefaultAppPool
queueLength                 : 1000
autoStart                   : True
enable32BitAppOnWin64       : True
managedRuntimeVersion       : v4.0
managedRuntimeLoader        : webengine4.dll
enableConfigurationOverride : True
managedPipelineMode         : Integrated
```



