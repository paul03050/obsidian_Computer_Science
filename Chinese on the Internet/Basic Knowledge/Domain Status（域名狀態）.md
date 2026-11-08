**Domain Status（域名狀態）**是指網域在 Registry（註冊管理機構）系統中的目前狀態，用來表示網域是否正常、是否受到鎖定，以及是否存在註冊或轉移方面的限制。

在 ICANN Lookup 查詢網域時，通常會看到這個欄位。

## 一、常見的 Domain Status

以 `.com`、`.net` 等 gTLD 為例，常見狀態如下：

|Domain Status|中文解釋|意義|
|---|---|---|
|`ok`|正常|沒有特殊限制|
|`clientTransferProhibited`|禁止轉移|註冊商設定轉移鎖定|
|`serverTransferProhibited`|禁止轉移|Registry 設定轉移限制|
|`clientUpdateProhibited`|禁止修改|註冊商限制網域資料更新|
|`serverUpdateProhibited`|禁止修改|Registry 限制網域資料更新|
|`clientDeleteProhibited`|禁止刪除|註冊商限制網域刪除|
|`serverDeleteProhibited`|禁止刪除|Registry 限制網域刪除|
|`pendingTransfer`|等待轉移|網域正在進行註冊商轉移|
|`pendingDelete`|等待刪除|網域進入刪除程序|

這些狀態通常是依據 EPP（Extensible Provisioning Protocol，可擴展配置協議）定義的狀態碼。

## 二、最重要的三種 Prohibited 狀態

在網路中文客服工作中，建議先熟悉以下三個：

轉移鎖定

clientTransferProhibited

客戶想將網域從網路中文轉移到其他註冊商，但系統顯示禁止轉移。

可能需要先解除 Registrar Lock（網域轉移鎖定）。

修改鎖定

clientUpdateProhibited

網域註冊資料受到限制，可能無法直接修改名稱伺服器或其他註冊資料。

刪除鎖定

clientDeleteProhibited

網域受到保護，不能直接執行刪除操作。

這裡的 `client` 並不是指一般使用者，而是指 Registrar（註冊商）端設定的限制。

相對地，`server` 表示 Registry（註冊管理機構）端設定的限制。

## 三、client 和 server 有什麼不同？

假設客戶透過網路中文註冊 `.com` 網域：

|狀態|誰設定限制？|
|---|---|
|`clientTransferProhibited`|網路中文等註冊商|
|`serverTransferProhibited`|Registry，例如 Verisign|

因此，如果客戶遇到網域無法轉移的問題，不能只看到 `TransferProhibited` 就直接認定是註冊商鎖定，還要確認是 `client` 還是 `server`。

## 四、Domain Status 和網域到期有什麼關係？

另外幾個狀態也值得認識：

|Domain Status|意義|
|---|---|
|`autoRenewPeriod`|註冊商自動續約後的寬限期間|
|`redemptionPeriod`|贖回期間，通常需要支付額外費用才能恢復|
|`pendingDelete`|即將刪除，通常已進入刪除流程|
|`inactive`|網域尚未具備可正常解析所需的名稱伺服器設定等條件|

注意：網域到期後的實際處理流程與時間，會依 TLD 和 Registry 政策而有所不同。

## 五、客服實務：如何判斷客戶的問題？

假設客戶說：

「我想把網域轉到其他公司，為什麼對方說無法轉移？」

你可以依序確認：

1. 查詢網域的 Domain Status。
    
2. 是否出現 `clientTransferProhibited`？
    
3. 是否出現 `serverTransferProhibited`？
    
4. 網域是否已到期，或處於其他限制狀態？
    
5. 再依照公司 SOP 確認能否解除鎖定，以及是否需要其他驗證。
    

最重要的記憶方式

- `client` = 註冊商端設定。
    
- `server` = Registry 端設定。
    
- `TransferProhibited` = 禁止轉移。
    
- `UpdateProhibited` = 禁止修改。
    
- `DeleteProhibited` = 禁止刪除。
    

Domain Status 不只是顯示網域是否正常，也能協助客服判斷網域操作為什麼受到限制。

補充一點：以上主要是 `.com`、`.net` 等採用 EPP 狀態碼的 gTLD。不同 TLD（例如 `.tw`）可能有不同的狀態表示方式，不能直接套用所有規則。