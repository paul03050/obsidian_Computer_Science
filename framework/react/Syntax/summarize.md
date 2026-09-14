
### 1. JSX (JavaScript XML)

- **概念**：允許開發者在 JavaScript 之中直接編寫 HTML 結構 。它本質上是 JavaScript 物件，會透過建構工具（如 Vite）編譯成瀏覽器看得懂的 JavaScript 。
**使用 JavaScript**：在 JSX 的 HTML 標籤內容或屬性中，可以使用**大括號 `{}`** 來嵌入 JavaScript 變數、表達式或呼叫函數 。

**語法限制**：

- 每個組件**只能回傳一個根元素**（可用空標籤 `< Fragment >` 將多個元素包起來，避免產生多餘的 HTML 標籤） 。
    
- 空標籤或單一標籤的結尾**必須加上正斜線 `/`**（例如：`<input />`） 。
    
- 屬性名稱使用**駝峰式命名法**（如 `className` 代替 `class`、`htmlFor` 代替 `for`） 。
    
- `return` 關鍵字旁如果換行，必須使用圓括號 `()` 將 JSX 包裹起來 。


### 2. Component (組件)

- **定義**：將網頁拆分成不同部分的獨立容器 。在現代 React 中，主要使用 **Function Component（函式組件）** 。只要一個 JavaScript 函式回傳了 JSX，它就是一個組件 。
    
- **命名規則**：組件名稱的開頭**必須大寫**（使用 Pascal Case，如 `MyComponent`），這樣 React 才能區分它與一般 HTML 標籤 。
    
- **重複與巢狀使用**：組件可以像 HTML 標籤一樣被重複呼叫，也可以互相嵌套形成父子組件（Parent/Child Component）的樹狀結構 。不應將一個組件的定義寫在另一個組件的內部 。
### 3. Style (樣式處理)

- **Inline CSS（行內樣式）**：在標籤的 `style` 屬性中需要使用雙大括號 `style={{ backgroundColor: 'red' }}`，外層代表 JSX 語法，內層代表 JavaScript 物件，且樣式名稱須改用駝峰式命名（如 `backgroundColor`） 。
    
- **外部 CSS 檔案**：在組件檔案頂端使用 `import './App.css'` 直接導入，Vite 會自動將樣式插入網頁中 。

### 4. 事件處理 (Event Handling)

- **語法**：在 HTML 開始標籤中綁定事件（如 `onClick`），並利用大括號傳入回呼函式（Callback Function） 。
    
- **注意事項**：綁定函式時**不要加上圓括號**（應寫 `onClick={handleClick}`，而非 `onClick={handleClick()}`），否則畫面渲染時函式會被立即執行，而不是等到點擊時才執行 。

### 5. 陣列與迴圈（陣列方法）

- **渲染列表**：React 允許在 JSX 中直接放入陣列 。通常會使用 **`map()`** 方法將原始資料陣列轉換成組件或 HTML 元素的陣列 。
    
- **`key` 屬性**：在陣列渲染多個元素時，每個根元素都**必須加上唯一且固定的 `key` 屬性**（例如：`key={item.id}`），這能幫助 React 精準定位元素以提升渲染效率 。
    
- **過濾列表**：常搭配 **`filter()`** 方法，根據條件保留（回傳 `true`）或剔除資料 。
    

### 6. 條件判斷 (Conditional Rendering)

- **三元運算子 (`? :`)**：最常用於 JSX 內部，根據條件展示不同的標籤或切換不同的 `className` 。
    
- **邏輯運算子 AND (`&&`)**：當左邊的值為真值（`true`）時，才會渲染右邊的元素；若左邊為 `false`，React 則會自動忽略不顯示任何東西 。
    

### 7. Props (屬性傳遞)

- **功能**：用於**將資料從父組件傳遞給子組件** 。React 的資料流是單向的，Props **只能由上往下傳遞** 。
    
- **接收方式**：子組件的函式參數會接收一個名為 `props` 的物件 ，在實務上常用 **解構賦值（Destructuring Assignment）** 的語法直接取出對應名稱的屬性（例如：`function MyComponent({ a, b })`） 。

### 8. State (狀態管理)

- **功能**：在 React 中，一般變數即使改變了，網頁畫面也不會重新渲染 。若要讓畫面隨著資料動態更新，就必須將資料儲存在 **State**（狀態）中 （主要透過 `useState` 鉤子來建立與修改，促使組件重新渲染並更新 UI） 。



`props` 的本質就是**組件的參數**。React 遵循**單向數據流**（Unidirectional Data Flow），數據總是從父組件流向子組件。
- 📥 **傳遞數據**：父組件可以像 HTML 標籤設定屬性一樣，將字串、數字、物件、陣列甚至函數傳遞給子組件。
    
- 🔒 **唯讀特性**：`props` 是**不可變的（Immutable）**。子組件只能讀取傳進來的 `props`，絕對不能直接修改它。
```ts

// 父組件 Parent
function App() {
  return <WelcomeMessage name="Alex" />;
}
```
```ts
// 子組件 Child（傳統寫法）
function WelcomeMessage(props) {
  return <h1>你好，{props.name}！</h1>;
}

```
我們就來深入探討 **單向數據流與改動數據** 🔄。

在 React 中，`props` 是唯讀（Immutable）的。這意味著子組件絕對不能直接修改傳進來的 `props`。

### 為什麼不能直接修改 props？ 🤔

想像一下，如果多個子組件都共享同一個來自父組件的資料，而其中一個子組件擅自修改了它，這會導致整個應用的資料狀態變得混亂、難以追蹤。

### 那子組件要怎麼改變資料？ 📥

如果子組件需要改變資料，它必須**通知父組件**，由父組件來修改狀態（State），再透過 `props` 把新資料傳下來。

這個「通知」的過程，是透過父組件傳遞一個函數（Function）給子組件來實現的。

```ts
function CounterApp() {
  const [count, setCount] = React.useState(0);

  // 改變狀態的函數
  const handleIncrement = () => {
    setCount(count + 1);
  };

  return (
    <div>
      {/* 把函數當作 props 傳給子組件 */}
      <Button triggerClick={handleIncrement} />
      <p>目前計數：{count}</p>
    </div>
  );
}





function Button({ triggerClick }) {
  // 當按鈕被點擊時，呼叫從父組件傳下來的函數
  return <button onClick={triggerClick}>點我加 1</button>;
}


```
**預設值與型態防錯** 🛡️。

在實際開發中，如果父組件忘記傳入某個 `props`，或者傳入的資料型態錯誤（例如：原本需要數字，卻傳了字串），組件很容易會出現錯誤甚至導致畫面崩潰 💥。

```ts
// 如果父組件沒傳入 name，則預設顯示「神秘嘉賓」
function WelcomeBanner({ name = "神秘嘉賓" }) {
  return <h1>歡迎，{name}！</h1>;
}

### 2. 型態檢查 (Type Checking) 🔍

在現代 React 開發中，大家普遍會使用 **TypeScript** 或者是內建的 `propTypes` 來限制傳入資料的型態：


// 使用 TypeScript 定義資料型態
interface UserCardProps {
  username: string; // 必須是字串
  age: number;      // 必須是數字
}

function UserCard({ username, age }: UserCardProps) {
  return <div>{username} ({age}歲)</div>;
}



function ProductItem({ title, price = 0 }) {
  return (
    <div>
      <h3>商品：{title}</h3>
      <p>價格：{price} 元</p>
    </div>
  );
}
```


