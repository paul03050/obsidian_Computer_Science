```ts
let a: number | string;    //Union
a = 1000
a = "hello"
```

```ts
type ID = string;
type Age = number;

const userId: ID = "usr_10293";

```

```ts
type User = {
  id: string;
  name: string;
  isAdmin: boolean;
};

const admin: User = { id: "1", name: "Alice", isAdmin: true };

```

- Union and Intersection Types
```ts
// Union Type (one OR the other)
type Status = "pending" | "approved" | "rejected";

// Intersection Type (combining multiple types)
type Worker = { name: string } & { salary: number };
```

- Function Signatures

You can explicitly define the expected parameters and return type of a function.
```ts
type MathOperation = (a: number, b: number) => number;

const add: MathOperation = (x, y) => x + y;

```
