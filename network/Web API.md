

## 一、 資料型態：靜態（Static）與 動態（Dynamic）

- **靜態資料：** 指下載後不會自動更新的資料，例如存在硬碟中的 CSV 檔、Excel 檔、文字檔或 SQL 資料傾卸檔（Data dumps）。這類資料容易隨時間而過期、無法反映最新現況 。
    
- **動態資料：** 始終保持最新狀態的資料，例如持續有銀行交易或問卷寫入的資料庫，或是透過 **Web API** 獲取的資料 。數據科學應用通常需要查詢動態資料，以確保使用最新的資訊進行分析或預測 。
    

## 二、 什麼是 API 與 Web API？

- **API（應用程式介面）：** 提供給程式設計師使用的**函數（Functions）**，其核心概念是讓開發者「不需知道內部實作細節」就能直接使用其功能（例如 Python 的 Scikit-learn 機器學習庫）。
    
- **Web API：** 建立在網路上的 API，是網站提供給程式設計師的一組**端點（Endpoints）**（可視為網路上的函數），用於獲取（Get）或推送（Post）資料 。
    

## 三、 Web API 的運作流程（客戶端與伺服器）

1. **閱讀文件：** 客戶端（開發者、Python 腳本或排程工作）必須先閱讀 API 文件，了解使用規範與契約 。
    
2. **發送請求：** 客戶端向伺服器發送一個格式正確的 **HTTP GET 請求**（通常是一個 URL，有時包含 Headers 或 Body） 。
    
3. **伺服器驗證與處理：** 伺服器接收請求後會進行驗證（如：確認身分與權限、檢查格式是否正確），若正確則會向資料庫查詢並重新封裝資料 。
    
4. **回傳回應：** 伺服器將資料打包成回應（Response）傳回給客戶端，最常見的格式為 **JSON**（其餘還有 XML 或 HTML） 。
    

## 四、 URL 請求的組成結構

以 Apple 提供的 iTunes 搜尋 API 為例（`https://itunes.apple.com/search?term=thor&media=movie`）：

|**組成部分**|**範例內容**|**說明**|
|---|---|---|
|**通訊協定 (Protocol)**|`https`|定義客戶端與伺服器如何溝通。後面的 **s** 代表 Secure（安全），表示通訊內容經過加密，非明文傳輸 。|
|**網域 (Domain / Subdomain)**|`itunes.apple.com`|`apple.com` 為網域，`itunes` 為子網域 。透過 **DNS（網域網路服務）** 會將這些人類好記的文字轉換為伺服器的 IP 位址 。|
|**路徑 (Path)**|`/search`|指向伺服器上的特定**端點（Endpoint）**，決定要執行伺服器上的哪一個函數 。|
|**查詢字串 (Query String)**|`?term=thor&media=movie`|由問號 `?` 開頭，後面跟著以 `&` 分隔的 `鍵=值 (key=value)` 配對。這是客戶端用來指定請求細節（如搜尋關鍵字、過濾類型）的地方 。|

## 五、 JSON 回應與解析

- **JSON 簡介：** 全名為 JavaScript Object Notation（JavaScript 物件表示法），本質上就是一個由 **鍵值對（Key-Value pairs）** 組成的字典（Dictionary） 。
    
- **資料解析：** 在 Python 中，通常會將收到的 JSON 回應直接轉換成 Python 字典 。如果資料層層嵌套，開發者會採用「由內而外（bottom-up）」的方式，先找出目標值對應的鍵，再一路推回最外層的根 JSON 物件（Root dictionary）來完成解析 。