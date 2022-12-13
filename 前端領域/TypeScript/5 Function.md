[[TypeScript]]

# 參數的型別
```ts
function get(a: number, b: string, c: boolean) {
  return a + b + c;
}
```

## 可選的參數
只要在參數的後方加入 `?` 符號，就代表是可選擇的傳入參數。需要特別注意，可選的參數 **必須放於必填參數的後方**。
```ts
function setUser(name: string, age?: string) {
  if (typeof age === 'string') {
    return age.split('');
  }
}
```

如果在使用可選參數的屬性或方法時，TypeScript 會提醒「可能為 undefinded」，這時需要做型別的檢查，才可以避免執行時的意外錯誤。

## 物件型別
```ts
type Info = {
  name: string,
  age: number
}

function creatUserInfo(info: Info) {
  console.log(info.name);
  return info;
}
```

## 建構函數
```ts
type Card = {
  name: '';
};

type CardConstructor = {
  new (name: string): Card;
};

function createCard(constructor: CardConstructor) {
  return new constructor('bruce');
}
```

# 回傳值的型別
```ts
function getName(): string {
  return '';
}
```

## 拋出錯誤
使用 `never` 型別定義。
```ts
function getUserData(): never {
  throw new Error('...');
}
```

## 陣列
如果在未明確定義回傳的陣列型別時，可能被判斷為 `union` 的陣列型別。
```ts
function getArr() {
  return [0, 1, 'bruce'];
}
```

`getArr()` 函數判斷為回傳 `(string | number)[]`，當今天進行解構時，每個變數會被定義為 `string | number`，因此在呼叫變數的方法時，就會發生問題。
```ts
const [id, age, userName] = getArr();

id.split(''); // 類型 'string | number' 沒有屬性 'split'
userName.split(''); // 類型 'string | number' 沒有屬性 'split'
```

這時只要明確的定義回傳的陣列內容型別，就可以避免上方的問題。
```ts
function getArr() {
  // 利用斷言的方式
  return [0, 1, 'bruce'] as [number, number, string];
}
// 或
function getArr(): [number, number, string] {
  return [0, 1, 'bruce'];
}
```