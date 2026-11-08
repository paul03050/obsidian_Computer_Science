Registry Domain ID（註冊機構網域識別碼），是由網域的 Registry（註冊管理機構） 指派給某個網域的唯一識別碼，用來在註冊機構的資料庫中辨識該筆網域註冊紀錄。

簡單來說：

> 網域名稱是給人辨識的名稱；Registry Domain ID 則是註冊管理機構用來辨識該筆網域紀錄的內部編號。

## 1. 舉例說明

假設你在 ICANN Lookup 查詢：

`example.com`

可能會看到：

```
Domain Name: EXAMPLE.COM
Registry Domain ID: 2336799_DOMAIN_COM-VRSN
Registrar: Example Registrar
Creation Date: 1995-08-14
```

其中：

|欄位|意義|
|---|---|
|Domain Name|網域名稱|
|Registry Domain ID|Registry 指派的網域識別碼|
|Registrar|網域註冊商|
|Creation Date|網域註冊日期|

`2336799_DOMAIN_COM-VRSN` 就是這個例子中的 Registry Domain ID。

## 2. Registry Domain ID 和 Registrar 有什麼不同？

這是客服工作上值得區分的概念。

假設客戶透過網路中文註冊 `.com` 網域：

客戶

申請 `example.com`

Registrar（註冊商）

網路中文

Registry（註冊管理機構）

`.com` 的管理機構 Verisign

在其資料庫中管理網域註冊紀錄及識別碼

兩者差異：

- Registrar：提供客戶網域註冊、續約、轉移等服務。
    
- Registry：負責管理特定頂級網域（TLD）的註冊資料庫。
    
- Registry Domain ID：Registry 用來識別特定網域紀錄的編號。
    

## 3. 和其他 ID 有什麼不同？

|名稱|用途|
|---|---|
|Domain Name|網域名稱，例如 `example.com`|
|Registry Domain ID|Registry 的網域紀錄識別碼|
|Registrar IANA ID|辨識網域註冊商的編號|
|Customer ID|註冊商內部辨識客戶的編號|
|Order ID|訂單編號|

要特別注意：Registry Domain ID 通常不是客戶可以自行設定的，也不是網域密碼或轉移授權碼（EPP/AuthInfo Code）。

客服實務記憶法：

- 客戶問「我的網域名稱是什麼？」→ Domain Name
    
- 客戶問「哪一家註冊商管理？」→ Registrar
    
- 客戶問「Registry Domain ID 是什麼？」→ Registry 資料庫中的網域識別碼
    
- 客戶問「轉移網域需要什麼密碼？」→ EPP/AuthInfo Code
    

這樣就不容易把網域識別碼、註冊商編號和轉移授權碼混淆。