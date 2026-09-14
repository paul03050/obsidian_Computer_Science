
### 1. 建立第一個組件（Components）

組件是網頁的一個獨立區塊（Piece of the website）。在 React 中，組件是以 **JavaScript 函式（Function）** 的形式來建立，並且必須遵循以下規範：

- **命名規範**：組件名稱的首字母**必須大寫**（使用 Pascal Case），例如 `ChatInput`，以便與一般 HTML 標籤區隔 。
    
- **回傳值**：組件函式內使用 `return` 來回傳 JSX（即 HTML 結構）。
    
- **嚴格的閉合標籤**：在 JSX 中，所有元素都必須有關閉標籤 。如果是沒有子內容的標籤，可以使用自我閉合元素（Self-closing element）快捷寫法（例如 `<input />`）。
    

**核心程式碼範例（`ChatInput` 組件）：**

JavaScript

```ts
function ChatInput() {
  return (
    <>
      <input type="text" placeholder="Search" />
      <button>Send</button>
    </>
  );
}
```

(註：在 React 中也可以使用 `<></>` 空標籤，這被稱為 **Fragment（片段）**，它能將多個元素包裹起來回傳，但不會在最終的網頁上產生額外的無意義 `<div>` 標籤 。)

### 2. 使用組件語法（Component Syntax）

React 允許我們像使用自訂 HTML 標籤一樣來載入組件，這也是 React 的核心概念之一 ：

JavaScript

```ts
// 渲染自訂的組件（使用自我閉合寫法）
<ChatInput />
```

### 3. 使用 Props 傳遞數據

**Props**（屬性）能讓組件變得可以重複使用（Reusable）。

- 每個組件函式都可以接收一個名為 `props` 的參數，它是一個包含了所有從外部傳入屬性的 JavaScript 物件 。
    
- 可以透過解構賦值（Destructuring）或直接讀取 `props.propertyName` 來獲取數值 。
    

**核心程式碼範例（`ChatMessage` 組件）：**

JavaScript

```ts
function ChatMessage(props) {
  return (
    <div>
      <div>Sender: {props.sender}</div>
      <p>{props.message}</p>
    </div>
  );
}
```

在主要的渲染區域（如 `App` 組件中），我們就可以多次呼叫同一個組件，並傳入不同的屬性 ：

JavaScript

```ts
<ChatMessage sender="user" message="hello chatbot" />
<ChatMessage sender="robot" message="hello how can I help you" />
```

### 4. 建立 App 根組件與整合渲染（Start the Chatbot Project）

最佳實踐是建立一個名為 `App` 的根組件來代表整個應用程式，並在裡面組合其他的子組件 。最後利用 `createRoot` 與 `render` 將其渲染至網頁的容器（Container）中 。

**Chatbot 專案初始完整結構程式碼：**

JavaScript

```ts
// 1. 定義輸入框組件
function ChatInput() {
  return (
    <>
      <input type="text" size="30" />
      <button>Send</button>
    </>
  );
}

// 2. 定義聊天訊息組件（使用 Props）
function ChatMessage(props) {
  return (
    <div>
      <strong>{props.sender}: </strong>
      <span>{props.message}</span>
    </div>
  );
}

// 3. 定義代表整個應用的 App 組件
function App() {
  return (
    <div>
      <ChatInput />
      <ChatMessage sender="user" message="Can you get me today's date?" />
      <ChatMessage sender="robot" message="Today is..." />
    </div>
  );
}

// 4. 將 React 綁定到 HTML 的容器並渲染出來
const container = document.querySelector('.js-container');
const root = ReactDOM.createRoot(container);
root.render(<App />);
```