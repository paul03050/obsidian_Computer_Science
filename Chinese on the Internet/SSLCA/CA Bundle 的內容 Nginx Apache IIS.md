這份筆記的主要重點摘要如下：

  

### 一、 筆記中需修正的 4 個重要技術細節

1. **CA Bundle 的內容**：通常主要包含一張或多張**中繼憑證（Intermediate Certificates）**，不一定包含根憑證（Root Certificate），因為根憑證多已存在於作業系統或瀏覽器的信任庫中。
    
      
    
2. **不同伺服器的設定差異**：
    
      
    - **Nginx**：`ssl_certificate` 通常需指定包含網站憑證與中繼憑證的完整鏈檔案（Full Chain）。
        
          
        
    - **Apache**：可透過 `SSLCertificateFile` 指定憑證檔案。
        
          
        
    - **IIS**：通常匯入 PFX 格式，並建立網站 HTTPS Binding。
        
          
        
3. **PFX 格式的定義**：PFX（PKCS#12）是一種**封裝格式**，通常受密碼保護，能將網站憑證、私密金鑰（Private Key）以及中繼憑證打包在同一個檔案中，而不僅僅是單純把 CRT 和 KEY 放在一起。
    
      
    
4. **憑證合併順序**：Sectigo 等憑證的中繼憑證順序與檔案不能固定套用，應依實際簽發的鏈結構確認。正確的排列方向通常為：**網站憑證 $\rightarrow$ 簽發它的中繼憑證 $\rightarrow$ 更上層的中繼憑證**。
    
      
    

### 二、 SSL 核心檔案的三種觀念

- **CRT / Certificate（公開憑證）**：用以證明網站身分，包含網域、有效期限與公開金鑰等資訊。
    
      
    
- **KEY / Private Key（私密金鑰）**：機密資料，與 CSR 配對，**必須嚴格保密**，絕不能交給不可信任的第三方。
    
      
    
- **CA Bundle（憑證鏈）**：提供必要的中繼憑證，供用戶端驗證憑證簽發鏈。
    
      
    

### 三、 客服人員處理 SSL 問題的關鍵三問

當客戶表示不知如何安裝憑證時，應優先確認：

  

1. **使用什麼伺服器？**（如 Apache、Nginx、IIS 或雲端平台）
    
      
    
2. **目前手邊有哪些檔案？**（如 CRT、KEY、CA Bundle 或 PFX）
    
      
    
3. **CSR 是在哪裡產生的？**（若 CSR 在客戶自身伺服器產生，Private Key 通常留在原伺服器，下載憑證時不會自動取得）
    
      
    

- **資安提醒**：客服絕不能要求客戶透過一般電子郵件傳送 Private Key 或 PFX 密碼。
    
      
    

### 四、 SSL 申請與安裝的知識架構流程

Plaintext

```
SSL 申請 ➔ 產生 Private Key ➔ 產生 CSR ➔ CA 驗證網域控制權（DCV） ➔ CA 簽發 SSL Certificate ➔ 下載 CRT + CA Bundle ➔ 搭配原本的 Private Key ➔ 依伺服器類型安裝 ➔ 檢查 HTTPS 與憑證鏈
```

(核心觀念：CA 負責簽發、伺服器負責安裝、CA Bundle 則用來提供完整憑證鏈)