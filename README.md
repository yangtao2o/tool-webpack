# Webpack

记录 Webpack 的一个小仓库，[webpack@3](./docs/webpack@3.md)。

## 搭建基本的前端开发环境（webpack@4）

主要有：

- 构建发布需要的 HTML、CSS、JS 文件
- 使用 CSS 预处理器来编写样式
- 处理和压缩图片
- 使用 Babel 来支持 ES 新特性
- 本地提供静态服务以方便开发调试

具体步骤：

- 关联 Html，使用`html-webpack-plugin`
- 构建 CSS，使用`mini-css-extract-plugin css-loader`
- 处理图片文件，使用`file-loader`
- 使用 Babel，使用`@babel/core @babel/plugin-transform-runtime @babel/preset-env @babel/runtime babel-loader webpack`
- 启动静态服务，使用`webpack-dev-server`

`webpack.config.js` 文件内容：

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  mode: "development",
  entry: "./main.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "./dist")
  },
  devServer: {
    contentBase: "./dist"
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html", // 配置输出文件名和路径
      template: "assets/index.html" // 配置文件模板
    }),
    new MiniCssExtractPlugin({
      // Options similar to the same options in webpackOptions.output
      // both options are optional
      filename: "[name].css",
      chunkFilename: "[id].css"
    })
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: "/public/path/to/"
            }
          },
          "css-loader"
        ]
      },
      {
        test: /\.(png|jpe?g|gif)$/i,
        use: [
          {
            loader: "file-loader",
            options: {
              // name: 'dirname/[contenthash].[ext]',
              name: "[path][name].[ext]?[contenthash]"
            }
          }
        ]
      },
      {
        test: /\.jsx?$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"],
            plugins: ["@babel/plugin-transform-runtime"]
          }
        }
      }
    ]
  }
};
```

package 文件主要内容：

```json
{
  "scripts": {
    "build": "webpack --mode production",
    "start": "webpack-dev-server --mode development"
  },
  "devDependencies": {
    "@babel/core": "^7.8.4",
    "@babel/plugin-transform-runtime": "^7.8.3",
    "@babel/preset-env": "^7.8.4",
    "@babel/runtime": "^7.8.4",
    "babel-loader": "^8.0.6",
    "css-loader": "^3.4.2",
    "file-loader": "^5.0.2",
    "html-webpack-plugin": "^3.2.0",
    "less-loader": "^5.0.0",
    "mini-css-extract-plugin": "^0.9.0",
    "style-loader": "^1.1.3",
    "webpack": "^4.41.5",
    "webpack-cli": "^3.3.10"
  }
}
```

参考：[使用 webpack 定制前端开发环境](https://juejin.im/book/5a6abad5518825733c144469/section/5a6abbe4518825734f52eb8f)

## 学习资料

- [webpack 指南](https://webpack.docschina.org/guides/)
- [Webpack 中文指南](https://zhaoda.net/webpack-handbook/index.html)
- [Webpack傻瓜式指南（一）](https://zhuanlan.zhihu.com/p/20367175) - 入门学习 webpack 的基本知识
- [Webpack傻瓜式指南](https://vikingmute.gitbooks.io/webpack-for-fools/content/) - 这是一本关于webpack这种构建工具的书
- [React 的 Webpack 配置](https://www.jianshu.com/p/0e01ca947e50)
- [带你深度解锁Webpack系列(基础篇)](https://juejin.im/post/5e5c65fc6fb9a07cd00d8838)
- [带你深度解锁Webpack系列(进阶篇)](https://juejin.im/post/5e6518946fb9a07c820fbaaf)
- [带你深度解锁Webpack系列(优化篇)](https://juejin.im/post/5e6cfdc85188254913107c1f)
