React

1.demo1

```jsx
<body>
	// 准备容器
    <div id="test"></div>
    //引入react
    <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
    <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
    <script type="text/javascript"  src="./js/babel.min.js"></script>
    <script type="text/babel">

        // 创建虚拟dom
        const VDOM = <h1>helloReact</h1>
       				 // 标签名，标签属性，标签内容
        // 渲染虚拟dom到界面
        ReactDOM.render(VDOM,document.getElementById("test"))
    </script>
</body>
```

2.jsx

3.虚拟dom是Object类型的对象 

4.json：parse（字符串变json，stringfy

5.jsx语法规则：

- 不要引号
- 标签里混js需要{}  标签中混入JS表达式时要用{}

```jsx
const myId='testId'
        const myData='testData'
        const VDOM2 =( 
            <h1 className="title" id={myId}>
                <span style={{background:'blue',color:'white',fontSize:'20px'}}>{myData}</span>
            </h1>
        )
        // 标签名，标签属性，标签内容
        // 渲染虚拟dom到界面
        // 样式的类名指定不要用class，要用className
        // 内联样式，要用style={{key:value}}\
        // 只有一个更标签
        // 标签闭合
        //  标签首字母
            // 1.若小写字母开头，将该标签转化为html同名元素
            // 大写字母找组件，否则报错

ReactDOM.render(VDOM2,document.getElementById("test"))
```



