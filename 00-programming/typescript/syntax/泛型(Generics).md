```ts
// 宣告一個泛型型別，把 data 的型別交給外部決定
type ApiResponse<Data> = {
  data: Data;
  isError: boolean;
};
```


```ts
// 限制了只能傳數字陣列
function getFirstElement(array: number[]) {
  return array[0];
}

const numbers = [1, 2, 3];
const firstNum = getFirstElement(numbers); // 型別為 number

const strings = ["a", "b", "c"];
// ❌ 這裡會報錯，因為 getFirstElement 不接受 string[]
const firstStr = getFirstElement(strings);

----------------------------------------------------------------


// 宣告泛型函數（建議初學者寫得具體一點，如 ElementType）
function getFirstElement<ElementType>(array: ElementType[]) {
  return array[0];
}

const numbers = [1, 2, 3];
const firstNum = getFirstElement(numbers); // 💡 自動推導型別為 number

const strings = ["a", "b", "c"];
const firstStr = getFirstElement(strings); // 💡 自動推導型別為 string

// 如果 TypeScript 無法自動推導，也可以手動傳入型別：
const firstStrManual = getFirstElement<string>(strings);
```

```ts
// 範例：支援第二個泛型參數
function getFirstElement<ElementType, ElementType2>(array: ElementType[]) {
  // ... 程式碼邏輯
}
```



