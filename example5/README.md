# 예제 5 - 플러그인 추가하기

이제 웹사이트의 스타일링을 위한 인프라를 구축했으니, 스타일을 적용할 실제 페이지가 필요하게 되었습니다. HTML 페이지를 생성하거나 기존의 것을 그대로 사용할 수 있는 [html-webpack-plugin](https://github.com/ampedandwired/html-webpack-plugin)을 이용하여 이 작업을 수행할 수 있습니다. 여기서는 기존의 `index.html`을 사용할 것입니다.

먼저 플러그인을 설치합니다:

    npm install --save-dev html-webpack-plugin@2

다음으로 설정을 추가 합니다.

```javascript
// webpack.config.js
var path = require('path')
var webpack = require('webpack')
var HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: ['./src/index'],
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin({
      compressor: {
        warnings: false,
      },
    }),
    new webpack.optimize.OccurenceOrderPlugin(),
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
  module: {
    loaders: [{
      test: /\.css$/,
      loaders: ['style', 'css']
    }]
  }
}
```

이번에 `webpack` 명령을 실행하면, `HtmlWebpackPlugin`이 `./src/index.html` 지정하기 때문에
`./src/index.html` 파일의 내용을 `dist` 폴더에 `index.html`파일로 생성할 것입니다.

`index.html`의 내용이 비어 있다면 기본 템플릿을 사용하기 때문에 아무런 문제가 없습니다. 하지만 그 내용을 채워 보도록 하겠습니다.

```html
<html>
<head>
  <title>Webpack Tutorial</title>
</head>
<body>
  <h1>Very Website</h1>
  <section id="color"></section>
  <button id="button">Such Button</button>
</body>
</html>
```

이제 `bundle.js`의 내용을 HTML의 `<script>` 태그에 직접 넣지 않아도 됩니다. 이 플러그인이 자동으로 넣어주기 때문입니다. 만약 스크립트 태그에 직접 넣은 경우라면, 같은 코드를 두 번 로드하게 될 것입니다.

이 시점에서 `styles.css`에 몇 가지 기본 스타일을 추가해 봅시다.

```css
h1 {
  color: rgb(114, 191, 190);
  text-align: center;
}

#color {
  width: 300px;
  height: 300px;
  margin: 0 auto;
}

button {
  cursor: pointer;
  display: block;
  width: 100px;
  outline: 0;
  border: 0;
  margin: 20px auto;
}
```

#### Side note

I feel like I should mention that the `html-webpack-plugin` should
be used sparingly. To me, webpack should generate HTML files if you just have a really simple
one to bootstrap a SPA. So while it was useful for the learning experience, which required only
one HTML file, I wouldn't recommend it to generate 12 HTML files. This doesn't mean you can't use
html files with something like angular directives, which require HTML template files. In that case
you could do something like:

```javascript
// ...directive stuff
template: require('./templates/button.html') // using raw loader
```

Instead, it means that you should not be doing something like this:

```javascript
new HtmlWebpackPlugin
  template: './src/index.html'
}),
new HtmlWebpackPlugin({
  template: './src/button.html'
}),new HtmlWebpackPlugin({
  template: './src/page2.html'
})
```

Anyone with other experience feel free to correct me if I'm wrong.
