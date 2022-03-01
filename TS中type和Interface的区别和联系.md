### 相同点   都可以描述一个对象或者函数   
##### interface   
```javascript   
interface User {
  name: string
  age: number
}

interface SetUser {
  (name: string, age: number): void;
}
```   
##### type   
```javascript   
type User = {
  name: string
  age: number
};

type SetUser = (name: string, age: number)=> void;
```   
### interface 和 type 都可以拓展，并且两者并不是相互独立的，也就是说 interface 可以 extends type, type 也可以 extends interface 。 虽然效果差不多，但是两者语法不同。   
##### interface extends interface      
```javascript   
interface Name { 
  name: string; 
}
interface User extends Name { 
  age: number; 
}
```   
##### type merge
```javascript   
type Name = { 
  name: string; 
}
type User = Name & { age: number  };
```   
### 不同点   
>ype 可以声明基本类型别名，联合类型，元组等类型   
>interface 能够声明合并. 
>type 还可以定义字符串字面量类型，type x = 'a' | 'b' | 'c' 那么使用该类型只能从这三个值中取，不在的就会报错。2、另外使用type比较多的地方就是联合类型，如函数返回类型是type x = string | object | void，就不用一次次的写，复用性就高了。
```javascript   
interface User {
  name: string
  age: number
}

interface User {
  sex: string
}

/*
User 接口为 {
  name: string
  age: number
  sex: string 
}
*/
```
