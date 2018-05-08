# iterator

iterator是统一的接口机制，用于处理array，object，set，map的数据结构

### Symbol.iterator最简单的实现是结合generator函数

```javascript
    const obj = {
        *[Symbol.iterator](){
            yield 1
            yield 2
            yield 3
        }
    }
    for(let item of obj){
        console.log(item)
    }
```

# generator函数

generator像一个分段执行的函数，通过指针来控制其执行位置，调用next执行，yield停止

```javascript
    function *generator(){
        console.log(1)
        yield 'a'
        console.log(2)
        yield 'b'
        console.log(3)
        return 'c'
    }

    const g = generator()
    g.next() // 1 {value: "a", done: false}
    g.next() // 2 {value: "b", done: false}
    g.next() // 3 {value: "c", done: true}
    g.next() // {value: undefined, done: true}
```

#### yield只能用在generator函数内，在其他地方或普通函数都会报错

```javascript
    const tree = [1,[2,3],[22,2,1,3],89]

    function *cons(arg){
        arg.forEach(item=>{
            if(typeof item !== 'number'){
                yield* cons(item)
            }else{
                yield item
            }
        })
    }

    for(let item of cons(tree)){
        console.log(item);
    }
    //上面的函数也会报错，因为forEach参数是一个普通函数
    //应该改成for
    const tree = [1,[2,3],[22,2,1,3],89]
    function *cons(arg){
        for(let i = 0; i < arg.length; i++){
            let item = arg[i]
            if(typeof item !== 'number'){
                yield* cons(item)
            }else{
                yield item
            }
        }
    }
    for(let item of cons(tree)){
        console.log(item)
    }
```

#### yield用于表达式中需要放在`()`中

```javascript
    console.log('111'+yield 'gaga')  //报错
    const a = 2 * yield 1  //报错
    //应该改成
    console.log('222'+(yield 'gaga'))
    const a = 2 * (yield 1)
    //函数传参或放在赋值表达式的右边不用使用()
    foo(yield 'a', yield 'b'); // OK
    let input = yield; // OK
```

#### next传参

```javascript
    function *go(){
        const x = 2*(yield 3)
    }
    const g = go()
    g.next()
    g.next(1)
    /*
    g.next(1)相当于
    const x = 2*1
    */
```

```javascript
    function *go(){
        console.log('start')
        const next1 = 2*(yield 'one')
        console.log(`传入的参数next1是：${next1}`)
        const next2 = yield 'two'
        console.log(`传入的参数next2是：${next2}`)
    }
    const g = go();
    g.next(1)
    // start {value: "one", done: false}
    g.next(2)
    // 传入的参数next1是：4 {value: "two", done: false}
    g.next(3)
    // 传入的参数next2是：3 {value: undefined, done: true}

    //这里要注意，第二次传入参数才是赋值给yield的值
```

#### Generator.prototype.throw

throw用于内部的try和catch捕获

```javascript
    function *test(){
        try{
            const x = yield 2
        }catch(e){

        }
    }
    const t = test()
    t.next()
    t.throw()
    /*
    t.throw()相当与
    const x = throw new Error()
    */
```

```javascript
    function *test(){
        yield console.log('begin')
        try{
            yield console.log('yield 1')
        }catch(e){
            console.log('error')
        }
        yield console.log('yield 2')
    }

    const t = test()
    
    //一开始我们先不用throw，因为还没try和catch捕获，如果直接throw，就直接报错了
    t.next()  // begin {value:undefine,done:false}
    //我们现在还不能throw，因为抛出错误后，只执行到yield console.log('yield 1')这就停止了，所以错误只能抛到外面来了
    t.next()  // yield 1 {value:undefine,done:false}
    //现在我们可以抛出错误了
    t.throw() // error yield 2 {vlaue:undefine,done:false}   为什么也输出yield 2呢，是因为throw会默认执行一直next()
    t.next() // {value:undefine,done:true}
```

#### Generator.prototype.return

返回一个值，并且终结generator函数

```javascript
    function *line(){
        const x = yield 1
        yield 2
    }
    const l = line()
    l.next()
    l.return(3)
    /*
    l.return(3)  相当于
    const x = return 3;
    */
```

```javascript
    function *line(){
        yield 1
        yield 2
        yield 3
    }
    const l = line()
    l.next() //{value:1,done:false}
    l.return(4) //{value:4,done:false}
    l.next() //{value:undefine,done:true}

```

#### yield*

```javascript
    function *one(){
        yield 1
        yield 2
        return 'haha'
    }
    function *run(){
        yield 0
        const x = yield* one()
        console.log(x)
        yield* 'hello'
    }
    [...run()]
    //haha [0, 1, 2, "h", "e", "l", "l", "o"]
```

* 用途

```javascript
    function *jiegou(arg){
        for(let i = 0;i < arg.length;i++){
            if(Array.isArray(arg[i])){
                yield* jiegou(arg[i])
            }else{
                yield arg[i]
            }
        }
    }
    const arr = [1,[2,3,[4,5],6,7],8]
    const jg = jiegou(arr)
    [...jg]

```

#### Generator用途

> 异步处理

> 状态管理

```javascript
    function *flag(){
        while(true){
            console.log(true)
            yield 'yes'
            console.log(false)
            yield 'no'
        }
    }
    const f = flag()
    f.next()  //状态yes
    f.next()  //状态no

```

> 部署Iterator接口

> 作为数据结构

```javascript
    function* doStuff() {
        yield fs.readFile.bind(null, 'hello.txt');
        yield fs.readFile.bind(null, 'world.txt');
        yield fs.readFile.bind(null, 'and-such.txt');
    }
    for(let x of doStuff()){
        //类似批量异步处理
    }
```

#### Generator函数异步使用

```javascript
    function *async(){
        const url = "/api/query"
        const res = yield fetch(url)
        console.log(res)
    }
    const run = async()
    const result = run.next()
    result.value.then(res=>{
        run.next(res)
    })

    //非常不方便，需要自己控制执行第一段和第二段
```

我们可以使用co模块来自动执行

使用 co 的前提条件是，Generator 函数的yield命令后面，只能是 Thunk 函数或 Promise 对象

这里我们就不写thunk函数了，直接Promise对象

```javascript
    let co = require('co');
    let fs = require('fs');

    function read(fileName) {
        return new Promise((resolve, reject) => {
            fs.readFile(fileName, (err, res) => {
                if (err) return reject(err);
                resolve(res);
            })
        })
    }

    function* gen() {
        let res1 = yield read('co.js');
        console.log(res1);
        let res2 = yield read('tree.js');
        console.log(res2);
    }

    co(gen);
```

因为有async，所以我们就不对co做详情介绍了
