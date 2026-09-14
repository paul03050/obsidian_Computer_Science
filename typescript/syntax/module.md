CommonJS 平滑遷移至現代的 ES Modules (ESM) 。

## 一、 核心觀念：腳本 (Script) vs. 模組 (Module)

在 TypeScript/JavaScript 的世界中，區分「腳本」與「模組」是理解程式碼範疇（Scope）的第一步：

- **腳本 (Script)**：若檔案內==沒有==任何 `import` 或 `export` 關鍵字，它會被視為全域腳本。檔案中定義的變數與函式會直接污染==全域範疇（Global Scope）== 。
    
- **模組 (Module)**：**只要檔案中包含了任一 `import` 或 `export` 關鍵字，該檔案就會升格為「模組」** 。模組擁有獨立的模組範疇（Module Scope），內部程式碼對外不公開，除非明確被導出 。
    

## 二、 導出與導入（Export & Import）全指南

TypeScript 提供了多種靈活的導出與導入語法，主要分為**具名**與**預設**兩大體系。

### 1. 具名導出與導入 (Named Export / Import)

要求導入時的名稱必須與導出時完全一致 。

- **行內導出**：直接在宣告前加上 `export` 。
    
    ```ts
    export const add = (a: number, b: number) => a + b;
    export const subtract = (a: number, b: number) => a - b;
    ```
    

````ts
* **導出陳述式 (Export Statements)**：將需要導出的東西打包成一個類似物件的形狀，便於集中管理，並可在此處移除函式前方的 `export` 關鍵字 。
  
    // math.ts
    const add = (a: number, b: number) => a + b;
    const subtract = (a: number, b: number) => a - b;
    
    export { add, subtract }; // 導出陳述式
    
* **具名導入**：必須使用花括號 `{}` 結構 。

    import { add, subtract } from "./math"; // 相對路徑導入
    

### 2. 預設導出與導入 (Default Export / Import)
* **每個模組只能有一個預設導出** [cite: 9]。如果模組需要導出多個函式或常數，建議使用具名導出 [cite: 9]。
* **優點**：消費者（Consumer）在導入時不需要使用花括號 `{}`，且可以**自由命名**（例如將 `add` 命名為 `adding`） [cite: 8]。

// math.ts
export default function add(a: number, b: number) { return a + b; }

// main.ts
import adding from "./math"; // 不需要花括號，名稱可自訂
````

### 3. 重命名 (Renaming with `as`)

不論是在「導出端」還是「導入端」，你都可以使用 `as` 關鍵字來進行重命名 。這在維護 API 相容性（如升級新舊函式但不想破壞既有客戶端程式碼）時非常實用 。

- **導出時重命名**：
    ```ts
    // 內部使用新版 subtractV2，但對外暴露仍叫 subtract，確保向後相容
    export { add as adding, subtractV2 as subtract };
    ```
- **導入時重命名**
    ```ts    
    import { add as adding } from "./math";
    ```
### 4. 命名空間導入 (Namespace Import)

當一個模組導出了非常多東西，或者你想避免**命名衝突**（例如兩個不同的模組都導出了同名的 `add` 函式）時，可以使用萬用字元 `*` 將所有內容打包到同一個傘下 。

TypeScript

```ts
import * as ourMath from "./ourMath";
import * as theirMath from "./theirMath";

console.log(ourMath.add(1, 2));
console.log(theirMath.add(3, 4));
```

## 三、 進階：副作用導入與全域擴充

### 1. 副作用導入 (Side-Effect Import)

當你引入一個傳統的 JavaScript 舊套件（Legacy Library），它既沒有 `import` 也沒有 `export`，只是單純往全域物件寫入資料或執行 `console.log` 時，這就被稱為「副作用」 。

- **關鍵地雷**：如果誤用命名空間導入（如 `import * as Legacy from "./legacy"`）卻在程式碼中沒有實際調用 `Legacy` 變數，**TypeScript 編譯器會自動把該行程式碼優化並移除（Tree-shaking）**，導致副作用完全沒被執行 。
    
- **正確做法**：直接使用 `import` 關鍵字後加上路徑，不加任何導入變數名稱 。
    ```ts
    import "./legacy.js"; // 正確：會完整載入並執行該檔案的副作用
    ```
    
- 註：若要在 TypeScript 中無痛使用 `.js` 舊檔案，記得在 `tsconfig.json` 將 `"allowJs": true` 開啟 。
    

### 2. 擴充全域物件型別 (Declaring Global Environment)

當舊腳本在全域物件（如 Node.js 的 `global` 或瀏覽器的 `window`）掛載了自訂變數（例如 `myFrameworkNumber`），TypeScript 會因為找不到型別定義而報錯 。我們必須透過 `declare global` 擴充宣告 ：

TypeScript

```ts
// 擴充現有的全域宣告
declare global {
  var myFrameworkNumber: number; // 使用 var 才能讓該變數具備可寫性（Writable）
}

// 這樣你就能安全地在 TypeScript 中存取它了
console.log(globalThis.myFrameworkNumber);
```

(提示：使用 `var` 宣告可以讓變數可讀寫；若使用 `const` 則是唯讀 )

## 四、 關鍵轉型：從 CommonJS 遷移至 ES Modules (ESM)

隨著標準演進，前端與後端逐漸從舊時代的 CommonJS (CJS) 邁向 EcmaScript Modules (ESM) 全球標準 。以下是兩者的關鍵差異與轉型步驟：

### 1. 架構對比

|**特性**|**CommonJS (CJS)**|**ES Modules (ESM)**|
|---|---|---|
|**標準制定**|Node.js 早期事實標準|EcmaScript 國際標準 (跨瀏覽器、Node, Deno)|
|**載入關鍵字**|`require()` / `module.exports`|`import` / `export`|
|**編譯輸出**|程式碼會被轉譯為含有 `require` 語法|程式碼幾乎保留現代 `import/export` 原貌|

### 2. 轉型三步驟（以 Node.js 18+ / TS 4.9+ 為例）

如果你想將既有的 CommonJS 專案升級為 100% ESM 相容，請執行以下調整 ：

- **步驟一：修改 `tsconfig.json`** 將模組系統從 `commonjs` 改為現代的 `esnext` 。
    
    JSON
    
    ```ts
    {
      "compilerOptions": {
        "module": "ESNext",
        "target": "ES2016",
        "outDir": "./dist",
        "strict": true
      }
    }
    ```
    
- **步驟二：修改 `package.json`** Node.js 預設將檔案視為 CommonJS 。必須明確加入 `"type": "module"`，告訴 Node.js 這是一個 ESM 專案，否則在執行時會噴出 `export token is unknown` 的語法錯誤 。
    
    JSON
    
    ```ts
    {
      "name": "my-typescript-project",
      "type": "module",
      "dependencies": {}
    }
    ```
    
- **步驟三：更新導入語法** 傳統 Legacy JavaScript 呼叫編譯後的檔案時若仍使用 `const math = require("./math.js")` 會壞掉（會噴出 `require is not defined` 的錯誤） 。必須全面翻新為現代的 `import` 語法 ：
    ```ts
    // 舊：const { add } = require("./dist/math.js");
    // 新：
    import { add } from "./dist/math.js";
    ```
    