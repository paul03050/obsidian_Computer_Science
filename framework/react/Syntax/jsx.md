
### 1. 什麼是 JSX？

- **定義**：JSX 代表==**JavaScript XML**== 。它是一種「增強版」的 JavaScript，允許開發者直接在 JavaScript 程式碼中撰寫 HTML 結構 。
    
- **本質**：JSX 可以被視為 ==JavaScript DOM== 操作的一種快捷方式（Shortcut） 。在 React 中，我們通常使用 JSX 代替傳統的 JavaScript DOM 操作，因為它更簡潔且直觀 。
    

### 2. 為什麼需要編譯工具（Babel）？

- **瀏覽器限制**：網頁瀏覽器本身是看不懂 JSX 的，它只看得懂標準的 JavaScript 。
    
- **Babel 的角色**：Babel 是一個 ==**JavaScript 編譯器（Compiler）**==，它的職責是將 JSX 程式碼翻譯（編譯）成瀏覽器能理解的普通 JavaScript 。
    
- **設定方式**：在 HTML 檔案中，除了載入 Babel 外部函式庫之外，必須在 `<script>` 標籤加上 `type="text/babel"` 屬性，藉此通知 Babel 去翻譯內部的 JSX 程式碼 。
    

### 3. JSX 的三大核心優勢

1. **更符合直覺（Natural）**：我們可以直接建立想要的 HTML 元素，然後交給 React 的 `render` 函式處理，它就會自動幫我們顯示在網頁的指定容器（Container）中 。
    
2. **更容易除錯（Find Errors Easier）**：因為 JSX 裡寫的是真實的元素節點而非單純的字串，若標籤拼錯（例如把 `button` 拼錯），編譯時就能馬上指出錯誤，讓除錯變得更簡單 。
    
3. **可直接嵌入 JavaScript 數值**：這種類似字串插值（String Interpolation）的功能，能將 JavaScript 與 HTML 更緊密地結合 。
    

### 4. JSX 核心語法與撰寫規範

- **直接撰寫 HTML**：可以直接將 HTML 元素賦值給變數，例如：
    
    ```ts
    const button = <button>hello</button>;
    ```
    
    這行程式碼會在 JavaScript 中建立一個按鈕元素並存入變數中 。
    
- **大括號 `{}` 嵌入運算式**：在 JSX 的 HTML 標籤內，只要使用單個大括號 `{}`，內部就可以放入任何有回傳值的 JavaScript 程式碼 。例如，在段落中輸入 `{2 + 2}`，網頁畫面上就會自動計算並渲染出 `4` 。
    
- **多行 HTML 格式化規範**：當編寫多行 HTML 時，最佳實踐（Best Practice）是用**小括號 `()`** 將 HTML 結構包起來，並讓標籤各佔一行配合縮排，以提升代碼的可讀性 。
    
- **必須回傳單一根元素（Single Element）**：JavaScript 的函式或變數一次只能回傳一個值（一個元素） 。
    
    - 如果需要同時渲染多個元素（例如一個按鈕加一個段落），必須將它們包裹在一個共同的父容器內，像是 `<div>` 標籤 。
        
    - 為了避免在網頁上產生過多無意義的 `<div>` 標籤，React 提供了 **Fragment（片段）** 語法（即空標籤 `<> ... </>`），它能達到群組元素的效果，但不會在最終的 HTML 網頁結構中安插額外的節點 。
        
- **嚴格的閉合標籤（Self-Closing Tags）**：JSX 的語法規範比傳統 HTML 更嚴格 。所有 HTML 元素在 JSX 中都必須有對應的關閉標籤 。對於像 `<input>` 這類本身沒有包夾文字的元素，必須使用「自我閉合（Self-closing）」的縮寫快捷語法，在標籤尾端加上斜線，例如 `<input type="text" />` 。


```html
<html> 
<head> 
<title>React Basics</title>
 </head> 
 <body> <div class="js-container"></div> 
 <script src="https://unpkg.com/react@19/umd/react.development.js"></script> <script src="https://unpkg.com/react-dom@19/umd/react-dom.development.js"></script> <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script> <script type="text/babel"> // 取得 HTML 中的容器節點 const container = document.querySelector('.js-container'); // 以該容器建立 React 的根節點（Root） 
 const root = ReactDOM.createRoot(container); // 使用 JSX 語法建立包含多個元素的變數（最佳實踐：使用小括號包覆並縮排） 
 const mainElement = ( <div> <button>hello</button> <p>paragraph of text {2 + 2}</p> </div> ); // 將 JSX 元素渲染（顯示）到網頁容器中 root.render(mainElement); </script> </body> </html>
```
