這份網路中文的 SSL 教學文件，主要在說明 **產生 CSR（Certificate Signing Request，憑證簽署請求）時，Common Name（CN）、Private Key（私密金鑰）應該如何設定，以及容易發生哪些錯誤。**

不過，裡面有幾個觀念需要特別釐清，尤其是「Common Name 是主機名稱」以及「網域與主機是不同主機」這兩段。

## **1. 什麼是 Common Name（CN）？**

Common Name 是 CSR 中的一個欄位，用來填寫這張 SSL 憑證所要保護的網域名稱（通常是 FQDN）。

例如：

- `www.example.com`
    
- `mail.example.com`
    
- `example.com`
    

這裡的 FQDN（Fully Qualified Domain Name）可以理解為完整網域名稱。

假設客戶的網站網址是：

`https://www.example.com`

那麼在一般單一網域 SSL 憑證的 CSR 中，Common Name 通常就填：

`www.example.com`

**網址拆解**

https://

**www**

.

**example.com**

協定

主機名稱 + 網域名稱

完整主機名稱：`www.example.com`

## **2. 為什麼 www.example.com 和 example.com 不一樣？**

這是文件中特別強調的地方。

|名稱|意義|
|---|---|
|`example.com`|主網域（裸網域、apex domain）|
|`www.example.com`|主網域底下的 www 子網域|
|`mail.example.com`|主網域底下的 mail 子網域|

它們是不同的 DNS 名稱。

例如：

- `example.com` 的 A 紀錄可以指向 IP `192.0.2.10`。
    
- `www.example.com` 的 A 紀錄也可以指向 `192.0.2.10`。
    

即使兩者指向同一個 IP，它們仍然是不同的主機名稱。

**這裡要修正文件的一個說法：兩者是否指向同一台主機，並不會自動決定 SSL 是否涵蓋兩個名稱。**

真正要看的是憑證的 SAN（Subject Alternative Name）欄位是否包含這兩個名稱。

## **3. 為什麼文件說 CSR 要填 www.example.com？**

這是網路中文文件提供的傳統申請範例，但不能理解成「只要申請 example.com，就一定要填 www.example.com」。

應該依照客戶實際需要保護的名稱來決定。

|客戶需要保護的網址|CSR 的 Common Name|憑證還需要涵蓋|
|---|---|---|
|`https://example.com`|`example.com`|`example.com`|
|`https://www.example.com`|`www.example.com`|`www.example.com`|
|兩者都需要|可選其中一個作為 CN|SAN 必須包含兩者|

**重點：現代 SSL/TLS 憑證主要透過 SAN（Subject Alternative Name）判斷網址是否受到憑證保護，而不是只看 Common Name。**

因此，如果客戶只申請單一網域憑證，卻希望同時保護：

- `example.com`
    
- `www.example.com`
    

就要確認產品是否支援這兩個名稱，並且兩者都列在 SAN 中。

## **4. 什麼是 Wildcard（通用型／萬用字元）？**

文件提到：

`*.yourdomain.com`

這裡的 `*` 就是萬用字元，代表符合特定層級的子網域。

例如憑證涵蓋：

`*.example.com`

通常可以保護：

- `www.example.com`
    
- `mail.example.com`
    
- `shop.example.com`
    

但不會自動涵蓋：

- `example.com`
    
- `a.b.example.com`
    

[What is an SSL certificate, and why is it important?](https://images.openai.com/static-rsc-4/HTcwG83YWcdNRsj7OF9BDALpV6lpa92vglPalHuWVyE5-pcTutzpcGL0mEP7200bcXqZE27uPiM7KMof1Fvb5dB80VxLtLq0n9eDO2lnc-aTOhv45oTISBDRsovzRPlk6IugfOSX4bFmeGsiW_Uu-gbTTwK9IN7HI4HHl3RNUaw?purpose=inline)

[us-stage.norton.com](https://us-stage.norton.com/blog/how-to/what-is-an-ssl-certificate?utm_source=chatgpt.com)

因此，如果客戶希望同時保護主網域及其第一層子網域，通常需要：

```
example.com
*.example.com
```

這兩個名稱都納入憑證的 SAN。

## **5. 為什麼 CSR 和 Private Key 不要加密？**

這段文件其實有一點容易讓人誤解。

首先要分清楚：

|項目|功能|是否包含私密金鑰|
|---|---|---|
|CSR|向 CA 申請憑證的資料|否|
|Private Key|與憑證配對的私密金鑰|是|
|SSL Certificate|CA 簽發的公開憑證|否|

CSR 內含公開金鑰及申請資料，並不包含 Private Key。

文件所說的「不要加密」，主要是指產生 Private Key 時不要設定密碼。

例如 OpenSSL 產生私密金鑰時，如果設定了密碼，伺服器每次啟動或載入憑證時，可能都需要輸入密碼。

因此有些伺服器部署情境會使用未加密的 Private Key，方便服務自動啟動。

但要注意：

**未加密不代表可以隨便存放。** Private Key 仍然必須妥善保管，限制存取權限，絕對不能寄給不相關的人或上傳到不可信任的網站。

## **6. 為什麼 Private Key 遺失就需要重新產生 CSR？**

這是非常重要的概念。

CSR 和 Private Key 是一組配對關係。

**憑證申請流程**

**產生 Private Key**

客戶自行保管

**產生 CSR**

包含對應的公開金鑰及申請資訊

**CA 簽發 SSL Certificate**

憑證包含對應的公開金鑰

**伺服器安裝憑證 + 原本的 Private Key**

假設：

1. 客戶產生 Private Key A。
    
2. 使用 Private Key A 產生 CSR A。
    
3. CA 根據 CSR A 簽發憑證 A。
    
4. 客戶卻把 Private Key A 刪掉了。
    

此時，即使客戶還保有憑證 A，也無法正常使用它與原本的私密金鑰配對。

處理方式通常是：

1. 重新產生 Private Key B。
    
2. 使用 Private Key B 產生 CSR B。
    
3. 申請 Reissue（重新簽發）。
    
4. 安裝新的憑證 B，搭配 Private Key B。
    

是否需要額外費用，依 CA 及原訂單條款而定。

## **7. 多台主機部署憑證的情況**

你貼上的最後一段沒有完整顯示，不過這裡也有一個重要觀念：

如果客戶有多台 Web Server，希望都使用同一張 SSL 憑證，通常需要讓每台伺服器都持有與憑證配對的 Private Key。

例如：

**SSL Certificate A**

對應 Private Key A

**Server 1**

Certificate A + Key A

**Server 2**

Certificate A + Key A

技術上可以將同一組憑證與私密金鑰部署到多台主機，但這會增加私密金鑰外洩的風險，也要確認產品授權及 CA 條款是否允許。

## **8. 網路中文客服應該記住的五個重點**

|客戶問題|你應該確認什麼|
|---|---|
|CSR 的 CN 要填什麼？|客戶實際要保護的完整網域名稱|
|www 和裸網域能共用嗎？|憑證 SAN 是否包含兩者|
|Wildcard 能保護哪些名稱？|是否包含裸網域及所需子網域|
|Private Key 遺失怎麼辦？|重新產生 Key、CSR，評估 Reissue|
|多台主機可以使用嗎？|Key 部署方式、授權及安全要求|

**我認為你最需要建立的觀念是：CSR 是申請憑證的資料，Private Key 是憑證配對及伺服器使用時的重要秘密，而 SSL Certificate 是 CA 簽發的公開憑證。三者不能混為一談。**