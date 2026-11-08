**Base64 是一種將二進位資料轉換為純文字（ASCII 字元）的「編碼」方式，而不是加密。** 它使用 64 個可列印字元（A–Z, a–z, 0–9, `+`, `/`）以及一個用來補位的字元 `=`，將任意的資料包裝成文字格式，以利於在不支援二進位資料的通道（如電子郵件、HTTP 協定）中安全傳輸。 [[1](https://upliftorch.com/tools/base64-codec/blog/base64-explained.html), [2](https://ithelp.ithome.com.tw/m/articles/10382871)]

以下為您整理 Base64 的運作原理、常見應用及重要觀念：

⚙️ 轉換原理：3 位元組變 4 字元

Base64 的核心邏輯是將 **3 個 Byte（24 個 bits）的二進位資料，拆成 4 個 6 bits 的區塊**。每一個 6 bits 的二進位值（範圍為 0 到 63），再對照 Base64 索引表換成對應的文字。 [[1](https://zh.wikipedia.org/zh-tw/Base64), [2](https://logto.io/zh-TW/base64-decoder)]

- **資料量會變大：** 由於把 3 個位元組擴展成 4 個字元，編碼後的**檔案體積會增加大約 33%**。
- **末尾的 `=` 是什麼？** 當最後剩餘的資料不滿 3 個位元組時，程式會用 `0` 補足位數，並在編碼結尾加上 1 個或 2 個 `=` 符號作為填充（Padding），用來告訴解碼器如何還原。 [[1](https://rakko.tools/zh-tw/tools/base64-converter), [2](https://logto.io/zh-TW/base64-decoder)]

---

🛠️ 常見的應用場景

1. **網頁內嵌圖片 (Data URI)：**  
    網頁設計時，常會將小圖示（如網頁選單的 Icon）直接轉成 Base64 字串寫死在 HTML 或 CSS 中（例如 `data:image/png;base64,...`），這樣可以**減少瀏覽器向伺服器發送 HTTP 請求的次數**，加速網頁載入。 [[1](https://rakko.tools/zh-tw/tools/base64-converter), [2](https://www.strerr.com/tw/base64_encode_img.html)]
2. **電子郵件附件 (MIME)：**  
    早期的電子郵件系統（SMTP）只能處理純文字（ASCII）。如果要傳送圖片、PDF 或壓縮檔等二進位檔案，就必須先透過 Base64 編碼，收件方收到後再解碼還原。 [[1](https://traditional-chinese.opswat.com/blog/how-base64-encoding-opens-the-door-for-malware), [2](https://developer.mozilla.org/zh-TW/docs/Glossary/Base64)]
3. **API 認證 (Basic Authentication)：**  
    許多 Web API 在進行簡單的身份驗證時，會將「帳號:密碼」組合成字串，並轉成 Base64 格式放在 HTTP 標頭（Header）的 `Authorization` 中傳送。 [[1](https://rakko.tools/zh-tw/tools/base64-converter)]
4. **URL 安全變種 (Base64URL)：**  
    標準的 Base64 含有 `+` 和 `/`，在網址（URL）中傳遞時會引發解析錯誤。因此衍伸出 [Base64URL](https://docs.python.org/zh-tw/3/library/base64.html) 變種，將這兩個字元替換成 `-` 與 `_`，並移除末尾的 `=`。 [[1](https://docs.python.org/zh-tw/3/library/base64.html), [2](https://logto.io/zh-TW/base64-decoder)]

---

⚠️ 重要安全性觀念：Base64 絕對不是加密！

這是開發者最容易混淆的盲點。**Base64 的目的是「資料傳輸與相容」，沒有任何保密功能**。 [[1](https://www.authgear.com/zh-hant/post/base64-encode-decode-guide/), [2](https://logto.io/zh-TW/base64-decoder)]

- 任何拿到 Base64 字串的人，都可以在一秒內透過[線上解碼工具](https://rakko.tools/zh-tw/tools/base64-converter)將其還原出原始內容。
- **千萬不能**用 Base64 來處理使用者的密碼、身分證字號或任何敏感隱私資料。需要保密時，請務必使用 AES、RSA 等標準的加密演算法。 [[1](https://www.youtube.com/watch?v=PtxWGw4to7E), [2](https://rakko.tools/zh-tw/tools/base64-converter), [3](https://www.authgear.com/zh-hant/post/base64-encode-decode-guide/)]

請問您目前是想：

1. 將 **特定的文字或圖片** 轉換成 Base64 編碼？
2. 尋找 **特定程式語言（如 Python, JavaScript, Java）** 實作 Base64 的語法範例？
3. 將一段已知的 Base64 字串 **解碼還原**？

您可以告訴我您的具體需求，我來為您提供對應的工具或程式碼。