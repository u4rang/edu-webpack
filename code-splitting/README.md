# Code Splitting



## 1. npm 초기화 및 패키지 설치

```shell
npm init -y

npm i webpack webpack-cli css-loader style-loader mini-css-extract-plugin -D
```



## 2. index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>CSS & Libraries Code Splitting</title>
  </head>
  <body>
    <header>
      <h3>CSS Code Splitting</h3>
    </header>
    <div>
      <!-- 웹팩 빌드 결과물이 잘 로딩되면 아래 p 태그의 텍스트 색깔이 파란색으로 표시됨 -->
      <p>
        This text should be colored with blue after injecting CSS bundle
      </p>
    </div>
    <!-- 웹팩의 빌드 결과물을 로딩하는 스크립트 -->
    <script src="./dist/bundle.js"></script>
  </body>
</html>

```



## 3. base.css

```
p {
  color : blue;
}

```



## 4. index.js

```javascript
import './base.css';
```



## 5. webpack.config.js

```javascript
var path = require('path');

module.exports = {
  mode: 'none', // production, development, none
  entry: './index.js', 
  output: {
    filename: 'bundle.js', // [chunkhash].js
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']  // 순서에 영향 있음 (오른쪽에서 왼쪽 순서로 적용)
      }
    ]
  },
}

```



## 6. package.json 추가

```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
```



## 7. webpack.config.js 수정

```javascript
// webpack.config.js
var path = require('path');
var MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  mode: 'none',
  entry: './index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: MiniCssExtractPlugin.loader },
          "css-loader"
        ]
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin()
  ],
}

```

