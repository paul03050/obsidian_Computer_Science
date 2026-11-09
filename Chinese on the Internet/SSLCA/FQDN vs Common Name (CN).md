**不完全一樣，但兩者在「申請網站 SSL 憑證」的場景下，答案常常會重疊。** [[1](https://ithelp.ithome.com.tw/articles/10414802), [2](https://www.ssl.com/zh-TW/%E5%B8%B8%E8%A6%8B%E5%95%8F%E9%A1%8C/%E9%80%9A%E7%94%A8%E5%90%8D/)]

簡單來說：**FQDN 是一種網路通訊的「地址格式」**，而 **Common Name (CN) 是數位憑證裡的一個「欄位名稱」**。 [[1](https://zh.wikipedia.org/zh-tw/%E5%AE%8C%E6%95%B4%E7%B6%B2%E5%9F%9F%E5%90%8D%E7%A8%B1), [2](https://www.facebook.com/NetworkSolutions/videos/ever-heard-of-an-fqdn-it-stands-for-fully-qualified-domain-name-which-is-like-yo/1483759989426289/), [3](https://bbs.mnya.tw/d/4930-common-name-eg-server-fqdn-or-your-name-shi-shi-mo), [4](https://www.ssl.com/zh-TW/%E5%B8%B8%E8%A6%8B%E5%95%8F%E9%A1%8C/%E9%80%9A%E7%94%A8%E5%90%8D/)]

為了讓你輕鬆理解，以下用表格直接對比兩者的核心差異：

|特性|Common Name (CN)|FQDN (Fully Qualified Domain Name)|
|---|---|---|
|**本質**|數位憑證（如 SSL 憑證）中的**主體名稱欄位**。|網際網路上精確定位某台主機的**完整網域名稱**。|
|**定義範疇**|屬於密碼學、PKI 憑證體系（X.509 標準）。|屬於網路架構、DNS 網域系統標準。|
|**填寫內容**|可以是網域（如 `www.google.com`），也可以是人名、系統代號、內網名稱。|必須符合 `主機名.網域.頂級網域` 的標準格式（如 `www.google.com`）。|

為什麼大家常把這兩個詞混在一起？

當你在建立網站並申請 [SSL/TLS 憑證](https://www.ssl.com/zh-TW/%E5%B8%B8%E8%A6%8B%E5%95%8F%E9%A1%8C/%E9%80%9A%E7%94%A8%E5%90%8D/)時，系統會要求你輸入 `Common Name (CN)`。因為這個憑證是要給特定網站伺服器使用的，為了讓瀏覽器認得，你**必須在 Common Name 欄位裡填入該網站的 FQDN**（例如 `shop.example.com`）。 [[1](https://www.ssl.com/zh-TW/%E5%B8%B8%E8%A6%8B%E5%95%8F%E9%A1%8C/%E9%80%9A%E7%94%A8%E5%90%8D/), [2](https://bbs.mnya.tw/d/4930-common-name-eg-server-fqdn-or-your-name-shi-shi-mo), [3](https://ithelp.ithome.com.tw/articles/10414802)]

- 如果你填寫的 Common Name 與使用者實際連線的 FQDN 對不上，瀏覽器就會跳出「不安全」的隱私權警告。 [[1](https://ithelp.ithome.com.tw/articles/10414802)]

現今憑證技術的轉變（補充知識）

過去一個憑證的 Common Name 只能填寫一個網域。現代網路安全標準已經逐漸弱化 Common Name 的單一功能，改為強制檢查 **SAN (Subject Alternative Name，主體備用名稱)** 欄位。SAN 允許在同一個憑證中填入多個不同的 FQDN 或萬用字元網域（例如 `*.example.com`），靈活度比傳統的 Common Name 更高。 [[1](https://www.ssl.com/zh-TW/%E5%B8%B8%E8%A6%8B%E5%95%8F%E9%A1%8C/%E9%80%9A%E7%94%A8%E5%90%8D/)]

如果你目前正在**產生 CSR (憑證簽署請求)** 或**設定 Web 伺服器**，可以告訴我：

- 你正在配置哪種**伺服器系統**（如 Nginx、Apache、IIS）？
- 你需要設定的是**單一網域**、**多網域**還是**萬用字元（Wildcard）**網域？

我可以為你提供對應的設定步驟或指令範例。