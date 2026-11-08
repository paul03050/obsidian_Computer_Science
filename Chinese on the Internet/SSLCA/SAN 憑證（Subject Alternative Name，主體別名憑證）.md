==**多網域憑證（Multi-Domain Certificate）又稱為 SAN 憑證（Subject Alternative Name，主體別名憑證），是一種只需要「一張證書」就能同時保護多個不同網域名稱（網址）的 SSL/TLS 加密憑證**==**。** [[1](https://www.ssl.com/zh-TW/%E7%94%A2%E5%93%81/%E7%B6%B2%E7%AB%99%E5%AE%89%E5%85%A8/TLS-SSL/%E5%A4%9A%E5%9F%9F/), [2](https://cms.aaasec.com.tw/index.php/2024/11/28/%E4%BB%80%E9%BA%BC%E6%98%AFssl%E6%86%91%E8%AD%89%EF%BC%9F/)]

在傳統情況下，一個 SSL 憑證只能保護一個特定的網址（例如 `www.example.com`）。但若公司旗下有多個完全不相干的品牌網站，使用 SAN 憑證就可以把所有網址都寫進同一張憑證裡，大幅簡化管理成本。 [[1](https://www.digicert.com/tw/faq/public-trust-and-certificates/what-is-a-multi-domain-san-certificate), [2](https://www.money888.cc/blog/multi-domain-ssl-certificate-guide/), [3](https://cms.aaasec.com.tw/index.php/2024/11/28/%E4%BB%80%E9%BA%BC%E6%98%AFssl%E6%86%91%E8%AD%89%EF%BC%9F/)]

---

💡 SAN 憑證的 3 大核心優勢

1. **網域不需有關聯性：** 可以同時保護完全不同的主網域。例如可以把 `my-shop.com`、`company-official.net` 和 `event2026.org` 通通包在同一張憑證裡。
2. **共用單一 IP 位址：** 在單台伺服器上架設多個 HTTPS 網站時，不需要為每個網址分配獨立的公網 IP，靠一張 SAN 憑證就能搞定，節省網路資源。
3. **管理與續約極方便：** 企業不需要維護幾十張憑證，只要追蹤這一張憑證的過期時間並統一進行一次續約即可。 [[1](https://www.digicert.com/tw/tls-ssl/multi-domain-ssl-certificates), [2](https://www.ssl.com/zh-TW/%E7%94%A2%E5%93%81/%E7%B6%B2%E7%AB%99%E5%AE%89%E5%85%A8/TLS-SSL/%E5%A4%9A%E5%9F%9F/), [3](https://cms.aaasec.com.tw/index.php/2024/11/28/%E4%BB%80%E9%BA%BC%E6%98%AFssl%E6%86%91%E8%AD%89%EF%BC%9F/), [4](https://www.asiaregister.com/tw/help/articles/86/What-is-a-Multi-domain-or-SAN-certificate), [5](https://www.digicert.com/tw/faq/public-trust-and-certificates/what-is-a-multi-domain-san-certificate), [6](https://shop.globalsign.com/cn-tw/ssl/multi-domain-ssl)]

---

🆚 常見憑證類型比較

為網站挑選 SSL 憑證時，通常會在 **單網域**、**萬用網域（Wildcard）** 與 **多網域（SAN）** 之間做抉擇： [[1](https://cms.aaasec.com.tw/index.php/2024/11/28/%E4%BB%80%E9%BA%BC%E6%98%AFssl%E6%86%91%E8%AD%89%EF%BC%9F/)]

|憑證類型|保護範圍|範例網站|適合誰？|
|---|---|---|---|
|**單網域憑證**|僅限單一特定網址|`www.example.com`|只有一個官網的小型企業或個人部落格。|
|**萬用網域憑證**  <br>(Wildcard)|單一主網域下的  <br>**所有一級子網域**|`*.example.com`  <br>(涵蓋 `shop.`、`blog.`、`mail.`)|擁有單一品牌，但旗下衍生出大量子系統或活動頁的網站。|
|**多網域憑證**  <br>(SAN / UCC)|**多個完全獨立**  <br>的主網域與子網域|`example.com`  <br>`shop.example.net`  <br>`my-app.org`|旗下經營多個不同品牌、不同網址，且網域數量相對固定的企業。|

---

⚠️ 使用注意事項

- **有數量上限：** 雖然一張憑證可以塞入多個網域，但各家憑證頒發機構（CA）都有上限規定（例如 100 到 250 個不等）。
- **變更需重新簽發：** 如果中途想要新增、刪除或修改憑證裡面的某個網址，必須向 CA 機構申請「重新簽發（Reissue）」才能生效。
- **資訊透明度：** 任何人去檢視該憑證的詳細資料時，都能看到這張憑證同時保護了哪些網址。如果旗下有些測試網站或不想公開的內部系統網址，就不建議與官網打包在同一張 SAN 憑證中。 [[1](https://www.asiaregister.com/tw/help/articles/86/What-is-a-Multi-domain-or-SAN-certificate), [2](https://shop.globalsign.com/cn-tw/ssl/multi-domain-ssl), [3](https://www.ssl.com/zh-TW/%E7%94%A2%E5%93%81/%E7%B6%B2%E7%AB%99%E5%AE%89%E5%85%A8/TLS-SSL/%E5%A4%9A%E5%9F%9F/), [4](https://medium.com/%E9%9A%A8%E7%AD%86%E9%9B%9C%E8%A8%98/%E7%94%A8-san-certificate-%E5%81%9A-multi-domain-certificate-c7403e05c697)]

_(註：資訊科技領域中還有另一個完全不同的技術也叫 **SAN（儲存區域網路，Storage Area Network）**，那是一種專門用來連接伺服器和硬碟儲存設備的高速網路架構，與本篇提到的網域/網址憑證無關。)_ [[1](https://www.netapp.com/zh-hant/data-storage/what-is-san-storage-area-network/), [2](https://zh.wikipedia.org/zh-tw/%E5%AD%98%E5%82%A8%E5%8C%BA%E5%9F%9F%E7%BD%91%E7%BB%9C), [3](https://www.digicert.com/tw/faq/public-trust-and-certificates/what-is-a-multi-domain-san-certificate), [4](https://cms.aaasec.com.tw/index.php/2024/11/28/%E4%BB%80%E9%BA%BC%E6%98%AFssl%E6%86%91%E8%AD%89%EF%BC%9F/)]

如果您正準備為公司網站申請憑證，可以告訴我：

- 您目前手邊總共有**幾個網域名稱**需要保護？
- 這些網域是屬於**同一個主網域底下的子網域**（如 `a.com`、`://a.com`），還是**完全不同的網址**（如 `a.com`、`b.net`）？

我可以幫您評估哪一種憑證類型對您最划算且最好管理！