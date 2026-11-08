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


網域註冊商的客服會查詢客戶的 Domain Status（域名狀態），通常是因為客戶遇到某種操作異常，需要確認網域目前是否受到 Registry 或 Registrar 的限制。

以你目前在網路中文擔任客戶關係維護專員的工作來說，可以把它理解成：

> 客戶反映網域無法註冊、轉移、修改、續約或正常使用時，客服透過 Domain Status 判斷網域目前處於什麼狀態，以及問題可能出在哪個環節。

## 一、最常見的 7 種客服情境

### 1. 客戶想把網域轉移到其他註冊商

常見情境

客戶反映：

「我想把網域從網路中文轉到其他註冊商，但對方說無法轉移。」

客服查詢：

- `clientTransferProhibited`
    
- `serverTransferProhibited`
    
- `pendingTransfer`
    

判斷方向：

|查詢結果|可能原因|
|---|---|
|clientTransferProhibited|註冊商端設定轉移鎖定|
|serverTransferProhibited|Registry 端設定轉移限制|
|pendingTransfer|已經有轉移申請正在處理|

客服下一步：確認網域是否符合轉移資格、是否需要解除鎖定，以及是否已有轉移申請。

### 2. 客戶想修改網域資料，但系統不允許

客戶反映：

「為什麼我無法修改網域的註冊人資料或聯絡信箱？」

客服查詢：

- `clientUpdateProhibited`
    
- `serverUpdateProhibited`
    

可能原因：

- 網域受到修改鎖定。
    
- Registry 有特定修改限制。
    
- 網域正在進行其他程序。
    

客服下一步：確認是哪一端設定限制，再依照相關規定處理。

### 3. 客戶發現網站無法正常開啟

需要區分問題類型

客戶反映：

「我的網域突然無法連線，網站打不開。」

客服可以先查詢 Domain Status，例如：

- `ok`
    
- `inactive`
    
- `clientHold`
    
- `serverHold`
    

其中兩個特別重要：

|狀態|意義|
|---|---|
|`clientHold`|註冊商端暫停網域的 DNS 委派|
|`serverHold`|Registry 端暫停網域的 DNS 委派|

這兩種狀態可能導致網域無法正常解析。

可能原因包括：

- 網域註冊資料驗證問題。
    
- 網域違反相關政策。
    
- 註冊或管理程序出現異常。
    
- 涉及主管機關或 Registry 的限制。
    

客服下一步：確認是否為網域狀態造成的解析問題，而不是直接認定網站主機故障。

### 4. 客戶反映網域到期後無法使用

客戶反映：

「我明明已經續約，為什麼網站還是不能使用？」

客服查詢：

- `autoRenewPeriod`
    
- `redemptionPeriod`
    
- `pendingDelete`
    

判斷方向：

|狀態|可能情境|
|---|---|
|autoRenewPeriod|Registry 處於自動續約寬限期間|
|redemptionPeriod|網域進入贖回階段|
|pendingDelete|網域即將從 Registry 資料庫刪除|

客服下一步：確認續約是否成功、網域是否已進入贖回或刪除程序，以及是否還能恢復。

注意：`autoRenewPeriod` 不代表客戶已經完成付款，仍須核對註冊商的訂單及帳務紀錄。

### 5. 客戶反映網域無法刪除

客戶反映：

「我不想繼續使用這個網域，為什麼無法刪除？」

客服查詢：

- `clientDeleteProhibited`
    
- `serverDeleteProhibited`
    

判斷方向：

- 是否啟用刪除鎖定？
    
- 是否有其他未完成的網域操作？
    
- 是否受到 Registry 政策限制？
    

客服下一步：確認刪除限制來源，以及是否符合解除限制的條件。

### 6. 客戶反映網域註冊成功，但無法正常使用

客戶反映：

「我已經買好網域，為什麼網站還是無法開啟？」

客服除了查詢 Domain Status，也應確認：

1. 網域是否已成功註冊？
    
2. Nameservers 是否正確？
    
3. DNS 記錄是否設定完成？
    
4. 是否有 `clientHold` 或 `serverHold`？
    
5. DNSSEC 是否設定錯誤？
    

這裡要注意：即使 Domain Status 顯示 `ok`，也不代表 DNS 一定設定正確，更不代表網站主機正常。

### 7. 客戶詢問網域目前是否正常

客戶反映：

「可以幫我確認這個網域目前有沒有異常嗎？」

客服可以透過 RDAP / ICANN Lookup 查詢：

- Domain Status
    
- Registrar
    
- Expiration Date
    
- Nameservers
    
- DNSSEC
    

再搭配公司內部系統確認網域的訂單、續約、付款及操作紀錄。

## 二、客服實際判斷流程

我建議你把網域狀態查詢整理成以下流程：

客戶反映網域異常

查詢 Domain Status

狀態正常

繼續檢查 DNS、主機、SSL、帳務或設定。

狀態異常

確認狀態碼、限制來源及可否解除。

依照公司 SOP 處理，必要時向上游 Registry 或服務供應商升級

## 三、建議優先熟悉的 Domain Status

如果你剛開始接觸網域客服，不需要一次記住所有狀態碼，我會建議先掌握這 10 個：

|狀態碼|核心意義|
|---|---|
|`ok`|正常|
|`inactive`|尚未具備正常委派所需條件|
|`clientTransferProhibited`|註冊商端禁止轉移|
|`serverTransferProhibited`|Registry 端禁止轉移|
|`clientUpdateProhibited`|註冊商端禁止修改|
|`clientDeleteProhibited`|註冊商端禁止刪除|
|`clientHold`|註冊商端暫停 DNS 委派|
|`serverHold`|Registry 端暫停 DNS 委派|
|`pendingTransfer`|正在轉移|
|`redemptionPeriod`|贖回期間|

## 四、我認為你最需要建立的客服觀念

客戶說「網域不能用」，其實可能是完全不同的問題。

|客戶問題|優先檢查方向|
|---|---|
|網域不能轉移|Domain Status、EPP/AuthInfo|
|網域不能修改|Domain Status、修改權限|
|網域不能解析|Domain Status、DNS、DNSSEC|
|網站無法開啟|DNS、主機、SSL、網站服務|
|網域到期無法使用|續約紀錄、Domain Status|
|SSL 無法核發|DCV 驗證、DNS、CA 狀態|

最重要的觀念：Domain Status 是客服排查問題的其中一項工具，不是所有網域異常的唯一判斷依據。

例如客戶網站無法開啟，但 Domain Status 是 `ok`，這時就應該往 DNS 設定、DNSSEC、主機或 SSL 方向繼續排查，而不是一直停留在網域註冊狀態。

這套判斷方式也能幫助你避免把所有技術問題都當成註冊商端的問題。