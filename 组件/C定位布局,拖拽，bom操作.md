# 29作业

## 定位布局

确定每个元素的定位的布局。

#### js

```js
  let ali=document.querySelectorAll("li");
            let lips=[];
            for(let i=0;i<ali.length;i++){
               lips.push([ali[i].offsetTop,ali[i].offsetleft]);
               setTimeout(function(){
                ali[i].style.position="absoulte";
                ali[i].style.left=lips[i][0] +"px";
                ali[i].style.top=lips[i][1]+"px";
                ali[i].style.margin=0+"px";
               },0)
            }
```

## 拖拽

#### js

拖拽与替换功能的实现。**最后的变量要重置**。

```js
    /*
                 获取元素的初始位置值
                 获取鼠标点击li元素的鼠标位置
                 获取鼠标滑动时的位置
                 计算被点击li的新位置
                 给被点击的li元素设置新位置
             */

        oNav.addEventListener("mousedown", drag);
        document.addEventListener("mousemove", drag);
        document.addEventListener("mouseup", drag);

        //判断下是否被点中
        let toggle = false
        let ele = null;
        let x1, y1, startX, startY, z = 1;
        let goalEle = null;

        function drag(ev) {
            ev.preventDefault();//阻止默认情况下不让拖拽的事件
            switch (ev.type) {//事件分流
                case "mousedown":
                    if (ev.target.parentNode.tagName === "LI") {//事件节流
                        toggle = true;
                        ele = ev.target.parentNode;
                        ele.style.zIndex = z++;
                        startX = ele.offsetLeft;
                        startY = ele.offsetTop;
                        x1 = ev.clientX;
                        x1 = ev.clientY;
                    }
                    break;
                case "mousemove":
                    if (toggle) {
                        let x2 = ev.clientX;
                        let y2 = ev.clientY;
                        let nowX = startX + x2 - x1;
                        let nowY = startY + y2 - x1;
                        ele.style.left = nowX + "px";
                        ele.style.top = nowY + "px";
                        let xr = x2 - oNav.offsetLeft;
                        let yr = y2 - oNav.offsetTop;
                        for (let n = 0; n < ali.length; n++) {
                            ali[n].style.transform = "";
                            if (xr > ali[n].offsetleft &&
                                xr < ali[n].offsetLeft + 200 &&
                                yr > ali[n].offsetTop &&
                                yr < ali[n].offsetTop + 120
                            ) {
                                console.log("碰撞成功");
                                ali[n].style.transform = "scale(1.05)";
                                goalEle = ali[n];
                                break;
                            }
                        }
                    }
                    break;
                case "mouseup":
                    if (toggle) {
                        toggle = false;
                        if (goalEle) {
                           ele.style.left= goalEle.left+"px";
                           ele.style.top= goalEle.top+"px";
                           goalEle.style.left = startX + "px";
                           goalEle.style.top = startY + "px";
                           ali[n].style.transform = "";
                           //数组参数还原
                           goalEle=null;
                           ele=null;

                        } else {
                            //还原
                            ele.style.left = startX + "px";
                            ele.style.top = startY + "px";
                        }
                    }
                    break;

            }
        }
```

![image-20201120103911114](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20201120103911114.png)



# bom操作

##### clientX、clientY点击位置距离当前body可视区域的x、y坐标。

##### pageX、pageY 对于整个页面来说，包括了被滚动条滚过去的body部分的长度。

##### screenX、screenY点击位置距离当前电脑屏幕最左上角的x、y坐标。

##### offsetX、offsetY相对于被点中的元素的左上角的偏移量。

##### x、y和clientX、clientY一样。

##### layerX和layerY，鼠标相较于当前坐标系的位置。如果触发元素没有设置绝对定位或相对定位，以页面为参考点。如果有，则以当前触发元素的左上角为坐标系。