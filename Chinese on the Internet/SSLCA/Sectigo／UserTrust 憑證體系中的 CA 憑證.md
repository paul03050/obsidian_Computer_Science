你列出的三個檔案，都是 Sectigo／UserTrust 憑證體系中的 CA 憑證。不過，它們的角色不完全相同，不能單憑檔名就認定三張都要放進 CA Bundle。

## 1. 分別代表什麼？

|檔案名稱|可能的角色|說明|
|---|---|---|
|`Sectigo Public Server Authentication CA OVR3L.crt`|中繼憑證|可能是簽發網站 SSL 憑證的 Intermediate CA|
|`Sectigo Public Authentication Root 46 - UserTrust.crt`|根憑證|Sectigo Public Authentication Root 46|
|`UserTrust RSA CA Verification Authority.crt`|根憑證或上層 CA 憑證|需檢查憑證內容才能確認其實際鏈結角色|

## 2. 三張憑證之間可能的關係

先用概念圖理解：

UserTrust RSA CA Verification Authority

可能的上層 CA

Sectigo Public Authentication Root 46

名稱含 Root

Sectigo Public Server Authentication CA OVR3L

中繼憑證候選

Domain Certificate

客戶的網站憑證

這只是依名稱推測的示意，不代表已確認這三張憑證的實際簽發關係。憑證鏈的方向應由 Issuer／Subject 與簽章驗證確認。

## 3. 如何確認它們真正的上下層關係？

最重要的是查看每張憑證的兩個欄位：

- Subject：這張憑證是誰？
    
- Issuer：這張憑證是由誰簽發？
    

例如：

|憑證|Subject|Issuer|
|---|---|---|
|A|Sectigo Intermediate CA|Sectigo Root CA|
|B|Sectigo Root CA|UserTrust CA|
|C|UserTrust CA|UserTrust CA|

這時候就能判斷：

- A 是由 B 簽發。
    
- B 是由 C 簽發。
    
- C 是自我簽署的根憑證。
    

但要注意，Subject 與 Issuer 名稱相同只是自我簽署的線索，仍應檢查憑證簽章及 Basic Constraints 等資訊。

## 4. 實務上怎麼檢查？

如果你取得這三個 `.crt` 檔案，可以使用 OpenSSL：

```
openssl x509 -in certificate.crt -noout -subject -issuer
```

查看憑證的 Subject 和 Issuer。

也可以查看是否為 CA 憑證：

```
openssl x509 -in certificate.crt -noout -text
```

尋找：

```
X509v3 Basic Constraints:
    CA:TRUE
```

`CA:TRUE` 表示該憑證具有 CA 憑證的屬性。

我建議你下一步直接把這三個 `.crt` 檔案上傳給我（不需要提供 Private Key）。 我可以幫你逐一檢查 Subject、Issuer、Basic Constraints、有效期限及簽章關係，進而確認哪些憑證應該放入 CA Bundle，以及正確的排列順序。