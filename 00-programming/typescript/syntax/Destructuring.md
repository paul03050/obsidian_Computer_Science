**解構賦值** (Destructuring) ==是一種快速提取物件或陣列資料並存入變數的方法==。這就像是把一個裝滿玩具的箱子打開，直接把玩具拿出來放在桌上。

### 1. 物件解構 (Object Destructuring)
- 物件解構是把物件裡的屬性拿出來。變數名稱必須和物件裡的屬性名稱一樣。 
```ts
const user = { name: "小明", age: 18 };

// 將 name 和 age 抽出來
const { name, age }: { name: string; age: number } = user;

console.log(name); // "小明"
console.log(age);  // 18

```
如果你想換變數名字，可以這樣寫：
```ts
const { name: userName }: { name: string } = user;
console.log(userName); // "小明"

```
### 2. 陣列解構 (Array Destructuring)
```ts
const numbers = [10, 20, 30];

// 依照順序把值放入變數
const [first, second]: [number, number] = numbers;

console.log(first);  // 10
console.log(second); // 20


const [, , third]: [number, number, number] = numbers;
console.log(third); // 30


```

### 3. 函式參數解構 (Function Parameter Destructuring)
- 解構最常在函式中使用。這樣一來，傳入參數時就不用管順序
```ts
interface Point {
  x: number;
  y: number;
}

// 直接在參數的地方進行解構並給予型別
function printCoordinate({ x, y }: Point) {
  console.log(`X 座標是 ${x}`);
  console.log(`Y 座標是 ${y}`);
}

printCoordinate({ x: 5, y: 10 });

```



