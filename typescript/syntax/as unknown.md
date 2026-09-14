- 在 TypeScript 中，`as unknown` 或雙重斷言模式 `as unknown as T` 是用來**繞過編譯器型別檢查並強行轉換型別**的手段。
- `unknown` 是 TypeScript 的頂層型別（Top Type），代表「**任何值都可以賦值給它，但它在被指派為特定型別前，不能進行任何操作**」。  
當你使用 `value as unknown` 時，意思是告訴編譯器：「先忘掉這個變數原本的型別，把它當作完全未知的東西」。 
```ts
const priceString = "100";

// ❌ Error: Conversion of type 'string' to type 'number' may be a mistake...
const price = priceString as number; 

//  Success: Bypasses the compiler restriction safely
const safePrice = priceString as unknown as number; 

```
2. 最常見的用法：雙重斷言（Double Assertion）

TypeScript 為了安全，不允許隨便將兩個毫不相關的型別互相轉換。例如，你不能直接把 `string` 斷言成 `number`：

```ts
const text = "123";
// ❌ 錯誤：TS2352，型別 string 無法重疊轉換為型別 number
const num = text as number; 

```
如果你非常確定自己在做什麼，就必須透過 `as unknown` 作為中介，進行**雙重斷言**： 
```ts
const text = "123";
// ✅ 成功繞過檢查：先抹除原本型別，再強制賦予新型別
const num = text as unknown as number; 

```

3. 常見應用場景

A. 覆蓋第三方庫不精準的型別定義
當你引用國外或第三方套件，發現其導出的介面（Interface）與實際回傳的 JSON 結構有落差，且你無法修改原始碼時
```ts
import { LegacyUser } from 'some-library';

// 強制將舊結構轉換為符合你專案內部的全新 User 介面
const currentUser = getLegacyData() as unknown as User;
```
B. 在單元測試中模擬（Mock）複雜物件

在編寫測試時，若某個函式需要傳入一個擁有 50 個屬性的龐大物件，但測試本身只需要其中 2 個屬性：
```ts
interface HeavyConfig {
  id: string;
  theme: string;
  // 還有其他 48 個極為複雜的屬性...
}

// 測試時只需模擬關鍵屬性，用雙重斷言避開補齊其餘屬性的麻煩
const mockConfig = { id: "test-123", theme: "dark" } as unknown as HeavyConfig;
```

4. `as unknown as T`
這兩種方式在**執行期（Runtime）達到的目的完全相同**，但在編譯階段的安全性上有微小差異： 

- **`as any`**：徹底關閉該變數在當前表達式後的所有型別系統監控，屬於直接放棄治療。
- **`as unknown`**：它是型別安全的替代方案。在 `as unknown` 的那一刻，你依然被限制不能對它做任何事，直到你明確接上 `as T` 之後，它才開始遵循 `T` 型別的規範進行檢查。