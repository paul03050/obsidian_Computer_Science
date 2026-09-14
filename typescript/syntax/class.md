
### private

```ts
class BankAccount {
  private balance: number;

  constructor(initialBalance: number) {
    this.balance = initialBalance;
  }

  public getBalance(): number {
    return this.balance; // Allowed inside the class
  }
}

const account = new BankAccount(1000);
console.log(account.getBalance()); // 1000
// console.log(account.balance); 
// ❌ Error: Property 'balance' is private and only accessible within class 'BankAccount'.

```

-  類別的私有欄位前綴（`#` Private Fields）
當您在類別的屬性或方法名稱前加上 `#` 時，該成員會變成真正的**執行期私有成員**。這意味著它在類別外部完全無法被存取，甚至在轉譯成 JavaScript 後依然受到保護。
```ts
class BankAccount {
  // 使用 # 前綴宣告私有欄位
  #balance: number = 0; 

  constructor(initialBalance: number) {
    this.#balance = initialBalance;
  }

  public getBalance() {
    return this.#balance; // 內部可以正常存取
  }
}

const account = new BankAccount(1000);
console.log(account.getBalance()); // 1000
// console.log(account.#balance);  // ❌ 錯誤！編譯與執行時皆會報錯，外部無法存取

```

### protected

`protected` 標記的屬性或方法，只能在「宣告它的類別內部」以及「繼承它的子類別（Subclasses）內部」被存取，無法從外部實例（Instance）直接存取。

這與 `private` 不同（`private` 連子類別都無法存取），也與 `public` 不同（`public` 允許任何地方存取）。

```ts
class Animal {
    // 宣告一個受保護的屬性
    protected name: string;

    constructor(name: string) {
        this.name = name; // ✅ 正確：可以在父類別內部存取
    }
}

class Dog extends Animal {
    constructor(name: string) {
        super(name);
    }

    public bark() {
        // ✅ 正確：子類別可以存取父類別的 protected 屬性
        console.log(`${this.name} 汪汪叫！`); 
    }
}

// ---- 外部測試 ----
const myDog = new Dog("小黑");

myDog.bark(); // 輸出: 小黑 汪汪叫！

// ❌ 錯誤！編譯時會報錯：
// Property 'name' is protected and only accessible within class 'Animal' and its subclasses.
console.log(myDog.name); 

```


|存取修飾詞|類別內部（Self）|子類別內部（Subclass）|外部實例（Outside）|
|---|---|---|---|
|**`public`** (預設)|✅|✅|✅|
|**`protected`**|✅|✅|❌|
|**`private`**|✅|❌|❌|

## class vs type
- The fundamental difference is that ==a **`class` acts as a runtime blueprint** that generates actual JavaScript code, whereas a **`type` is a compile-time construct** used solely for static type checking and is completely erased when compiled==. 

- Quick Summary

| Feature              | `class`                                                    | `type` (Type Alias)                                |
| -------------------- | ---------------------------------------------------------- | -------------------------------------------------- |
| **Runtime Presence** | ✅ Yes, compiled to a JavaScript function/prototype.        | ❌ No, entirely stripped out during compilation.    |
| **Main Use Case**    | Instantiating objects, managing state, and grouping logic. | Defining pure data structures, unions, and tuples. |
| **Advanced Types**   | ❌ Cannot represent unions or intersections.                | ✅ Can use unions (`\|`) and intersections (`&`).   |
| **Instantiable**     | ✅ Yes, via the `new` keyword.                              | ❌ No, it is only a declaration.                    |
| **Runtime Check**    | ✅ Yes, supports `instanceof` safety checks.                | ❌ No, checks require manual type guards.           |


### 1. Class: Data and Behavior

`class` 是 JavaScript 的原生特性（TypeScript 加強了型別安全）。它不僅定義了資料的形狀，還包含**實際的程式邏輯（方法）**與**初始化狀態（建構函數）**。

```ts
class UserClass {
  // 屬性與存取修飾詞
  public name: string;

  constructor(name: string) {
    this.name = name;
  }

  // 實際包含的方法邏輯 (執行期可呼叫)
  greet() {
    return `Hello, ${this.name}`;
  }
}

// 執行期行為：可以 new，也可以用 instanceof 檢查
const userInstance = new UserClass("Alex");
console.log(userInstance.greet()); // 輸出: "Hello, Alex"
console.log(userInstance instanceof UserClass); // 輸出: true

```

### 2. type (型別別名)：純粹的型別合約

`type` 是 TypeScript 特有的語法。它只用來給特定的型別結構**起一個新名字**。它不能包含實作程式碼，也無法在執行期使用。

```ts
type UserType = {
  name: string;
};

// 只能當作標註使用，物件必須以字面量形式建立
const userObject: UserType = {
  name: "Alex"
};

// ❌ 錯誤：'UserType' 僅代表型別，但在此處被當作值使用
// const broken = new UserType(); 
// const brokenCheck = userObject instanceof UserType; 

```
此外，`type` 具備極高的靈活性，能做到 `class` 無法做到的進階型別操作（如聯集型別）：

```ts
type ID = string | number; // 聯集型別 (Union Type)
type Status = "success" | "error" | "pending"; // 字面量聯集

```
