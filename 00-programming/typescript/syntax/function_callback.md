

文件物件模型（DOM）api是==瀏覽器提供的一組程式介面，允許開發者透過 JavaScript 等程式語言存取、創建、修改或刪除網頁中的節點==。它將 HTML 或 XML 文件以邏輯樹狀圖表示，是動態控制網頁結構、樣式與事件的核心工具。 


`const` ==用於宣告**無法重新賦值**的常數，但其內部屬性仍可修改==

```ts
const API_URL = "https://example.com";
// API_URL = "new_url"; // 編譯錯誤：無法重新指派 const 變數

const config = {
  host: "localhost",
  port: 8080
} as const;

// config.host = "127.0.0.1"; // 編譯錯誤：屬性為唯讀

const enum Direction {
  Up,
  Down
}
let move = Direction.Up; // 編譯後會直接變成 let move = 0;

```

---

We use TypeScript enums ==to **define a structured set of named constants**, which makes your codebase more readable, maintainable, and type-safe==.
```ts
// 1. String Enum (Recommended for clear debugging)
enum OrderStatus {
  Pending = "PENDING",
  Shipped = "SHIPPED",
  Delivered = "DELIVERED"
}

// 2. Numeric Enum (Automatically auto-increments from 0, 1, 2...)
enum UserRole {
  Admin,  // 0
  Editor, // 1
  Guest   // 2
}

// How to use it in a function
function processOrder(status: OrderStatus) {
  if (status === OrderStatus.Pending) {
    console.log("The order is waiting to be processed.");
  }
}

// Safe execution
processOrder(OrderStatus.Pending); 

// Compiler error: Argument of type '"PENDING"' is not assignable to parameter of type 'OrderStatus'
processOrder("PENDING"); 

```

>在 JavaScript 中，**函式是一等公民（First-Class Citizens）**。這意味著函式就像普通變>一樣，可以被==賦值給變數、當作參數傳遞給另一個函式，或者作為函式的回傳值。==
---
## ⚖️ 對比：[[Callback]] 模式 vs 物件導向設計（OOP）

這兩者在設計哲學與記憶體結構上有著本質的差異。

| **維度**    | **Callback 模式（函數式傾向）**             | **物件導向設計（OOP）**                                            |
| --------- | ---------------------------------- | ---------------------------------------------------------- |
| **核心概念**  | 關注**行為（Behavior）**的傳遞。             | 關注**狀態（State）與行為**的封裝。                                     |
| **記憶體結構** | 傳遞的是獨立函式的**[[記憶體指標]]**。            | 建立包含屬性與方法的**實例（Instance）**物件，通常透過原型鏈（Prototype Chain）共享方法。 |
| **耦合度**   | 鬆散耦合，呼叫者不需要知道接收者的內部結構，只需符合定義的參數特徵。 | 緊密耦合（尤其是繼承），子類依賴父類的結構。                                     |
| **程式碼複用** | 透過**組合（Composition）**，將不同行為（Callback）注入同一個函式。 | 透過**繼承（Inheritance）**或多型（Polymorphism）來複用。                 |
---
### 關鍵差異點：

- **Callback 追求的是==「控制反轉（Inversion of Control）==」**：我（主函式）不知道什麼時候該做這件事，所以我把這件事（Callback）交給你，由你決定何時執行。
    
- **OOP 追求的是「職責封裝」**：物件自己管好自己的資料（屬性）和方法。如果 OOP 要實現類似 Callback 的行為，通常會使用**觀察者模式（Observer Pattern）**或**策略模式（Strategy Pattern）**，這需要定義完整的介面或類別。


```ts
// 1. 定義一個非同步登入函式，最後一個參數接收一個 callback 函式
function loginUser(username, password, callback) {
    window.setTimeout(() => {
        console.log("🔑 [1/2] 伺服器驗證成功！");
        const userData = { id: 9527, name: username, role: "VIP" };
        
        // 核心關鍵：資料拿到了！這時候才呼叫 callback，並把資料傳出去
        callback(userData); 
    }, 2000); // 模擬網路延遲 2 秒
}

// 2. 定義一個拿到資料後，接下來要執行的行為
function handleLoginSuccess(user) {
    console.log(`✨ [2/2] 歡迎回來，${user.name}！您的權限是：${user.role}`);
}

// 3. 呼叫主程式
console.log("🚀 開始登入...");
loginUser("Alex", "123456", handleLoginSuccess);
console.log("⏳ 主程式繼續往下跑，畫面不會卡死...");
```
### 🏃 執行結果與順序

當你在瀏覽器執行這段程式碼時，主執行緒不會卡住等 2 秒，而是會完美按照 Event Loop 的機制依序印出：

1. `🚀 開始登入...`
    
2. `⏳ 主程式繼續往下跑，畫面不會卡死...`
    
3. _(等待 2 秒鐘後)_
    
4. `🔑 [1/2] 伺服器驗證成功！`
    
5. `✨ [2/2] 歡迎回來，Alex！您的權限是：${user.role}` ➡️ **成功拿到物件，再也不會是 `undefined` 了！**

---


```ts
function greet() {
    console.log("Hello!");
}

// 🎯 沒錯！你可以像對待物件一樣，幫函式新增屬性
greet.language = "Chinese"; 
greet.version = 1.0;

console.log(greet.language); // 輸出: "Chinese"
greet(); // 輸出: "Hello!" （它依然可以被呼叫）
```

# 🎯 我們在傳遞 [[Callback]] 函式時，實際上传遞的正是這張表的**記憶體地址（指標）**

💡 記憶體地址帶來的關鍵特性：閉包（Closure）

因為傳遞的是==記憶體地址==，這個==函式物件==可以一直存活在 Heap 中，直到沒有人引用它為止。這就引出了一個非常有名的現象：==**閉包**。==

當一個內部函式被當作 [[Callback]] 傳到外面時，它不只帶走了自己的記憶體地址，還順便把創建它時的==外層環境（變數）一起綁定並留在了 Heap 中。==
```ts
function createCounter() {
    let count = 0; // 這個變數儲存在外層環境中
    return function() {
        count++;
        console.log(count);
    };
}

const myCounter = createCounter();
```

### JavaScript 的**非同步執行機制**與**事件循環（Event Loop

https://www.youtube.com/watch?v=jzOy07fw2vY



- **記憶體與全域執行上下文（Global Execution Context）**：在執行任何代碼前，JavaScript 會先==為變數與函式配置記憶體==，並創建==全域執行上下文==放入==呼叫堆疊中 ==。
- **呼叫堆疊（Call Stack）**：負責管理程式碼的==執行順序 ==。==採「後進先出（LIFO）」機制，函式被呼叫時推入（Push）堆疊，執行完畢後彈出（Pop） 。
- **[[Web API]]**：由==瀏覽器環境==（而非 JavaScript 引擎本身）提供的超能力 。負責處理==需要等待或與外部聯絡的任務，==例如 `setTimeout` 定時器 、`fetch` 網路請求 、DOM 事件監聽 及地理位置服務（Geolocation）。Callback function會和Web APIs去做註冊。
- 
- **工作佇列 / 回呼佇列（Task Queue / [[Callback]] Queue==）**：傳統非同步任務（如定時器、DOM 事件、地理位置回呼）完成後，其==回呼函式==會先在此排隊，等待被執行的機會 。
- ==**微任務佇列（Microtask Queue）**==：存放==高優先權任務的佇列== 。主要接收 Promise 的回呼（`.then`、`.catch`、`finally`） 、`async/await` 的後續程式碼 、`MutationObserver` 以及 `queueMicrotask` 。**微任務的優先權絕對高於一般工作佇列** 。
- **事件循環（Event Loop）**：扮演橋樑的角色 ，持續監控**呼叫堆疊**與**這兩個佇列** 。


==**Microtask Starvation（微任務飢餓）**== 現象，最終導致瀏覽器分頁或 Node.js 執行緒卡死。 

- **微任務優先級最高：** 每次 JavaScript 執行完當前任務（執行棧 Call Stack 清空）後，Event Loop 就會去處理 Microtask Queue。
- **無止境的排隊機制：** Event Loop 規定，當它開始清空 Microtask 佇列時，**必須一口氣將佇列裡的所有任務全部執行完畢**。如果在執行過程中，又不斷有新的微任務被加入進來，Event Loop 也會繼續執行它們 。
- **遞迴引發無窮迴圈：** 在你的 `starve()` 函數內呼叫 `queueMicrotask(starve)`，等於在每一個微任務結束前，又把下一個 `starve` 推入佇列中。這導致 Microtask 佇列永遠不會淨空。
```ts
// This macrotask is forced to wait indefinitely
setTimeout(() => {
  console.log("Macrotask executed!"); 
}, 0);

function starve() {
  // queueMicrotask continuously feeds the microtask queue
  //「把 `starve` 這個函數放入等待執行的微任務佇列（Microtask Queue）中」
  queueMicrotask(starve);
}

// Triggering this will freeze your browser tab or Node.js process
//當目前的程式碼全部跑完、執行棧（Call Stack）清空後，JavaScript 的**事件循環（Event Loop）**才會去翻這個待辦清單，並幫你呼叫 `starve`
starve(); 

```
為何 function 內部可以呼叫自己？

在程式設計中，函數內部呼叫自己是一個非常核心且常見的技巧，稱為**「遞迴」（Recursion）**。

JavaScript 允許這樣寫，原因如下：

- **函數名稱也是一個變數：** 當你宣告了 `function starve() {...}`，在全域環境中就存在了一個叫做 `starve` 的變數，它的內容指向這段程式碼。
- **執行時才尋找變數：** 當函數內部的程式碼被執行時，JavaScript 會去尋找 `starve` 這個名字代表什麼。因為它已經被宣告了，所以函數內部當然找得到自己、也能夠引用自己。

```ts
function countdown(number) {
  if (number <= 0) return; // 終止條件：到 0 就停下
  console.log(number);
  countdown(number - 1);  // 呼叫自己，傳入新參數
}
countdown(3); // 輸出 3, 2, 1

```
你的 `starve` 函數之所以會出事，是因為它滿足了兩個致命條件：

1. **它沒有設定任何停止的條件（如 `if` 判斷）。**
2. **它利用了==微任務永遠優先於畫面渲染==的特性。**

所以它不是傳統的「無限遞迴導致記憶體滿出來（Stack Overflow）」，而是「==**無限排程==導致 CPU 沒空理會滑鼠和畫面（Microtask Starvation）**」。

