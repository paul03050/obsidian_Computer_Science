In TypeScript, `Readonly<Type>` is ==a built-in tool that makes all properties of an object unchangeable==. Once you create an object, you cannot update its values. This helps prevent accidental changes to your data.

```ts
interface User {
  name: string;
  age: number;
}

// Make all properties readonly
const user: Readonly<User> = {
  name: "Alice",
  age: 25,
};

// Error: Cannot assign to 'age' because it is a read-only property.
user.age = 26; 

```

Why use it?

- **Safe Functions:** Use it for function parameters. It proves your function only _reads_ data and will not accidentally change the original data.
- **State Management:** It is very helpful in frameworks like React to prevent changes to state objects outside of update functions.

Important Facts

- **It is only for TypeScript:** The `readonly` rules are only checked while you write code. After the code turns into JavaScript, the rules disappear, and the object can be changed at runtime.
- **Shallow Lock:** It only locks the top level of the object. If your object has another object inside it, you can still change the inner properties.