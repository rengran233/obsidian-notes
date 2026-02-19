JS中有三种`for`循环
```js
for (let i = 0; i < 10; i += 1) {
	console.log(i);
}
const arr = [{name: "fuck", age: 18,}]

for (const a of arr) {
    for (const p in a) {
        console.log(`The ${p} is ${a[`${p}`]}`);
    }
}
```
`for...of`遍历一个可迭代对象（如`Array`）并获取其中的每一个元素，`for...in`遍历一个可枚举对象（如`Object`）并获取其中每一个属性（键值对中的键）

`forEach()`是一个数组方法，用于遍历数组中的元素并产生副作用（区别于`map()`），接受一个回调函数作为参数，该函数可以拥有三个参数
```js
let numbers = [1, 2, 3, 4, 5];
numbers.forEach((number, index, array) => {
  console.log(`Element ${number} is at index ${index} in array ${array}`);
});
```

`map()`也是一个数组方法，用于遍历操作数组的每一个元素，并返回一个新数组
```js
const numbers = [3, 4, 5, 6, 7].map((element, index, array) => {
  console.log("Element:", element);
  console.log("Index:", index);
  console.log("Array:", array);
  return element * 2;
});
```
`filter()`, `refuce()`, `every()`, `some()`