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



### MiMall项目

- 安装最新版本的vue：`cnpm i -g @vue/cli`
- 查看vue版本：`vue --version`
- 创建项目：`vue create mimall`
- 试运行：`cd mimall`, `cnpm run serve`
- UI方式管理项目：`vue ui`, 会出现一个清爽地项目配置页面，可以在线查找安装依赖

- 安装调试插件：在chrome的商店当中搜索`vue devtools`安装