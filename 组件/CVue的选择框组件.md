# Vue的选择框组件

## input选择框

### JS

##### 子组件的input组件

```js
//    input组件
Vue.component("select-text", {
    template: '  <div id="selectText">\n' +
    '                    <input type="text" class="inputText" placeholder="请选择" readonly="readonly" @click="show"  v-model="getdata" >\n' +
    '                    <div >\n' +
    '                        <div  v-show="isshow" class="shield" @click="cancelShow">\n' +
    '                            <div class="option">\n' +
    '                                <div class="cancelBtn" @click="cancelShow">\n' +
    '                                    <div class="cancelText">取消</div>' +
    '                                 </div>\n' +
    '                                <div class="optiondata">\n' +
    '                                   <ul>\n' +
    '                    <li v-for="data in detaildata":data-key="data.key"   @click="choice(data)" >{{data.name}}</li>\n' +
    '                                    </ul>\n' +
    '                                </div>\n' +
    '                            </div>\n' +
    '                        </div>\n' +
    '                    </div>\n' +
    '                </div>'
    ,
    props:{
        detaildata:{
            type: Array,
            default:function(){
                return []
            }
        },
        field:{
            type:String,
            default:''
        }
    },
    data() {
        return {
            isshow: false,
            getdata:''
        }


    },
    methods: {
        show(){
            if(this.isshow==true){
                this.isshow=false;
            }
            else
                this.isshow=true;
        },
        cancelShow(){
            this.isshow=false;
        },
        choice(options){
            this.isshow=false;
            this.getdata=options.name;
            this.$emit("choicedata",options, this.field);
        }
    }
})
```



##### 父组件的js

```js
<script type="text/javascript">
    //    本界面Vue
    new Vue({
        el: '#content',
        data: {
            pageTitle: '个人资料',
            page: '1',
//            第一页 个人资料
            personalData: {
                gender: [
                    {key: "1", name: "男"},
                    {key: "2", name: "女"}
                ]
            },

//            选中值传参点
            fields:["personal.gender"],
//            存储值
            formdata: {
                "personal":[],
                getdataCheckKey: [],
                getdataCheckDayKey: [],
                eduExperience: [
                    {"school": ""},
                ],
                foreAbility:[
                    {"foreign": ""},
                ],
                workExperience:[
                    {"form": ""},
                ]
            }
        },
        methods: {
            //层级的选择
            choice(data, field) {
                var  arr_field = field.split('.'),
                len = arr_field.length;
                var formdata = this.formdata;
                switch (len) {
                        case 1: {
                            this.formdata[arr_field[0]] = data.name;
                            break;
                        }
                        case 2: {
                            this.formdata[arr_field[0]][arr_field[1]] = data.name;
                            alert(this.formdata.foreign.language);
                            break;
                        }
                        case 3:{
                            this.formdata[arr_field[0]][arr_field[1]][arr_field[3]] = data.name;
                            break;
                        }
                    default:
                        break;
                }
            },
            //            添加教育经历组件
            addEduExperience() {
                this.formdata.eduExperience.push({});
            },
			//            删除教育经历组件
            deledu(index) {
                this.formdata.eduExperience.splice(index, 1);
            },
```



### html

##### 父搭载子组件的html

```html
  <div class="">
                  <select-text v-bind:detaildata="personalData.gender" @choicedata="choice" v-bind:field="fields[0]"></select-text>
                </div>
```

### CSS

##### 初始化所有

```css
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    list-style: none;
}

html, body {
    font-family: 'sans-serif';
    /*font-size: 62.5%;*/
    background-color: rgb(255, 255, 255);
    font-size: 50px;
}

.bodyContainer {
    position: relative;
    width: 100%;
    margin: 0 auto;
}
```

##### css组件

```css
.shield {
    width: 100%;
    height: 100%;
    position: fixed;
    left: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.3);
    z-index: 999998;
}

.option {
    width: 100%;
    max-height: 50%;
    position: fixed;
    left: 0;
    bottom: 0;
    z-index: 999999;
    font-size: 0.35em;
    background-color: #FFFFFF;
    overflow-y: auto;
}

.cancelBtn {
    height: 50px;
    margin-bottom: 15px;
}

.cancelText {
    padding: 0.7em 0em;
    text-align: center;
    font-size: 0.40rem;
    background-color: rgba(0, 0, 0, 0.05);
}

.optiondata > ul > li {
    margin: 0.7em 4em;
    padding: 0.7em 0em;
    text-align: center;
    color: #393939;
    border-bottom: 0.03em solid rgba(0, 0, 0, 0.3);
    font-size: 0.30rem;
}

.dataAdd {
    font-size: 0.3rem;
    text-align: center;
    color: #393939;
    margin: 0.35rem 0em 0.25rem 0em;
}

.dataAdd > span {
    border-bottom: 0.03em solid #393939;

}
```

## 选择器加载

### JS

##### 外语能力组件

```js
//     外语能力
Vue.component("foreign-ability", {
    props: ['index'],
    template: '<div class="dataWarp">\n' +
    '            <div class="dataBox">\n' +
    '                <div class="dataBoxLeft">\n' +
    '                    <div class="dataBoxLeftCT">第{{index+1}}外语</div>\n' +
    '                    <div class="dataBoxLeftET">first</div>\n' +
    '                </div>\n' +
    '                <div class="dataBoxRight">\n' +
    '                    <div class="deleteText"  v-show="this.index!=0"><span @click="del(index)">删除</span></div>\n' +
    '                 </div>\n' +
    '            </div>\n' +
    '            <div class="dataBox ">\n' +
    '                <div class="dataBoxLeft">\n' +
    '                    <div class="dataBoxLeftCT">外语</div>\n' +
    '                    <div class="dataBoxLeftET">english english</div>\n' +
    '                </div>\n' +
    '                <div class="dataBoxRight">\n' +
    '                    <select-text  v-bind:detaildata="foreignAbility.foreign"  @choicedata="choiceFaS"  v-bind:field="fieldt[0]"></select-text>\n' +
    '                </div>\n' +
    '            </div>\n' +
    '            <div class="dataBox ">\n' +
    '                <div class="dataBoxLeft">\n' +
    '                    <div class="dataBoxLeftCT">等级</div>\n' +
    '                    <div class="dataBoxLeftET">english english</div>\n' +
    '                </div>\n' +
    '                <div class="dataBoxRight">\n' +
    '                    <input type="text" class="inputText" placeholder="请输入"/>\n' +
    '                </div>\n' +
    '            </div>\n' +
    '            <div class="dataBox lastDataBox">\n' +
    '                <div class="dataBoxLeft">\n' +
    '                    <div class="dataBoxLeftCT">熟练程度</div>\n' +
    '                    <div class="dataBoxLeftET">english english</div>\n' +
    '                </div>\n' +
    '                <div class="dataBoxRight">\n' +
    '                    <input type="text" class="inputText" placeholder="请输入"/>\n' +
    '                </div>\n' +
    '            </div>\n' +
    '        </div>'
    ,
    data() {
        return {
            foreignAbility: {
                foreign: [
                    {key: "1", name: "英语"},
                    {key: "2", name: "阿拉伯语"}
                ]
            },
            fieldt:["foreign.language"],
            formdatas:{
                "foreign":[]
            }

        }
    },
    methods: {
        del(index) {
            this.$emit('delfore', index)
        },
        choiceFaS: function (data,field) {
            var arr_field = field.split('.'),
                len = arr_field.length;
            var formdatas = this.formdatas;
            switch (len) {
                case 1:
                    this.formdatas[arr_field[0]] = data.name;
                    break;
                case 2:
                    this.formdatas[arr_field[0]][arr_field[1]] = data.name;
                    alert(this.formdatas.foreign.language);
                    break;
                case 3:
                    this.formdatas[arr_field[0]][arr_field[1]][arr_field[3]] = data.name;
                    break;
                default:
                    break;
            }

        }

    }
})
```

### html

##### 父搭载子组件的html

```html
    <div v-for="(d,index) in formdata.foreAbility " :key="index">
            <foreign-ability @delfore="delfore" v-bind:index="index"></foreign-ability>
        </div>
```



### CSS

##### css组件

```css
.inputText {
    width: 100%;
    text-align: right;
    font-size: 0.25rem;
    line-height: 0.62rem;
    color: #393939;
    outline: none;
    border: 0;
}

.deleteText {
    width: 100%;
    text-align: right;
    font-size: 0.28rem;
    line-height: 0.62rem;
    color: red;
    outline: none;
    border: 0;
}

.inputDiv {
    height: 0.62rem;
}

.btnWrap {
    /*width: 4.04rem;*/
    width: 70%;
    font-size: 0;
    margin: 0 auto;
    padding: 0.46rem 0 1.02rem;
}
.btnBoxLeft{
    position: relative;
    float: left;
}
.btnBoxRight{
    float: right;
}
.btnSub {
    width: 2.02rem;
    height: 0.86rem;
    line-height: 0.86rem;
    text-align: center;
    font-size: 0.32rem;
    color: #FFFFFF;
    outline: none;
    border: 0;
    border-radius: 0.32rem;
    background-color: #1d2088;
    box-shadow: 0 0 0.24rem #cbcce0;
    -moz-box-shadow: 0 0 0.24rem #cbcce0;
    -webkit-box-shadow: 0 0 0.24rem #cbcce0;
}


.shield {
    width: 100%;
    height: 100%;
    position: fixed;
    left: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.3);
    z-index: 999998;
}

.option {
    width: 100%;
    max-height: 50%;
    position: fixed;
    left: 0;
    bottom: 0;
    z-index: 999999;
    font-size: 0.35em;
    background-color: #FFFFFF;
    overflow-y: auto;
}

.cancelBtn {
    height: 50px;
    margin-bottom: 15px;
}

.cancelText {
    padding: 0.7em 0em;
    text-align: center;
    font-size: 0.40rem;
    background-color: rgba(0, 0, 0, 0.05);
}

.optiondata > ul > li {
    margin: 0.7em 4em;
    padding: 0.7em 0em;
    text-align: center;
    color: #393939;
    border-bottom: 0.03em solid rgba(0, 0, 0, 0.3);
    font-size: 0.30rem;
}

.dataAdd {
    font-size: 0.3rem;
    text-align: center;
    color: #393939;
    margin: 0.35rem 0em 0.25rem 0em;
}

.dataAdd > span {
    border-bottom: 0.03em solid #393939;

}

```

