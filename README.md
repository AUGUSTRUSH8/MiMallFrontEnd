# MiMallFrontEnd

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
=======



### 环境安装

- 安装node环境

  - 简单使用

    - 创建工程目录

    - cd 进目录

    - 随便创建一个js文件，如index.js，内容如下

    - ```js
      function sum(a,b) {
          return a+b;
      }
      
      var s = sum(1, 3);
      console.log(s)
      ```

    - 执行node index.js，会输出：4

- 换安装源：npm install -g cnpm --registry=https://registry.npm.taobao.org

  - 简单使用
    - 创建工程目录
    - cd 进目录
    - 安装模块如：cnpm i vue --save

- vue安装使用

  - 教程网站：<https://cn.vuejs.org/v2/guide/installation.html>

  - 第一种使用方式：直接通过script脚本引入

    - ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
          <script src="https://cdn.jsdelivr.net/npm/vue@2.6.11/dist/vue.min.js"></script>
      </head>
      <body>
      
          <div id="app">
              <span>{{message}}</span>
          </div>
          <script>
              new Vue({
                  el: "#app",
                  data:{
                      message:"hello,vue"
                  }
              });
          </script>
      </body>
      </html>
      ```

  - 第二种使用方式：通过`cnpm i vue --save`的方式先安装，然后按照相对位置引入

  - 第三种使用方式：首先安装vue脚手架`cnpm i -g vue-cli`，然后执行类似于`vue init webpack-simple demo3`这样的指令去生成一个基本的代码框架结构，下面是生成的默认结构

    - ```tex
      │  .babelrc
      │  .editorconfig
      │  .gitignore
      │  index.html
      │  package.json
      │  README.md
      │  webpack.config.js
      │
      └─src
          │  App.vue
          │  main.js
          │
          └─assets
                  logo.png
      ```

    - 一般从GitHub下载的项目都要先执行`npm install`指令，然后`npm run dev`等等



- ### MiMall项目

  - 安装最新版本的vue：`cnpm i -g @vue/cli`
  - 查看vue版本：`vue --version`
  - 创建项目：`vue create mimall`
  - 试运行：`cd mimall`, `cnpm run serve`
  - UI方式管理项目：`vue ui`, 会出现一个清爽地项目配置页面，可以在线查找安装依赖

  - 安装调试插件：在chrome的商店当中搜索`vue devtools`安装
  - 前端跨域解决方案
    - CORS跨域：服务端设置，前端直接调用
    - jsonp：前后端都需要改动，它不是一个真正的请求，在XHR里面看不到，只有在JS当中才能看到
    - 接口代理：通过修改nginx服务器配置来实现，前端修改，后端不动
  - 设置项目基本目录框架

  ```xml
  E:.
  ├─api
  ├─assets
  ├─components
  ├─pages
  ├─storage
  ├─store
  └─util
  ```

  - 安装需要的插件

  ```shell
  cnpm i vue-lazyload element-ui node-sass sass-loader vue-awesome-swiper vue-axios vue-cookie --save-dev
  ```

  - 封装路由信息

  主要在`router.js`当中书写路由信息：

  ```js
  import Vue from 'vue'
  import Router from 'vue-router'
  import Home from './pages/home'
  import Index from './pages/index'
  Vue.use(Router);
  
  export default new Router({
    routes:[
      {
        path:'/',
        name:'home',
        component:Home,
        redirect:'/index',
        children:[
          {
            path: '/index',
            name: 'index',
            component: Index,
          }, {
            path: '/product/:id',
            name: 'product',
            component: () => import('./pages/product.vue')
          }, {
            path: '/detail/:id',
            name: 'detail',
            component: () => import('./pages/detail.vue')
          }
        ]
      },
      {
        path: '/login',
        name: 'login',
        component: () => import('./pages/login.vue')
      },
      {
        path: '/cart',
        name: 'cart',
        component: () => import('./pages/cart.vue')
      },
      {
        path: '/order',
        name: 'order',
        component: () => import('./pages/order.vue'),
        children:[
          {
            path: 'list',
            name: 'order-list',
            component: () => import('./pages/orderList.vue')
          },
          {
            path: 'confirm',
            name: 'order-confirm',
            component: () => import('./pages/orderConfirm.vue')
          },
          {
            path: 'pay',
            name: 'order-pay',
            component: () => import('./pages/orderPay.vue')
          },
          {
            path: 'alipay',
            name: 'alipay',
            component: () => import('./pages/alipay.vue')
          }
        ]
      }
    ]
  });
  ```

  - storage封装

  先了解cookie和storage的区别：

  1. 存储大小：Cookie4K，storage 5M
  2. 有效期：Cookie有有效期，Storage永久存储
  3. Cookie会发送到服务端，存储在内存当中，storage只存储在浏览器端
  4. 路径：Cookie有路径限制，storage只存储在域名下
  5. Api：cookie没有特定的Api，Storage有

  其他几点：

  1. storage本身有api，但只是简单的key/value形式。
  2. storage只存储字符串，需要人工转换成json对象
  3. storage只能一次性清空，不能单个清空

  主要代码如下：

  ```js
  /**
   * Storage封装
   */
  const  STORAGE_KEY = 'mall';
  export default{
    // 存储值
    setItem(key,value,module_name){
      if (module_name){
        let val = this.getItem(module_name);
        val[key] = value;
        this.setItem(module_name, val);
      }else{
        let val = this.getStorage();
        val[key] = value;
        window.sessionStorage.setItem(STORAGE_KEY, JSON.stringify(val));
      }
    },
    // 获取某一个模块下面的属性user下面的userName
    getItem(key,module_name){
      if (module_name){
        let val = this.getItem(module_name);
        if(val) return val[key];
      }
      return this.getStorage()[key];
    },
    getStorage(){
      return JSON.parse(window.sessionStorage.getItem(STORAGE_KEY) || '{}');
    },
    clear(key, module_name){
      let val = this.getStorage();
      if (module_name){
        if (!val[module_name])return;
        delete val[module_name][key];
      }else{
        delete val[key];
      }
      window.sessionStorage.setItem(STORAGE_KEY, JSON.stringify(val));
    }
  }
  ```

  - 统一错误拦截

  主要利用axios的特性，代码如下：

  ```js
  import Vue from 'vue'
  import App from './App.vue'
  import axios from 'axios'
  import VueAxios from 'vue-axios'
  import router from './router'
  
  axios.defaults.baseURL='/api';
  axios.defaults.timeout=8000;
  
  axios.interceptors.response.use(function(response){
    let res = response.data;
    if(res.status==0){
      return res.data;
    }else if(res.status == 10){
      window.location.href = '/#/login';
    }else{
      alert(res.msg);
    }
  });
  Vue.use(VueAxios,axios);
  Vue.config.productionTip = false
  
  new Vue({
    router,
    render: h => h(App),
  }).$mount('#app')
  
  ```

  - 环境配置

  主要是编写`env.js`文件

  ```js
  let baseURL;
  switch (process.env.NODE_ENV) {
    case 'development':
      baseURL = 'http://dev-mall-pre.springboot.cn/api';
      break;
    case 'test':
      baseURL = 'http://test-mall-pre.springboot.cn/api';
      break;
    case 'prev':
      baseURL = 'http://prev-mall-pre.springboot.cn/api';
      break;
    case 'prod':
      baseURL = 'http://mall-pre.springboot.cn/api';
      break;
    default:
      baseURL = 'http://mall-pre.springboot.cn/api';
      break;
  }
  
  export default {
    baseURL
  }
  ```

  - 数据mock

  主要有三种数据mock方法

  1. 本地json
  2. easy-mock平台（调的人多，容易崩）
  3. mock api(不会发生真正的请求，而是在代码层面直接进行拦截)

  主要看看mockapi的方式：

  1. 安装：`cnpm i mockjs --save-dev`
  2. 使用

  ```js
  import Mock from 'mockjs'
  Mock.mock('/api/user/login',{
      "status": 0,
    "data": {
      "id": 12,
      "username": "admin",
      "email": "admin@51purse.com",
      "phone": null,
      "role": 0,
      "createTime": 1479048325000,
      "updateTime": 1479048325000
    }
  });
  ```
