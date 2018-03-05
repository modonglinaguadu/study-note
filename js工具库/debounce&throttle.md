# debounce和throttle的区别


-----------------------

## debounce(去抖动)

debounce就是设定一个时间，如果在这个时间内，事件一直触发，那指定的cbk就不执行，直到在这个时间内事件不再触发才去执行。

在我们日常中用到的案例有：

输入框，如果用户一直在输入，我们就不执行某任务，直到用户在500毫秒内不再输入，我们才去执行




> 代码简单实现

我们根据上面的特性，我们可以设计如下，设置一个定时器，如果在这个定时器的时间内事件触发，我们就重置这个定时器，如果没有事件触发，那定时器就触发

```javascript

    function debounce(cbk,time){
        let timer = '';
        return function(...args){
            clearTimeout(timer);
            timer = setTimeout(()=>{
                cbk.apply(this,args)
            },time)
        }
    }

```


------------------------

## throttle(节流)

throttle就是控制流量，在一点时间内，不管时间触发几次，指定的cbk就执行一次。

在我们日常中用到的案例有：

拖动窗口，因为鼠标的移动，窗口也要跟着移动，如果不做任何处理，那cbk会执行很多次，这大大影响性能。所以我们控制一下，在20毫秒内，不管鼠标怎么移动，都只执行一次。



> 代码简单实现

我们根据刚才的特性，我们设计如下，我们记录每次执行的时间，然后每次触发，我们都比较和上次执行的时间差，如果超过我们设定的时间，我们就执行cbk

我们这里分为前置执行和后置执行,也就是在这个时间内先执行，然后在这个时间内我都不执行，知道超过这个时间

* 后置执行

```javascript

    function throttle(cbk,time){
        let runTime = +new Date();
        let nowTime = '';
        return function(...args){
            nowTime = +new Date();
            if(nowTime - runTime >= time){
                cbk.apply(this,args);
                runTime = nowTime;
            }
        }
    }

```

也看到了别人用setTimeout的方式去实现，我觉得挺好的

```javascript

    function throttle(cbk,time){
        let timer = null;
        let firstRun = true;
        return function(..args){
            if(firstRun){
                cbk.apply(this,args)
                return firstRun = false;
            }

            if(timer){
                return false;
            }

            timer = setTimeout(()=>{
                clearTimeout(timer);
                timer = null;
                cbk.apply(this,args)
            },time)
        }
    }

```


* 前置执行

```javascript

    function throttle(cbk,time){
        let runTime = '';
        let nowTime = '';
        let run = true;
        return function(...args){
            if(run){
                cbk.apply(this,args);
                runTime = +new Date();
                run = false;
            }
            nowTime = +new Date();
            if(nowTime - runTime > time){
                run = true;
            }
        }
    }

```