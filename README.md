# 🚀 Welcome to your new awesome project!

This project has been created using **webpack scaffold**, you can now run

```
npm run build
```

or

```
yarn build
```

to bundle your application

# Настройка SCSS

Выполняем команду в Терминале, чтобы установить **_style-loader, css-loader, sass-loader, node-sass,
mini-css-extract-plugin_**.

```ssh
npm install style-loader css-loader sass-loader node-sass mini-css-extract-plugin -D
npm install --save-dev postcss-loader postcss
```

Добавляем плагин «Mini сss extract plugin» в начало webpack.config.js. Он отвечает за перемещение CSS в отдельный файл.

```ssh
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
```

Теперь указываем Webpack, что необходимо обработать .scss файлы. Он выполнит их через «Mini сss extract plugin», CSS- и
Sass-загрузчик. Вставляем следующий код после фигурной скобки группы «output». Обратите внимание на запятую!

```ssh
output: {
    ...
},
module: {
    rules: [{
        test: /\.scss$/,
        use: ExtractTextPlugin.extract({
            fallback: 'style-loader',
            use: ['css-loader', 'sass-loader']
        })
    }]
}
```

Делаем ссылку на «Extract Text Plugin» прямо перед последней фигурной скобкой. Это даст знать Webpack, что все CSS-файлы
необходимо соединить в отдельный файл и назвать его «style.css».

```ssh
plugins: [
    new ExtractTextPlugin('style.css')
]
```

Webpack.config.js теперь выглядит так:
```ssh
require('dotenv').config();
const HtmlWebpackPlugin = require('html-webpack-plugin');
const path = require('path');
const {CleanWebpackPlugin} = require('clean-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const Dotenv = require('dotenv-webpack');
module.exports = {
  entry: __dirname + "/src/index.js", // webpack entry point. Module to start building dependency graph
  output: {
    path: __dirname + '/dist', // Folder to store generated bundle
    filename: '[name].bundle.js',  // Name of generated bundle after build
    publicPath: '/' // public URL of the output directory when referenced in a browser
  },
  module: {  // where we defined file patterns and their loaders
    rules: [
      {
        test: /\.(png|jpg|gif|svg)$/,
        use: {
          loader: 'file-loader',
          options: {
            name: '[name]-[hash].[ext]',
            outputPath: process.env.ASSET_IMAGES_PATH,
          },
        },
      },
      {
        test: /\.(ttf|woff|woff2)$/,
        use: {
          loader: 'file-loader',
          options: {
            name: '[name]-[hash].[ext]',
            outputPath: process.env.ASSET_FONT_PATH,
          },
        },
      },
      {
        test: /\.(scss|css)$/i,
        use: [
          MiniCssExtractPlugin.loader,
          //'style-loader',
          'css-loader',
          'sass-loader'
        ],
      },

    ]
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      title: "Webpack Index",
      template: __dirname + "/src/pages/index.html",
      inject: 'body',
      filename: "index.html"
    }),
    new HtmlWebpackPlugin({
      title: "Webpack About",
      template: __dirname + "/src/pages/about.html",
      inject: 'body',
      filename: "about.html"
    }),
    new Dotenv(),
    new MiniCssExtractPlugin({
      filename: process.env.STYLE_FILE
    })
  ],
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    compress: true,
    port: 9000,
    open: true,
  },
}

```
