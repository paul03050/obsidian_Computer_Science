根據您提供的筆記內容，DNS CNAME 記錄（Canonical Name，規範名稱記錄）的主要重點與相關限制如下：

  

### 1. 什麼是 DNS CNAME 記錄？

- **別名指向**：CNAME 記錄用作網域的別名，將一個別名網域或子網域指向另一個「規範」網域（真實名稱）。
    
      
    
- **永不指向 IP**：所有 CNAME 記錄都必須指向另一個網域，**絕不能直接指向 IP 位址**。
    
      
    
- **運作原理**：例如將 `blog.example.com` 的 CNAME 指向 `example.com`，當 DNS 伺服器查詢時，會觸發對 `example.com` 的另一次 DNS 查詢，並透過其 A 記錄取得對應的 IP 位址。
    
      
    

### 2. CNAME 記錄是否可以指向另一個 CNAME？

- 雖然技術上可行，但將一個 CNAME 記錄指向另一個 CNAME 記錄會導致多次 DNS 查詢，進而拖慢使用者體驗，因此**應盡量避免**。
    
      
    
- 最好的做法是讓所有的子網域 CNAME 記錄直接指向根網域（Root Domain）。
    
      
    

### 3. 使用 CNAME 記錄的限制

- **無重複名稱（No duplicate names）**：任何擁有 CNAME 記錄的名稱，都不能再有其他類型的 DNS 記錄（如 MX、TXT、A 或 SOA 等），也就是說其他記錄必須放在真實的網域底下（除非使用 CNAME 展平 / CNAME flattening）。
    
      
    
- **MX 與 NS 記錄限制**：MX 記錄（郵件交換記錄）和 NS 記錄（名稱伺服器記錄）**絕對不能**指向 CNAME 記錄，它們必須直接指向 A 記錄（針對 IPv4）或 AAAA 記錄（針對 IPv6）。