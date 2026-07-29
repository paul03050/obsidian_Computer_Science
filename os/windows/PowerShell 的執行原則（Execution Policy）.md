這個錯誤是因為 PowerShell 的執行原則（Execution Policy）預設禁止執行指令碼，以防止未簽署的指令碼帶來風險。

### 立即解決方法

請以**系統管理員身分**開啟 PowerShell，然後執行以下其中一個命令來暫時放行：

*   **方法一（建議，僅為目前使用者放行）**：
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
    ```

*   **方法二（臨時解決，僅影響目前視窗）**：
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process
    ```

完成後，再次執行啟動虛擬環境的命令：
```powershell
.venv\Scripts\activate
```
成功後，命令列前面會出現 `(.venv)` 的提示。

### 原理與其他方案

*   **為什麼會發生？** PowerShell 的 `Restricted` 預設原則阻止了所有 `.ps1` 指令碼執行，這是 Windows 的預設安全設定。
*   **永久修改（需審慎）**：若需永久修改，請以系統管理員身分執行 `Set-ExecutionPolicy RemoteSigned`，這允許執行本機指令碼和來自可信來源的遠端指令碼。
*   **替代啟動方式**：如果不便修改原則，也可以使用傳統的命令提示字元（cmd）來啟動：
    ```cmd
    .venv\Scripts\activate.bat
    ```

為確保系統安全，建議優先使用**方法一**。完成設定後，若仍有問題，歡迎隨時提出。