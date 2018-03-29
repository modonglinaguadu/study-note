# class

就是一个语法糖，就是写法和es5不一样，写法更像java了

```javascript
    class Person{
        constructor(){

        }

        method(){

        }
    }
    //上面的方法就是挂载在原型上的
    //等价于下面的
    Person.prototype = {
        constructor(){

        },
        method(){

        }
    }
```

* 用class写的内部方法不可以枚举
* class不会变量提升

#### constructor

new的时候自动执行constructor方法，所以class内必须有constructor方法，如果不手写，会默认添加。
constructor方法默认return this，你可以手动更改。

class构造函数不能直接调用，只能new。

#### 立即执行的类

```javascript

    const man = new class{

        constructor(sex){
            this.sex = sex
        }

        sayName(){
            console.log(this.sex)
        }

    }('man')

    man.sayName()
```

#### 私有变量和方法

可以用Symbol来实现

```javascript
    const x = Symbol('x')
    const fn = Symbol('fn')

    class Ha{
        constructor(){
            this[x] = 123
        }
        run(){
            this[fn]()
        }
        [fn](){
            console.log(this[x])
        }
    }
```

#### class内getter和setter

```javascript
    class A{
        get prop(){
            return '哈哈'
        }
        set prop(val){
            console.log(`设置 ${val} ?`)
        }
    }

    const a = new A()
    a.prop = 123 // 设置 123 ?
    a.prop // 哈哈
```

#### 静态方法

```javascript
    class A{
        static gg(){
            //这里的this指向的是类，不是实例
            this.bz()
        }
        bz(){
            console.log('原型上方法')
        }
        //静态方法可以和原型上方法同名
        static bz(){
            console.log('静态方法')
        }
    }

    //es5我们是这么写的
    A.gg = function(){

    }
    //可是class内没有静态属性，es5是可以有的
    A.prop = 123;
```

#### new.target

new实例后new.target可以获取类

继承关系的化，new.target显示的是子的类

```javascript
    class Parent{
        constructor(){
            console.log(new.target === Parent)
        }
    }

    class Son extends Parent{
        constructor(){
            super()
        }
    }

    new Son() //返回 false

```

可以利用这个特性制作不可实例化的类,就是类似java的抽象类

```javascript
    class Car{
        constructor(){
            if(new.target === Car){
                throw new Error('该类不能实例化')
            }
        }
    }

    class Baoma extends Car{
        constructor(){
            super()
        }
    }

    const car = new Car() //报错
    const baoma = new Baoma() //正确

```

#### 继承

继承使用extends关键字，并且在constructor调用super(),因为es6中的子构造函数没有创建this，是父构造函数创建this，子类继承父类this，并且对之修改，这和es5不一样，

es5是子类创建this，然后把父类方法添加到this中`Parent.apply(this)`

```javascript
    class Son extends Parent {
        constructor(){
            super()
        }
    }
    //super指父构造函数
```

* 父类的静态方法也会被子类继承
* super()当作方法时，只能在子构造函数中调用
* super当作对象时，super指的是父类原型(Parent.prototype)
* 如果super对象用在静态方法中，super则指的是父类，而不是父类原型
* 继承中apply和call是调用构造函数

#### 原生构造函数继承

ECMAScript 的原生构造函数在es5时我们没有创建一个新的对象继承它们，因为没办法获取它们构造函数内部的属性，即使使用apply调用传参，它的构造器也会忽略传入的值。

而使用es6的extends我们可以做得到，因为extends是先创建父类的实例对象this，然后再使用子类的构造函数修饰this，使得父类的所有行为都可以继承。

