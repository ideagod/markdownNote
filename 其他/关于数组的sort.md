## Array所有属性与方法（分类）

##### Object.assign(target,...source);

方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象

！注：如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign({},target, source);
console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }

const returnedTarget = Object.assign(target, source);
console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }
console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

## 总结

##### 迭代 （item,index,array)

some	boolean

every	boolean



filter	新数组

map     新数组

foreach     原来数组有循环

##### 归并

reduce	单个返回值

reduceRight

## 静态方法

##### Array.from()    生成数组

```js
Array.from(arrayLike[, mapFn[, thisArg]])
```

```js
			Array.from('mary');//Array ["m", "a", "r", "y"]
console.log(Array.from([1, 2, 3], x => x + x));
// expected output: Array [2, 4, 6]

```

 // ES6去重

```js
            const set = new Set([1, 2, 3, 4, 1, 1, 1]);
            console.log(set);//变成了对象
            console.log(Array.from(set));//[1, 2, 3, 4]
```

```js
  			 const map = new Map([[1, 2], [2, 4], [4, 8]]);
             console.log(map);
             console.log( Array.from(map)) ;
```

##### Array.isArray()   用于确定传递的值是否是一个 Array。

```js
Array.isArray(obj)
```

```js
			let arr = new Array('a', 'b', 'c')
            console.log(arr);
			console.log(arr.isArray());
```

##### Array.of()      方法创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。

```js
Array.of(element0[, element1[, ...[, elementN]]])
```

```js
 			console.log(Array.of(7))
            console.log(Array(7))
```

## 实例方法（以下方法都在Array.prototype)

### 一  遍历

##### forEach()

```js
arr.forEach(callback(currentValue [, index [, array]])[, thisArg])
```

callback

为数组中每个元素执行的函数，该函数接收一至三个参数：

- `currentValue`

  数组中正在处理的当前元素。

- `index` 可选

  数组中正在处理的当前元素的索引。

- `array` 可选

  `forEach()` 方法正在操作的数组。

- `thisArg` 可选

可选参数。当执行回调函数 `callback` 时，用作 `this` 的值。

```js
			const arr = ['a', 'b', 'c'];
            for (let i = 0; i < arr.length; i++) {
                console.log(arr[i]);
            }
            arr.forEach((item, index) =>
                console.log(item, index)
            );
//两种都相等
```

##### reduce()    	方法对数组中的每个元素执行一个由您提供的reducer函数(无顺序)，将其结果汇总为单个返回值。

```js
				var total = [2, 1, 4, 0].reduce(
                    (acc, cur) => acc + cur,
                    7
                );
                console.log(total);
```

##### some() 		方法测试数组中是不是至少有1个元素通过了被提供的函数测试。返回boolean

```js
arr.some(callback(element[, index[, array]])[, thisArg])
```

参数

- `callback`

  用来测试每个元素的函数，接受三个参数：`element`数组中正在处理的元素。`index` 可选数组中正在处理的元素的索引值。`array`可选`some()`被调用的数组。

- `thisArg`可选

  执行 `callback` 时使用的 `this` 值。

返回值

数组中有至少一个元素通过回调函数的测试就会返回**`true`**；所有元素都没有通过回调函数的测试返回值才会为false。

```js
 			let array = [1, 2, 3, 4, 5, 6];
 			console.log(array.some((element) => element % 2 === 0));		//true
			 let arr1 = [1, 12, 12, 4, 1, 2, 5, 2];
			 console.log(arr1.some((element) => element > 10));				//true
```

​		确认里面是否含有某个元素

```js
           likeIncludes(val) {
                let fruits = ['apple', 'banana', 'mango', 'guava'];
                return fruits.some((element) => val === element);
            }
		   console.log(linkeIncludes('mango'));
```

##### every() 		方法测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值。

```js
arr.every(callback(element[, index[, array]])[, thisArg])
```

```js
  			 let arr1 = [1, 30, 10, 20, 5, 2, 4, 5];
      		 return arr1.every((element) => typeof (element) == Number);
```

### 二  衍生

##### *concat()		 合并，返回新数组

```js
			  const array1 = ['a', 'b', 'c'];
     		  const array2 = ['d', 'e', 'f'];
              const array3 = array1.concat(array2);
              const array4 = ['e', 't', 'v'];

              const array5 = array1.concat(array2, array3, array4);
              return array5;
```

##### *slice()			返回一个新数组对象，由begin和end决定的浅拷贝

```js
 				let arr1 = ['ant', 'bison', 'camel', 'duck', 'elephant'];
                let arr2 = arr1.slice(2, 7);
                arr1.push('dfsaf');

                // 一个函数中的  arguments 就是一个类数组对象的例子。
                function list() {
                    return Array.prototype.slice.call(arguments);
                }
                var list1 = list(1, 2, 3);
                return list1;
```

##### *flat()			返回新数组，深度遍历数组

```js
				 let arr1 = [0, 1, 2, [3, 4]];
				 let arr2 = arr1.flat();
			     return arr2;
```

##### *flatMap()			 压缩成新的数组，通常在合成一种方法的效率高点,替代方案 reduce和concat

```js
  				let arr1 = [1, 2, 3, 4];
 			    arr1.map(x => [x * 2]);
 			    arr1.flatMap(x => [x * 2]);
```

##### *filter() 			方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 

```js
				const arr1 = ['11111', '2', '5453', '7486', '123485'];
                const result = arr1.filter((element) => element.length > 3);

                const arr2 = [123, 456, 123, 7862, 41, 1, 2, 3, 4];
                const result2 = arr2.filter((element) => element > 10);


                const fruits = ['apple', 'banana', 'grapes', 'mango', 'orange'];

                /**
                ES5过滤的实现
                 * Array filters items based on search criteria (query)
                 */
                const filterItems = (query) => {
                    return fruits.filter((el) =>
                        el.toLowerCase().indexOf(query.toLowerCase()) > -1
                    );
                }

                console.log(filterItems('ap')); // ['apple', 'grapes']
                console.log(filterItems('an')); // ['banana', 'mango', 'orange']
             
```

##### *map() 			新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值。

```js
			    const arr1 = [1, 4, 5, 7, 9];
                const map1 = arr1.map(element => element * 2);

                let kvArray = [
                    { key: 1, value: 10 },
                    { key: 2, value: 20 },
                    { key: 3, value: 30 }
                ];

                var kv = kvArray.map(function (obj) {
                    var rObj = {};
                    rObj[obj.key] = obj.value;
                    return rObj;
                })

                // 接口数据映射
                // 从接口得到数据 res:
                let r = res.map(item => ({

                    title: item.name,
                    sex: item.sex === 1 ? '男' : item.sex === 0 ? '女' : '保密',
                    age: item.age,
                    avatar: item.img

                })
                );
```

### 三    查找

##### find()			返回第一个查找到的*值*，否则undefined

```js
			    const arr1 = [5, 8, 12, 30, 400];
                const found = arr1.find((element) => element > 7);
                //用对象的属性查找数组里的对象
                var inventory = [
                    { name: 'apples', quantity: 2 },
                    { name: 'bananas', quantity: 0 },
                    { name: 'cherries', quantity: 5 }
                ];
                function findC(fruit) {
                    return fruit;
                }
                return inventory.find(findC);
```

##### findIndex()			方法返回数组中满足提供的测试函数的第一个元素的*索引*。若没有找到对应元素则返回-1。

```js
  				const array1 = [5, 12, 8, 130, 44];
                const isLargeNumber = array1.findIndex((element) => element > 13);
                return isLargeNumber;
```

##### includes() 			判断数组是否包含一个指定的*值*，boolean。

```js
 				const arr1 = [1, 2, 3, 4, 5];
                return arr1.includes(2);
```

##### IndexOf()			方法返回在数组中可以找到一个给定元素的第一个*索引*，如果不存在，则返回-1。第二个参数是第几个索引开始找，如果-1则是最后一个，-2则是倒数第二个。

```js
   			    const arr1 = [1, 2, 3, 4, 5];
                return arr1.indexOf(2, 5);
```

#####  lastIndexOf() 			方法返回指定元素（也即有效的 JavaScript 值或变量）在数组中的最后一个的索引，如果不存在则返回 -1。从数组的后面向前查找，从 fromIndex 处开始。

arr.lastIndexOf(searchElement[, fromIndex]) fromIndex逆向查找

```

```

### 四    修改

##### +fill() 			 方法用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引。

```js
                const arr1 = [1, 2, 3, 4, 5];
                return arr1.fill('i', 1, 3);		//Array [1, "i", "i", 4, 5]
```

##### +pop()		 	方法从数组中删除最后一个元素，并返回该元素的值。此方法更改数组的长度。

返回该元素的值

```js
				const arr1 = [1, 2, 3, 4, 5];
                arr1.pop();
                return arr1;
```

##### +push() 			方法将一个或多个元素添加到数组的末尾，并返回该数组的新长度。

```js
  			    const arr1 = [1, 2, 3, 4, 5];
                arr1.push(6, 7, 8, 9, 10);
                return arr1;
```

##### +shift() 			方法从数组中删除第一个元素，并返回该元素的值。此方法更改数组的长度。

删除，返回该元素的值

```
			    const arr1 = [1, 2, 3, 4, 5];
                arr1.shift();
                return arr1;    //[2,3,4,5]
```

##### +unshift() 			方法将一个或多个元素添加到数组的开头，并返回该数组的新长度(该方法修改原有数组)。

```js
 			    const arr1 = [1, 2, 3, 4, 5];
                arr1.unshift(0, 0, 0, 0, 0);
                return arr1;        //[0, 0, 0, 0, 0, 1, 2, 3, 4, 5]
```

##### +splice() 方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。此方法会改变原数组。

array.splice(start[, deleteCount[, item1[, item2[, ...]]]])

修改或替换

```js
 			    const arr1 = [1, 2, 3, 4, 5];
                arr1.splice(2, 0, 6, 7, 8);
                return arr1;    //[1, 2, 6, 7, 8, 3, 4, 5]
```

##### +reverse() 			方法将数组中元素的位置颠倒，并返回该数组。

```js
				const arr1 = [1, 2, 3, 4, 5];
                arr1.reverse();
                return arr1;    // [5, 4, 3, 2, 1]
```



##### +sort() 排序

按数字排序

```js
function myFunction2() {
  points.sort(function(a, b){return a - b});
  document.getElementById("demo").innerHTML = points;
}
```

##### +Array.sort(function(a, b){return a - b}) 才可以排数字

当比较 40 和 100 时，sort() 方法会调用比较函数 function(40,100)。

该函数计算 40-100，然后返回 -60（负值）。

排序函数将把 40 排序为比 100 更低的值。



##### copyWithin() 方法浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度。

arr.copyWithin(target[, start[, end]])

```js

```



### 转字符串

##### toString()方法	不可以传东西 拼接

##### join() 方法将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串。如果数组只有一个项目，那么将返回该项目而不使用分隔符。可以传东西	拼接

```js
   				const elements = [1, 2, 3, 4, 5];
                log(elements.join());
                // expected output: "Fire,Air,Water"
                log(elements.join(''));
                // expected output: "FireAirWater"
                log(elements.join('-'));
                // expected output: "Fire-Air-Water"
```

##### split() 字符串变数组  解构

split() 方法用于把一个字符串分割成字符串数组。

```js
var str="How are you doing today?"

document.write(str.split(" ") + "<br />")	//How,are,you,doing,today?
document.write(str.split("") + "<br />")//H,o,w, ,a,r,e, ,y,o,u, ,d,o,i,n,g, ,t,o,d,a,y,?
document.write(str.split(" ",3))	//How,are,you
```

