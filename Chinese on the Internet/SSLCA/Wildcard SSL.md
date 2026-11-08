==**Wildcard SSL（萬用字元 SSL 憑證）是一種允許在單張憑證中，同時保護一個網域名稱（主網域）以及其旗下「無限個」第一層子網域的數位憑證。**== 它在技術上使用星號（`*`）來指定保護範圍（例如 `*.example.com`），能大幅簡化擁有多個子網域網站的管理成本與流程。 [[1](https://shop.globalsign.com/cn-tw/ssl/wildcard-ssl), [2](https://www.digicert.com/tw/faq/public-trust-and-certificates/what-is-a-wildcard-certificate), [3](https://gs.cloudmax.com.tw/ssl/ssl_wildcard_ssl.html), [4](https://www.godaddy.com/zh/help/what-is-a-wildcard-ssl-certificate-567)]

運作原理與範例

當申請一張網域名稱為 `*.example.com` 的萬用字元憑證時，它可以同時保護以下網站：

- `example.com`（多數 CA 預設包含主網域）
- `www.example.com`
- `blog.example.com`
- `shop.example.com` [[1](https://www.godaddy.com/zh/help/what-is-a-wildcard-ssl-certificate-567)]

⚠️ **重要限制：** 星號範圍**不包含點號（`.`）**。這意味著它**只能保護一級子網域**。  
例如 `*.example.com` **無法**保護二級子網域 `internal.blog.example.com`。若要保護該層級，則需要另外申請一張 `*.blog.example.com` 的憑證。 [[1](https://www.digicert.com/tw/faq/public-trust-and-certificates/what-is-a-wildcard-certificate)]

---

萬用字元憑證 vs. 其他憑證類型

| 憑證類型                       | 保護範圍                              | 優點            | 缺點                 |
| -------------------------- | --------------------------------- | ------------- | ------------------ |
| **單一網域憑證 (Single Domain)** | 僅限一個精確的 FQDN（如 `www.example.com`） | 價格最便宜、發卡速度快   | 每個子網域都要分開買與管理      |
| **萬用字元憑證 (Wildcard)**      | 主網域 + 所有**第一層**子網域                | 子網域數量無上限、方便擴充 | 價格較單網域高、有單一私鑰外洩的風險 |
| **多網域憑證 (SAN / UCC)**      | 多個完全不同的主要網域（如 `a.com`, `b.com`）   | 適合管理多個不同品牌的網站 | 新增網域時通常需要重新審核與核發   |

---

優點與潛在風險

**核心優勢：**

- **高靈活性：** 未來網頁隨時要上線新的子網域（如 `news.example.com`），直接佈署同一張憑證即可，不需付費重新申請。
- **管理輕鬆：** 管理員只需要記住一個過期時間，並在伺服器上維護一套憑證檔案即可。 [[1](https://shop.globalsign.com/cn-tw/ssl/wildcard-ssl)]

**安全性風險：**

- **金鑰外洩株連廣：** 由於所有子網域共用同一張憑證與私鑰，若其中一個防護較弱的測試用子網域（例如 `test.example.com`）被駭客攻破並取得私鑰，駭客就能用來偽造或竊聽主網域及其他子網域的流量（如 ALPACA 攻擊）。 [[1](https://blog.darkthread.net/blog/wildcard-tls-cert-risk/)]

---

如何取得？

1. **免費方案：** [Let's Encrypt](https://letsencrypt.org/) 提供免費的萬用字元憑證，但通常必須透過 DNS-01 驗證機制（修改 DNS TXT 紀錄），且需要設定自動化腳本（如 Certbot）每 90 天進行更新。 [[1](https://medium.com/learn-or-die/%E5%88%A9%E7%94%A8-lets-encrypt-%E4%BE%86%E8%87%AA%E5%8B%95%E7%B0%BD%E7%BD%B2%E4%B8%A6%E6%9B%B4%E6%96%B0-ssl-%E6%86%91%E8%AD%89-wildcard-26b49114bf73)]
2. **商業方案：** 可向 [DigiCert](https://www.digicert.com/tw/)、GlobalSign、GoDaddy 等各大憑證發行機構（CA）購買。商業方案通常提供更高的相容性、網站安全標章以及實質的商業保險（Warranty）。 [[1](https://gs.cloudmax.com.tw/ssl/ssl_wildcard_ssl.html), [2](https://www.digicert.com/tw/faq/public-trust-and-certificates/what-is-a-wildcard-certificate), [3](https://www.godaddy.com/zh/help/what-is-a-wildcard-ssl-certificate-567), [4](https://www.taki.com.tw/wildcard-ssl/?srsltid=AYCX9vKYDIUpMJ8lDwHWddV1PY8hoerK5DJYQTthcdSSi8z1jHPQDiza)]

您目前是在規劃架設新網站，還是現有網站有評估憑證的需求呢？如果需要，我可以進一步為您提供：

**Let's Encrypt 萬用憑證的自動申請與安裝教學**（如 Nginx / Apache）**商業憑證的驗證等級（DV 域名驗證 vs. OV 組織驗證）差異比較****如何防範萬用字元憑證的安全漏洞與風險**


==**是的，業界常說的「通用型 SSL」指的就是 Wildcard SSL（萬用字元憑證）。**==

「通用型」是台灣電信商（如中華電信）、網路中文、捕夢網等國內網域主機商在銷售時**最常用的中文俗稱**，因為它的星號（`*`）具備「通用」於所有子網域的特性。

為了讓您在採購或與工程師溝通時更精準，以下幫您整理常見的中文名稱對應，以及採購時的一個**常見陷阱**：

1. 各種名稱對應

在國內外網站上，它們指的都是同一個東西：

- **英文正式名稱：** Wildcard SSL Certificate
- **台灣常見俗稱：** 通用型憑證、萬用字元憑證、星號憑證、野卡憑證（由 Wildcard 直譯而來）

2. ⚠️ 採購時最容易搞混的「陷阱」

在跟廠商採購時，千萬不要只看中文「通用」兩個字，必須確認英文原文，因為有時候會跟另一個字搞混：

- **通用型 (Wildcard SSL)：** 保護 `*.example.com`（單一網域下的**無限個子網域**）。
- **多網域型 (SAN / UCC)：** 英文有時會寫作 **Unified Communications Certificate (UCC)**。有些廠商會把 UCC 翻譯成「統一通訊憑證」或誤稱為「多網域通用型」，這常導致客戶買錯。

**最簡單的確認方法：**  
看規格內有沒有寫 **「支援星號 (`*`)」** 或 **「子網域數量無限制」**。只要這兩個條件成立，它就是您所知道的 Wildcard SSL。

您目前是在台灣的主機商（如中華電信、遠傳、遠振等）看到「通用型」這個名稱在評估預算嗎？

如果您正在考慮購買，我們可以聊聊：

- 該廠商的通用型是屬於 **DV（域名驗證）** 還是 **OV（企業驗證）**？（這兩者價差非常大）
- 您預計要安裝的**伺服器環境**（例如：IIS, Nginx, 或是雲端 Load Balancer），我可以提醒您安裝通用型憑證時的注意事項。
