- 初始專案
```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
npm run dev
```


- 先使用 TypeScript 編譯器進行專案檢查與增量編譯，成功後再執行 Vite 進行生產環境的代碼打包」
- `package.json` 的 `scripts` 中加上 `tsc -b && vite build`，確保**「代碼完全沒有型別錯誤」**才進行最後的產品打包。
```bash
tsc -b && vite build
```

