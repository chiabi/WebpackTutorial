# 초보자용 Webpack 튜토리얼 파트1 - Webpack 소개:zap:

Webpack을 시작하는 것은 저에게 쉽지만은 않았습니다. 그래서 친절하고 개괄적인
초보자용 Webpack 입문서를 만들었습니다. 저는 이 튜토리얼을 통해 Webpack의
사용법을 쉽게 배울수 있기를 바랍니다.

## 요구사항

적어도 Node.js와 npm의 기본을 알고 있어야 합니다.

## 목차

* [Webpack을 왜 사용하나요?](#webpack을-왜-사용하나요)
* [기본 익히기](#기본-익히기)
  * [설치하기](#설치하기)
  * [번들하기](#번들하기)
  * [로더란?](#로더란)
  * [플러그인](#플러그인)
* [설정 파일 구성](#설정-파일-구성)
  * [최소한의 예제](#최소한의-예제)
  * [플러그인 이해](#플러그인-이해)
* [조금 더 복잡한 예제](#조금-더-복잡한-예제)
  * [로더 이해하기](#로더-이해하기)
  * [플러그인 추가하기](#플러그인-추가하기)
  * [개발서버 구성하기](#개발서버-구성하기)
  * [코딩 시작하기](#코딩-시작하기)
* [결론](#결론)

## Webpack을 왜 사용하나요?

여기에 Webpack을 사용해야 할 몇 가지 현실적인 이유가 있습니다.

* 하나의 파일로 js 파일을 번들할 수 있습니다.
* 프론트엔드 코드에 npm 패키지를 사용할 수 있습니다.
* ES6/ES7 자바스크립트 코드를 작성할 수 있습니다. (Babel을 이용하여)
* 코드를 압축 또는 최적화할 수 있습니다.
* LESS/SCSS를 CSS로 돌릴 수 있습니다.
* HMR(Hot Module Replacement)을 사용할 수 있습니다.
* 자바스크립트로 모든 유형의 파일을 포함할 수 있습니다.
* 이 글에서 다루지 못한 아주 많은 고급기능이 있습니다.

##### 왜 이러한 기능이 필요한가요?

* js 파일 번들 - 자바스크립트를 모듈로 작성할 수 있습니다, 그래서 각각의 파일에 대해서 `<script>`
태그를 별도로 작성할 필요가 없습니다. (상황에 따라서 둘 이상의 js 파일이 필요한 경우 구성 가능함)

* npm 패키지 사용 - npm은 인터넷상에서 오픈소스 코드의 커다란 생태계입니다.
npm 코드를 저장할 기회가 주어지며, 원하는 프론트엔드 패키지를 가져다 쓸 수 있습니다.

* ES6/ES7 - 많은 기능을 추가되어 더 강력하고 더 쉽게 자바스크립트를 작성할 수 있습니다.
[여기에 소개하는 글](https://github.com/DrkSephy/es6-cheatsheet)이 있습니다.

* 코드 압축/최적화 - 배포되는 파일의 크기를 줄입니다. 페이지 로딩이 빨라지는 등의 장점을 포함합니다.

* LESS/SCSS를 CSS로 돌리기 - CSS를 작성하는 더 좋은 방법입니다.
[여기에 소개하는 글](http://alistapart.com/article/why-sass)이 있습니다.

* HMR 사용 - 생산성이 향상됩니다. 코드를 저장할 때 마다 페이지의 리프레시가 자동으로 이루어집니다.
코드를 작성하는 동안 페이지의 상태를 최신으로 유지해야 하는 경우 정말 편리합니다.

* 자바스크립트로 모든 유형의 파일을 포함 - 추가적인 빌드 도구의 수를 줄일 수 있고,
프로그램적으로 파일을 사용 및 수정할 수 있습니다.

## 기본 익히기

### 설치하기

Webpack의 모든 기능을 사용하려면 전역으로 설치해야 합니다:

    npm install -g webpack

그러나 Webpack의 일부 기능이나 최적화 플러그인 정도만 필요한 경우라면 로컬에 설치합니다:

    npm install --save-dev webpack

### 실행 명령

Webpack을 실행하려면:

    webpack

Webpack에서 파일의 상태가 변경되면 자동으로 빌드하려는 경우:

    webpack --watch

특정한 이름의 사용자가 정의한 Webpack 설정 파일을 사용하려면:

    webpack --config myconfig.js

### 번들하기

[예제 1](https://github.com/firejune/WebpackTutorial/tree/master/example1)

![Official Dependency Tree](http://i.imgur.com/YU4xBPQ.png)

Webpack은 공식적으로 모듈 번들러라고 합니다. 다음의 두 가지 훌륭한 글은 모듈 액세스에 대한
깊이 있는 설명과 명확한 모듈 번들링에 대하여 다루고 있습니다:
[이 것](https://medium.freecodecamp.com/javascript-modules-a-beginner-s-guide-783f7d7a5fcc#.jw1txw6uh)
과 [이 것](https://medium.com/@preethikasireddy/javascript-modules-part-2-module-bundling-5020383cf306#.lfnspler2).

간단하게 봅시다. 작동시키는 방법은 하나의 파일을 진입점으로 지정하는 것입니다.
이 파일은 트리의 루트가 될 것입니다. 그러면 `require`에 의해 다른 파일이 트리에 추가됩니다.
`webpack` 명령을 실행하면, 모든 파일과 모듈은 하나의 파일에 번들로 제공됩니다.

다음은 간단한 예제입니다:

![Dependency Tree](http://i.imgur.com/dSghwwL.png)

이 그림은 다음과 같은 디렉터리 구조를 가진다고 가정합니다:

```
MyDirectory
|- index.js
|- UIStuff.js
|- APIStuff.js
|- styles.css
|- extraFile.js
```

이것은 파일의 내용입니다.

```javascript
// index.js
require('./styles.css')
require('./UIStuff.js')
require('./APIStuff.js')

// UIStuff.js
var React = require('React')
React.createClass({
  // stuff
})

// APIStuff.js
var fetch = require('fetch') // fetch polyfill
fetch('https://google.com')
```

```css
/* styles.css */
body {
  background-color: rgb(200, 56, 97);
}
```

`webpack` 명령을 실행하면, 이 트리의 내용을 번들로 얻을 수 있겠지만,
같은 디렉터리에 있는 `extraFile.js`는 `require`에 참조되지 않았기 때문에
결코 번들의 일부가 되지 않습니다.

`bundle.js`는 다음과 같이 표시됩니다:

```javascript
// contents of styles.css
// contents of UIStuff.js + React
// contents of APIStuff.js + fetch
```

즉, 번들로 제공되는 것들은 파일의 참조를 통한 경우의 것들입니다.

### 로더란?

이미 눈치챘겠지만, 위의 예제에서 이상한 일을 저질렀습니다.
바로 CSS 파일을 자바스크립트 파일에 `require`를 사용한 것입니다.

이것은 정말 멋집니다, Webpack의 흥미로운 점은
`require`에 자바스크립트 파일 말고도 다른 것을 더 할 수 있다는 것입니다.

Webpack에는 로더라는 것이 있습니다. 이 로더를 사용하면,
`require`를 이용하여 `.css`와 `.html`, `.png` 등을 불러올 수 있습니다.

위 그림의 예를 들어 보겠습니다.

```javascript
// index.js
require('./styles.css')
```

Webpack의 구성에 [스타일-로더](https://github.com/webpack/style-loader)와
[CSS-로더](https://github.com/webpack/css-loader)를 포함하는 경우,
이것은 단지 완전히 유효하지만 않을 뿐, 실제로는 페이지에 CSS를 적용하게 됩니다.

이것은 Webpack과 함께 사용할 수 있는 수많은 로더들 중 하나의 사용 예제일 뿐입니다.

### 플러그인

플러그인은 이름에서 알 수 있듯이, Webpack에 사용할 수 있는 추가 기능입니다.
자주 사용하는 플러그인 중 하나는 `UglifyJsPlugin`입니다.
이는 자바스크립트 코드를 압축(minify)해 줍니다. 이 사용법에 대해서는
나중에 다룰 것입니다.

## 설정 파일 구성

Webpack은 박스(?) 밖에서 작동하지 않기 때문에 필요에 맞게 작성해야 합니다.
이를 위해 다음과 같은 파일을 생성할 수 있습니다.

    webpack.config.js

이것은 Webpack이 기본적으로 인식하는 파일명입니다.
다른 이름을 사용하려면 해당 파일의 이름을 지정할 수 있는 `--config` 플래그를 사용해야 합니다.

### 최소한의 예제

[예제 2](https://github.com/firejune/WebpackTutorial/tree/master/example2)

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

### 플러그인 이해

[예제 3](https://github.com/firejune/WebpackTutorial/tree/master/example3)

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

또한 [OccurenceOrderPlugin](https://webpack.github.io/docs/list-of-plugins.html#occurrenceorderplugin)을 추가할 수도 있습니다.

> 이 플러그인은 발생 횟수에 따라서 모듈 및 청크 id를 할당합니다. 자주 사용되는 id가 낮은(짧은) id를 얻습니다. 이 id는 예측(predictable)이 가능하며, ~~전체 파일 크기를 줄이는데~~(역자주: 파일 용량을 줄이는 것과는 무관함) 추천됩니다.

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

## 조금 더 복잡한 예제

추가적으로, Webpack은 자바스크립트에 관련한 단순 작업보다 더 많은 일을 할 수 있으므로, 수동으로 복사-붙여넣기 하는 일을 없애고 Webpack으로 전체 프로젝트를 관리할 수 있습니다.

다음 섹션에서는, Webpack을 사용하여 아주 간단한 웹사이트를 만들 것입니다. 예제를 수행하고자 하는 경우, 다음과 같은 구조의 디렉터리를 생성하세요.

```
MyDirectory
|- dist
|- src
   |- index.js
   |- index.html
   |- styles.css
|- package.json
|- webpack.config.js
```

#### 학습 내용

1. [로더 이해하기](#로더-이해하기) - 번들에 CSS를 추가할 수 있도록 로더를 추가해 볼 것입니다.
2. [플러그인 추가하기](#플러그인-추가하기) - HTML 파일을 생성하고 사용할 수 있도록 도와주는 플러그인을 추가해 볼 것입니다.
3. [개발서버 구성하기](#개발서버-구성하기) - `development`와 `production`을 구분한 Webpack의 구성 파일을 분할하고
`webpack-dev-server`를 이용하여 HMR을 활성화해 볼 것입니다.
4. [코딩 시작하기](#코딩-시작하기) - 실제로 자바스크립트의 일부를 작성해 볼 것입니다.

#### 로더 이해하기

[예제 4](https://github.com/firejune/WebpackTutorial/tree/master/example4)

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

#### 플러그인 추가하기

[예제 5](https://github.com/firejune/WebpackTutorial/tree/master/example5)

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

#### 개발서버 구성하기

[예제 6](https://github.com/firejune/WebpackTutorial/tree/master/example6)

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

#### 코딩 시작하기

[예제 7](https://github.com/firejune/WebpackTutorial/tree/master/example7)

많은 사람이 Webpack에 당황해할 것 같습니다. 이유는 실제 작업용 자바스크립트 코드를 작성하기까지 여태것 공부했던 여러 과정을 모두 숙지해야 하기 때문입니다; 다행히도 이 튜토리얼의 클라이막스에 도달했습니다.

`npm run dev` 명령 실행 및 `http://localhost:8080`를 하지 않았으면 수행합니다. 개발 서버에 핫-리로드를 설정하는 것은 단순히 보기만을 위한 것이 아닙니다. 프로젝트 일부를 편집하고 저장하는 매시간 변경사항을 표시하도록 브라우저는 자동으로 다시 로드합니다.

이제 이것을 프론트엔드에서 사용할 수 있는 방법을 보여주기 위해 몇몇 npm 패키지가 필요합니다.

    npm install --save pleasejs

PleaseJS는 임의 색상 발생기입니다. 특정 버튼을 이용해 div의 색상을 변경해 보겠습니다.

```javascript
// index.js

// Accept hot module reloading
if (module.hot) {
  module.hot.accept()
}

require('./styles.css') // The page is now styled
var Please = require('pleasejs')
var div = document.getElementById('color')
var button = document.getElementById('button')

function changeColor() {
  div.style.backgroundColor = Please.make_color()
}

button.addEventListener('click', changeColor)
```

흥미롭게도, [HMR이 작동하려면](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html#what-is-needed-to-use-it) 다음과 같은 코드를 포함해야 합니다:

```javascript
if (module.hot) {
  module.hot.accept()
}
```

모듈 또는 상위 모듈에서요.

이제 마지막입니다!

**노트:** 예제를 통해 CSS를 적용하면서 꺼림칙한 부분은, CSS가 자바스크립트 파일에 있다는 사실입니다. 다른 파일에 CSS를 넣는 방법에 대한 자세한 설명을 별도로 작성했습니다. [css-extract](https://github.com/firejune/WebpackTutorial/tree/master/css-extract)를 확인하세요.

## 결론

축하합니다! div의 색상을 변경하는 버튼을 만들기까지 모두 학습했습니다! Webpack, 참 훌륭하죠?

Webpack은 처음 접한 모듈 번들러입니다. 그리고 매우 유용한 도구였습니다. 파트 1에서는 가장 일반적인 사용 사례를 다루었지만, 아직 ES6과 React를 연결하여 사용하는 방법에 대해서는 다루지 않았습니다.

나중에

* 파트 2에서는 Webpack과 Babel을 함께 사용하여 ES6을 ES5로 transpile하는 방법을 살펴보겠습니다.
* 파트 3에서는 Webpack과 React + Babel을 함께 사용하는 방법을 살펴보겠습니다.

도움이 되었기를 바랍니다. :anguished:
