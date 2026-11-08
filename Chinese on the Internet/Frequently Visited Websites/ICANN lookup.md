以下是針對「網域註冊資料查詢與協議指南」內容的重點摘要：

  

- **工具概述**：ICANN 設立的免費公共服務網站，讓用戶能夠即時查詢網域、IP 網路或自主系統編號（ASN）等互聯網資源的公開註冊與聯絡資訊。
    
      
    
- **運作原理（RDAP 與 WHOIS）**：
    
      
    - 主要採用基於 JavaScript 的**註冊數據訪問協議 (RDAP)** 來查詢、解析並以易讀格式顯示即時數據。
        
          
        
    - 若 RDAP 無法提供，系統會自動轉向 gTLD 註冊商的 **WHOIS 服務**作為備用查詢（WHOIS failover lookup）。
        
          
        
    - ICANN 先前的 WHOIS 查詢服務 (`whois.icann.org`) 已停止使用並直接重定向至此查詢工具。
        
          
        
- **數據隱私與可用性**：並非所有網域註冊資料都會公開；部分資料可能因法律或 ICANN 政策而隱藏，或僅對具備合法查看目的並獲批准的用戶可見。
    
      
    
- **查詢結果欄位**：查詢結果涵蓋網域資訊、日期、聯絡方式、註冊商與經銷商資訊、DNSSEC 資訊以及授權伺服器等，資料來源可能來自註冊機構或註冊商。
    
      
    
- **資料不準確或服務不可用時**：若聯絡資訊不準確，或因 RDAP 與 WHOIS 服務均不可用而無法取得結果，用戶可提交網域註冊數據不準確投訴，交由 ICANN 合規部門審查。

你的摘要把重點抓得很準，我幫你補幾個實務上容易卡住的細節，讓這份指南更完整。

### RDAP 與 WHOIS 的過渡現況
你摘要提到「RDAP 優先、WHOIS 備用」，這符合目前的政策方向，但要注意：ICANN 董事會已要求簽約方提供 RDAP，並**逐步淘汰 WHOIS 的強制義務**（過渡期至 2025 年 1 月 28 日左右）。所以 WHOIS failover 是過渡期的保險機制，長期會越來越少用到。查詢結果的**格式與欄位會因 RDAP 而更標準化**，但不同註冊商回傳的細節完整度仍有差異。

### 隱私遮蔽不是「查不到」，而是「分層可見」
你提到資料可能因法律或政策隱藏，這點很重要。實務上多數 gTLD 的註冊人姓名、地址、電話、email 會被**編輯或替換為假名化 email / 網路表單連結**（例如《註冊資料政策》允許的作法）。但這不代表完全查不到：**註冊商、註冊局、技術聯絡人、DNSSEC 狀態、授權伺服器**等欄位通常仍會公開。若你有合法目的（如商標爭議、執法），可透過註冊商提供的表單或 RDAP 的特定查詢路徑申請揭露。

### 投訴管道的實際用途
你摘要最後提到「資料不準確或服務不可用可投訴」，這條路的**適用範圍要分清楚**：
- **資料不準確**：確實可向 ICANN 合規部門投訴，但 ICANN 只會要求註冊商去驗證與修正，**不會直接幫你改資料**。
- **服務不可用**：若 RDAP 和 WHOIS 都掛掉，投訴是合理的，但 ICANN 的處理是要求簽約方恢復服務，不是代替你查詢。
- **隱私遮蔽導致查不到聯絡人**：這**不算**「資料不準確」，而是政策允許的遮蔽，投訴通常不會成立。這時應走註冊商提供的聯絡表單，或依爭議程序（如 UDRP）處理。

### 一個實務提醒
查詢結果中的「註冊商」與「經銷商」要分開看：**註冊商（Registrar）** 是 ICANN 認證、在註冊庫登記的簽約方；**經銷商（Reseller）** 可能是你實際購買域名的商家（如網路中文這類代理）。投訴或申請揭露時，對象通常是**註冊商**，不是經銷商。

---
**如果你是想把這份摘要整理成教材或內部文件，我可以幫你改寫成更結構化的版本；如果你是想實際查某個域名的資料卻卡住，告訴我域名和遇到的狀況，我幫你判斷是隱私遮蔽、服務問題還是投訴路徑。**


這份摘要介紹的是 ICANN Lookup（網域註冊資料查詢工具）。它與你前面學習的 WHOIS、RDAP、DNSSEC、網域註冊商等概念密切相關。

我幫你從網路中文客服工作的角度整理，並補充幾個容易混淆的地方。

## 一、ICANN Lookup 是什麼？

[Trouver les informations sociales associées à un nom de domaine](https://images.openai.com/static-rsc-4/MqbBH4kXGRWJSabuMXjEoJxDgpWWPpUWWd_uEhLtE9qRAD2_Ub4Uh8Z9zC_nX9XP5vhDjNVEUkSA_Q9i4Wu3ydsNcsAX2g9y6Ud8oYlG99bFgVbi0lKOXleU8f98xn8IpBVtmDTK65OaYxasQldn0vOwIQAZ2tSsKUzAmQ4vvvg?purpose=inline)

ICANN Lookup

網域註冊資料公開查詢工具

[前往 ICANN Lookup](https://lookup.icann.org/?utm_source=chatgpt.com)

它的用途是讓使用者查詢網域名稱的註冊資料，例如：

- 網域目前由哪一家註冊商管理？
    
- 網域何時註冊、何時到期？
    
- 使用哪一家 DNS 伺服器？
    
- 是否啟用 DNSSEC？
    
- 註冊資料中有哪些公開的聯絡資訊？
    

例如輸入：

`example.com`

就可以取得該網域的相關註冊資料。

要注意：ICANN Lookup 並不是網域註冊系統，也不能直接修改網域資料，它主要是查詢工具。

## 二、RDAP 與 WHOIS 有什麼不同？

這是整份內容最重要的部分。

|比較|WHOIS|RDAP|
|---|---|---|
|全名|WHOIS Protocol|Registration Data Access Protocol|
|資料格式|傳統文字格式|結構化資料（JSON）|
|標準化程度|不同服務可能有不同格式|較標準化|
|國際化支援|有限|較完善|
|存取控制|傳統公開查詢為主|支援分級存取與權限控制|
|現況|傳統查詢協議|gTLD 註冊資料查詢的主要協議|

可以用一個例子理解：

WHOIS 傳統回應可能長這樣：

```
Domain Name: EXAMPLE.COM
Registrar: EXAMPLE REGISTRAR
Creation Date: 2020-01-01
Expiry Date: 2027-01-01
Name Server: NS1.EXAMPLE.COM
```

RDAP 則以 JSON 結構傳遞資料：

```
{
  "ldhName": "example.com",
  "status": ["active"],
  "nameservers": [
    {
      "ldhName": "ns1.example.com"
    }
  ]
}
```

因此，RDAP 不只是 WHOIS 換一種顯示方式，而是採用更標準化、可擴充的資料存取協議。

重點

對 gTLD（例如 `.com`、`.net`），RDAP 已成為主要的註冊資料查詢機制，WHOIS 則逐步退居相容性或備援用途。

## 三、RDAP 查詢結果有哪些重要欄位？

以網域客服工作來說，可以特別熟悉以下資訊：

|欄位|意義|客服應用|
|---|---|---|
|Domain Name|網域名稱|確認客戶查詢的網域|
|Registrar|註冊商|確認網域由誰管理|
|Registration Date|註冊日期|確認網域建立時間|
|Expiration Date|到期日期|協助確認續約問題|
|Nameservers|授權名稱伺服器|判斷 DNS 由哪裡管理|
|Status|網域狀態|判斷是否被鎖定或有其他限制|
|DNSSEC|DNS 安全簽章狀態|協助排查 DNSSEC 問題|
|Registrant|註冊人|確認公開的持有人資訊|

其中我認為最值得你先熟悉的是 Registrar、Nameservers、Status、DNSSEC。

## 四、RDAP、WHOIS、DNSSEC 的關係

這三個概念很容易混在一起，但其實工作完全不同。

查詢註冊資料

RDAP / WHOIS

查詢網域註冊商、註冊日期、到期日期、名稱伺服器等資料。

驗證 DNS 資料

DNSSEC

驗證 DNS 回應的數位簽章，降低 DNS 資料遭偽造或竄改的風險。

解析網域

DNS

將網域名稱解析成 IP 位址，或取得其他 DNS 記錄。

簡單記憶：

- RDAP / WHOIS：查詢「這個網域的註冊資料」。
    
- DNSSEC：驗證「DNS 回答是否可信」。
    
- DNS：處理「這個網域要連到哪裡」。
    

## 五、實際客服情境

假設客戶打電話詢問：

「我的網域明明在網路中文註冊，為什麼 ICANN Lookup 顯示另一家註冊商？」

你可以先從以下方向判斷：

1. 客戶是否曾經將網域轉移至其他註冊商？
    
2. 是否查錯網域或查到其他相似網域？
    
3. RDAP 資料是否尚未更新？
    
4. 該網域的 TLD 是否由不同的註冊資料服務機制管理？
    

另一個情境：

「我的網域已經設定 DNSSEC，為什麼 ICANN Lookup 沒有顯示？」

可以先確認：

- 網域是否真的已經啟用 DNSSEC？
    
- 是否已向上層註冊機構正確提交 DS 記錄？
    
- RDAP 查詢結果是否有提供相關欄位？
    

注意：ICANN Lookup 的資料顯示與實際 DNS 驗證結果是兩件事，不能只憑查詢頁面就斷定 DNSSEC 一定正常或異常。

## 六、補充：這份摘要有兩個地方要特別注意

1. RDAP 無法查詢時，不代表一定會自動切換 WHOIS。

是否有 WHOIS 備援，取決於查詢工具及該 TLD 的服務支援情況。不要把它理解成所有網域都一定有自動備援。

2. ICANN 不負責所有網域的註冊資料。

ICANN Lookup 主要服務於 ICANN 管理框架下的網域查詢，尤其是 gTLD。像 `.tw` 網域，則應同時理解 TWNIC 的註冊資料查詢機制及相關政策。

對你目前的客服工作而言，最值得建立的概念是：RDAP / WHOIS 是查詢網域註冊資料的工具，而不是用來修改 DNS 或檢查網站主機是否正常的工具。