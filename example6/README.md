# 예제 6 - 개발서버 구성하기

이제 실제로 브라우저에서 웹사이트를 볼 수 있도록, 지금까지 만들어진 코드를 제공하는 웹서버가 필요하게 되었습니다. 편리하게도, Webpack은 `webpack-dev-server`를 제공합니다. 로컬과 글로벌에 모두 설치합니다.

    npm install -g webpack-dev-server
    npm install --save-dev webpack-dev-server

개발 서버는 작업 된 웹사이트를 브라우저에서 바로 확인할 수 있어 매우 유용하며, 더 빠른 개발을 할 수 있습니다. 기본으로는 브라우저에서 `http://localhost:8080`으로 방문할 수 있습니다. 아쉽지만, 핫-리로드 기능은 박스(?) 밖에서는 작동하지 않아서 약간의 추가 구성이 필요합니다.

이 시점에서 개발용(development)과 제품용(production)을 구분해 보겠습니다. 이 튜토리얼은 간단함을 유지하고 있으므로 큰 차이는 없지만, Webpack의 기능 설정에 관한 단적인 예입니다.
`webpack.config.dev.js`와 `webpack.config.prod.js`를 호출할 수 있도록 합니다.

```javascript
// webpack.config.dev.js
var path = require('path')
var webpack = require('webpack')
var HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  devtool: 'cheap-eval-source-map',
  entry: [
    'webpack-dev-server/client?http://localhost:8080',
    'webpack/hot/dev-server',
    './src/index'
  ],
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
  module: {
    loaders: [{
      test: /\.css$/,
      loaders: ['style', 'css']
    }]
  },
  devServer: {
    contentBase: './dist',
    hot: true
  }
}
```


**바뀐점**

1. 개발 설정에서 지속해서 다시 구축하거나 최적화하는 일은 불필요한 오버헤드가 발생하기 때문에 생략합니다. 그래서 `webpack.optimize` 플러그인이 없습니다.

2. 개발 설정은 개발 서버에 필요한 것만 작성합니다. 더 자세한 내용을 [여기](https://webpack.github.io/docs/webpack-dev-server.html)에서 볼 수 있습니다.

요약:

* entry: 두 개의 새로운 엔트리 포인트는 HMR이 가능하도록 브라우저에 서버를 연결합니다.
* devServer
  * contentBase: 브라우저에서 접근하는 파일의 위치입니다.
  * hot: HMR 사용 여부입니다.

제품용 설정의 구성은 별로 변경되지 않습니다.

```javascript
// webpack.config.prod.js
var path = require('path')
var webpack = require('webpack')
var HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  devtool: 'source-map',
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

또한, 개발용 구성과 제품용 구성 모두 새로운 속성을 추가했습니다:

* [devtool](https://webpack.github.io/docs/configuration.html#devtool) - 디버깅을 지원합니다. 오류가 발생하는 경우, 크롬 개발자 콘솔과 같은 도구를 이용하여 실수한 위치를 확인하는 데 도움됩니다.
`source-map`과 `cheap-eval-source-map`의 차이에 대해서는 문서를 읽고도 이해하기가 조금 어려웠지만, 확실히 알 수 있었던 것은 `source-map`이 제품용 모드에서 오버헤드가 많다는 점과,
`cheap-eval-source-map`이 더 작은 오버헤드를 가지며, 이것은 단지 개발을 위한 것이라는 점입니다.

개발 서버는 다음과 같이 실행합니다.

    webpack-dev-server --config webpack.config.dev.js

제품용 코드를 구축하기 위해서는 다음과 같이 실행합니다.

    webpack --config webpack.config.prod.js

이와 같은 명령을 매번 입력하지 않아도 되도록 `package.json`에 약간의 기능을 작성하는 것으로 더 간단하게 명령을 수행할 수 있습니다.

설정에 `scripts` 속성을 추가합니다.

```javascript
// package.json
{
  //...
  "scripts": {
    "build": "webpack --config webpack.config.prod.js",
    "dev"  : "webpack-dev-server --config webpack.config.dev.js"
  }
  //...
}
```

이제 더욱 간단하게 명령을 실행할 수 있습니다.

```
npm run build
npm run dev
```

`npm run dev` 명령을 실헹하고 `http://localhost:8080`으로 이동하여 작업 된 웹사이트를 볼 수 있습니다.

**노트:** 이 부분을 테스트하는 동안 `index.html` 파일을 수정할 때 서버가 핫-리로드 되지 않는 것을 깨달았습니다. 이 문제에 대한 해결책은 [html-reload](https://github.com/firejune/WebpackTutorial/tree/master/html-reload)에 있습니다. 이것은 Webpack 옵션에 대하여 조금 더 유용한 정보를 얻을 수 있어서 읽어보길 추천합니다. 너무 사소한 내용이고 튜토리얼을 너무 길게 쓰는 느낌이 들었기 때문에 별도로 구분했습니다.
