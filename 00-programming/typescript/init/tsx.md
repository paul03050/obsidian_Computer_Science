當你使用 `tsx watch src/index.ts` 搭配 VS Code 的 Live Server 卻發現網頁沒有自動更新時，核心問題在於這兩個工具運作在完全不同的維度。

`tsx watch` 是一個後端（Node.js） 監聽工具，它只會在後端偵測 `src/index.ts` 的更動並重啟服務，但它絕對不會去通知前端瀏覽器或 Live Server 進行刷新。而 Live Server 只能監聽並刷新前端靜態檔案（如 `.html`、`.js`、`.css`）。 [1, 2, 3]

以下是兩種最常見的開發情境與對應的解決方案：

## 情境 A：你在開發「前端網頁」（如 React / 網頁應用）

如果你希望修改 `.ts` 或 `.tsx` 後瀏覽器自動刷新，你不應該使用 `tsx watch` 和 Live Server。 [4]

- 最推薦的解決方法：改用現代前端建構工具 [Vite](https://vite.dev/)。
    
    1. 在專案目錄執行：`npm create vite@latest .`
    2. 選擇你的前端框架與 TypeScript。
    3. 執行 `npm run dev`。
    4. 只要修改程式碼，網頁就會透過熱重載（HMR）秒速更新，不再需要 Live Server。 [2, 4, 5]
    

---

## 情境 B：你在開發「後端 API / Node.js 伺服器」

如果你是在寫後端（例如 Express API），並使用 Live Server 開啟一個 `index.html` 去透過 `fetch()` 呼叫後端 API，那麼網頁沒更新是因為 Live Server 不知道後端程式碼被修改了。 [2]

請依照以下 3 個步驟調整，即可讓前後端完美連動：

## 步驟 1：安裝 `concurrently` 同步執行工具

我們需要讓「後端重啟」與「前端刷新」同時發生。請先安裝整合工具：

```bash
npm install -save-dev concurrently
```

## 步驟 2：設定 `package.json`

在 `package.json` 的 `scripts` 欄位加入以下設定，讓 `tsx watch` 在重啟時，自動去觸發（觸碰）Live Server 正在監聽的 HTML 檔案：

```json
"scripts": {
  "dev:backend": "tsx watch src/index.ts",
  "dev:trigger-fe": "tsx watch --quiet src/index.ts --exec \"touch index.html || echo >> index.html\"",
  "start": "concurrently \"npm run dev:trigger-fe\" \"live-server\""
}
```

_(註：Windows 系統請將 `--exec` 後面改為 `"echo. >> index.html"`)_

## 步驟 3：啟動專案

之後在終端機統一輸入以下指令啟動，當你修改 `src/index.ts` 時，後端會重啟，同時也會觸發 Live Server 讓網頁自動刷新： [6, 7]

```bash
npm start
```

---

為了幫你精準定位問題，可以告訴我：

- 你目前正在開發的是前端網頁畫面，還是後端 API 伺服器？
- 你的 `index.html` 裡面是用 `<script>` 標籤去引入 `src/index.ts` 嗎？ [5, 8]

  

[1] [https://dev.to](https://dev.to/mangadev/set-up-hot-reload-for-typescript-esm-projects-42en)

[2] [https://blog.csdn.net](https://blog.csdn.net/2511_93835513/article/details/157552188)

[3] [https://blog.csdn.net](https://blog.csdn.net/m0_59951054/article/details/161643003)

[4] [https://stackoverflow.max-everyday.com](https://stackoverflow.max-everyday.com/2026/01/preview-tsx/)

[5] [https://www.reddit.com](https://www.reddit.com/r/vscode/comments/ok1l7n/error_executing_typescript_in_the_browser_using/)

[6] [https://stackoverflow.com](https://stackoverflow.com/questions/45622125/how-can-i-add-live-reload-to-my-nodejs-server)

[7] [https://medium.com](https://medium.com/@ajeeshRS/setting-up-a-node-js-typescript-server-with-automatic-restart-dd9212360145)

[8] [https://stackoverflow.com](https://stackoverflow.com/questions/76285899/how-to-run-tsx-files-on-a-browser-directly-without-compiling)