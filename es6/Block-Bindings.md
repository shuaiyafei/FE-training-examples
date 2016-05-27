### var 有什么问题？
1.作用域提升：
```javascript
function someFunction(condition) {
  if(condition) {
    var a = 123
    return a
  } else {
    // a在这里也存在
    return null
  }
  // a在这里同样可以访问到
}
```
引擎改变后的代码：
```javascript
function someFunction(condition) {
  var a
  if(condition) {
    a = 123
    return a
  } else {
    // a是undefined
    return null
  }
  // a是undefined
}
```
2.一个经典的循环问题：
```javascript
var data = [1,2,3]
var result = []

for(var i=0; i<data.length; i++) {
  result.push(function plus1() {
    return data[i] + 1
  })
}
```

### let 关键字
```javascript
/* 试图访问未定义的变量会抛出异常: ReferenceError */
function getValue(condition) {

    if (condition) {
        console.log(value)
        let value = "blue"

        console.log(value)
    } else {
        console.log(value)
    }

    console.log(value)
}
```

### 不能重复声明变量
```javascript
var a = 123
let a = 234 // 报错
```
```javascript
var a = 123
if(condition) {
  let a = 234 // 正常运行，在if条件语句的作用域中，我们创建了一个新变量也叫作a，它会在当前作用域中覆盖全局作用域中的a
}
```

### const 关键字
```javascript
const name = 'woota' 
name = 'lijun' // 报错，const关键字声明的变量不能更改它的值

const anotherName // 报错，const关键字声明的变量必须在声明的时候给定初始化值

const obj = { name: 'woota' }
obj.name = 'lijun' // 这条语句是否合法？
obj = { name: 'lijun' } // 这个呢？
```

### const 在 for 与 for in / for of 中的区别
```javascript
for(const i=0; i<9; i++) {
  console.log(i)
}

for(const item of [1,2,3]) {
  console.log(item)
}
```

### 全局环境中的 var, let, const
```javascript
var console = null
window.console.log(123) // 报错，console 是 null
window.console === console // true

let alert = function(msg) {
  return msg
}
alert('hey') // 输出 'hey'
window.alert === alert // false
```

### 总结

1. 尽量使用 const，let 代替 var 进行变量声明；
2. 如果必要，还是可以使用 var。；
3. 一般来说，先考虑使用 const，只有在确定某个变量的值需要变更的情况下才用 let;


