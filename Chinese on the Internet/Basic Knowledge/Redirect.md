==**301 永久性轉址（Permanent Redirect）與 302 暫時性轉址（Temporary Redirect）的核心差異，在於「搜尋引擎（SEO）權重的轉移方式」與「舊網址是否保留在搜尋結果中」。**== 選擇錯誤的轉址方式會直接影響網站的關鍵字排名與流量。 [[1](https://www.ibest.tw/seo-detail/301-move-permanently/), [2](https://www.ridea.com.tw/blog/d/seo-301-redirects)]

301 與 302 轉址快速對比

|特性|[301 永久性轉址](https://zh.wikipedia.org/zh-tw/HTTP_301)|[302 暫時性轉址](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Reference/Status/302)|
|---|---|---|
|**定義**|頁面已**永久**搬移到新網址。|頁面**暫時**引導到其他網址。|
|**SEO 權重**|**完整轉移**至新網址。|**保留**在舊網址，不轉移。|
|**Google 索引**|舊網址會被移出，改收錄新網址。|預設保留舊網址索引，新網址不被當作標準。|
|**對使用者的體驗**|自動跳轉，體感無差異。|自動跳轉，體感無差異。|

---

💡 什麼時候用 301 永久轉址？

當你**未來不打算再使用舊網址**時，就必須使用 301 轉址。這能確保過去累積的 SEO 分數與流量不會白費。 [[1](https://www.awakening-design.com.tw/blog/domain-url-knowledge/purpose_of_forwarding/), [2](https://seo.whoops.com.tw/301-vs-302-redirects/), [3](https://www.ibest.tw/seo-detail/301-move-permanently/)]

- **網站改版或更換網域：** 例如從 `old-domain.com` 永久更換為 `new-domain.com`。
- **網址結構調整：** 調整內部目錄結構，例如 `blog/post-1` 變成 `article/post-1`。
- **規格合併：** 將全站由 HTTP 協議永久遷移至更安全的 HTTPS。
- **處理重複內容：** 將 `://example.com` 與 `example.com` 統一導向其中一個標準網址。 [[1](https://www.tsg.com.tw/blog/blog-119-301-302-404), [2](https://zh.wikipedia.org/zh-tw/HTTP_301), [3](https://sec-site.com/wiki-302-redirect/), [4](https://www.ibest.tw/seo-detail/301-move-permanently/)]

💡 什麼時候用 302 暫時轉址？

當你**短期內需要跳轉，但未來還要繼續經營原網址**時使用。 [[1](https://www.pongo.com.tw/what-is-a-301-or-302-redirect/), [2](https://www.cadiis.com.tw/blog/what-is-301-redirect)]

- **網站臨時維護：** 系統維護期間，將使用者暫時導向「維護中」的公用頁面。
- **短期促銷活動：** 節慶（如雙 11）期間將首頁暫時導向活動特賣頁，活動結束後即恢復。
- **A/B 測試（網頁測試）：** 測試新版頁面的轉換率，但仍想保留舊網頁的搜尋排名。
- **地區或語系自動轉換：** 根據使用者 IP 暫時引導到對應語系（如 `/zh/` 或 `/en/`），但原網址依舊存在。 [[1](https://www.cadiis.com.tw/blog/what-is-301-redirect), [2](https://www.ibest.tw/seo-detail/301-move-permanently/), [3](https://www.tsg.com.tw/blog/blog-119-301-302-404), [4](https://ftdesign.tw/how-to-choose-a-forwarding-service-understand-301-and-302/)]

---

⚠️ 常見的 SEO 踩雷誤區

1. **網站改版卻誤用 302 轉址：**  
    若將網站永久搬家誤設為 302，Google 會判定這是暫時的，進而**不把舊網址的排名和權重分給新網址**。長期下來，新網站拿不到排名，舊網址又因為呈現跳轉狀態而被扣分，導致兩敗俱傷。 [[1](https://www.ridea.com.tw/blog/d/seo-301-redirects), [2](https://www.pongo.com.tw/what-is-a-301-or-302-redirect/)]
2. **轉址後立刻關閉舊主機：**  
    不論是 301 或 302，搜尋引擎都需要時間重新檢索（Crawl）。設定 301 後，舊網域和轉址設定至少需要維持**半年到一年以上**，確保 Google 機器人完全解讀並轉移權重後，才能安全地廢棄舊網域。

您目前正遇到需要設定轉址的狀況嗎？如果願意分享，我可以為您提供更具體的建議：

- 您的網站目前是要做**全面改版**、**換網域**，還是**短期活動跳轉**？
- 您打算透過哪種方式設定（例如：**WordPress 外掛**、**Apache .htaccess**、**Nginx 基礎設定**，或是 **Cloudflare**）？