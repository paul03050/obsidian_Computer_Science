If you define two interfaces with the exact same name in the same scope, TypeScript automatically merges their properties into a single definition. This is critical for authoring public npm libraries or extending global objects like `window`. ==`type` aliases will throw a compilation error if duplicated==. 

```ts
// Interfaces merge seamlessly
interface User { name: string; }
interface User { age: number; }

const user: User = { name: "Alice", age: 30 }; // Valid

// Types throw an error
type Admin = { name: string; };
type Admin = { role: string; }; // Error: Duplicate identifier 'Admin'

```


- 1. Optional Properties in an Interface
```ts
interface User {
  id: number;
  name: string;
  email?: string; // This is optional
}

// Both are valid:
const user1: User = { id: 1, name: "Alice", email: "alice@example.com" };
const user2: User = { id: 2, name: "Bob" }; // email is omitted

```

2. Optional Function Parameters
```ts
function greet(name: string, greeting?: string) {
  if (greeting) {
    console.log(`${greeting}, ${name}`);
  } else {
    console.log(`Hello, ${name}`);
  }
}
```

###  implements
在 TypeScript 中，類別（Class）使用 **`implements`** 關鍵字來實作介面（Interface）。這是一種強制的**型別契約**，代表該類別必須完全符合介面所定義的屬性與方法，否則編譯時會報錯。

```ts
// 1. 定義介面
interface Animal {
  name: string;
  makeSound(): void;
}

// 2. 類別實作介面
class Dog implements Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  makeSound(): void {
    console.log("汪汪！");
  }
}

```

#### 1. 實作多個介面
```ts
interface CanFly {
  fly(): void;
}

interface CanSwim {
  swim(): void;
}

// 同時強制要求實作飛行與游泳的行為
class Duck implements CanFly, CanSwim {
  fly() {
    console.log("鴨子飛高高");
  }
  swim() {
    console.log("鴨子划 social");
  }
}

```

#### 2. 介面包含選填與唯讀屬性
- 介面可以使用 `?` 標記選填屬性，或使用 `readonly` 標記不可修改的屬性。
```ts
interface Car {
  readonly brand: string; // 唯讀，初始化後不可修改
  model: string;
  color?: string;         // 選填，類別可寫可不寫
}

class SportsCar implements Car {
  readonly brand: string;
  model: string;

  constructor(brand: string, model: string) {
    this.brand = brand;
    this.model = model;
  }
}

```

#### 3. 類別可擴充額外成員
介面只規範了類別的**最低要求**。實作介面的類別完全可以自由增加介面中沒有定義的屬性或方法。

```ts
interface Alarm {
  ring(): void;
}

class SmartClock implements Alarm {
  ring() {
    console.log("嗶嗶嗶！");
  }

  // 額外增加介面沒有定義的方法
  showTime() {
    console.log(new Date().toLocaleTimeString());
  }
}

```

#### ⚠️ 重要注意事項

- **不能約束私有成員**：介面只能用來描述類別的 **`public`（公開）** 部分，無法用來檢查 `private` 或 `protected` 的屬性與方法。
- **僅在編譯期生效**：TypeScript 的介面屬於「零執行時開銷（Zero runtime overhead）」，程式碼編譯成 JavaScript 後，所有的 `interface` 和 `implements` 關鍵字都會被**完全移除**。