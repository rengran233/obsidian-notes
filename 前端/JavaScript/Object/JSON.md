## 什么是JSON

JSON stands for JavaScript Object Notation.

```js
// JSON文件
{
  "name": "Alice",
  "age": 30,
  "isStudent": false,
  "list of courses": ["Mathematics", "Physics", "Computer Science"]
}

// 如何访问
import data from "./example.json" with { type: "json" };
console.log(data.age);
console.log(data["list of courses"]); // 这里必须用括号访问
```


## JS中的JSON处理

JavaScript中有两种强大的方法用于处理JSON数据：`JSON.parse()` and `JSON.stringify()`。

