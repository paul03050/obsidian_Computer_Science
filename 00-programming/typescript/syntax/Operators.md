
## 一、 TypeScript 特有的型別運算子 (Type Operators)

這些運算子專門用於型別定義與編譯期的型別檢查，不會產生實際的 JavaScript 執行代碼。 

- `keyof`（索引型別查詢）：獲取一個物件型別的所有鍵（Key），並組合成聯合型別。
    
    ```typescript
    type User = { id: number; name: string };
    type UserKeys = keyof User; // 'id' | 'name'
    ```
    
- `typeof`（型別空間用法）：在型別定義中，直接提取一個現有變數的型別結構。
    
    ```typescript
    const config = { port: 8080, host: "localhost" };
    type ConfigType = typeof config; // { port: number; host: string }
    ```
    
- `as`（型別斷言）：告訴編譯器「我知道這個變數是什麼型別」，用來覆蓋預設的型別推導。
    
    ```typescript
    let data: any = "Hello World";
    let len = (data as string).length;
    ```
    
- `satisfies`：既能驗證某個物件是否符合特定型別，又能保留該物件最精準的字面量型別推導。
    
    ```typescript
    type Palette = { colors: string[] | string };
    const theme = { colors: ["red", "blue"] } satisfies Palette;
    theme.colors.map(c => c.toUpperCase()); // 正常運作，TS 知道這是陣列而非字串
    ```
    
- `is`（型別謂詞 Type Predicates）：用於自訂型別守衛（Type Guards），明確告知編譯器條件成立時的精確型別。
    
    ```typescript
    function isString(val: any): val is string {
      return typeof val === "string";
    }
    ```
    
- `&` 與 `|`（型別交集與聯集）：`|` 代表聯集（符合其中之一），`&` 代表交集（同時符合所有型別特性）。
    
    ```typescript
    type Admin = { privileges: string[] };
    type Employee = { name: string };
    type Staff = Admin & Employee; // 必須同時擁有 privileges 與 name
    ```

---

## 二、 常用執行時運算子 (Runtime Operators)

這類運算子在 JavaScript 與 TypeScript 中均通用，但 TypeScript 會額外進行嚴格的型別安全檢查。 [1, 14]

|運算子分類|語法範例|說明與安全特性|
|---|---|---|
|可選鏈 (Optional Chaining)|`obj?.prop`|當 `obj` 為 `null` 或 `undefined` 時直接回傳 `undefined`，避免報錯。|
|空值合併 (Nullish Coalescing)|`val ?? default`|只有當 `val` 為 `null` 或 `undefined` 時才使用預設值（與 `\|` 不同，`0` 或 `""` 不會被覆蓋）。|
|非空斷言 (Non-null Assertion)|`obj!.prop`|在變數後方加上 `!`，強迫編譯器排除 `null` 與 `undefined` 的可能性。|
|條件（三元）運算子|`cond ? x : y`|`if-else` 的簡短寫法，常在賦值或 JSX 中使用。|
|展開運算子 (Spread)|`[...arr]`|展開陣列或物件，TypeScript 會嚴格檢查被展開物件的型別屬性。|
|`in` 運算子|`'key' in obj`|檢查物件是否包含某屬性，常用於聯合型別的型別限縮（Type Narrowing）。|

想了解更多型別工具，可以參考官方的 [TypeScript 實用型別文件 (Utility Types)](https://www.typescriptlang.org/docs/handbook/utility-types.html)。
