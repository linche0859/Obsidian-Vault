[[TypeScript]]

# 基本定義
```ts
class User {
  constructor(userName: string) {
    this.name = userName;
  }
  name: string;
  age: number;
  address: string;

  add() {}
  update() {}
  delete() {}
}

const user = new User('bruce');
```

# 實作 Interface
> 如果要看在 `class` 的泛型應用可以參考 [[6.4 Function 和 Class 的泛型應用]]

實作 `interface` 的目的是 **約束** `class` 的內容需要遵守 `interface` 的定義。
使用 `implements` 的關鍵字表示實作的動作。
```ts
interface IUser {
  id: number;
  name: string;
  age: number;
  address: string;

  add: (data: any) => void;
  update: (id: number) => boolean;
  delete: (id: number) => boolean;
}

class User implements IUser {
  constructor(userName: string) {
    this.name = userName;
  }
  id: number;
  name: string;
  age: number;
  address: string;

  add(data: any) {}
  update(id: number) {
    return true;
  }
  delete(id: number) {
    return true;
  }

  // 額外新增的方法
  get() {
    return {};
  }
}
```

# 繼承
`class` 的繼承會使用 `extends` 的關鍵字。
```ts
class Animal {
  name: string;
  run() {}
}

class Dog extends Animal {
  age: number;
  address: string;
  constructor() {
	// 如果有使用 constructor 函數就必須呼叫 super
    super();
  }
  run() {
    super.run();
    // ...
  }
}
```

上方範例值得一提的是，在 `Dog` 的類別中有覆寫 `run` 的方法，其中也呼叫 `super.run` 的方法，表示會先呼叫 `Animal` 的 `run` 方法，再接續執行方法的內容。 

# 抽象類別
可以 **實作** 和 **定義繼承後需要實作的方法**。

⚠ 抽象類別不能使用 `new` 關鍵字建立物件。

在定義抽象類別時，使用 `abstract` 的 關鍵字，需要給繼承類別實作的方法，是將 `abstract` 放置於方法的前方。
```ts
abstract class Animal {
  name: string;
  run() {}
  abstract hello(): string;
}

class Dog extends Animal {
  hello() {
    return `Hello ${this.name}`;
  }
}
```

⚠ 這邊需要別注意，在定義給繼承類別實作的方法時，是使用 `函數名稱(): 回傳型別` 的寫法，而不是 `函數名稱: () => 回傳型別` 的寫法。


