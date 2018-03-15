# symbol

## 基础知识

1. 互不相等

```javascript
    const a = Symbol('x')
    const b = Symbol('x')
    // a !== b
```
2. Symbol用于对象属性时，因为Symbol()不是字符串，所以需要放在[]内

```javascript
    const a{
        [Symbol()]:11
    }
```

3. 用于对象中的属性不能被for in 或 Object.getOwnPropertyNames遍历得到,Object.getOwnPropertySymbols可以遍历到

```javascript
    const a = {
        a:'1',
        [Symbol('x')]:'2'
    }
    for(x in a){
        console.log(x)
    }
    const res = Object.getOwnPropertyNames(a)

    const res1 = Object.getOwnPropertySymbols(a)
```

4. 相等的Symbol

```javascript
    //Symbol.for为 Symbol 值登记的名字，是全局环境的
    Symbol.for('a') === Symbol.for('a')
    const b = Symbol.for('x');
    Symbol.keyFor(b)  //'x'
```

## 实用举例

#### 消除魔法字符串

```javascript

    //有时候我们在函数封装里面用到类型判断，例如switch这种情况

    function deal(type){
        switch(type){
            case 'a':
                console.log('第一种')
                break;
            case 'b':
                console.log('第二种')
                break;
        }
    }

    deal('a')

    //如果type种类太多，我们没办法记住，我们一般用对象管理
    const types = {
        a:'a',
        b:'b'
    }
    function deal(type){
        switch(type){
            case types.a:
                console.log('第一种')
                break;
            case types.b:
                console.log('第二种')
                break;
        }
    }
    deal(types.a)
    //但是我们发现其type.a或type.b的字符串并不重要，只要他们之间不相等就可以了，所以我们可以改成Symbol
    const types = {
        a:Symbol(),
        b:Symbol()
    }

```


#### 作为对象非私有内部方法

```javascript
    //因为Symbol作为变量时，没办法被for in 或 Object.getOwnPropertyNames遍历到，所以我们可以用于对象非私有的只有内部可用的方法

    const body = Symbol('body')

    class People{
        constructor(){
            this[body] = '身体' 
        }

        run(){
            console.log(`奔跑吧${this[body]}`)
        }
    }

```