在 SSL 憑證的實務操作中，**組織驗證（OV）與延伸驗證（EV）的「預先審查流程」（Organization Pre-vetting）**，是大型企業、跨國公司或政府機構非常愛用的進階功能。

簡單來說，**預先審查就是「先把你的公司身份審好、存檔（有效期約 1-2 年）。之後你要為任何子網域或新產品重發、新申購憑證時，CA 就能跳過繁瑣的人工審查，幾分鐘內直接核發。」**

---

🏢 為什麼需要「預先審查」？

一般的 OV/EV 憑證審核非常耗時（通常需要 2 到 7 個工作天）。如果每次申請一張新憑證，都要讓工程師、法務、行政人員重複提交營業執照、接聽憑證機構（CA）的國際照會電話，效率會非常低。 [[1](https://ftdesign.tw/how-to-apply-for-ssl-certificate-process-cost-verification/), [2](https://www.taki.com.tw/organization-validation-ssl/?srsltid=AYCX9vKD4TfXoT8b7ebICCSpgE26mfdeLR47C56tRceDYH9T5VXYoa-m), [3](https://inspire-dt.com/article/detail/ssl-certificate-application-common-mistakes-guide), [4](https://zhuanlan.zhihu.com/p/1982092886716725149)]

透過 **Pre-vetting（預先審查）**，企業可以將「驗證公司身份」與「申請實體憑證」這兩件事分開處理。

---

⏳ 預先審查的 4 大步驟

當您在 DigiCert、Sectigo 或 GlobalSign 等國際大廠後台啟動「預先審查」時，流程如下：

```
[企業提交公司資料] ──> [CA 進行權威資料庫人工核對] ──> [電話照會驗證] ──> [核發「預審通過」狀態與編號]
```

1. **提交官方證明：**  
    企業需要提交商業登記證、營業執照、組織機構代碼，以及填寫授權書（Subscriber Agreement）。 [[1](https://www.sectigo.com/faqs/detail/EV-Validation-Sample-Documents), [2](https://ftdesign.tw/how-to-apply-for-ssl-certificate-process-cost-verification/)]
2. **第三方資料庫核對：**  
    CA 的審查團隊會透過鄧白氏編號（D-U-N-S Number）、政府商工登記公示系統、或是特定的黃頁資料庫，核對公司的法定名稱、註冊地址是否完全屬實。 [[1](https://zhuanlan.zhihu.com/p/1982092886716725149), [2](https://ftdesign.tw/how-to-apply-for-ssl-certificate-process-cost-verification/)]
3. **電話照會（Duns & Bradstreet / 國際電話）：**  
    CA 會透過第三方登記的法人電話打給企業的聯絡人（通常是 IT 主管或行政主管），確認「貴公司確實有委託申請 SSL 憑證」。 [[1](https://www.digicert.com/tw/difference-between-dv-ov-and-ev-ssl-certificates), [2](https://zhuanlan.zhihu.com/p/1982092886716725149)]
4. **配發「預審編號/狀態」：**  
    審查通過後，系統會生成一個**預審編號（Vetting ID / Organization ID）**，並在後台顯示該組織為「已驗證（Validated/Pre-vetted）」狀態。

---

⚡ 預審通過後的好處

- **瞬間核發（Instant Issuance）：**  
    在接下來的 13 到 27 個月內（視 CA 規定而定），只要公司資料沒變，技術人員在申請新憑證或 Reissue（重發）時，只要勾選該「預審編號」，CA 就會**略過公司審核**。原本需要好幾天的企業憑證，能在 **15 分鐘到 1 小時內**直接核發，速度就跟最便宜的 DV（網域驗證）憑證一樣快。
- **網域彈性大：**  
    大型企業隨時可能上線新產品（例如 `://company.com`、`://company.com`），只要網域所有權驗證（DNS/Email）通過，憑證就能立刻綁定該企業名稱並上線，完全不會耽誤專案時程。

📝 實務總結

如果您在後台看到系統要求進行「組織預審」或配發了「預審編號」，代表您的**公司基本資料目前正在被 CA 審核中（或已經審好備用）**。這是一次性的痛，只要熬過這次的人工電話核對，未來 1~2 年內該公司的憑證維護與新購都會變得極度方便。

您目前正在操作哪一家憑證商（例如 **DigiCert、Sectigo 還是 GlobalSign**）的預審呢？如果卡在**電話照會**或**文件審查不通過**的階段，您可以告訴我，我能幫您確認台灣企業常見的補件方式。