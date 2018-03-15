# Set

## 基础

```javascript
    const set = new Set([1,2,3,1])
    // [1,2,3]
```

1. 四个操作方法

* .add()
* .delete()
* .has()
* .clear()

个数

* .size()

2. WeakSet

* 只能存放对象
* 因为是弱引用，即不计入垃圾回收机制，不适合用于引用，只能临时存放对象，如果对象不在被其他地方引用，垃圾回收了这个对象，那weakSet中对象也会消失。所以weakSet不能遍历

weakSet的操作方法有.add() .delete() .has() 没有.size()

```javascript   
    const weakSet = new WeakSet();
    weakSet.add({a:1})
```

# Map

## 基础

```javascript
    const items = [
        ['name':'haha'],
        ['age':11]
    ]
    const map = new Map(items)

    //等价于
    const map = new Map()
    items.forEach([key,value]=>
        map.set(key,value)
    )

    map.get('age') //11
```

1. 方法

* .set(key,value)
* .get(key)
* .size()
* .has(key)
* .delete(key)
* .clear()

2. 遍历

* .keys() 返回键名的遍历
* .values() 返回键值的遍历
* .entries() 返回键值对的遍历

3. weakMap

WeakMap的键值名只接受对象

```javascript
    const obj = {}
    const weakMap = new WeakMap()
    weakMap.set(obj,'haha')
```