# Javascript 资源

标签（空格分隔）： Javascript

---

## 类库

- lodash：是一个通过 Lodash 限制操作频率的函数。
- axios: Ajax 库。

## 代码片段

### 查看对象类型

使用`Object.prototype.toString`方法，通过第二个参数返回的构造函数判断对象类型

```javascript
Object.prototype.toString.call(2); // "[object Number]"
Object.prototype.toString.call(""); // "[object String]"
Object.prototype.toString.call(true); // "[object Boolean]"
Object.prototype.toString.call(undefined); // "[object Undefined]"
Object.prototype.toString.call(null); // "[object Null]"
Object.prototype.toString.call(Math); // "[object Math]"
Object.prototype.toString.call({}); // "[object Object]"
Object.prototype.toString.call([]); // "[object Array]"
```

### JavaScript 对象的元属性

```javascript
{
  value: 123,
  writable: false,
  enumerable: true,
  configurable: false,
  get: undefined,
  set: undefined
}
```

### 将类似于数组的对象转换为数组

`slice()`方法的一个重要应用，是将类似数组的对象转为真正的数组。

```javascript
Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 })
// ['a', 'b']

Array.prototype.slice.call(document.querySelectorAll("div"));
Array.prototype.slice.call(arguments);
```
