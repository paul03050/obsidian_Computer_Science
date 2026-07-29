您可以在 Windows 的命令提示字元 (CMD) 或 PowerShell 中，使用 `wsl` 相關命令來管理與執行適用於 Linux 的 Windows 子系統 (WSL)。以下為最常用的 WSL 指令集指南： [1]

## 🛠️ 安裝與初始化

- `wsl --install`：安裝 WSL 以及預設的 Linux 發行版 (Ubuntu)。
- `wsl --list --online`：列出所有可供下載的 Linux 發行版。
- `wsl --install -d <發行版名稱>`：下載並安裝指定的 Linux 發行版 (例如 `wsl --install -d Debian`)。 [2, 3]

## 📊 狀態查看

- `wsl -l -v`：查看已安裝發行版的清單、運行狀態與使用的 WSL 版本 (1 或 2)。
- `wsl --status`：檢查 WSL 的總體配置資訊。 [4]

## 🚀 啟動與關閉

- `wsl`：直接進入預設的 Linux 發行版。
- `wsl -d <發行版名稱>`：指定並開啟某個特定的 Linux 發行版。
- `wsl -u root`：以 Root 最高權限管理員身份登入預設發行版。
- `wsl --terminate <發行版名稱>`：立即強制停止指定的 Linux 發行版。
- `wsl --shutdown`：立刻關閉所有正在運行的 WSL 虛擬機器與發行版。 [4, 5, 6]

## 💾 備份與還原 (轉移硬碟空間)

- `wsl --export <發行版名稱> <路徑\檔名.tar>`：將 Linux 系統匯出備份成一個壓縮檔。
- `wsl --import <新發行版名稱> <安裝路徑> <備份檔路徑.tar>`：將備份檔匯入到指定路徑還原。
- `wsl --unregister <發行版名稱>`：登出並徹底刪除該發行版 (注意：此操作會清除該發行版內所有檔案)。

## 🔄 版本轉換

- `wsl --set-default-version 2`：設定未來安裝發行版時，預設使用 WSL 2。
- `wsl --set-version <發行版名稱> 2`：將現有的特定發行版轉換為 WSL 2 版本。 [7]

---

