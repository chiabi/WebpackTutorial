# 예제 4 - 로더 이해하기

이전 튜토리얼에서 [로더](#로더)에 대해 언급했습니다.
이제 자바스크립트가 아닌 파일을 다루어 보기로 하겠습니다. 스타일 로더와 CSS 로더가 필요하게 되었습니다. 먼저 로더를 설치해 봅시다:

    npm install --save-dev style-loader css-loader

설치된 CSS 로더를 포함하도록 설정 파일을 조정해 봅시다:

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
  ],
  module: {
    loaders: [{
      test: /\.css$/,
      loaders: ['style', 'css']
    }]
  }
}
```

새롭게 보이는 속성을 각각 살펴봅시다:

* [module](http://webpack.github.io/docs/configuration.html#module) - 이 옵션은 파일에 영향을 줍니다.
  * [loaders](http://webpack.github.io/docs/configuration.html#module-loaders) - 애플리케이션에서 사용할 로더를 배열로 지정합니다.
    * test - 정규식을 이용해서 로더에 사용될 파일을 검출합니다.
    * loaders - 일치하는 파일에 사용되는 로더를 호출합니다.

`webpack` 명령을 실행하면, `.css`로 확장자를 가진 파일을 `require`하는 경우,
이 파일은 `style`과 `css` 로더에 적용되고, 번들에 CSS가 추가됩니다.

로더를 가지고 있지 않은 경우, 다음과 같은 오류를 보게 될 것입니다:

```
ERROR in ./test.css
Module parse failed: /Users/Developer/workspace/tutorials/webpack/part1/example1/test.css
Line 1: Unexpected token {
You may need an appropriate loader to handle this file type.
```

**선택사항**

만약 CSS 대신 SCSS를 사용하는 경우 다음과 같이 실행해야 합니다:

    npm install --save-dev sass-loader node-sass webpack

그리고 로더는 다음과 같이 작성되어야 합니다.

```javascript
{
  test: /\.scss$/,
  loaders: ["style", "css", "sass"]
}
```

이 과정은 LESS도 비슷합니다.

중요한 것은 지정할 *순서* 가 존재한다는 것입니다. 위의 예제에서 `sass` 로더에 가장 먼저 `.scss` 파일을 적용하고,
그 다음으로 `css` 로더, 마지막에 `style` 로더에 적용합니다. 즉, 순서 패턴은 오른쪽에서 왼쪽으로 로더에 적용되는 것입니다.
