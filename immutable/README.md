# immutable
Immutable: 不可变对象，一旦创建不可更改。immutable 可以基于共享部分对象来创建新的对象 :可以理解为两个对象，相同的地方引用的都是同一部分，是相同的。不同的地方是不同的。

### 三大特性（原理）：
1. 持久化数据结构：使用旧数据创建新数据时，要保证旧数据同时可用且不变
2. 结构共享（字典树）：如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享
3. 惰性操作：取到需要的值之后就不再操作了

### 优点：
1. 降低mutable带来的复杂度（mutable指的是频繁的操作数据都是在原对象的基础上进行修改，不会创建新对象，可以有效的利用内存。会造成数据修改过程中的不可控性）
2. 节省内存
3. 历史追溯性（时间旅行）：每时每刻的值都被保存了，想回退到哪一步只要简单的将数据取出就行。
4. 拥抱函数式编程：输入一致，输出必然一致

### 注意：
1. fromJs和toJs会深度转换数据，尽量少用，单层数据转换使用Map（）和List（）
2. Map类型的key必须是string
3. 每次针对immutable变量的增删改左边必须要有赋值，因为所有操作都不会改变原来的值，只是生成一个新的变量
4. 在smart组件中使用immutable.js对象，但是永远不要在dumb组件中使用
5. 绝对不要在 mapStateToProps 中使用 toJS()，此时每次都会返回一个新对象，会导致不必要的重新渲染


# 常用api
```python
//Map()  原生object转Map对象 (只会转换第一层，注意和fromJS区别)
immutable.Map({name:'danny', age:18})

//List()  原生array转List对象 (只会转换第一层，注意和fromJS区别)
immutable.List([1,2,3,4,5])

//fromJS()   原生js转immutable对象  (深度转换，会将内部嵌套的对象和数组全部转成immutable)
immutable.fromJS([1,2,3,4,5])    //将原生array  --> List
immutable.fromJS({name:'danny', age:18})   //将原生object  --> Map

//toJS()  immutable对象转原生js  (深度转换，会将内部嵌套的Map和List全部转换成原生js)
immutableData.toJS();

//查看List或者map大小  
immutableData.size  或者 immutableData.count()

// is()   判断两个immutable对象是否相等
immutable.is(imA, imB);

//merge()  对象合并
var imA = immutable.fromJS({a:1,b:2});
var imA = immutable.fromJS({c:3});
var imC = imA.merge(imB);
console.log(imC.toJS())  //{a:1,b:2,c:3}

//增删改查（所有操作都会返回新的值，不会修改原来值）
var immutableData = immutable.fromJS({
    a:1,
    b:2，
    c:{
        d:3
    }
});
var data1 = immutableData.get('a') //  data1 = 1  
var data2 = immutableData.getIn(['c', 'd']) // data2 = 3   getIn用于深层结构访问
var data3 = immutableData.set('a' , 2);   // data3中的 a = 2
var data4 = immutableData.setIn(['c', 'd'], 4);   //data4中的 d = 4
var data5 = immutableData.update('a',function(x){return x+4})   //data5中的 a = 5
var data6 = immutableData.updateIn(['c', 'd'],function(x){return x+4})   //data6中的 d = 7
var data7 = immutableData.delete('a')   //data7中的 a 不存在
var data8 = immutableData.deleteIn(['c', 'd'])   //data8中的 d 不存在
```
