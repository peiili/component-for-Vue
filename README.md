# 项目过程

#### 使用vue功能完成列表新增/删除/搜索的功能

### 1.利用组件创建静态页面;

```js
//创建根实例;  
var app = new Vue({
        el: "#expamle",
        data:{
            msg:"hellow word"
        }
    });
```



1.1创建构造器的四种方法;

```js
1.在组件外创建构造器,构造器内容简单的时候适合使用,不适用复杂的构造器
 const loginComponent = Vue.extend({
        template: `
            <div>
            用户名:<input :value="v1"/><br/>
            密码:<input :value="v2"/><br/>
            <button>登陆</button>
            </div>
            `,
   //组件中的数据;
             data: function () {
                 return {
                     v1: 'Walter',
                     v2: 'White',
                 }
             }
    });
 Vue.component("login", loginComponent);

2,构造器与组件一起写;
 Vue.component("login",{
        template: `
            <div>
            用户名:<input :value="v1"/><br/>
            密码:<input :value="v2"/><br/>
            <button>登陆</button>
            </div>
            `,
   //组件中的数据
        data: function () {
            return {
                v1: 'Walter',
                v2: 'White',
            }
        }
    });

3.利用X-template创建构造器;
 		<template id="templateid">
          //子组件必须有根组件包裹;
                <div>
                    用户名:<input v-model="username" /><br />
                    密码:<input v-model="password" /><br />
                    <button @click="login">登陆</button>
                </div>
        </template>
 4.利用<script type="text.html" id="template">创建构造器;
		<script type="text.html" id="template">
          //子组件必须有根组件包裹;
                <div>
                    用户名:<input v-model="username" /><br />
                    密码:<input v-model="password" /><br />
                    <button @click="login">登陆</button>
                </div>
        </script>
                    
```

1.2,将构造器注册

```js
 Vue.component("login", {
        //引入构造器;
        template: `#templateid`,
        data: function () {
            return {
                username: '',
                password: '',
                msg:"hello"
            }
        }
    });
```

### 2.将内存数据渲染到表格

2.1准备内存数据

- [ ] ```js
       Vue.component("brand-manager", {
              template: `#templateId`,
              //data必须使一个函数,因为ia一个项目中会有多个组件,
              data() {
                  return {
                    
                    //内存数据
                      brandlist: [{
                              id: 1,
                              name: "LV",
                              time: new Date()
                          },
                          {
                              id: 2,
                              name: "GUCCI",
                              time: new Date()
                          }
                      ],
                      oldBrandList: [],
                      id: "",
                      name: "",
                      keyword: ""
                  }
              },
       });
      ```



2.2循环内存数据,将数据渲染到组件中 

```js
<tr v-for="(item,index) in brandlist" :key="item.id">
       <td>{{ item.id }}</td>
       <td>{{ item.name }}</td>
       <td>{{ item.time | formateDate}}</td>
       //将下标存入调用的方法中;方便在删除的时候找到对应下标的元素;
       <td><a href="javascript:void(0)" @click = "delList(index)" >删除</a></td>
   </tr>
```

### 3.新增表单内容

```js
.......
  methods: {
    //创建新增数据的方法;
            add() {
                this.brandlist.push({
                    id: this.id,
                    name: this.name,
                    time: new Date()
                })
                //新增完成以后将新数组保存起来,用于后期搜索的完以后恢复原来的数据;
                this.oldBrandList = this.brandlist;
            },
.......
```

### 4.删除列表内容

```js
 .......
delList(index) {
                //删除对应的列表值;
                this.brandlist.splice(index, 1);
                //删除数据以后,将删除数组重新保存,
                this.oldBrandList = this.brandlist;
            },
.......
```

### 5.搜索列表

利用Array filter()方法;

```js
array.filter(function(currentValue,index,arr){
  return 条件;
}, thisValue);
currentValue	必须。当前元素的值
index	可选。当前元素的索引值
arr	可选。当前元素属于的数组对
返回值为查询的数组中符合条件的元素;
......
watch{//当输入keyword的值发生改变的时候触发,进行查询;
 keyword(newValue,oldValue){
               //如果输入框为空,则显示全部的数据;
                if (this.keyword.length === 0) {
                    this.brandlist = this.oldBrandList;
                    return
                }
                //array.filter(function(currentValue,index,arr), thisValue)
                //brand 为遍历的每一个数组的value的值,在这里为么一个数据对象;arr为原数组对象
                var newBrandList = this.brandlist.filter(function (currentValue, index, arr) {
                    return currentValue.name.includes(this.keyword)
                },this);//在作为函数回调的时候应添加this,
                this.brandlist = newBrandList;
         }
    }
}
......

```



