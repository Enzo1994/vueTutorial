# Element UI
- 安装npm i element-ui -D==--save-dev
- 引入 `import ElementUI from 'element-ui'`和`import 'element-ui/lib/theme-default/index.css'`
- 安装 less 和 less-loader
- 在webpack.config.js里添加file-loader
  ```javascript
  {
      test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
      loader: 'file-loader'
  },
  ```
- 使用 `Vue.use(ElementUI)`



- **weboack的css-loader 也要style-loader，styleloader在前**
- css包引入直接写在index.html里

###### 注意事项：
- icon用法：`<el-button type="primary" icon="close">主要按钮</el-button>`

###### 按需加载：
- babel-plugin-component `cnpm i babel-plugin-component -D`
- .babelrc 新增配置：
  ```json   
  "plugins";[["component",[
      {
          "libraryName":"element-ui",
          "styleLibraryName":"theme-default"
      }
  ]]]

  ```
- 想用哪个用哪个main.js引入：
  ```javascript
  import {Button,Radio} from 'element-ui'
  Vue.use(Button)
  Vue.use(Radio)
  ```


# Mint UI
- 安装npm i mint-ui -S==--save
- 引入：`import Mint from 'mint-ui` `Vue.use(Mint)`
