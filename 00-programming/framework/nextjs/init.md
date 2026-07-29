
npx create-next-app@13.4
Next.js 13 採用**基於檔案系統 (File-system based)** 的路由機制，著重於「約定優於配置」 。
```bash
npx create-next-app@13.4
```

```bash
npm run dev
```
 
不是原生的編譯指令
讀取package.jon的`scripts`
自動呼叫對應的工具鏈（例如 `tsc`、`vite`、`esbuild` 或 `tsx`)

nextjs是全端框架，當你啟動開發伺服器（`npm run dev`）時，它只會佔用一個 Port（預設是 `localhost:3000`）。

- 你的前端網頁在 `http://localhost:3000/`
- 你的後端 API（Route Handlers / API Routes）在 `http://localhost:3000/api/...`
因為 **Protocol、Domain、Port 完全一模一樣**，這在瀏覽器中叫做「同源（Same-Origin）」**。既然是同源請求，瀏覽器的安全機制就不會阻擋，自然**永遠不會觸發 CORS 錯誤**。
