IIS（Windows 網頁伺服器）之所以需要用轉檔程式，==最核心的原因在於 **Windows 系統與 Linux 系統對於憑證儲存的「設計邏輯」完全不同**==。 [[1](https://blogs.reliablepenguin.com/2025/12/06/converting-aws-acm-pem-certificates-to-pfx-for-windows-iis)]

當你在 Sectigo 等機構下載憑證時，預設通常是給 Linux（Nginx/Apache）用的 **PEM 格式**（純文字，拆成 `.crt`、`.key`、`bundle.txt` 多個檔案）。但 **IIS 唯一看得懂且只接受加密打包好的 PFX（PKCS#12）二進位格式**。 [[1](https://cheapsslsecurity.com/p/pfx-vs-pem-whats-the-difference/), [2](https://www.alibabacloud.com/help/tc/ssl-certificate/common-formats-of-ssl-certificates), [3](https://shop.trustico.com/blogs/stories/understanding-ssl-certificate-file-formats-and-extensions)]

這背後有三個非轉不可的關鍵原因：

1. 🔑 私鑰（Private Key）和憑證必須「綁定加密」

- **Linux 的做法：** 網域憑證、中繼憑證、私鑰，全部都是獨立的純文字檔（PEM）。Nginx 啟動時自己去不同路徑讀取並拼湊。
- **Windows/IIS 的做法：** 基於資安考量，Windows 認為把最珍貴的「私鑰（`.key`）」當成純文字放著太危險了。因此，**IIS 規定必須把「域名憑證 + 中繼憑證 + 私鑰」這三者，利用密碼打包加密成一個二進位的 `.pfx` 檔案**。沒有密碼，誰都別想解開看到私鑰。 [[1](https://learn.microsoft.com/en-us/answers/questions/738994/import-ssl-to-existing-windows-iis-server), [2](https://www.alibabacloud.com/help/tc/ssl-certificate/common-formats-of-ssl-certificates), [3](https://cheapsslsecurity.com/p/pfx-vs-pem-whats-the-difference/), [4](https://blogs.reliablepenguin.com/2025/12/06/converting-aws-acm-pem-certificates-to-pfx-for-windows-iis), [5](https://shop.trustico.com/blogs/stories/understanding-ssl-certificate-file-formats-and-extensions)]

2. 🏛️ Windows 有中央集權的「憑證儲存區」

- **Linux 的做法：** 設定檔直接寫死路徑（例如：`/etc/nginx/ssl/server.crt`），每個網站的檔案自己顧自己。
- **IIS 的做法：** IIS 內建沒有自己獨立的憑證管理夾。它是把憑證全部匯入到 Windows 作業系統的**「電腦憑證儲存區（Certificate Store）」**中。Windows 為了統一管理，規定要進這個儲存區的網站身分憑證，一律必須是 `.pfx` 包裝格式。匯入後，IIS 只是去向作業系統「借用」這個憑證來綁定網站。 [[1](https://www.reddit.com/r/sysadmin/comments/tk5v70/does_anyone_have_good_documentation_on_the/), [2](https://blogs.reliablepenguin.com/2025/12/06/converting-aws-acm-pem-certificates-to-pfx-for-windows-iis), [3](https://shop.trustico.com/blogs/stories/understanding-ssl-certificate-file-formats-and-extensions)]

3. 🖱️ 圖形化介面（GUI）的防呆設計

Windows 的設計初衷是讓管理員用滑鼠點一點就能完成部署。  
使用 `.pfx` 檔，你在 IIS 控制台點選「匯入」，輸入一組密碼，**Windows 就會自動把裡面的域名憑證、中繼憑證、私鑰各自拆開，精準放進作業系統對應的系統資料夾**（不需要像 Nginx 那樣手動複製貼上、擔心順序錯亂或換行黏在一起）。 [[1](https://blogs.reliablepenguin.com/2025/12/06/converting-aws-acm-pem-certificates-to-pfx-for-windows-iis)]

---

🛠️ 怎麼轉檔最快？

要把你手邊的 Sectigo 純文字檔案轉成 IIS 要的 `.pfx`，主要有兩種方式：

- **方法 A（推薦，最安全）：使用伺服器上的 OpenSSL 指令**  
    如果你有安裝 OpenSSL（或用 Git Bash），可以執行一行指令，把網域憑證 (`domain.crt`)、私鑰 (`domain.key`) 和中繼鏈 (`bundle.txt`) 打包：
    
    bash
    
    ```
    openssl pkcs12 -export -out server.pfx -inkey domain.key -in domain.crt -certfile bundle.txt
    ```
    
    請謹慎使用程式碼。
    
    _(執行後系統會要求你輸入一組密碼，這組密碼就是在 IIS 匯入時要輸入的防護密碼)_ [[1](https://serverfault.com/questions/244711/import-pem-into-iis-7), [2](https://blog.csdn.net/linuxghs/article/details/138647970)]
- **方法 B（最快速）：使用線上憑證轉換工具**  
    如果是公開不敏感的測試環境，可以直接使用像是 [SSL Shopper - SSL Converter](https://www.sslshopper.com/ssl-converter.html) 的線上免費工具，把檔案上傳並設定密碼，就能直接下載 `.pfx` 檔。

您目前有安裝 **OpenSSL 工具**嗎？還是您希望直接在 **Windows 內建的 IIS 管理員中，透過當初產生 CSR 的方式**來還原並完成安裝？如果是後者，其實有機會不需要轉檔程式喔！