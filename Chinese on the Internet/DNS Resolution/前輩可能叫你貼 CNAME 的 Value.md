在網路中文客服的情境中，**DNS Checker 與 NS Lookup 搭配使用，最重要的目的，是確認客戶新增的 CNAME 是否真的出現在「正確的權威 DNS 伺服器」上，以及外部 DNS 是否已經查得到。**

兩個工具的角色不太一樣：

- **NS Lookup：先確認 DNS 由誰管理。**
    
- **DNS Checker：再確認 CNAME 記錄是否能從不同 DNS 節點查詢到。**
    

不要只看 DNS Checker 顯示成功，就直接認定 CA（憑證機構）的 CNAME 驗證一定會通過。

## **一、建議的交叉查詢流程**

Step 1

**先用 NS Lookup 查詢網域的 NS**

確認客戶目前使用哪一家 DNS 服務商

例如查詢 `example.com` 的 NS，結果可能是：

- `ns1.cloudflare.com`
    
- `ns2.cloudflare.com`
    

Step 2

**確認客戶 CNAME 設定的位置**

如果 NS 指向 Cloudflare，原則上就要到 Cloudflare 新增 CNAME，而不是只在網路中文的 DNS 管理介面新增

Step 3

**用 DNS Checker 查詢 CNAME**

輸入 CA 提供的 CNAME Host，Record Type 選 CNAME

例如：

- Host：`_abc123.example.com`
    
- Type：`CNAME`
    
- Target：`_xyz789.ca-validation.com`
    

Step 4

**比對查詢結果與 CA 提供的 Target**

確認不同 DNS 節點是否解析出相同的 CNAME 目標

## **二、實際操作範例**

假設 CA（例如 Sectigo）要求客戶新增：

|欄位|CA 提供的資料|
|---|---|
|Record Type|CNAME|
|Host / Name|`_abc123.example.com`|
|Target / Value|`_xyz789.ca-validation.com`|

### **1. 先用 NS Lookup**

輸入：

`example.com`

假設查到：

```
example.com
NS: ns1.cloudflare.com
NS: ns2.cloudflare.com
```

這代表目前權威 DNS 伺服器指向 Cloudflare。

**客服判斷：** 如果客戶是在網路中文的 DNS 管理介面新增 CNAME，但實際 NS 指向 Cloudflare，那麼客戶可能是在錯誤的平台設定。

### **2. 再用 DNS Checker**

輸入完整的 CNAME Host：

`_abc123.example.com`

然後選擇 `CNAME`，查看不同 DNS 節點的回應。

預期結果：

```
_abc123.example.com
CNAME
_xyz789.ca-validation.com
```

你要比對的是：

- 查詢的 Host 是否正確。
    
- 回傳的 CNAME Target 是否與 CA 提供的完全一致。
    
- 不同 DNS 節點是否都查得到。
    

DNS Checker 的多節點查詢可以協助判斷不同公開 DNS Resolver 是否已取得新紀錄。

DEMARCA+1

## **三、如何判斷查詢結果？**

|NS Lookup|DNS Checker|客服判斷|
|---|---|---|
|指向正確 DNS 商|全部查到正確 Target|設定看起來成功|
|指向正確 DNS 商|全部查不到|可能沒新增、名稱錯誤或尚未套用|
|指向正確 DNS 商|部分查到、部分查不到|可能是 DNS 快取尚未更新|
|指向 Cloudflare|客戶卻在網路中文新增|可能設定位置錯誤|
|NS 查詢結果異常|CNAME 查詢也失敗|先確認 NS 委派及 DNS 服務狀態|

這裡有一個重要觀念：**DNS Checker 查得到，不代表 CA 一定已經驗證成功；DNS Checker 查不到，也不一定代表客戶設定錯誤。**

因為公開 DNS Resolver 可能還保留舊的快取資料。

Check DNS Records for Any Domain+1

## **四、進一步確認：直接查詢權威 DNS**

如果你想更精確地判斷，可以使用 Windows 命令提示字元（CMD）。

先查詢 NS：

```
nslookup -type=NS example.com
```

假設取得：

```
ns1.cloudflare.com
ns2.cloudflare.com
```

接著直接詢問其中一台權威 DNS：

```
nslookup -type=CNAME _abc123.example.com ns1.cloudflare.com
```

預期結果：

```
_abc123.example.com
canonical name = _xyz789.ca-validation.com
```

這個方法很有價值，因為它是直接詢問權威 DNS，而不是只看一般公開 DNS Resolver 的快取結果。

support.huaweicloud.com+1

## **五、客服實際判斷流程**

**客戶表示已經新增 CNAME，但 SSL 還是驗證失敗**

1. **NS Lookup**
    
    確認客戶實際使用的 DNS 服務商。
    
2. **DNS Checker**
    
    輸入完整 CNAME Host，選擇 CNAME，確認 Target 是否正確。
    
3. **比對結果**
    
    若各節點不一致，先考慮快取或傳播問題；若全部查不到，確認客戶是否設定在正確 DNS 平台。
    
4. **直接查詢權威 DNS**
    
    若公司有提供相關工具或允許使用 CMD，直接查詢 NS，確認權威 DNS 是否真的發布正確紀錄。
    
5. **通知 CA／內部 SSL 技術人員**
    
    若權威 DNS 已經查到正確紀錄，但 CA 仍然驗證失敗，就可以整理查詢結果並請技術人員協助確認 CA 端狀態。
    

## **六、客服最容易忽略的三個細節**

1. **查詢完整 Host，不是只查主網域。**  
例如 CA 要求 `_abc123.example.com`，就不能只查 `example.com`。

2. **Host 與 Target 不要混淆。**  
Host 是客戶要新增紀錄的位置；Target 是 CNAME 要指向的目的名稱。

3. **注意 DNS 平台自動補上網域名稱。**  
有些平台客戶輸入 `_abc123.example.com` 後，平台又自動補上 `.example.com`，可能造成名稱重複，實際變成 `_abc123.example.com.example.com`。

**我建議你在客服工作中記住這個順序：**

`NS Lookup → 確認 DNS 管理商 → DNS Checker → 比對 CNAME → 必要時直接查詢權威 DNS → CA 端確認`

這樣你就不只是告訴客戶「CNAME 有沒有設定成功」，而是能進一步判斷問題究竟在客戶設定、DNS 傳播，還是 CA 驗證端。



了解，前輩使用的應該是 Windows CMD 裡面的 `nslookup` **互動模式（Interactive Mode）**，而不是每次都在命令後面帶參數。

這種方式很適合客服人員逐步查詢 DNS。

## **一、進入 nslookup 互動模式**

先開啟 Windows CMD，輸入：

```
nslookup
```

會看到類似：

```
Default Server:  dns.google
Address:  8.8.8.8

>
```

看到 `>` 就代表已經進入互動模式，可以直接輸入查詢指令。

## **二、先查詢 NS**

輸入：

```
set type=NS
```

再輸入：

```
example.com
```

可能得到：

```
example.com
 nameserver = ns1.cloudflare.com
 nameserver = ns2.cloudflare.com
```

這樣就能確認網域目前委派的 DNS 伺服器。

## **三、再查詢 CNAME**

接著切換查詢類型：

```
set type=CNAME
```

輸入 CA 提供的完整 Host：

```
_abc123.example.com
```

如果設定成功，可能看到：

```
_abc123.example.com
 canonical name = _xyz789.ca-validation.com
```

這時就可以比對 `canonical name` 是否與 CA 提供的 Target 一致。

## **四、進一步：指定 DNS 伺服器查詢**

這個功能很重要。

假設 NS 查詢結果是：

```
ns1.cloudflare.com
ns2.cloudflare.com
```

你可以輸入：

```
server ns1.cloudflare.com
```

切換成指定 DNS 伺服器，再查詢：

```
_abc123.example.com
```

這樣就能直接詢問該 DNS 伺服器，而不是只詢問原本預設的 DNS Resolver。

## **五、整理成前輩可能使用的操作流程**

CMD 操作流程

```
nslookup

set type=NS
example.com

set type=CNAME
_abc123.example.com

server ns1.cloudflare.com
_abc123.example.com
```

**每個指令的用途**

|指令|功能|
|---|---|
|`nslookup`|進入互動模式|
|`set type=NS`|查詢 NS 記錄|
|`set type=CNAME`|查詢 CNAME 記錄|
|`server`|指定要詢問的 DNS 伺服器|
|`exit`|離開互動模式|

**一個容易忽略的地方：** `nslookup` 的查詢類型會持續沿用，直到你再次執行 `set type=` 改變它。因此，查完 NS 後記得切換成 CNAME。

另外，直接指定 `server ns1.cloudflare.com` 是向該伺服器發問，但如果要嚴格確認權威 DNS 的回應，還要確認它確實是該網域的權威伺服器，而不是只憑名稱判斷。

這套操作很適合你在接到 SSL CNAME 驗證失敗案件時，先自行做第一線 DNS 排查。


以下整理成一套可以直接在 Windows CMD 操作的完整流程，使用 `nslookup` **互動模式，並指定 Google Public DNS** `8.8.8.8`，用來檢查客戶的 SSL CNAME 是否設定成功。

## **一、完整指令操作**

假設客戶的 CA 提供以下資料：

|項目|範例|
|---|---|
|網域|`example.com`|
|CNAME Host|`_abc123.example.com`|
|CNAME Target|`_xyz789.ca-validation.com`|

**第一步：進入 nslookup**

```
nslookup
```

**第二步：指定 DNS Server**

```
server 8.8.8.8
```

預期顯示：

```
Default server: dns.google
Address: 8.8.8.8
```

**第三步：查詢 NS 記錄**

```
set type=NS
example.com
```

預期結果：

```
example.com
 nameserver = ns1.cloudflare.com
 nameserver = ns2.cloudflare.com
```

這裡可以確認 Google DNS 查到的 NS 委派資訊。

**第四步：查詢 CNAME 記錄**

```
set type=CNAME
_abc123.example.com
```

如果設定成功，預期結果：

```
_abc123.example.com
 canonical name = _xyz789.ca-validation.com
```

比對 `canonical name` 是否與 CA 提供的 Target 完全一致。

**第五步：離開 nslookup**

```
exit
```

## **二、完整指令一次複製**

**nslookup 互動模式操作清單**

Google DNS

```
nslookup
server 8.8.8.8

set type=NS
example.com

set type=CNAME
_abc123.example.com

exit
```

複製完整指令

實際使用時，將 example.com 和 _abc123.example.com 替換成客戶的網域及 CA 提供的 CNAME Host。

## **三、查詢結果如何判讀？**

|查詢結果|判斷|
|---|---|
|出現正確 canonical name|CNAME 查詢成功|
|`Non-existent domain`|查不到該紀錄，可能尚未設定或名稱錯誤|
|`Non-authoritative answer`|Google DNS 提供快取或遞迴查詢結果，不代表設定失敗|
|`DNS request timed out`|查詢逾時，不能直接判定 CNAME 錯誤|
|Target 與 CA 提供的不一致|可能設定錯誤|

## **四、特別提醒：8.8.8.8 的角色**

`server 8.8.8.8` 是指定 Google Public DNS 作為查詢對象。

因此，你查到的是 Google DNS 對外解析的結果，而不是直接詢問客戶的權威 DNS。

這對客服很有用，因為可以確認外部 DNS Resolver 是否已經查得到客戶新增的 CNAME。

如果要更進一步確認權威 DNS 是否真的發布紀錄，才需要另外指定 NS 伺服器查詢。

**記住：**

- `NS`：確認網域委派給哪些 DNS 伺服器。
    
- `CNAME`：確認 CA 要求的別名紀錄是否存在。
    
- `server 8.8.8.8`：透過 Google DNS 查詢公開解析結果。

對，你記得的方式很可能是：**把 CA 提供的 CNAME Target（值／Value）貼到 DNS Checker，檢查這個目標網域是否能被解析。**

不過，這裡要區分 CNAME 的兩個欄位，因為兩者用途不同。

## **一、CNAME 有兩個重要欄位**

假設 CA 提供：

|欄位|範例|
|---|---|
|Host / Name|`_abc123.example.com`|
|Value / Target|`_xyz789.ca-validation.com`|

- **Host**：客戶要新增 CNAME 的位置。
    
- **Value / Target**：CNAME 要指向的目的地。
    

## **二、DNS Checker 有兩種查法**

**方法 A：輸入 Host，查詢 CNAME（最重要）**

主要驗證方式

[Các bản ghi DNS thông dụng – Knowledge Base](https://images.openai.com/static-rsc-4/o94UV7jHLVW1pF9Nq_RpvDXRNLFFnnoitbzY3bgmLad_6iUlimZoYJku6JJzIOA1TDrLrHXRdte09-v-pYM4u-AA5VtG5hrZimt1ae1NtMlUOPMH2TiaDNoylABc9nSOmHj5v4XMV0S_qptnkueh5nqQ_NmPPsjYSZ4ZSG_3iwI?purpose=inline)

[kb.vinahosting.com](https://kb.vinahosting.com/cac-ban-ghi-dns-thong-dung/?utm_source=chatgpt.com)

1. 輸入 `_abc123.example.com`
    
2. Record Type 選擇 `CNAME`
    
3. 執行查詢
    
4. 檢查結果是否指向 `_xyz789.ca-validation.com`
    

**方法 B：輸入 Value，查詢 DNS**

輔助確認

1. 輸入 `_xyz789.ca-validation.com`
    
2. 選擇適當的 Record Type（例如 A 或 CNAME）
    
3. 查看這個目標名稱本身是否有 DNS 解析結果。
    

但這只能確認 Target 本身的 DNS 狀態，**不能證明客戶的 CNAME 已設定成功**。

## **三、為什麼前輩可能叫你貼 CNAME 的 Value？**

有幾種可能：

1. 前輩是要確認 CA 提供的 Target 本身是否有效。
    
2. 前輩可能使用的工具或內部 SOP，是將 Target 貼入 DNS Checker 做輔助檢查。
    
3. 你可能記得的是把 Host 貼進去，但當時前輩口頭稱它為 CNAME 值。
    

因此，我會建議你實際處理 SSL 驗證案件時，優先採用：

`CA 提供的 Host → DNS Checker 選 CNAME → 比對查詢結果與 CA 提供的 Value`

這才是驗證客戶是否正確設定 CNAME 的直接方法。

你也可以把前輩當時使用的 DNS Checker 網頁截圖給我，我可以幫你辨認他究竟是輸入 Host 還是 Value。對，你記得的方式很可能是：**把 CA 提供的 CNAME Target（值／Value）貼到 DNS Checker，檢查這個目標網域是否能被解析。**

不過，這裡要區分 CNAME 的兩個欄位，因為兩者用途不同。

## **一、CNAME 有兩個重要欄位**

假設 CA 提供：

|欄位|範例|
|---|---|
|Host / Name|`_abc123.example.com`|
|Value / Target|`_xyz789.ca-validation.com`|

- **Host**：客戶要新增 CNAME 的位置。
    
- **Value / Target**：CNAME 要指向的目的地。
    

## **二、DNS Checker 有兩種查法**

**方法 A：輸入 Host，查詢 CNAME（最重要）**

主要驗證方式

[Các bản ghi DNS thông dụng – Knowledge Base](https://images.openai.com/static-rsc-4/o94UV7jHLVW1pF9Nq_RpvDXRNLFFnnoitbzY3bgmLad_6iUlimZoYJku6JJzIOA1TDrLrHXRdte09-v-pYM4u-AA5VtG5hrZimt1ae1NtMlUOPMH2TiaDNoylABc9nSOmHj5v4XMV0S_qptnkueh5nqQ_NmPPsjYSZ4ZSG_3iwI?purpose=inline)

[kb.vinahosting.com](https://kb.vinahosting.com/cac-ban-ghi-dns-thong-dung/?utm_source=chatgpt.com)

1. 輸入 `_abc123.example.com`
    
2. Record Type 選擇 `CNAME`
    
3. 執行查詢
    
4. 檢查結果是否指向 `_xyz789.ca-validation.com`
    

**方法 B：輸入 Value，查詢 DNS**

輔助確認

1. 輸入 `_xyz789.ca-validation.com`
    
2. 選擇適當的 Record Type（例如 A 或 CNAME）
    
3. 查看這個目標名稱本身是否有 DNS 解析結果。
    

但這只能確認 Target 本身的 DNS 狀態，**不能證明客戶的 CNAME 已設定成功**。

## **三、為什麼前輩可能叫你貼 CNAME 的 Value？**

有幾種可能：

1. 前輩是要確認 CA 提供的 Target 本身是否有效。
    
2. 前輩可能使用的工具或內部 SOP，是將 Target 貼入 DNS Checker 做輔助檢查。
    
3. 你可能記得的是把 Host 貼進去，但當時前輩口頭稱它為 CNAME 值。
    

因此，我會建議你實際處理 SSL 驗證案件時，優先採用：

`CA 提供的 Host → DNS Checker 選 CNAME → 比對查詢結果與 CA 提供的 Value`

這才是驗證客戶是否正確設定 CNAME 的直接方法。

你也可以把前輩當時使用的 DNS Checker 網頁截圖給我，我可以幫你辨認他究竟是輸入 Host 還是 Value。