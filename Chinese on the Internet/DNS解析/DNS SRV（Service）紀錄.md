==**DNS SRV（Service）紀錄是用來指定特定服務（如 VoIP、SIP、XMPP 或 Minecraft 伺服器）之主機位置與連接埠（Port）的 DNS 資源紀錄**==**。** [[1](https://www.cloudflare.com/zh-tw/learning/dns/dns-records/dns-srv-record/), [2](https://zh.wikipedia.org/zh-tw/SRV%E8%AE%B0%E5%BD%95)]

傳統的 A 紀錄或 CNAME 只能將網址導向某個 IP 位址，但無法指定連接埠。SRV 紀錄解決了這個問題，它允許用戶端直接透過 DNS 查到該服務在「哪一台伺服器」的「哪一個連接埠」上運作。 [[1](https://docs.gandi.net/zh-hant/domain_names/faq/record_types/srv_record.html), [2](https://www.cloudflare.com/zh-tw/learning/dns/dns-records/dns-srv-record/)]

---

🧩 SRV 紀錄的標準格式

SRV 紀錄在 DNS 設定中的標準格式如下：  
`_service._proto.name. TTL IN SRV Priority Weight Port Target`

各欄位所代表的意義：

- **`_service`（服務名稱）：** 該服務的法定名稱（前方必須加上底線），例如 `_sip`、`_ldap`、`_minecraft`。
- **`_proto`（通訊協定）：** 該服務使用的傳輸協定（前方必須加上底線），通常為 `_tcp` 或 `_udp`。
- **`name`（網域名稱）：** 提供該服務的網域。
- **`Priority`（優先順序）：** 數值**越低越優先**。用戶端會先連線到數值最小的伺服器，若該伺服器斷線，才會嘗試次低的伺服器（做為備援機制）。
- **`Weight`（權重）：** 用於負載平衡。當兩台伺服器的優先順序相同時，數值**越高**的伺服器會被分配到越多流量。
- **`Port`（連接埠）：** 該服務實際運作的 TCP 或 UDP 埠號。
- **`Target`（目標主機）：** 提供該服務的主機名稱（注意：**必須指向 A 紀錄或 AAAA 紀錄**，規範上不可指向 CNAME 紀錄）。 [[1](https://zh.wikipedia.org/zh-tw/SRV%E8%AE%B0%E5%BD%95), [2](https://www.cloudflare.com/zh-tw/learning/dns/dns-records/dns-srv-record/), [3](https://jackkuo.org/post/why_browsers_not_support_srv/), [4](https://www.cloudflare-cn.com/learning/dns/dns-records/dns-srv-record/), [5](https://check-host.cc/zh-tw/dns/srv-record)]

---

💡 實際設定範例

假設你想為你的網域 `example.com` 架設一個運作在 Port `25565` 的 Minecraft 伺服器，主機名稱是 `://example.com`：

|欄位|填寫內容|說明|
|---|---|---|
|**服務 (Service)**|`_minecraft`|指定 Minecraft 服務|
|**協定 (Protocol)**|`_tcp`|使用 TCP 協定|
|**名稱 (Name)**|`example.com`|網域主體|
|**優先級 (Priority)**|`0`|最高優先權|
|**權重 (Weight)**|`5`|流量分配權重|
|**連接埠 (Port)**|`25565`|遊戲伺服器的 Port|
|**目標 (Target)**|`://example.com`|實際指向的伺服器主機|

**在 DNS 系統中的呈現形式：**  
`_minecraft._://example.com. 86400 IN SRV 0 5 25565 ://example.com.` [[1](https://www.cloudflare.com/zh-tw/learning/dns/dns-records/dns-srv-record/)]

---

🛠️ 常見的應用場景

1. **企業目錄服務：** 微軟的 Active Directory（AD）高度依賴 SRV 紀錄，讓網域內的手機、電腦等設備能自動尋找到「網域控制站（Domain Controller）」。 [[1](https://learn.microsoft.com/zh-tw/troubleshoot/windows-server/networking/verify-srv-dns-records-have-been-created)]
2. **網路語音與即時通訊：** 像是 SIP（網路電話）或是 XMPP（即時通訊服務）常透過 SRV 紀錄來指引通訊埠。 [[1](https://www.cloudflare.com/zh-tw/learning/dns/dns-records/dns-srv-record/), [2](https://zh.wikipedia.org/zh-tw/SRV%E8%AE%B0%E5%BD%95)]
3. **遊戲伺服器：** 很多 Minecraft 玩家在租用外部伺服器時，若不想讓朋友在輸入網址時還要加上一串 `:連接埠`（例如 `://example.com`），就會設定 SRV 紀錄，這樣朋友只需輸入 `://example.com` 即可直接連線。 [[1](https://zh.wikipedia.org/zh-tw/SRV%E8%AE%B0%E5%BD%95), [2](https://www.reddit.com/r/CloudFlare/comments/1i0rw8f/what_is_srv_in_dns_i_see_it_with_settings_from_my/?tl=zh-hant)]

_註：一般的網頁瀏覽器（如 Chrome, Safari）預設**不支援**也**不會查詢** SRV 紀錄，因此建置一般的 Web 網站（HTTP/HTTPS）通常不需要設定 SRV 紀錄。_ [[1](https://jackkuo.org/post/why_browsers_not_support_srv/)]