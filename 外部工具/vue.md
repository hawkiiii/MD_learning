# vant

vue中js模板

```js
  import {
    Toast,
    PullRefresh,
    Swipe,
    SwipeItem
  } from 'vant';
  export default {
    comments:{
      [PullRefresh.name]: PullRefresh,
      [Swipe.name]: Swipe,
      [SwipeItem.name]: SwipeItem
    },
    data() {
      return {
        categories: '',
        phones: '',
        show: true,
        sku: '',
        goods: ''
      }
    },
    created(){
      const _this = this
      axios.get('http://192.168.31.86:8181/phone/index').then(function (resp) {
        _this.phones = resp.data.data.phones
        _this.categories = resp.data.data.categories
      })
    },
    methods: {
      onClick(index) {
        //静态测试
        // switch (index) {...}
        //alert(index)
        const _this = this
        axios.get('http://192.168.31.86:8181/phone/findByCategoryType/'+this.categories[index].type).then(function (resp) {
          _this.phones = resp.data.data
        })
      },
      buy(index){
        this.show = true
        const _this = this
        axios.get('http://192.168.31.86:8181/phone/findSpecsByPhoneId/'+this.phones[index].id).then(function (resp) {
          _this.goods = resp.data.data.goods
          _this.sku = resp.data.data.sku
        })
      },
      onBuyClicked(item){
        this.$store.state.specsId = item.selectedSkuComb.s1
        this.$store.state.quantity = item.selectedNum
        this.$router.push('/addressList')
      }
    }
  }
```

**onClick/ onSave/ onDelete/ onEdit/ onSelect/ onAdd:**

以上都是网页绑定事件

vue文件

```js
import ...
export default{
		comments:{},	 //下拉刷新、滑动、滑动 item
  	data() {},		//数据
  	created(){
      	onClick(){},
      	onSave(){},
        onDelete(){}
    }
}
```

created:在模板渲染成html前调用，即通常初始化某些属性值，然后再渲染成视图。

mounted:在模板渲染成html后调用，通常是初始化页面完成后

![img](/Users/hawkii/自学MD/面试img/Center.png)

**let和var**

let	  局部
var	 全局或者整个函数块
const 声明常量



**alert和toast**： 弹窗插件

**console.log( )**: 打印信息，Chrome 开发模式中

**axios用法** 用于浏览器和 nodejs 的 HTTP client，也有post、delete、put、get方法

请求方法+网址+ .then(function (resp) )，然后讲resp 后端数据传给data块中数据

```js
axios.get('http://192.168.31.86:8181/phone/index').then(function (resp) {
    _this.phones = resp.data.data.phones
    _this.categories = resp.data.data.categories
})
```

**$mount**手动挂载

**$destroy**销毁一个实例，清理所有连接

**$remove**清除掉这个实例上渲染到页面上点

**$route**是一个地址，可以看成url

**$router**是一个全局的对象，是工程下面路径，常见 **$router.option.routes**，对应index.js下面配置信息

**前端路由：**对于单页面应用程序来说，主要通过URL中的hash(#号)来实现不同页面之间的切换，同时，hash有一个特点：HTTP请求中不会包含hash相关的内容；所以，单页面程序中的页面跳转主要用hash实现

**form表单** 向路由器提交数据一种形式 html中的

---

> src
> --views	视图，vue文件，html+js+css
> --router	配置路径，两种方式 1 import 和 2 component

# element

1. 首先选一个布局，head-aside-main

2. 复制过来对应代码，进行剪裁

   1. head 题目 账户 设置等信息，映射
   2. 后台管理页面

   ```vue
   <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
     			<!-- 菜单，已经打开菜单1级、2级 -->
           <el-menu router :default-openeds="['0', '1']">
             <!-- 循环router/index.js/routes下面的对象，item({对象1},{对象2})和index(./children[{对象1},{对象2}])双重循环, item.show判断是否遍历 -->
             <el-submenu v-for="(item,index) in $router.options.routes" :index="index+''" v-if="item.show">
               <!-- template和el-submenu 搭配创建新的子菜单 -->
               <template slot="title">{{item.name}}</template>
               <!-- 三目运算符，标记高亮信息 -->
               <el-menu-item v-for="(item2,index2) in item.children" :index="item2.path"
                             :class="$route.path==item2.path?'is-active':''">
                 {{item2.name}}
               </el-menu-item>
             </el-submenu>
           </el-menu>
         </el-aside>
   ```

   

3. 添加更新页面

   ```vue
   <template>
       <el-form style="width: 60%" :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
   
           <el-form-item label="图书名称" prop="name">
               <el-input v-model="ruleForm.name"></el-input>
           </el-form-item>
   
           <el-form-item label="作者" prop="author">
               <el-input v-model="ruleForm.author"></el-input>
           </el-form-item>
   				<!-- button按钮，@事件 触发函数 -->
           <el-form-item>
               <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
               <el-button @click="resetForm('ruleForm')">重置</el-button>
           </el-form-item>
   
       </el-form>
   </template>
   
   <script>
       export default {
           data() {
               return {
                 	//数据格式
                   ruleForm: {
                       name: '',
                       author: ''
                   },
                 	//表单信息，必填，提示消息，失焦触发
                   rules: {
                       name: [
                           { required: true, message: '图书名称不能为空', trigger: 'blur' }
                       ],
                       author:[
                           { required: true, message: '作者不能为空', trigger: 'blur' }
                       ]
                   }
               };
           },
           methods: {
               submitForm(formName) {
                   const _this = this
                   this.$refs[formName].validate((valid) => {
                     //判断数据是否合法
                       if (valid) {
                         //this.ruleForm表单
               axios.post('http://localhost:8181/book/save',this.ruleForm).then(function(resp){
                               //如果提交成功
                 							if(resp.data == 'success'){
                                   _this.$alert('《'+_this.ruleForm.name+'》添加成功！', '消息', {
                                       confirmButtonText: '确定',
                                       callback: action => {
                                         	//跳转到同层别的页面
                                           _this.$router.push('/BookManage')
                                       }
                                   })
                               }
                           })
                       } else {
                           return false;
                       }
                   });
               },
               resetForm(formName) {
                 //
                this.$refs[formName].resetFields();
               }
           }
       }
   </script>
   
   ```
   
   