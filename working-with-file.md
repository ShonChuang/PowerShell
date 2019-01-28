# Working With File

### 讀取檔案

```bash
不需要解析資料時使用，否則使用 Export-xx 方式
Get-Content "get-os.ps1" //讀取檔案內容
$a = Get-Content "get-os.ps1" //存入變數中
$a[0] //讀取第一行
$a[1] //讀取第二行

//組合
$a = Get-Content "get-os.ps1"
$b = [system.environment]::newLine //換行符號
$all = [string]::Join($b,$a) //組合
$all

// 複製檔案內容到新檔案
set-content -Value $a -path "new file.txt"

// 取代檔案內容 
set-content -Value "Test" -path "new file.txt" // 內容只剩 Test

// 增加檔案內容
add-content -Value "Test" -path "new file.txt"

// 匯出檔案
Get-Process | Export-Csv "Processes.csv"

```

### 匯出 Export

```bash
匯出檔案格式
get-service | Export-csv service.csv
// XMK => Clixml
載入檔案
Import-csv service.csv 

```

### 輸出 Out

```text
輸出成.txt
get-service | out-File service.txt
// 可以用  > 表示 Out-file

```

### 轉換 ConvertTo

```text
get-service | convertTo-Html

更多轉換格式
Name                              
----                             
ConvertTo-Csv                     
ConvertTo-Xml                    
ConvertTo-SecureString            
ConvertTo-ProcessMitigationPolicy
ConvertTo-TpmOwnerAuth           
ConvertTo-WebApplication
```

### 轉換後儲存檔案

```text
get-service | converto-html | out-fiel service.html
```

### 比對 Diff

```text
 diff -Reference (Import-Csv .\proces.csv) `
 -Difference (Get-Process) -Property Name
 
 // 不存在 用 => ，不同 用 <= ，一樣 不顯示
 Name         SideIndicator
----         -------------
chrome       =>
cmd          =>
Code         =>
Code         =>
Code         =>
Code         =>
Code         =>
Code         =>
Code         =>
CodeHelper   =>
conhost      =>
conhost      =>
winpty-agent =>
svchost      <=
```

### 

