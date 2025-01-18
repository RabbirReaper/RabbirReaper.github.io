---
title: Mongoose 的基礎語法筆記
date: 2025-01-15 11:56:28
tags: 
  - Mongoose
  - MongoDB
  - node.js
categories:
  - [程式語言筆記,node.js]

description: 這是一篇介紹Mongoose基礎語法的文章，內容有新增、修改、刪除，以及資料關聯`.populate`的基礎用法。
---
# 前言

這是一篇介紹Mongoose基礎語法的文章，內容有新增、修改、刪除，以及資料關聯`.populate`的基礎用法。


# 建立連接 MongoDB

如果**MongoDB**是建立在本地端的話，網址會像下面這樣
```js
await mongoose.connect('mongodb://127.0.0.1:27017/shopApp')
.then(() => {
  console.log("Connection OPEN!!!")
})
.catch((err) => {
  console.log("OH NOOOOO")
  console.log(err)
})
```
如果是用雲端的話網址會長一點
## [官方文檔](https://mongoosejs.com/docs/connections.html)

# Schema 與 Model 設計基礎

## 程式碼範例：
```js
const expenseCategorySchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    unique: true,
  }
});

const incomeCategorySchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    unique: true,
  }
});

const cashFlowSchema = new mongoose.Schema({
  amount: {
    type: Number,
    min: 0,
    required: true
  },
  type: {
    type: String,
    enum: ['income', 'expense']
  },
  category: {
    type: mongoose.Schema.Types.ObjectId,
    required: true,
    ref: function() {
      return this.type.toString() === 'expense' ? 'ExpenseCategory' : 'IncomeCategory';
    }
  },
  date: {
    type: Date,
    default: Date.now
  },
  description: {
    type: String,
    required: true
  }
});
```
**MongoDB**在定義`Schema`是一個`key`對上一個`value`
`value`裡面需要指定`type`,其他相關的`required`,`default`,`enum`,都是可選的
根據需要加入適當的條件,官網裡有列出所有`validator`及相關的範例這裡只用到了一些

## 補充說明
**可以自訂義不符合時的報錯訊息**
> You can configure the error message for individual validators in your schema. There are two equivalent ways to set the validator error message:

>```Array syntax: min: [6, 'Must be at least 6, got {VALUE}']```
>```Object syntax: enum: { values: ['Coffee', 'Tea'], message: '{VALUE} is not supported' }```

## [官方文檔](https://mongoosejs.com/docs/validation.html)

## Schema 導出的部分
```js
const ExpenseCategory = mongoose.model('ExpenseCategory', expenseCategorySchema);
const IncomeCategory = mongoose.model('IncomeCategory', incomeCategorySchema);
const CashFlow = mongoose.model('CashFlow', cashFlowSchema);

module.exports = { ExpenseCategory, IncomeCategory, CashFlow };
```
>mongoose.model('ExpenseCategory', expenseCategorySchema);
程式碼的意思是建立一個名叫**ExpenseCategory**的`model`,參照**expenseCategorySchema**的`Schema`

# 插入資料

## 語法
>A.insertMany(陣列)
>陣列裡面是Object

## 範例程式：
```js
const makeExpenseCategory = async () => {
  const temp = [
    {
      name: 'Else'
    },
    {
      name: 'Rent'
    },
    {
      name: 'Food'
    },
    {
      name: 'Drink'
    },
    {
      name: 'Bill'
    }
  ]
  await connectDB()
  ExpenseCategory.insertMany(temp)
    .then((p) => { console.log(p) })
    .catch(e => console.log(e))
}

makeExpenseCategory()
```
這邊先連接了**Database**後再進行插入
如果沒有`await`會有高機率報錯,因為**Database**還沒連接進行插入

## 執行結果
```
Connection OPEN!!!
[
  {
    name: 'Else',
    _id: new ObjectId('67861bba58428e2f0bba12e7'),
    __v: 0
  },
  {
    name: 'Rent',
    _id: new ObjectId('67861bba58428e2f0bba12e8'),
    __v: 0
  },
  {
    name: 'Food',
    _id: new ObjectId('67861bba58428e2f0bba12e9'),
    __v: 0
  },
  {
    name: 'Drink',
    _id: new ObjectId('67861bba58428e2f0bba12ea'),
    __v: 0
  },
  {
    name: 'Bill',
    _id: new ObjectId('67861bba58428e2f0bba12eb'),
    __v: 0
  }
]
```
## [官方文檔](https://mongoosejs.com/docs/api/model.html#Model.insertMany())

# 刪除資料

## 語法
>await Character.deleteMany({ name: /Stark/, age: { $gte: 18 } }); 
>// returns {deletedCount: x} where x is the number of documents deleted.

## 範例程式碼：
```js
const deleteExpenseCategory = async () => {
  await connectDB()
  await ExpenseCategory.deleteOne({ name: 'Food' })
    .then(p => { console.log(p)})
  await ExpenseCategory.find()
    .then(p => console.log(p))
}
deleteExpenseCategory()
```
## 範例輸出
```
{ acknowledged: true, deletedCount: 1 }
[  
  {
    _id: new ObjectId('67861bba58428e2f0bba12e7'),
    name: 'Else',
    __v: 0
  },
  {
    _id: new ObjectId('67861bba58428e2f0bba12e8'),
    name: 'Rent',
    __v: 0
  },
  {
    _id: new ObjectId('67861bba58428e2f0bba12ea'),
    name: 'Drink',
    __v: 0
  },
  {
    _id: new ObjectId('67861bba58428e2f0bba12eb'),
    name: 'Bill',
    __v: 0
  }
]
```
## [官方文檔](https://mongoosejs.com/docs/api/model.html#Model.deleteMany())

# 修改資料

## 語法
```js
A.findOneAndUpdate(conditions, update, options)  // returns Query
A.findOneAndUpdate(conditions, update)           // returns Query
A.findOneAndUpdate()                             // returns Query
```

## 範例程式碼：
```js
const updateExpenseCategory = async () => {
  await connectDB()
  await ExpenseCategory.findOneAndUpdate({ name: 'Bill' }, { name: 'Utility' })
    .then(p => { console.log(p) })
  await ExpenseCategory.find()
    .then(p => console.log(p))
}
updateExpenseCategory()
```
## 範例輸出
```
Connection OPEN!!!
{ _id: new ObjectId('67861bba58428e2f0bba12eb'), name: 'Bill', __v: 0 }
[
  {
    _id: new ObjectId('67861bba58428e2f0bba12e7'),
    name: 'Else',
    __v: 0
  },
  {
    _id: new ObjectId('67861bba58428e2f0bba12e8'),
    name: 'Rent',
    __v: 0
  },
  {
    _id: new ObjectId('67861bba58428e2f0bba12ea'),
    name: 'Drink',
    __v: 0
  },
  {
    _id: new ObjectId('67861bba58428e2f0bba12eb'),
    name: 'Utility',
    __v: 0
  }
]
```

最後還有個參數`Options`這邊沒有多做介紹有興趣可以去[官方文檔](https://mongoosejs.com/docs/api/query.html#Query.prototype.setOptions())查看

# 資料關聯與查詢

注意到我**CashFlowSchema**的**category** `type` 是
```js
category: {
  type: mongoose.Schema.Types.ObjectId,
  required: true,
  ref: function () {
    return this.type.toString() === 'expense' ? 'ExpenseCategory' : 'IncomeCategory';
  }
}
```
`ref` 的意思就是參照哪一個`collection` 
這邊注意到我寫了一個`function`來控制要選擇哪一個`collection`
這邊不能用`arrow function`,`arrow function`不會指定`this` 而是繼承外部的`this`
所以用`function`的`this`會給定當前資料的`this`這樣才能動態選定要哪一個`collection`

## 範例程式：
```js
const makeCashFlow = async () => {
  try {
    await connectDB()
    const Food = await ExpenseCategory.findOne({ name: 'Food' })
    const Rent = await ExpenseCategory.findOne({ name: 'Rent' })
    const Drink = await ExpenseCategory.findOne({ name: 'Drink' })
    const Salary = await IncomeCategory.findOne({ name: 'Salary' })
    const Allowance = await IncomeCategory.findOne({ name: 'Allowance'})
    const WindFall = await IncomeCategory.findOne({ name: 'WindFall' })
    // 建立範例 CashFlow 資料
    const cashFlows = [
      // 支出範例
      {
        amount: 500,
        type: 'expense',
        category: Utility,
        date: new Date('2025-01-01'),
        description: 'Month utility'
      },
      {
        amount: 1000,
        type: 'expense',
        category: Rent,
        date: new Date('2025-01-05'),
        description: 'Monthly rent payment'
      },
      {
        amount: 200,
        type: 'expense',
        category: Drink,
        date: new Date('2025-01-10'),
        description: 'Coffee and snacks'
      },
      // 收入範例
      {
        amount: 5000,
        type: 'income',
        category: Salary,
        date: new Date('2025-01-15'),
        description: 'January salary'
      },
      {
        amount: 300,
        type: 'income',
        category: Allowance,
        date: new Date('2025-01-20'),
        description: 'Weekly allowance'
      },
      {
        amount: 800,
        type: 'income',
        category: WindFall,
        description: 'Lottery prize'
      }//沒有給予date 自動指定當前時間
    ];

    // 插入資料到資料庫
    CashFlow.insertMany(cashFlows)
      .then((p) => { console.log(p) })
      .catch(e => console.log(e))
  } catch (error) {
    console.error(error);
  }
};

makeCashFlow()
```

## 範例輸出
```
Connection OPEN!!!
[
  {
    amount: 500,
    type: 'expense',
    category: new ObjectId('67861bba58428e2f0bba12eb'),
    date: 2025-01-01T00:00:00.000Z,
    description: 'Month utility',
    _id: new ObjectId('67867473d6935a7b89fd3c1e'),     
    __v: 0
  },
  {
    amount: 1000,
    type: 'expense',
    category: new ObjectId('67861bba58428e2f0bba12e8'),
    date: 2025-01-05T00:00:00.000Z,
    description: 'Monthly rent payment',
    _id: new ObjectId('67867473d6935a7b89fd3c1f'),
    __v: 0
  },
  {
    amount: 200,
    type: 'expense',
    category: new ObjectId('67861bba58428e2f0bba12ea'),
    date: 2025-01-10T00:00:00.000Z,
    description: 'Coffee and snacks',
    _id: new ObjectId('67867473d6935a7b89fd3c20'),
    __v: 0
  },
  {
    amount: 5000,
    type: 'income',
    category: new ObjectId('6785ec3ca2ddc9c34522ceca'),
    date: 2025-01-15T00:00:00.000Z,
    description: 'January salary',
    _id: new ObjectId('67867473d6935a7b89fd3c21'),
    __v: 0
  },
  {
    amount: 300,
    type: 'income',
    category: new ObjectId('6785ec3ca2ddc9c34522cecb'),
    date: 2025-01-20T00:00:00.000Z,
    description: 'Weekly allowance',
    _id: new ObjectId('67867473d6935a7b89fd3c22'),
    __v: 0
  },
  {
    amount: 800,
    type: 'income',
    category: new ObjectId('6785ec3ca2ddc9c34522cec9'),
    description: 'Lottery prize',
    _id: new ObjectId('67867473d6935a7b89fd3c23'),
    date: 2025-01-14T14:28:03.474Z,
    __v: 0
  }
]
```

這邊介紹`.populate`的用法
可以看到上面的範例輸出**category**後面的`value`是一個`ObjectId`
我們希望找到對應的**category**裡面的資料,這裡就會用到`.populate`

## 範例程式：
```js
const findCashFlow = async () => {
  await connectDB()
  const cashFlows = await CashFlow.find().populate('category','name')
  console.log(cashFlows)

  await CashFlow.find().populate('category').then(e => {
    e.forEach(p => {
      console.log(p.category.name)
    })
  })
}
findCashFlow()
```

## 範例輸出
```
Connection OPEN!!!
[
  {
    _id: new ObjectId('67867473d6935a7b89fd3c1e'),
    amount: 500,
    type: 'expense',
    category: { _id: new ObjectId('67861bba58428e2f0bba12eb'), name: 'Utility' },
    date: 2025-01-01T00:00:00.000Z,
    description: 'Month utility',
    __v: 0
  },
  {
    _id: new ObjectId('67867473d6935a7b89fd3c1f'),
    amount: 1000,
    type: 'expense',
    category: { _id: new ObjectId('67861bba58428e2f0bba12e8'), name: 'Rent' },
    date: 2025-01-05T00:00:00.000Z,
    description: 'Monthly rent payment',
    __v: 0
  },
  {
    _id: new ObjectId('67867473d6935a7b89fd3c20'),
    amount: 200,
    type: 'expense',
    category: { _id: new ObjectId('67861bba58428e2f0bba12ea'), name: 'Drink' },
    date: 2025-01-10T00:00:00.000Z,
    description: 'Coffee and snacks',
    __v: 0
  },
  {
    _id: new ObjectId('67867473d6935a7b89fd3c21'),
    amount: 5000,
    type: 'income',
    category: { _id: new ObjectId('6785ec3ca2ddc9c34522ceca'), name: 'Salary' },
    date: 2025-01-15T00:00:00.000Z,
    description: 'January salary',
    __v: 0
  },
  {
    _id: new ObjectId('67867473d6935a7b89fd3c22'),
    amount: 300,
    type: 'income',
    category: {
      _id: new ObjectId('6785ec3ca2ddc9c34522cecb'),
      name: 'Allowance'
    },
    date: 2025-01-20T00:00:00.000Z,
    description: 'Weekly allowance',
    __v: 0
  },
  {
    _id: new ObjectId('67867473d6935a7b89fd3c23'),
    amount: 800,
    type: 'income',
    category: { _id: new ObjectId('6785ec3ca2ddc9c34522cec9'), name: 'WindFall' },
    description: 'Lottery prize',
    date: 2025-01-14T14:28:03.474Z,
    __v: 0
  }
]
Utility
Rent
Drink
Salary
Allowance
WindFall
```

`.populate`還有很多有用的功能
詳細用法可以到[官方文檔](https://mongoosejs.com/docs/populate.html)查看