---
description: 紀錄操作的一些小技巧與資訊
---

# 小技巧

## 啟用時載入預設模組

1. 在我的文件夾中建立一個名為【WindowsPowerShell】，若已存在此資料夾無需重建。
2. 建立一個名為【profile.ps1】。
3. 裡面可以撰寫預設載入有哪些模組。範例如下
4. 重開PowerShell 就會載入。若出現錯誤可以輸入\(Set-ExecutionPolicy RemoteSigned，再重開PS\)

```text
# profile.ps1 

# 載入 IIS 管理模組
Import-Module WebAdministration

# 載入 SQLServer 管理模組
Import-Module SqlServer

# 回到根目錄
cd \
```



