在 [ICANN Lookup](https://lookup.icann.org/zh) 查詢網域時，顯示的「域名狀態」（Domain Status）採用的是國際標準的 **EPP 狀態碼（Extensible Provisioning Protocol Status Codes）**。 [[1](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en), [2](https://jikensitu.com/seo-knowledge/domain-lookup/)]

這些狀態碼分為兩大來源，且可同時存在多個狀態： [[1](https://www.domainnameapi.com/blog/domain-status-codes-epp-explained), [2](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en)]

- **client 開頭**：由「網域註冊商」（Registrar，如 GoDaddy、Namecheap、Gandi 等）設定。
- **server 開頭（或無前綴）**：由「頂級域名註冊局」（Registry，如 Verisign）設定，層級與優先權高於註冊商。 [[1](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en)]

以下依性質分類整理最常見的狀態碼含義（完整官方定義可參考 [ICANN EPP 狀態碼頁面](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en)）：

1. 正常運作與安全鎖定狀態（最常見）

這類狀態代表網域正常運作中，通常無需擔心：

- **ok / active**：網域處於正常註冊與啟用狀態，沒有設定任何限制，可正常解析、更新或轉移。
- **clientTransferProhibited**：禁止轉移。這是多數註冊商預設開啟的「安全鎖」，防止網域被未經授權轉移到其他註冊商；若要搬家，需先到註冊商後台解鎖。
- **clientUpdateProhibited / serverUpdateProhibited**：禁止更新。防止網域資料（如 DNS 名稱伺服器或聯絡人）被修改。
- **clientDeleteProhibited / serverDeleteProhibited**：禁止刪除。防止網域被意外或惡意刪除。
- **clientRenewProhibited / serverRenewProhibited**：禁止續約。通常出現在網域即將刪除，或特定合約限制下（一般正常網域較少見）。 [[1](https://www.icann.org/en/resources/registrars/transfers/epp-status-codes-30jun11-en.pdf), [2](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en), [3](https://jikensitu.com/seo-knowledge/domain-lookup/), [4](https://www.domainnameapi.com/blog/domain-status-codes-epp-explained), [5](https://opensrs.com/blog/what-do-domain-status-codes-mean)]

2. 暫停解析與異常狀態（網站會打不開）

當網域出現以下狀態時，**DNS 解析會被中斷**，導致網站無法連線、Email 無法收發： [[1](https://opensrs.com/blog/what-do-domain-status-codes-mean), [2](https://jikensitu.com/seo-knowledge/domain-lookup/)]

- **clientHold**：註冊商暫停解析。常見原因包含：網域過期未續費、註冊人信箱未通過 ICANN 驗證、付款爭議或違反註冊商條款。需聯繫註冊商處理。
- **serverHold**：註冊局暫停解析。通常涉及法律爭議、濫用（如釣魚網站）、實名認證未過，或註冊商與註冊局之間的帳務問題。
- **inactive**：未啟用。代表該網域尚未設定任何名稱伺服器（Name Servers），因此無法在網路上被解析。 [[1](https://doc.openresty.com/tw/edge/edge-admin/troubleshooting/domain-name-resolve/), [2](https://opensrs.com/blog/what-do-domain-status-codes-mean), [3](https://www.webpay.com.tw/index.php?rp=/knowledgebase/16/ICANN%E5%9F%9F%E5%90%8D%E9%A9%97%E8%AD%89%E6%A9%9F%E5%88%B6%E5%A6%82%E4%BD%95%E9%80%B2%E8%A1%8C.html), [4](https://www.icann.org/en/resources/registrars/transfers/epp-status-codes-30jun11-en.pdf), [5](https://jikensitu.com/seo-knowledge/domain-lookup/), [6](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en)]

3. 過期與刪除生命週期狀態

當網域到期未續費，會依序進入以下階段： [[1](https://snapshot.internetx.com/en/epp-domain-status-codes/), [2](https://jikensitu.com/seo-knowledge/domain-lookup/)]

- **autoRenewPeriod**：自動續約寬限期（通常為到期後 1～45 天內）。註冊局已自動幫網域續約並向註冊商扣款，若持有人決定不續約，註冊商可在此期間刪除網域並取回退款。
- **redemptionPeriod**：贖回期（通常為 30 天）。網域已被刪除，無法正常解析，但原持有人仍可透過註冊商支付一筆較高的「贖回費」救回網域。
- **pendingRestore**：等待恢復中。原持有人已申請贖回並付費，註冊局正在等待註冊商提交相關證明文件（通常約 7 天）。
- **pendingDelete**：等待刪除（通常為 5 天）。贖回期已過且未救回，網域處於徹底清除前的最後倒數，**此時已無法救回**，5 天後將重新開放給大眾公開註冊。 [[1](https://jikensitu.com/seo-knowledge/domain-lookup/), [2](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en), [3](https://www.icann.org/en/resources/registrars/transfers/epp-status-codes-30jun11-en.pdf), [4](https://opensrs.com/blog/what-do-domain-status-codes-mean)]

4. 處理中狀態（暫時性）

代表系統正在執行某項指令，通常幾分鐘到幾天內會自動消失：

- **pendingCreate**：正在建立網域。
- **pendingTransfer**：網域正在轉移到另一家註冊商的過程中（通常需 5～7 天）。
- **pendingUpdate**：正在更新網域資料。
- **pendingRenew**：正在處理續約請求。
- **addPeriod / renewPeriod / transferPeriod**：分別代表新註冊、手動續約或轉移完成後的短期寬限期（通常為 5 天）。 [[1](https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en), [2](https://opensrs.com/blog/what-do-domain-status-codes-mean), [3](https://www.icann.org/en/resources/registrars/transfers/epp-status-codes-30jun11-en.pdf)]

如果你正在查詢某個特定網域，可以告訴我：

- 畫面上顯示的**具體狀態碼**
- 你目前遇到的**狀況**（例如網站打不開、準備轉移網域、或想搶註過期網域）

我可以幫你判斷原因並提供對應的處理步驟。