# 예제 3 - 플러그인 이해

모든 파일의 번들에 Webpack을 사용했고 모두 합쳐서 900KB 짜리 파일을 얻었다고 가정해 봅시다.
덩치가 큰 문제는 번들 파일의 압축으로 개선될 수 있습니다. 이 작업을 수행하려면 앞서 언급했던 [UglifyJsPlugin](https://webpack.github.io/docs/list-of-plugins.html#uglifyjsplugin)라는
플러그인을 사용합니다.

또한 실제로 플러그인을 사용할 수 있도록 Webpack을 로컬에 설치해야 합니다..

    npm install --save-dev webpack

이제 Webpack에서 필요로 하는 코드를 압축할 수 있습니다.

```javascript
// webpack.config.js
var path = require('path')
var webpack = require('webpack')

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
    })
  ]
}
```

새롭게 보이는 속성을 각각 살펴봅시다:

* plugins - 보유 중인 플러그인의 배열입니다.
  * [webpack.optimize.UglifyJsPlugin](https://webpack.github.io/docs/list-of-plugins.html#uglifyjsplugin) - 코드를 축소하고 경고 메시지는 표시하지 않습니다.

이제, `webpack` 명령을 실행하면, `UglifyJsPlugin`에 의해 모든 공백을 제거하는 등의 과정을 거쳐
900KB 짜리 파일을 200KB로 줄일 수 있습니다.

또한 [OrderOccurencePlugin](https://webpack.github.io/docs/list-of-plugins.html#occurrenceorderplugin)을 추가할 수도 있습니다.

> 이 플러그인은 발생 횟수에 따라서 모듈 및 청크 id를 할당합니다. 자주 사용되는 id가 낮은(짧은) id를 얻습니다. 이 id는 예측(predictable)이 가능하며, 전체 파일 크기를 줄이는데 추천됩니다.

솔직히 말해서 기반 메커니즘이 어떻게 작동하는지 잘 모르지만,
[Webpack2 베타 버전에는 기본으로 포함](https://gist.github.com/sokra/27b24881210b56bbaff7) 되어 있다고 합니다.

```javascript
// webpack.config.js
var path = require('path')
var webpack = require('webpack')

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
    new webpack.optimize.OccurenceOrderPlugin()
  ]
}
```

여기까지 자바스크립트의 번들을 압축하는 설정을 작성했습니다.
이 번들을 다른 프로젝트의 디렉터리에 붙여넣고  `<script>` 태그에 대입할 수 있습니다.
여기에서 [결론](#결론)으로 바로 넘어가도 좋습니다. *오직 자바스크립트* 에 대한
기본적인 Webpack 사용법만 필요하다면 말이죠.
