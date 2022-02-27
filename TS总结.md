### Any 和unknown   
> any 绕过类型检查，因此可以访问任意方法和属性，并且可自由转换为其他任意类型。

```javascript   
any 绕过类型检查，因此可以访问任意方法和属性，并且可自由转换为其他任意类型。

```   
> unknown 标识一个元素的类型未知，不可以调用任何属性或方法，也不可以转换为其他任意类型   
```javascript   
var b:unknown = "123";
b.length; // 错误
// 出错，无法将 unknown 赋值给 string，尽管其值确实是一个string
let c:string = b;
// 但是可以通过 as 转换一下
let c:string = b as string;
```
### never   
> never ，永不存在的值的类型，是 typescript 2.0 中引入的新类型，那什么是永不存在的类型，我们知道变量一旦声明，都会默认初始化为 undefined ，也不是永不存在的值，但其实有一些场景，值会永不存在，例如，那些总是会抛出异常或函数中执行无限循环的代码（死循环）的函数返回值类型

```javascript   
// 抛出异常
function error(msg: string): never {
    throw new Error(msg);
} // 抛出异常会直接中断程序运行，这样程序就运行不到返回值那一步了，即具有不可达的终点，也就永不存在返回了
 
// 死循环
function loopForever(): never {
    while (true) {};
} //同样程序永远无法运行到函数返回值那一步，即永不存在返回
```   
