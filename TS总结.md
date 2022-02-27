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
