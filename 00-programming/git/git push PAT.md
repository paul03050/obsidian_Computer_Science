在 WSL2 中使用 GitHub 個人存取權杖 (PAT, Personal Access Token) 進行 `git push`，最有效且一勞永逸的方法是將 WSL2 的 Git 認證直接關聯至 Windows 憑據管理器 (Git Credential Manager)。這樣您只需在 Windows 登入過一次，WSL2 就會自動共享認證，再也不用每次手動輸入 PAT。 [1, 2]

## 🛠️ 步驟一：在 WSL2 中設定認證管理器

請在 WSL2 終端機中執行下列指令，將憑據管理器指向 Windows 主機的 `git-credential-manager.exe`： [2]

```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"
```

_(注意：請確保您的 Windows 已經安裝了官方的 [Git for Windows](https://git-scm.com/download/win)。)_

如果上述路徑因 Git 版本不同而找不到，您也可以嘗試設定為新版的通用名稱：

```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager-core.exe"
```

## 🔑 步驟二：取得您的 GitHub PAT

如果您還沒有 PAT，請至 GitHub 網站產生：

1. 登入 GitHub，進入 `Settings` -> `Developer settings` -> `Personal access tokens` -> `Tokens (classic)`。
2. 點擊 `Generate new token`。
3. 勾選 `repo` 權限（這是推送代碼的基本權限）。
4. 產生後立即複製並儲存 Token（關閉網頁後將無法再次觀看）。 [3]

## 🚀 步驟三：執行 Push 進行驗證

當您在 WSL2 專案目錄下第一次執行推送時：

```bash
git push origin main
```

1. 畫面會跳出 Windows 的 GitHub 登入視窗。
2. 您可以直接點擊 "Sign in with your browser" 用瀏覽器完成驗證。
3. 或者點擊 "Token"，並將剛剛複製的 PAT 貼入。
4. 驗證成功後，Token 會自動儲存在 Windows 中，未來在 WSL2 內進行 `git push` 將完全免輸入密碼。 [2]

---

## 💡 替代方案：直接將 PAT 嵌入遠端網址 (不推薦長期使用)

如果您不想設定憑據管理器，只想臨時快速推送，可以直接修改 Remote URL，把 PAT 寫入網址中：

```bash
git remote set-url origin https://<您的GitHub帳號>:<您的PAT斷詞>@github.com/<帳號>/<專案名>.git
```

_⚠️ 缺點：此方法會將您的 PAT 以明文形式紀錄在本地 `.git/config` 檔案中，有安全風險。_

---