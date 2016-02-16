# 예제 2 - 최소한의 예제

디렉터리 구조는 다음과 같습니다:

```
MyDirectory
|- dist
|- src
   |- index.js
|- webpack.config.js

```

다음으로 최소한의 Webpack 설정이 있습니다.

```javascript
// webpack.config.js
var path = require('path')

module.exports = {
  entry: ['./src/index'], // file extension after index is optional for .js files
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  }
}
```

새롭게 보이는 속성을 각각 살펴봅시다:

* [entry](https://webpack.github.io/docs/configuration.html#entry) - 번들의 엔트리 포인트로써 [번들하기](#번들하기)
색션에서 이미 논의했습니다. Webpack은 여러 번들을 생성하는 진입점을 허용하기 때문에 배열입니다.

* [output](https://webpack.github.io/docs/configuration.html#output) - Webpack의 최종 결과물이 되는 형태를 명시합니다.
  * [path](https://webpack.github.io/docs/configuration.html#output-path) - Webpack의 최종 결과물이 되는 형태를 명시합니다.
  * [filename](https://webpack.github.io/docs/configuration.html#output-filename) - 번들 파일의 이름을 지정합니다.

이제 `webpack` 명령을 실행하면, dit라는 폴더에 `bundle.js` 파일을 생성합니다.
