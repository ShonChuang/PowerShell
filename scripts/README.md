# Scripts

## Variable

變數前加 `$`

讀取型別  
`$hi.getType()  
42.getType()`

輸出變數 `write-host $hi`

### 定義

定義有型別的變數：

```bash
[system.int32]$myint =42; 
[int]$myotherint=42
[string]$mystring = "test"
```

長的定義方式：

```bash
New-Varible -Name var -Value 123 //定義 var 變數
Get-Varible var -valueonly // 取得 var 變數值
Get-Varible var //取得變數名與值，如果不存在會動自動新增
Get-Varible //取得所有變數
```

### 清除

```text
Clear-Varibale -name var // var值清空，但變數還是存在
Remove-Varible -name var // 變數與值都清空
```

字串轉大寫  
`"test".ToUpper()`

判斷字串中是否有包含  
`"test".Contan("ts")`

### 比較

-qt 大於、-lt 小於、-eq 等於、-ne 不等於、-le 小於等於、-ge 大於等於

-like、-notlike、-match、-notmatch

```bash
42 -eq 42 : true
42 -eq "42" : true
42 -eq "042" : true
"042" -eq 42 : false !!
```

### 計算

$var = 3 \* 11  
$var ++

### 字串

```bash
"test" // 雙引號 test
'test' // 單引號 test
'Hello World "test", ok?' //Hello World "test", ok? 
"I can't believe " //I can't believe 
"Hello World ""test"", ok?"  //Hello World "test", ok? 
'I can''t believe ' //I can't believe 
```

```bash
"test`b123" // `b = Backspace tes123 需在 console 上執行
"test`n123" // `n 換行
"test`t123" // 't Tab test	123
```

```bash
$Mytext=@"
123123123
dsfsadfasdf
dsfasdf
"@
// @" 後面不能接東西
// .....
//  @" 後面不能接東西
// 斷行字串
```

```bash
//Format
$items=(Get-ChildItem).Count
$loc = Get-Location
"There are $items items are in the folder $loc." //取代變數
"There are `$items items are in the folder `$loc." //跳脫符號不會取代變數
'There are $items items are in the folder $loc.' //單引號包住不會取代變數
```

```bash
//占位符號
"There are {0} items." -f $items //There are 7 items.
"There are {0} items in the location {1}." -f $items ,$ioc
[string]::Format("There are {0} items.",$items)
```

支援 Regular Expressions 

```bash
"888-369-1240" -match "[0-9]{3}-[0-9]{3}-[0-9]{4}"
```

註解加`#`

附檔名為 `*.ps1`

執行Script`./xxxx.ps1`

### Array

```text
$array = "test1","test2" // $array[0]="test1",$array[1]="test2"
$array =@("test1","test2") //一般定義 array 方式
$array.GetType() // 型別 Object
$array =@() //只有這個方式建立空陣列
$array =1..5 //自動增加有序陣列 1,2,3,4,5
$array -contains 4 //是否含有4
$array -notcontains 4 //是否不含有4
```

### Hash Tables

```bash
$hash = @{"key"      ="value";
          "key1"     ="value1";
          "key2"     ="value2"}
Result:          
Name                           Value  
----                           -----       
key                            value  
key1                           value1 
key2                           value2 
//取值
$hash[key] // value      
$hash."key" // value    
$hash.$["ke" + "y"] // value  
//增加
$hash[key3] = "value3"
//移除
$hash.Remove("key2")
//查詢key
$hash.Contains("key")
//查詢value
$hash.ContainValue("value")
```

### 系統綁定變數

```bash
$false // false
$true //true
$NULL // 
$pwd // 目前位置
$home // 家目錄
$host //Info about a users machine
$PID //Porcess ID
$PSVersionTable //powershell Info
$_ // 目前物件
```

### Set-ExecutionPolicy 設定執行權限

參數：Unrestricted \| Remote Signed \| All Signed \| Restricted \| Default \| Bypass \| Undefined\(不受限制\| 遠程簽名\| 全部簽名\| 受限制\| 默認\| 旁路\| 未定義\)

## Parameters

參數使用方式，底下指令結果一樣

```bash
$OS=Get-CimInstance Win32_OperatingSystem | select Caption
$os2 = (Get-CimInstance Win32_OperatingSystem).Caption
```

## Logic

撰寫讀取Service 資訊為 Stopped 的服務

```bash
#Get-StoppedServices.ps1
$service = Get-Service -ComputerName c010700043 | where Status -EQ "Stopped"
foreach ($a in $service){
$Name = $a.DisplayName
$Status = $a.Status
Write-Output "$Name is $Status"
}
```

撰寫可以互動讀取輸入資料執行指令

```bash
$Computername = Read-Host "Enter Computername" <==提示輸入訊息
Get-CimInstance -ClassName win32_OperatingSystem ` <==換行符號
-ComputerName $Computername | <==換行
Select-Object -Property csname,lastbootuptime
```

{% hint style="info" %}
若無法執行請參考：[https://support.microsoft.com/zh-tw/help/2905339/winrm-client-cannot-process-the-request-error-when-you-connect-to-exch](https://support.microsoft.com/zh-tw/help/2905339/winrm-client-cannot-process-the-request-error-when-you-connect-to-exch)
{% endhint %}

撰寫加入參數且含邏輯判斷script

```bash
#定義輸入參數 Mandatory為強制輸入，[型別]$變數名稱
param(
    # Parameter help description
    [Parameter(Mandatory = $true)]
    [string]
    $ComputerName
)
#定義執行指令
$service = Get-Service -ComputerName $ComputerName 
#$service = Get-Service -ComputerName c010700043 | where Status -EQ "Stopped"
foreach ($a in $service) {
    # 讀取屬性
    $Name = $a.DisplayName
    $Status = $a.Status
    #加入邏輯判斷
    if ($Status -eq "Running") {
        Write-Output "$Name is $Status"
    }
    else {
        Write-Output "$Name is $Status"
    }
}

```

### Switch

```bash
switch ($var)
{
 41 {"Forty One"; break}
 42 {"Forty Two" break}
 default {"default"}
}

// 多個參數比對
switch (41,42,11,33)
{
 41 {"Forty One"; break}
 42 {"Forty Two" break}
 default {"default"}
}

// 區分大小寫
switch -casesensitive ("TEST")
{
 "TEST" {"Forty One"; break}
 "test" {"Forty Two" break}
 default {"default"}
}

// 通配符
switch -Wildcard ("TEST123")
{
 "TEST*" {"Forty One"; break}
 "?test" {"Forty Two" break}
 "test???" {"Forty Two" break}
 default {"default"}
}
```

### Blocks

```bash
{Clear-Host; "Mytest String"} //不執行，直接輸出 Clear-Host; "Mytest String"
$cool = {Clear-Host; "Mytest String"} //可以指定給變數，一樣部會執行
& {Clear-Host; "Mytest String"} //加 & 才會執行
& $cool //加 & 才會執行

$value ={41 + 1}
& $value // 42
1 + (&$value) // 43
1 + &$value //失敗
```

```bash
// 定義傳入參數
$qa ={
 $question = $args[0]
 $answer = $args[1]
 Wtite-Host "Here is your question: $question The answer is $answer"
}
// 傳入參數
&$qa "what is cool?" "you" 
// Here is your question: what is cool? The answer is you
```

```bash
// param 定義傳入參數
$qa ={
 param($question,$answer)
 Wtite-Host "Here is your question: $question The answer is $answer"
}
// 傳入參數
&$qa "what is cool?" "you" 
// Here is your question: what is cool? The answer is you

//傳入參數2
&$qa -question "what is cool?" -answer "you"
傳入參數3
&$qa -q"what is cool?" -a"you" //簡短方式
```

```bash
//  設定 Default 值
$qa ={
 param($question,$answer="default answer")
 Wtite-Host "Here is your question: $question The answer is $answer"
}

// 定義傳入參數型別
$math ={
param([int] $x,[int] $y)
return $x * $y
}
&$math 3 11 // 33
```

### Pipeline

```bash
set-Location "C:\Users\Documents" //設定路徑
$onlyCoolFiles=
{
  process{
  if($_.name -like "*.ps1") //$_ 當前的物件
    { return $_.name}
  }
}
Clear-Host
Get-ChildItem | &$onlycoolfiles // pipeline 執行$ onlycoolfiles
```

```bash
// 使用 begin and end
$onlyCoolFiles=
{
 begin {$retval = "Here are some cool files: `r`n"}
  process{
  if($_.name -like "*.ps1") //$_ 當前的物件
    { return $_.name}
  }
  end{return $retval}
}
Clear-Host
Get-ChildItem | &$onlycoolfiles
```

### Variable Scope

```bash
$var = 42
& {$var = 33; write-host "Inside block:$var"}
write-host "Outside block:$var"
----------------
Inside block:33
Outside block:42
```

```bash
//設定父層區域變數
Get-Variable var -valueOnly -scope 1; 
Set-Variable var 33 -scope 1;
&{$global:var = 33} //全域變數var = 33
&private:var2 = 22 //設定private 變數
```

### Functions

```bash
function Get-FullName($firstName,$lastName)
{
 Write-Host ($firstName + " " + $lastName)
}
Get-FullName "Shon" "Chuang"

// 傳參考
function Set-FVar([ref] $myparam){
$myparam.Value = 33
}
$fvar =42
"before:$fvar"
Set-FVar([ref] $fvar) //呼叫方式
"after:$fvar"
----------
before:42
after:33
```

### Filters

```bash
filter Show-PSfilters
{
   $filename =$_.name
   if($filename -like "*.ps1"){
     return $_
   }
}

Get-ChildItem | Show-PSfilters
```

### Error Handling

```bash
trap
{
  Write-Host $_.ErrorID
  Write-Host $_.Exception.Message
  continue or break
}
```



