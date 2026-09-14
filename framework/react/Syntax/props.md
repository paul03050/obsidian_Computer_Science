在 TypeScript 中對 **props** 進行**[[Destructuring]]**，可以讓程式碼更簡短。你不必每次都寫 `props.name`，只要直接使用變數 `name` 即可。

1. 基礎解構賦值
```ts
// 定義 props 的型別
interface Props {
  name: string;
  age: number;
}

// 解構賦值： { name, age } 直接從 props 提取出來
function UserCard({ name, age }: Props) {
  return (
    <div>
      姓名: {name}, 年齡: {age}
    </div>
  );
}

```
2. 設定預設值
```ts
interface Props {
  name: string;
  age?: number; // 這是可選屬性 (非必填)
}

function UserCard({ name, age = 18 }: Props) {
  return (
    <div>
      姓名: {name}, 年齡: {age}
    </div>
  );
}

```
3. 重新命名變數
```ts
interface Props {
  title: string;
}

function Header({ title: headingText }: Props) {
  return <h1>{headingText}</h1>;
}

```
