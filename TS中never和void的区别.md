### void 类型表示没有任何类型   
```javascript   
function warnUser(): void {
    console.log("This is my warning message");
}
```    
> 申明为 void 类型的变量，只能赋予 undefined 和 null   
### never 类型表示永远不会有值的一种类型   
never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型   
```javascript    
// 返回never的函数必须存在无法达到的终点

// 因为总是抛出异常，所以 error 将不会有返回值
function error(message: string): never {
    throw new Error(message);
}

// 因为存在死循环，所以 infiniteLoop 将不会有返回值
function infiniteLoop(): never {
    while (true) {
    }
}
```   
```javascript   
const foo = 123;
if (foo !== 123) {
    const bar = foo;    // bar: never
}
```   
### never 类型的一个应用场景   
```javascript   
// ../../type.ts
enum Letter {
    A,
    B,
    C,
}

// 假如我们有如下一个通用型的业务逻辑
// ../../business.ts
switch (letter) {
    case Letter.A:
        // ...
        break;
    case Letter.B:
        // ...
        break;
    case Letter.C:
        // ...
        break;
    default:
        const check: never = customType;
         
        // ...
        break;
}

}
```      
> 由于任何类型都不能赋值给 never 类型的变量，所以当存在进入 default 分支的可能性时，TS的类型检查会及时帮我们发现这个问题      
### never 和 void 的差异   
void 表示没有任何类型，never 表示永远不存在的值的类型。   
```javascript   
// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}

// 这个函数不能申明其返回值类型是 never
function warnUser(): void {
    console.log("This is my warning message");
}
```

