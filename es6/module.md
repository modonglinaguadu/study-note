# module

#### 与commonJs区别

* commonJs是运行时加载，es6的module是编译时加载
* commonJs实质是加载整个模块，生成一个对象，es6模块不是对象，而是export指定输出代码，然后使用import引入

#### export

* export只能在最外层，不能在任何块级作用域中
* import不可以修改export出来的变量，对象可以往对象里面添加值，但不能直接修改

> 变量

```javascript
    export const a = 123
    export const b = 345

    //等价于
    export {
        a,
        b
    }
    //错误的写法
    // export a  相当直接export 123
```

> 函数

```javascript
    export function a(){
        console.log(123)
    }
    export function b(){
        console.log(345)
    }
    //等价于
    export {
        a,
        b
    }
    //也可以重命名
    export {
        a as aa,
        b as bb
    }
```

> 默认输出

上面的输出我们都需要命名，import的时候我们也必须知道其名字才可以import，


#### import

> 局部引入

import会提升到模块头部执行，因为import是编译执行，所以不要使用变量或表达式

```javascript
    //大括号内变量名要和export的相同
    import {a,b} from './test.js'
    //重命名
    import {a as aa,b as bb} from './test.js'
```

报错的情况

```javascript
    const src = './test.js'
    import {a,b} from src //error

    import {'a'+'a'} from './test.js'//error

    //编译执行，没办法做判断
    const flag = true
    if(flag){
        import {a} from './test.js'
    }else{
        import {b} from './test.js'
    }
```

> 整体加载

```javascript
    import * as val from './test.js'
    console.log(val.a,val.b)
```

#### 默认加载

```javascript
    export default function a(){

    }
    export default class A{

    }

    //也可以,这个和之前的不一样，可以理解是把a赋值给default
    export default 123

    function a(){

    }
    export default a

    //不可以
    export default const a = 1 //error

```
