可以，而且這是很常見的做法。**客戶可以只向網域註冊商購買網域，再自行向 Sectigo 購買 SSL 憑證。網域註冊商和 SSL 憑證供應商不必是同一家公司。**

這也是你在網路中文客服工作上很重要的觀念：網域註冊與 SSL 憑證是兩項獨立服務。

## **一、實際架構範例**

假設客戶想經營 `example.com`：

網域註冊商

**網路中文資訊**

客戶只向網路中文購買 `example.com`

取得網域使用權

憑證授權機構（CA）

**Sectigo**

客戶另外向 Sectigo 購買 SSL 憑證

完成網域控制權驗證（DCV）

網站伺服器

**客戶自己的主機或雲端平台**

安裝 SSL 憑證，啟用 HTTPS

這三個角色可以由不同業者提供服務。

## **二、客戶向 Sectigo 購買 SSL，需要什麼？**

即使網域是在網路中文註冊，客戶仍可以自行向 Sectigo 申請。

通常需要：

|項目|用途|
|---|---|
|網域名稱|例如 `example.com`|
|CSR|Certificate Signing Request，憑證簽署要求|
|Private Key|產生 CSR 時所建立的私密金鑰|
|DCV 驗證|證明客戶具有網域控制權|

Sectigo 官方說明，申請者必須擁有或獲得授權使用該網域，並完成網域控制權驗證。

docs.sectigo.com+1

**這裡有一個很重要的細節：**

客戶不需要把網域轉移到 Sectigo，也不需要把 DNS 代管服務轉移到 Sectigo。

例如，客戶可以：

- 網域註冊：網路中文
    
- DNS 代管：Cloudflare
    
- SSL 憑證：Sectigo
    
- 網站主機：AWS
    

這樣完全可以運作。

## **三、那 Sectigo 如何確認客戶真的擁有這個網域？**

這就是你之前學過的 DCV（Domain Control Validation）。

假設客戶向 Sectigo 申請 SSL，Sectigo 要求客戶新增一筆 CNAME：

|Type|Host|Target|
|---|---|---|
|CNAME|`_abc.example.com`|`_xyz.validation.sectigo.com`|

客戶必須登入**實際管理該網域 DNS 的平台**新增記錄。

例如：

- 如果 Nameserver 指向 Cloudflare，就在 Cloudflare 設定。
    
- 如果 Nameserver 指向網路中文，就在網路中文的 DNS 管理介面設定。
    

Sectigo 再透過 DNS 查詢驗證結果。

docs.sectigo.com+1

## **四、這對網路中文客服有什麼意義？**

假設有客戶打電話：

「我的網域是在你們這裡買的，但 SSL 是我自己跟 Sectigo 買的，現在 CNAME 驗證失敗，可以幫我處理嗎？」

你應該先判斷：

1. 網域是否註冊在網路中文？
    
2. 客戶目前使用哪家 DNS 代管商？
    
3. 客戶是否需要協助新增 CNAME？
    
4. CNAME 已經設定，但 Sectigo 仍然驗證失敗嗎？
    

如果 DNS 由網路中文代管，你們可能可以協助確認記錄設定；如果 DNS 在 Cloudflare，就要引導客戶到 Cloudflare 管理。

至於是否提供代客設定、技術支援或收費服務，則要依照公司內部 SOP。

## **五、最重要的觀念整理**

|客戶購買項目|是否必須向同一家公司購買？|
|---|---|
|網域註冊＋SSL|不必|
|網域註冊＋DNS 代管|不必|
|DNS 代管＋網站主機|不必|
|SSL 憑證＋網站主機|不必|

**一句話記住：網域註冊商負責網域註冊，CA 負責簽發 SSL，DNS 代管商負責 DNS 記錄，主機商負責網站運作。這些服務可以分開購買。**

補充：Sectigo 也有官方銷售及合作夥伴管道，實際購買方式與帳戶管理功能會依通路而異。

Sectigo® Official+1

你可以接著熟悉一個很實用的客服情境：**客戶在網路中文買網域、在 Cloudflare 管 DNS、在 Sectigo 買 SSL，當 CNAME 驗證失敗時，三家公司各自應負責什麼。**