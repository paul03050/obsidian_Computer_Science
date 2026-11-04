這些檔案與副檔名都是在建立與部署 **SSL/TLS 網路安全憑證** 時會遇到的核心元件。它們的關係可以簡化為**「流程中的不同階段」**與**「檔案的封裝格式」**。

**最核心的結論是：`.key`（私鑰）與 `.crt`（憑證）是最終運作必備的成對元件。而 `.csr` 是申請的過程檔，`.pem` 與 `.pfx` 則是這些元件的不同檔案封裝方式。** [[1](https://medium.com/chouhsiang/%E8%83%8C%E6%99%AF%E7%9F%A5%E8%AD%98-pem-%E8%88%87-der-dad659e0a40d), [2](https://blog.miniasp.com/post/2018/04/21/PKI-Digital-Certificate-Format-Convertion-Notes)]

---

一、 從憑證申請流程看關係：`.key` ➔ `.csr` ➔ `.crt`

當你想要為網站啟用加密傳輸（HTTPS），整個申請與核發流程就像是去政府機關辦理護照： [[1](https://zhuanlan.zhihu.com/p/702745054)]

```
[你] 生成私鑰 (.key) 
      └───➔ 提取出公鑰，附上公司資訊，打包成申請書 (.csr)
               └───➔ 送交憑證機構 (CA) 審核簽署 
                        └───➔ [CA] 核發給你正式憑證 (.crt)
```

1. **`.key` (Private Key, 私鑰)**
    - **角色：** 你的**印章或密碼本**。
    - **說明：** 這是最先在伺服器上隨機生成的私有金鑰，**絕對不能外流**。它與最終拿到的憑證（包含公鑰）是一對的，用來解密網頁瀏覽器傳過來的加密資料。 [[1](https://blog.csdn.net/HD243608836/article/details/127441701), [2](https://domain.cloudmax.com.tw/faq-new.php?q=faq-ssl-csr-key)]
2. **`.csr` (Certificate Signing Request, 憑證簽署要求)**
    - **角色：** 你的**護照申請書**。
    - **說明：** 系統會從你的 `.key` 中提取出「公開金鑰（Public Key）」，並加上你的網域、公司名稱、國家等資訊，打包成一個 `.csr` 檔。你會把這個檔案傳給憑證機構（CA，如 DigiCert、Let's Encrypt）進行審核。**拿到憑證後，這個檔案就沒用了**。 [[1](https://developer.aliyun.com/article/623181), [2](https://domain.cloudmax.com.tw/faq-new.php?q=faq-ssl-csr-key)]
3. **`.crt` (Certificate, 憑證)**
    - **角色：** 你的**正式護照或數位身分證**。
    - **說明：** 憑證機構（CA）驗證你的身分後，用他們的官方私鑰對你的資料進行數位簽章，最後核發給你的正式憑證檔案。裡面包含了你的網域、公鑰以及 CA 的簽章。 [[1](https://kanrays.net/tutorials/cpanel/ssl-tools/), [2](https://medium.com/chouhsiang/%E8%83%8C%E6%99%AF%E7%9F%A5%E8%AD%98-pem-%E8%88%87-der-dad659e0a40d), [3](https://zhuanlan.zhihu.com/p/702745054), [4](https://blog.miniasp.com/post/2018/04/21/PKI-Digital-Certificate-Format-Convertion-Notes)]

---

二、 從檔案包裝格式看關係：`.pem` 與 `.pfx`

`.key` 和 `.crt` 只是代表檔案的「內容角色」，但在電腦裡儲存時，會使用不同的「封裝格式」。這兩者就像是衣服不同的摺法或不同的包裝盒： [[1](https://medium.com/chouhsiang/%E8%83%8C%E6%99%AF%E7%9F%A5%E8%AD%98-pem-%E8%88%87-der-dad659e0a40d), [2](https://blog.miniasp.com/post/2018/04/21/PKI-Digital-Certificate-Format-Convertion-Notes)]

| 格式副檔名      | 格式本質與特性                                                                                               | 常見應用與搭配                                                                                                                |
| ---------- | ----------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **`.pem`** | **文字檔 (Base64 編碼)**  <br>用文字編輯器打開會看到 `-----BEGIN...-----`。  <br>它可以是純憑證、純私鑰，或是**把憑證與私鑰合併寫在同一個純文字檔裡**。 | **Linux 體系**  <br>（如 Nginx、Apache）  <br>通常會分別指定：  <br>ssl_certificate `site.crt`;  <br>ssl_certificate_key `site.key`; |
| **`.pfx`** | **二進位包裝檔 (PKCS#12)**  <br>它是一個**加密的保險箱**，用來把 `.crt`（憑證）和 `.key`（私鑰）**打包並加上密碼保護**成單一檔案。                | **Windows 體系**  <br>（如 IIS 伺服器）  <br>因為 Windows 匯入憑證時，習慣一鍵匯入含有私鑰與憑證的單一加密檔案。                                            |

> 💡 **小迷思破解：**  
> 有時候你會看到有人把 `.crt` 改名成 `.pem`，或是把 `.key` 改名成 `.pem`。這是因為 `.pem` 是一種文字容器格式；如果你的 `.crt` 檔打開來是文字（Base64 碼），那它在本質上就是一個 PEM 格式的憑證檔。 [[1](https://docs.aws.amazon.com/zh_tw/acm/latest/userguide/import-certificate-format.html), [2](https://blog.miniasp.com/post/2018/04/21/PKI-Digital-Certificate-Format-Convertion-Notes), [3](https://kanrays.net/tutorials/cpanel/ssl-tools/)]

---

三、 總結：部署網站時我需要什麼？

- 如果你使用的是 **Nginx / Apache (Linux)**：你需要準備好 `.crt`（憑證）和 `.key`（私鑰）這兩個檔案（它們通常都是 PEM 文字格式）。 [[1](https://www.alibabacloud.com/help/tc/ssl-certificate/common-formats-of-ssl-certificates)]
- 如果你使用的是 **IIS (Windows)**：你需要使用 OpenSSL 等工具，把 `.crt` 和 `.key` 打包合併轉換成一個 `.pfx` 檔案，再設定密碼匯入到 Windows 中。 [[1](https://ithelp.ithome.com.tw/articles/10424609), [2](https://www.alibabacloud.com/help/tc/ssl-certificate/common-formats-of-ssl-certificates), [3](https://inbound.technology/ssl-%E6%86%91%E8%AD%89%E6%95%99%E5%AD%B8%EF%BC%9A%E5%A6%82%E4%BD%95%E5%B0%87-ptx-%E8%BD%89%E7%82%BA-key-%E8%88%87-crt-%E6%AA%94%E6%A1%88%EF%BC%9F/)]

你目前是要準備**申請全新憑證**，還是正在**要把拿到的憑證部署到特定的網頁伺服器（例如 Nginx 或 IIS）**呢？如果需要，我可以提供你轉換檔案（例如 **PEM 轉 PFX**）的指令。