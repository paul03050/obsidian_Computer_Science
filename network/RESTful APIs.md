RESTful API 是一種基於 REST（具象狀態傳輸）架構風格所設計的應用程式介面 (API)，旨在讓兩個不同的電腦系統透過網路進行安全且高效的資訊交換。它將網路上的所有內容（如使用者資料、圖片、訂單等）都視為「資源」，並統一使用 HTTP 協議標準的方法來對這些資源進行操作。 

## 🛠️ RESTful API 的四大核心操作

在 RESTful API 中，網址（URL）只代表「資源的位置」，而具體要做什麼操作，則是由 HTTP 方法（Method）來決定： 

- GET：用來「取得」資源。
    
    - 例如：`GET /users`（取得所有使用者清單）。 [4]
    
- POST：用來「新增」資源。
    
    - 例如：`POST /users`（建立一個新使用者）。 [4]
    
- PUT / PATCH：用來「更新」資源。
    
    - 例如：`PUT /users/1`（修改 ID 為 1 的使用者資料）。 [4]
    
- DELETE：用來「刪除」資源。
    
    - 例如：`DELETE /users/1`（刪除 ID 為 1 的使用者）。 [4]
    

---

## ⚖️ 正確與錯誤的設計對比

使用 RESTful 風格最大的好處是簡化溝通成本。透過網址和 HTTP 方法的搭配，工程師不需要看文件就能猜出 API 的用途。 [5]

|操作類型|❌ 傳統非 RESTful 設計（動詞放網址）|RESTful 設計（名詞+HTTP方法）|
|---|---|---|
|取得商品|`GET /get_all_products`|`GET /products`|
|新增商品|`POST /create_product`|`POST /products`|
|刪除商品|`GET /delete_product?id=5`|`DELETE /products/5`|

---

## 🔑 RESTful 的關鍵特性

1. 無狀態 (Stateless)：伺服器不會保存任何客戶端的狀態。每一次的請求（Request）都必須包含完成該操作所需的全部資訊，這讓系統更容易擴展與維護。
2. 資源導向：每一個資源在網路上都有一個唯一的識別碼（通常就是 URL）。
3. 常規資料格式：通常使用 [JSON (JavaScript Object Notation)](https://ithelp.ithome.com.tw/articles/10330702) 作為資料交換的格式，因為它輕量且易於閱讀。 [4, 6]
