[[MongoDB]]

# 新增本地端資料庫
- 顯示各個資料庫：`show dbs`
- 切換資料庫：`use [資料庫名稱]`
- 新增資料到 collections： `db.rooms.insertOne({"rating":4.5,"price":1000,"name":"標準單人房"})`
- 尋找資料：`db.rooms.find()`

>  1. 在還沒新增 Collention 下的 Database，使用 `show dbs` 會看不到特定的 Database，等到新增一個 Collection 和一筆 Document 後，才會顯示出來。
> 2. 新增一筆 Document 至 Collection 後，會自動新增其編號 (`_id`)，用於判斷該資料的唯一值。
> ![Untitled](https://i.imgur.com/pqQRnuR.png)

# 新增 Document
> [MongoDB 新增 Document 文件](https://docs.mongodb.com/manual/tutorial/insert-documents/)

## 新增單筆
`db.collection.insertOne(<data>, <options>)`

```jsx
db.inventory.insertOne(
   { item: "canvas", qty: 100, tags: ["cotton"], size: { h: 28, w: 35.5, uom: "cm" } }
)
```

## 新增多筆
`db.collecyion.insertMany(<data>, <options>)`

```jsx
db.rooms.insertMany(
  [
    {
      rating: 4,
      name: "標準單人房",
      price: 100,
    },
    {
      rating: 2,
      name: "標準雙人房",
      price: 200,
    }
  ]
)
```

# 更新 Document
## 更新單筆 (指定 field 更新)
`db.collection.updateOne(<filter>, <update>, <options>)`

```jsx
db.rooms.updateOne(
  {
    _id: ObjectId('622d9cbbd44eba6caefaa244'),
  },
  {
    $set: {
      name: '標準單人房升級版',
      rate: 4.3,
    },
  }
);
```

### 新增一個值於陣列中
> 參考：[$push — MongoDB Manual](https://www.mongodb.com/docs/manual/reference/operator/update/push/)

```jsx
db.posts.updateOne(
  {
    _id: ObjectId("62528fa0ff8c335be6873360"),
  },
  { $push: { tags: "遊記" } }
)
```

## 更新多筆 (指定 field 更新)
`db.collection.updateMany(<filter>, <update>, <options>)`

```jsx
db.rooms.updateMany(
  {
    rating: 4.3,
  },
  {
    $set: {
      rating: 0,
    },
  }
);
```

### 移除陣列裡的某一個值
> 參考：[3 Ways to Remove a Value from an Array in MongoDB (database.guide)](https://database.guide/3-ways-to-remove-a-value-from-an-array-in-mongodb/)

```jsx
db.posts.updateMany(
  { tags: { $in: ["感情"] } },
  { $pullAll: { tags: ["感情"] } }
)
```

## 更新代辦 (替換整個 document)
`db.collection.replaceOne(<filter>, <update>, <options>)`

```jsx
db.rooms.replaceOne(
  {
    name: "標準雙人房",
  },
  {
    name: "標準雙人房升級版",
  }
);
```

# 刪除 Document
## 刪除單筆
`db.collection.deleteOne(<filter>, <options>)`

```jsx
db.rooms.deleteOne(
  {
    _id: ObjectId("622d9cbbd44eba6caefaa245"),
  }
);
```

## 刪除多筆
`db.collection.deleteMany(<filter>, <options>)`

當 `<filter>` 傳入空物件代表刪除全部的資料。

```jsx
db.rooms.deleteMany(
  {
    rating: 4
  }
);
```

# 尋找 Document
- 基本搜尋
    - 查詢全部：`db.collection(collections name).find()`
    - 尋找對應屬性：`db.collections.find({ 屬性名稱: 屬性值 })`
    - 數值區間：`db.collections.find({ 屬性名稱: { $lte: 400 } })`
    - 複合條件：`db.collections.find({ 屬性名稱 :{ $lte: 400 }, rating: { $gte: 4.8 } })`
    - 關鍵字搜尋：`db.rooms.find({ "name": /雙/ })`
- 常用語法
    - project 保護欄位：`db.rooms.find({ "name": /雙/ }, { "_id": 0 })`
    - 搜尋陣列裡面的值：`db.rooms.find({ "payment": { $in: ["信用卡"] } })`

<aside>
📍 operator：

- `$eq`：等於
- `$ne`：不等於
- `$gt`：大於
- `$lt`：小於
- `$gte`：大於等於
- `$lte`：小於等於
- `$in`：存在某個值
- `$nin`：不存在某個值
</aside>

## 尋找單筆
`db.collection.findOne(<filter>, <options>)`

```jsx
db.rooms.findOne(
  {
    price: 2500
  }
);
```

## 尋找多筆
`db.collection.find(<filter>, <options>)`

```jsx
db.rooms.find(
  {
    price: 2500,
		rating: 4
  }
);
```

## 數值區間查詢
```jsx
db.rooms.find({
  rating: { $gte: 4.7 },
  price: { $lte: 2000 },
});
```

## 關鍵字搜尋
可以使用正規表達式做為搜尋的語法。

```jsx
db.rooms.find({
  name: /雙人/,
});
```

## 保護欄位
回傳的資料沒有 `options` 的指定屬性。

```jsx
db.rooms.find(
  {
    name: /雙人/,
  },
  {
    _id: 0,
    rating: 0,
  }
);

// 回傳結果
{ "price" : 2000, "name" : "標準雙人房", "payment" : [ "信用卡", "ATM" ] }
{ "price" : 2500, "name" : "豪華雙人房", "payment" : [ "ATM" ] }
```

## 尋找陣列裡的值
```jsx
db.rooms.find({
  payment: { $in: ['信用卡', 'ATM'] },
});
```

以上方的語法會搜尋出 `payment` 屬性中有「信用卡」或「ATM」的資料。

## 排序欄位
`1` 代表從舊到新，`-1` 代表由新到舊。

```jsx
db.posts.find({ name: "Ray Xu" }).sort({ createdAt: -1 })
```

## 只顯示前幾筆資料
```jsx
db.posts.find({ name: "Ray Xu" }).limit(30)
```

## 略過前幾筆資料
```jsx
db.posts.find({ comments: { $gt: 100 } }).skip(30).limit(30)
```

## 邏輯運算子 (`AND` & `OR`)
> 注意：陣列裡的篩選條件需各自包裹於物件中，不可寫在同一物件中。

- AND
    
    ```jsx
    db.posts.find({
      $and: [{ likes: { $gte: 1000 } }, { comments: { $gte: 1000 } }],
    })
    ```
    
- OR
    
    ```jsx
    db.posts.find({
      $or: [{ likes: { $gte: 1500 } }, { comments: { $gte: 1500 } }],
    })
    ```
    

# References 轉換至 Embedded
```jsx
db.rooms.aggregate( [{
    $lookup: {
        from: 'user',
        localField: 'users',
        foreignField: '_id',
        as: 'users'
    }
}])
```