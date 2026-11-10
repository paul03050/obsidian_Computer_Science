在 Sectigo（前身為 Comodo）的 SSL 憑證鏈結中，==**Root R46 與 USERTrust（通常在組織驗證中對應包含 CAOV36）最核心的差異在於「新舊世代交替」與「角色功能的不同」**==。

Sectigo 為了符合現代瀏覽器的安全規範，自 2025 年起將憑證核發架構全面移轉。以下是兩者的直接對比與差異： [[1](https://www.sectigo.com/knowledge-base/detail/Sectigo-Public-Root-CAs-Migration), [2](https://www.net-chinese.com.tw/pageNews/10600/)]

核心差異對比

| 特性 / 項目     | **Sectigo Root R46**                        | **USERTrust (含 CAOV36)**                             |
| ----------- | ------------------------------------------- | ---------------------------------------------------- |
| **憑證角色**    | **全新主根憑證 (Modern Root)**                    | **傳統根憑證 / 交叉簽署憑證 (Legacy Root)**                     |
| **主要定位**    | 現代安全規範下的單一用途（伺服器驗證）根憑證                      | 用於**相容舊版設備**與舊系統的橋樑                                  |
| **中繼憑證代碼**  | 對應全新的核發中繼（如 **OV R36**）                     | 舊架構下的組織驗證中繼（如 **CA OV R36** / 舊版 `USERTrust RSA...`） |
| **現代瀏覽器相容** | **原生信任**（Chrome, Edge, Safari, Firefox 最新版） | 現代瀏覽器因政策正逐步對其 TLS 功能實施「不信任（Distrust）」                |
| **舊設備相容性**  | 較差（極舊的 Android/Java 系統可能未內建）                | **極佳**（已內建於絕大多數舊系統中多年）                               |

---

詳細差異剖析

1. 為什麼會有 Root R46？（全新世代的主根）

- **背景：** 舊的根憑證（如 USERTrust、AAA）屬於多功能憑證（同時用於網頁 TLS、代碼簽署、郵件加密）。各大瀏覽器（如 Google Chrome、Mozilla）為了提高資安層級，規定未來的 Root CA 必須「單一用途化」。 [[1](https://www.sectigo.com/knowledge-base/detail/Sectigo-Root-Certificates), [2](https://www.sectigo.com/resource-library/changes-to-root-ca-hierarchies-and-trust-status)]
- **由來：** 為了符合新規，Sectigo 推出了全新的主根憑證，全名為 **Sectigo Public Server Authentication Root R46**。現在所有新核發的 Sectigo SSL 憑證，其原生信任鏈的最頂端都是 R46。 [[1](https://www.sectigo.com/knowledge-base/detail/Sectigo-new-Public-Roots-and-Issuing-CAs-Hierarchy), [2](https://www.reddit.com/r/fortinet/comments/1m19kcq/fortigate_ssl_termination_and_new_sectigo/?tl=zh-hant), [3](https://support.boomi.com/boomi_support?id=kb_article_view&sysparm_article=KB0010421), [4](https://www.sectigo.com/knowledge-base/detail/Sectigo-Public-Root-CAs-Migration)]

2. 什麼是 USERTrust 與 CAOV36？（相容性的重要橋樑）

- **背景：** 雖然 R46 在現代作業系統和瀏覽器中已經內建，但是許多舊設備（例如：未聯網的 Java 環境、舊版 Linux、舊款行動裝置或工業控制設備）的內建憑證清單還沒有及時更新 R46。 [[1](https://help.racent.com/zh-Hant/ssl/support-services/sectigo-new-root-cross-cert/), [2](https://www.sectigo.com/knowledge-base/detail/Sectigo-new-Public-Roots-and-Issuing-CAs-Hierarchy), [3](https://sctgo.com/blog/42/)]
- **功能：** **USERTrust RSA Certification Authority** 是一個已經存在多年且被所有新舊設備廣泛信任的舊根憑證。而帶有 `CAOV36`（意指 **CA OV R36**）的中繼憑證，則是 Sectigo 用來**交叉簽署（Cross-Signing）**的鏈結工具。 [[1](https://support.boomi.com/boomi_support?id=kb_article_view&sysparm_article=KB0010421), [2](https://sctgo.com/blog/42/), [3](https://www.tbs-certificates.co.uk/FAQ/en/SectigoPublicServerAuthenticationCAOVR36.html), [4](https://juejin.cn/post/7511910020524965942), [5](https://www.sectigo.com/knowledge-base/detail/Sectigo-new-Public-Roots-and-Issuing-CAs-Hierarchy)]
- **運作機制：** 透過交叉簽署，新憑證在遇到舊設備時，會繞道指向 USERTrust 舊根，確保舊設備不會跳出「憑證不受信任」的資安警告。 [[1](https://sctgo.com/blog/42/), [2](https://help.racent.com/zh-Hant/ssl/support-services/sectigo-new-root-cross-cert/)]

---

網頁伺服器（如 Apache, Nginx, IIS）該如何部署？

現在從 [Sectigo 官方](https://www.sectigo.com/) 或代理商下載憑證時，通常會提供包含這兩者的 **CA Bundle（中繼憑證包）**。 [[1](https://www.sectigo.com/knowledge-base/detail/Sectigo-Intermediate-Certificates), [2](https://www.ssl.com.tw/Down/index.asp)]

- **如果您服務的用戶都是現代化裝置：**  
    憑證鏈只需指向：`您的域名憑證` ➡️ `Sectigo Public Server Authentication CA OV R36` ➡️ `Sectigo Public Server Authentication Root R46`。
- **如果您需要服務舊型用戶端（如 Java 舊環境、老舊系統）：**  
    您必須在伺服器上安裝**完整的交叉鏈結憑證（Cross-signed Bundle）**，這時憑證鏈會延伸為四層：`您的憑證` ➡️ `OV R36` ➡️ `Root R46` ➡️ **`USERTrust RSA CA`**。這樣不論新舊裝置都能完美識別。 [[1](https://learn.microsoft.com/en-sg/answers/questions/6019635/front-door-standard-serves-self-signed-sectigo-roo), [2](https://www.reddit.com/r/fortinet/comments/1m19kcq/fortigate_ssl_termination_and_new_sectigo/?tl=zh-hant), [3](https://sctgo.com/blog/42/)]

為了給您最精準的配置建議，請問：

1. 您目前是在進行 **網頁伺服器（Nginx / Apache / IIS）** 的憑證安裝，還是遇到了特定的**連線錯誤錯誤訊息**？
2. 您的網站或 API 服務是否需要對接 **Java 舊程式、舊版 Android** 等特定用戶端？