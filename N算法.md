# N算法

## 数组去重

```js
 class unique {
            first() {
                let arr1 = [1, 1, 1, 2, 3, 74, 4, 5, 6, 7, '1'];
                let arr2 = Array.from(new Set(arr1));
                let arr3 = [...new Set(arr1)];
                return arr3;
            }
            second() {
                let arr1 = [1, 1, 1, 2, 3, 74, 4, 5, 6, 7, '1'];
                let arr2 = arr1.concat().sort();
                var same;
                let arr3 = [];
                for (let i = 0; i < arr2.length; i++) {
                    if (same !== arr2[i]) {
                        arr3.push(arr2[i]);
                    }
                    same = arr2[i];
                }
                return arr3;
            }

        }
        var quchong = new unique();
```

```js
		function unique(arr){
			let a=[];
			for(var i=0;i<arr.length;i++){
				if(a.indexOf(arr[i])==-1)
					a.push(arr[i])
			}
			return a;
		}
		function unique1(arr){
			arr=arr.sort();
			var a=[],temp;
			for(var i=0;i<arr.length;i++){
				if(arr[i]!=temp){
					temp=arr[i]
					a.push(temp)
				}
			}
			return a;
		}
```

## 排序

```js
   class sort {

            bubbling() {
                let arr = [1, 3, 2, 4, 5];
                let len = arr.length, temp;
                for (let i = 0; i < len; i++) {
                    for (var y = 0; y < len - i; y++) {
                        if (arr[y] > arr[y + 1]) {
                            temp = arr[y];
                            arr[y] = arr[y + 1];
                            arr[y + 1] = temp;
                        }
                    }
                }
                log(arr);
            }

			//插入排序
            insert() { 
                             let arr = [1, 3, 2, 4, 5];
                let handle = [];
                handle.push(arr[0]);
                for (let i = 1; i < arr.length; i++) {
                    let a = arr[i];
                    for (let y = handle.length - 1; y >= 0; y--) {
                        let b=handle[y];
                        if (a > handle[y]) {
                            // 向数组当中追加元素，想把a放在b的后面
                            handle.splice(y+1,0,b)
                            break;
                        }
                        if (y === 0)
                            handle.unshift(a)
                    }
                    
                }
                return handle;
            }
        }
        let sorts = new sort();

```

```js
   function quick(ary) {
            if (ary.length <= 1) {
                return ary;
            }


            let middleIndex = Math.floor(ary.length / 2);
            let middleValue = ary.splice(middleIndex, 1)[0];

            let aryLeft = [], aryRight = [];
            for (let i = 0; i < ary.length; i++) {
                let item = ary[i];
                item < middleValue ? aryLeft.push(item) : aryRight.push(item);
            }
            return quick(aryLeft).concat(middleValue, quick(aryRight));
        }
        let ary = [1, 3, 2, 4, 5]
        //    ary=quick(ary);
        //    log(ary);
```



## 整数翻转

```js
//整数反转
        function test7() {
            var x = 123;
            //整数反转
            function out(x) {
                console.log(x);
                var arr1 = x.split(' ');
                console.log(arr1);
                var arr = [];
                //   for(var i=x.length;i>0;i--){
                //         arr.push
                //   }
            }
            out(x);
        }
```

## 动态规划

```js
 //动态规划
        class solution{
			//爬楼梯的问题
            climbStairs(val){
              let arr=[];
              arr[0]=arr[1]=1;
              for(let i=2;i<=val;i++){
                  arr[i]=arr[i-1]+arr[i-2];
              }
              return arr[val];//☜问题：我这里不知道return什么，其实就是return那个数值数组的值。
            }
        }
        var solutions=new solution();
        log(solutions.climbStairs(5));
```

此题的重点是搞懂规律，arr[i]=arr[i-1]+arr[i-2]

而且是要做完for循环后再return

## 两数之和

```js
 function test1() {
            var nums = [3, 2, 4];
            var target = 6;
            //两数之和
            // 利用比较，然后用if查看是否是其目标值，若是则将它push进新的数组里
            function twoSum(nums, target) {
                var arr = [];
                var len = nums.length;
                for (var i = 0; i < len; i++) {
                    for (let j = i + 1; j < nums.length; j++) {
                        if (nums[i] + nums[j] === target) {
                            arr.push(i, j);
                        }
                    }
                }
                console.log(arr);
                return arr;
            };
            twoSum
```

