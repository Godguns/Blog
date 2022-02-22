### compose函数可以将需要嵌套执行的函数平铺，嵌套执行就是一个函数的返回值将作为另一个函数的参数。我们考虑一个简单的需求：   
>给定一个输入值x，先给这个值加10，然后结果乘以10
```javascript   
const calculate = x => (x + 10) * 10;
let res = calculate(10);
console.log(res);    // 200
```   
### 我们可以将复杂的几个步骤拆成几个简单的可复用的简单步骤，于是我们拆出了一个加法函数和一个乘法函数:   
```javascript   
const add = x => x + 10;
const multiply = x => x * 10;

// 我们的计算改为两个函数的嵌套计算，add函数的返回值作为multiply函数的参数
let res = multiply(add(10));
console.log(res);    // 结果还是200
```   
而我们compose的作用就是将嵌套执行的方法作为参数平铺，嵌套执行的时候，里面的方法也就是右边的方法最开始执行，然后往左边返回，   
我们的compose方法也是从右边的参数开始执行，所以我们的目标就很明确了，我们需要一个像这样的compose方法：      
```javascript   
// 参数从右往左执行，所以multiply在前，add在后
let res = compose(multiply, add)(10);
```   
const compose = function(){
  // 将接收的参数存到一个数组， args == [multiply, add]
  const args = [].slice.apply(arguments);
  return function(x) {
    return args.reduceRight((res, cb) => cb(res), x);
  }
}

// 我们来验证下这个方法
let calculate = compose(multiply, add);
let res = calculate(10);
console.log(res);    // 结果还是200
```   

