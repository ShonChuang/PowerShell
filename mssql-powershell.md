---
description: >-
  有兩個 SQL Server PowerShell 模組：SqlServer 和 SQLPS。 SQLPS 模組隨附於 SQL Server 安裝
  (基於回溯相容性)，但不再更新。 最新版 PowerShell 模組是 SqlServer 模組。 SqlServer 模組包含 SQLPS 中
  Cmdlet 的更新版本，此外還加入新的 Cmdlet 以支援最新版 SQL 功能。 舊版 SqlServ
---

# MSSql PowerShell

### 安裝與載入模組

```text
Install-Module -Name SqlServer -AllowClobber
//安裝 Module 是套過 PowerShellGet 安裝
```

![](.gitbook/assets/tu-pian%20%283%29.png)

查看安裝的模組

```text
Get-Module SqlServer -ListAvailable
```

![](.gitbook/assets/tu-pian%20%284%29.png)

載入模組

```text
Import-Module SqlServer
查看載入模組
Get-Module | where -Property name -eq SqlServer
```

![](.gitbook/assets/tu-pian%20%281%29.png)

### 連線資料庫

#### Local DB

```text
Get-SqlDatabase -ServerInstance "(LocalDb)\MSSQLLocalDB"

Name                 Status           Size Space Avai Recovery 
                                                lable  Model   
----                 ------           ---- ---------- -------- 
master               Normal        6.69 MB  824.00 KB Simple   
                                                               
model                Normal       16.00 MB    5.62 MB Simple   
                                                               
msdb                 Normal       15.81 MB    1.11 MB Simple   
                                                               
MyTestDb             Normal       16.00 MB    4.22 MB Simple  
```

#### 遠端 DB

```text
$cred = Get-Credential
// 跳出示窗，輸入登入資訊
Get-SqlDatabase -ServerInstance "遠端SQL" -Credential $cred
```

### 更安全的連線方式

* 將密碼加密存入檔案中

```text
PS C:\> (Get-Credential).Password | ConvertFrom-SecureString | Out-File "C:\Password.txt"
 //會將連線密碼加密放入Password.txt 中
 
PS C:\> $user="Username"
PS C:\> $File = "C:\Password.txt"
PS C:\> $cred = New-Object -TypeName system.management.automation.pscredential -ArgumentList $user, (get-content $file |
converto-securestring)
PS C:\> Get-SqlDatabase -ServerInstance "遠端SQL" -Credential $cred
```

### 讀取資料

[Read-SqlTableData](https://docs.microsoft.com/zh-tw/powershell/module/sqlserver/Read-SqlTableData?view=sqlserver-ps)

```text
$Instance ="(LocalDb)\MSSQLLocalDB"
$DataBase = "MydbTest"
$Table ="testtable"
Read-SqlTableData -ServerInstance $Instance `
                  -DatabaseName $DataBase `
                  -SchemaName "dbo" `
                  -TableName $Table
```

### 寫入資料

#### [Write-SqlTableDate](https://docs.microsoft.com/zh-tw/powershell/module/sqlserver/write-sqltabledata?view=sqlserver-ps)

必須按照攔位順序插入

```text
$Insert = @([ordered]@{
            id=""
            Name="test"
            Age="11"
            Address="ABCD123"
            }
            [ordered]@{
            id=""
            Name="test1"
            Age="111"
            Address="ABCD1231"
            }
            )              
               
$Insert.ForEach({$_.Foreach({[PSCustomObject]$_}) | 
Write-SqlTableData -ServerInstance $Instance `
                   -DatabaseName $DataBase `
                   -SchemaName dbo `
                   -TableName $Table
                    })

Id	Name	AGE	Address
1	11	ABCD123   	NULL
2	111	ABCD1231  	NULL
3	11	ABCD123   	NULL
4	111	ABCD1231  	NULL
5	test	11        	ABCD123   
6	test1	111       	ABCD1231  
```

### 建立資料庫

#### 使用SMO

```text
$srv = new-Object Microsoft.SqlServer.Management.Smo.Server("Local")
$db = New-Object Microsoft.SqlServer.Management.Smo.Database($srv, "Test_SMO_Database")
$db.Create()
Write-Host $db.CreateDate
```

#### 安'裝第三方PS腳本[ SQLCommands](https://www.powershellgallery.com/packages/SQLCommands/0.0.4)

```text
New-SQLDatabase -SqlInstance "localhost\SQLEXPRESS" -DatabaseName "MyNewdatabase" -Path "
C:\Program Files\Microsoft SQL Server\MSSQL12.SQLEXPRESS\MSSQL\DATA"
```

### 加入User

```text
加入既有User

invoke-Sqlcmd `
 -Query "CREATE USER [shon] FOR LOGIN [shon];ALTER ROLE [db_owner] ADD MEMBER [shon]" `
 -Database "MyNewdatabase" -ServerInstance "localhost\SQLEXPRESS"

加入新的User
invoke-Sqlcmd `
-Query "CREATE LOGIN [TEST] WITH PASSWORD=N'Test123', DEFAULT_DATABASE=[MyNewdatabase], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF " `
-Database "Master" `
-ServerInstance "localhost\SQLEXPRESS"

建立對應
invoke-Sqlcmd `
-Query "CREATE USER [test] FOR LOGIN [test];ALTER ROLE [db_owner] ADD MEMBER [Test]" `
-Database "MyNewdatabase" `
-ServerInstance "localhost\SQLEXPRESS"

```

```text

```

