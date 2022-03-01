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
