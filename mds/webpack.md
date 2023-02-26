# Webpack
## 安装
`npm i webpack webpack-cli -D`

## 打包
`npx webpack ./src/main.js --mode=development` （开发环境）
也可以使用 `--mode=production` 来测试生产环境，他会将代码进行压缩


会将打包好的文件放在dist目录下， 可以将其引入index.html

## 基本配置
### 5个核心概念
1. entry 入口
指示Webpack从哪个文件开始打包
2. output 输出
输出到哪里去，如何命名等
3. loader 加载器
webpack只能处理js，json等资源，其他资源需要戒指loader
4. plugins 插件
扩展webpack的功能
5. mode 模式
开发者模式和生产模式

### 配置文件
`webpack.config.js`与packjson同目录

```javascript
const path = require("path")

module.exports = {
    entry: "./src/main/js",
    output:{
        path: path.resolve(__dirname, 'dist'), //文件的输出路径 (绝对路径)
        filename: "js/main.js",
        clean: true, //清除上次打包结果（dist目录）
    }，
    module:{
        rules:[
            //loader的配置
        ],
        mode:"development"
    }
}
```

通过`npx webpack`来运行


## 开发模式介绍
1. 编译代码，使浏览器能识别运行
2. 代码质量检查，梳理规范

## 处理样式资源
Css, Less, Sass, Scss

### Css资源
1. 下载
`npm i --save-dev css-loader` 
`npm i style-loader -D`
2. webpack config文件
```javascript
module:{
    rules:[
        //loader的配置
        {
            test: /\.css$/, //只检测.css文件
            use: [
                "style-loader", //后将js中css通过创建style标签添加到html文件中 
                "css-loader", //先将css资源编译成commonjs的模块js中
            ], //执行顺序从下到上 
        }
    ],
    mode:"development"
}
```

### Less资源
`npm i less-loader -D `
```json
test: /\.less$/,
use: ["style-loader","css-loader","less-loader"]
```

### Sass和Scss
`npm i sass-loader sass -D` 
```json
test: /\.s[ac]ss$/, //是相同loader的所以匹配sacc和scss
use: ["style-loader","css-loader","sass-loader"]
```

### 处理图片资源
```json
test: /\.(png|jpe?g|git|webp|svg)$/, 
type: "asset",
parser:{
    dataUrlCondition:{
        //小于10kb的图片转base64
        //优点：减少请求数量。缺点：体积会更大
        maxSize: 10*1024, //10kb
    },
},
generator:{
    //输出图片名字
    filename:"images/[hash:10][ext][query]",//取hash值前十位就够了
}
```

### 其他资源
```json
test: /\.(map3|map4|avi)$/, 
type: "asset/resource",
parser:{
    dataUrlCondition:{
        //小于10kb的图片转base64
        //优点：减少请求数量。缺点：体积会更大
        maxSize: 10*1024, //10kb
    },
    
},
generator:{
    filename:"media/[hash:10][ext][query]",
}
```

## Eslint
用来检查js和jsx语法的工具。我们可以继承官方给定的规则。

## Babel
主要用于将ES6语法编写的代码转换为向后兼容的JS语法
 
## 处理Html资源
`npm install --save-dev html-webpack-plugin
`
```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const path = require('path');

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js',
  },
  plugins: [new HtmlWebpackPlugin()],
};
```
这将会生成一个包含以下内容的 dist/index.html 文件：
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>webpack App</title>
  </head>
  <body>
    <script src="index_bundle.js"></script>
  </body>
</html>
```