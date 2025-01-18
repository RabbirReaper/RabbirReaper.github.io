---
title: Express 中的 Middleware：語法筆記
date: 2025-01-19 03:56:06
tags: node.js
categories:
  - [程式語言筆記,node.js]
description: 這是一篇介紹Express裡Middleware的功能。
---

# 甚麼是`Middleware`
```js
app.get('/', (req, res) => {
  res.send("Hello world")
})
```
```js
app.use(function (req, res, next) {
  console.log('Time:', Date.now());
  next();
});
```
```js
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

# `Middleware`可以做什麼
> 執行任何程式碼。
> 對要求和回應物件進行變更。
> 結束要求/回應循環。
> 呼叫堆疊中的下一個中介軟體函數。

# `Middleware` 語法介紹

## 參數不同的`Middleware`介紹

### 兩個參數的`Middleware`
當預設`/`，回傳`Hello Woeld!`
```js
var express = require('express');
var app = express();

app.get('/',(req, res) => {
  res.send('Hello World!');
});

app.listen(3000);
```

### 三個參數的`Middleware`
不一定要指定路徑，沒有指定代表都會觸發
每個參數的位置是固定的，雖然名字可以亂取但不會因為`req`的位置改叫`res`就變`res`的功能
如果`res`沒有使用，必須使用`next()`到下一個`middleware` 不然會卡在這裡
```js
app.use('/user/:id', logRequest, validateUserId, checkPermissions);

function logRequest(req, res, next) {
  console.log('Request URL:', req.originalUrl);
  console.log('Request Type:', req.method);
  next();
}

function validateUserId(req, res, next) {
  if (!req.params.id) {
    return res.status(400).send('Missing ID');
  }
  next();
}

function checkPermissions(req, res, next) {
  if (req.params.id !== '123') {
    return res.status(403).send('Access Denied');
  }
  console.log('User is valid');
  next();
}

```

### 應用程式層次的`middleware`
`middleware`裡面的`function`沒有限制幾個可以一直跌加，會依序執行
為什麼不要把所有邏輯寫在同一個`middleware`?
因為大量邏輯塞在同一個`function`裡不好維護

```js
app.use('/user/:id', logRequest, validateUserId, checkPermissions);

function logRequest(req, res, next) {
  console.log('Request URL:', req.originalUrl);
  console.log('Request Type:', req.method);
  next();
}

function validateUserId(req, res, next) {
  if (!req.params.id) {
    return res.status(400).send('Missing ID');
  }
  next();
}

function checkPermissions(req, res, next) {
  if (req.params.id !== '123') {
    return res.status(403).send('Access Denied');
  }
  console.log('User is valid');
  next();
}
```

### 錯誤處理`middleware`
> 錯誤處理中介軟體一律會使用**四個引數**。您必須提供這四個引數，將它識別為錯誤處理中介軟體函數。即使您不需要使用 next 物件也必須指定，以維護簽章。否則，會將 `next` 物件解譯為一般中介軟體，而無法處理錯誤。
```js
app.use(function(err, req, res, next) {
  console.error(err.stack);
  console.log('I need next help.')
  next(err)
});
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

## 路由器層次的`middleware`

### 程式碼範例
```js
// userRouter.js
var express = require('express');
var userRouter = express.Router();

userRouter.get('/:id', function (req, res) {
  res.send('User ' + req.params.id);
});
module.exports = userRouter;
```

```js
// productRouter.js
var express = require('express');
var productRouter = express.Router();

productRouter.get('/:id', function (req, res) {
  res.send('Product ' + req.params.id);
});
module.exports = productRouter;
```
```js
// app.js
var express = require('express');
var app = express();
var userRouter = require('./userRouter');
var productRouter = require('./productRouter');

app.use('/user', userRouter); // Mount user routes
app.use('/product', productRouter); // Mount product routes

app.listen(3000);
```

### 為什麼要用`Router`?

因為可以把邏輯切分開，不必把所有東西都寫在同個檔案裡
增加可讀性，可維護性
`router`可以嵌套使用

# 程式範例

## 範例 1 : 密碼驗證系統
```js
const express = require('express')

const verifyPassword = (req, res, next) => {
  const { password } = req.query
  if (password === 'ccc') {
    next()
  } else {
    throw new Error("password Error!");
  }
}

app.get('/products/new', verifyPassword, (req, res) => {
  res.render('products/new')
})

app.use((err, req, res, next) => {
  console.log("***********ERROR*************")
  res.status(500).send(err);
})
```

如果密碼錯誤會丟出`err`，下方是密碼錯誤的報錯
會看到第一行是我們定義的錯誤訊息，後面的內容是在告訴你程式哪邊出了問題
```
Error: passord Error!
    at verifyPassword (D:\Coding_training\aboutWeb\mongoTest\index.js:28:11)
    at Layer.handle [as handle_request] (D:\Coding_training\aboutWeb\mongoTest\node_modules\express\lib\router\layer.js:95:5)
    at next (D:\Coding_training\aboutWeb\mongoTest\node_modules\express\lib\router\route.js:149:13)
    at Route.dispatch (D:\Coding_training\aboutWeb\mongoTest\node_modules\express\lib\router\route.js:119:3)
    at Layer.handle [as handle_request] (D:\Coding_training\aboutWeb\mongoTest\node_modules\express\lib\router\layer.js:95:5)
    at D:\Coding_training\aboutWeb\mongoTest\node_modules\express\lib\router\index.js:284:15
    at Function.process_params (D:\Coding_training\aboutWeb\mongoTest\node_modules\express\lib\router\index.js:346:12)
    at next (D:\Coding_training\aboutWeb\mongoTest\node_modules\express\lib\router\index.js:280:10)
    at D:\Coding_training\aboutWeb\mongoTest\index.js:38:3
    at Layer.handle [as handle_request] (D:\Coding_training\aboutWeb\mongoTest\node_modules\express\lib\router\layer.js:95:5)
```
## 範例 2 : 異步操作的處理方式
```js
app.use(async (req, res, next) => {
  try {
    const data = await fetchData();
    req.data = data;
    next();
  } catch (error) {
    next(error);
  }
});
```

# 常見的`middleware`使用

```js
app.use(express.urlencoded({ extended: true }))
```
> http://localhost:3000/test?key1=value1&key2[subkey]=value2

如果沒有上述的`middleware`，伺服器將無法解析後面的`key1=value1&key2[subkey]=value2`
`req.body` 將會是`undefine`

# 官方文檔
https://expressjs.com/zh-tw/guide/using-middleware.html
