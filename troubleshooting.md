# Troubleshooting

## 處裡順序

1. Get-Command 查找使用指令
2. Help 查詢語句用法
3. Get-Member 查詢方法與屬性

## Computer and Hardware

### Get-conter 計算硬體資源

輸入 `git-conter`，查詢系統基本資源，CPU、網路、記憶體、硬碟資源使用率

![](.gitbook/assets/tu-pian%20%2814%29.png)

輸入 `git-counter -listset *memory*`，查詢所有memory相關資源

![](.gitbook/assets/tu-pian%20%2825%29.png)

輸入 `git-counter -listset *memory* | wehre countersetname -eq memory`  
查詢名稱為 momery 的資源

![](.gitbook/assets/tu-pian%20%2818%29.png)

輸入 `get-counter -listset *memory*| where countersetname -eq memory | select -expand paths`  
查詢欄位 paths 的詳細資訊

![](.gitbook/assets/tu-pian%20%2824%29.png)

接著輸入get-counter "\memory\% committed Bytes in use" ，直接計算記憶體區塊查詢

![](.gitbook/assets/tu-pian%20%2822%29.png)

### get-cimclass 查詢 cmdlets namespace 類別

輸入 `Get-CimClass -ClassName *disk*` 查詢disk相關指令

![](.gitbook/assets/tu-pian%20%287%29.png)

輸入 `get-wmiobject -class win32_logicaldisk` 查詢單一className 硬碟資訊

![](.gitbook/assets/tu-pian%20%2813%29.png)

### get-wmiobject 獲取WMI類的實例或有關可用類的信息

輸入`get-wmiobject win32_bios` 獲取bios 訊息

![](.gitbook/assets/tu-pian%20%282%29.png)

### get-eventlog 讀取事件紀錄

輸入 `get-eventlog -log system -newest 1000 | where-object eventid -eq '1074' | format-table machinenam e,username,timegenerated -autosize`  
查詢 eventid 為 1074 的事件紀錄，1074 為電腦重開機紀錄，可以看到重啟時間點。

![](.gitbook/assets/tu-pian%20%2830%29.png)

## Networking

### IP

輸入 `gcm *ip*` 查詢相關 ip 命令

輸入 `get-netipaddress` 查詢所有網路ip位置

![](.gitbook/assets/tu-pian%20%2819%29.png)

輸入 get-netipconfiguration 查詢ip配置訊息

![](.gitbook/assets/tu-pian%20%2811%29.png)

### DNS

輸入 `gcm *dns*` 查詢相關 dns 命令

輸入 `Get-DnsClient` 查詢Dns資訊

![](.gitbook/assets/tu-pian%20%286%29.png)

輸入 `Get-DnsClientServerAddress` 查詢 DNS Server 位置

![](.gitbook/assets/tu-pian%20%2826%29.png)

輸入`Get-DnsClientCache` 查詢DNS快取。

### 網路磁碟

輸入 `gcm *smb*` 查詢相關 網路磁碟命令

輸入 `Get-SmbMapping` 查詢網路磁碟

輸入 `New-SmbMapping -LocalPath 'X:' -RemotePath '\Contoso-SO\VMFiles'`   
設定網路磁碟

### 連線測試

輸入`ping 168.95.1.1` 測試連線是否正常

![](.gitbook/assets/tu-pian%20%289%29.png)

輸入 `test-netconnection 168.95.1.1` 查看詳細資訊

![](.gitbook/assets/tu-pian%20%288%29.png)

輸入 `test-netconnection 168.95.1.1 -traceroute` 包括路由路徑

![](.gitbook/assets/tu-pian%20%2817%29.png)

輸入 `test-netconnection -CommonTCPPort http -computername 127.0.0.1` 使用80 port 測試連線

## Files

### Get-childitem 取得目前目錄

如同 dir 指定，因為使用dir別名

### copy-item 複製檔案或資料夾

### move-item 移動檔案或資料夾

### rename-item 重新命名

### icacls.exe 檔案權限管理

輸入icacls.exe c:\temp，查詢資料夾權限

![](.gitbook/assets/tu-pian%20%2812%29.png)

代號說明

![](.gitbook/assets/tu-pian%20%2815%29.png)



