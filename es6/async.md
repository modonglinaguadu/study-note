# async/await

```javascript
    import fs from 'fs'
    function read(url){
        return new Promise((resolve,reject)=>{
            fs.readFile(url,(err,res)=>{
                if(err) reject(err)
                resolve(res)
            })
        })
    }
    async function run(){  
        const res = await read('async.md')
        const res1 = await read('README.md')
        return [res,res1]
    }

    run().then(arr=>{
        console.log(arr)
    })

    //async函数返回是一个promise，await后面如果是异步，需要是Promise

```

* async函数返回是一个Promise
* await后面默认是一个Promise，如果不是Promise，就会转换成Promise.resolve()
* async函数里面报错，在返回的catch可以捕获
* await后面的Promise.reject()会被async返回的catch捕获，而且async函数整体中断，不会执行下面的

可是有时候我们不想因为一个错误导致后面的async函数中止，那可以用try catch捕获或者在await后面的Promise加上catch，例子如下

```javascript
    async function run(){
        try{
            await Promise.reject()
        }catch(e){
            console.log(e)
        }
        //或则
        await Promise.reject().catch((e)=>{
            console.log(e)
        })

        await Promise.resolve()
    }

```

#### 注意事项

* await只能放在async函数里面，和yield一样

```javascript
    async function run(){
        const urls = ['/api1','/api2']
        urls.forEach(item=>{
            await fetch(item)
        })
        //上面的会报错，因为await在普通的函数里面
        //也不能
        urls.forEach(async (item)=>{
            await fetch(item)
        })
        //因为按上面的写法，这几个async函数就是并发执行，而不是继发执行了。所以也错误
    }
```

* 如果异步不是继发关系，最好让他们同步触发

并发我们可以先处理异步，返回的promise再继发处理，这样就可以大大节省时间

```javascript
    async function run(){
        let res = await fetch('/api1')
        let res1 = await fetch('/api2')
        //上面两个是同步执行，即请求完api1才会请求api2，这不影响了速度，应该同时触发

        //同时触发第一种方式
        let p1 = fetch('/api3')
        let p2 = fetch('/api4')
        let res2 = await p1;
        let res3 = await p2;

        //同时触发第二种方式
        let [res4,res5] = await Promise.all([fetch('/api5'),fetch('/api6')])
    }
```



