# 예제 1 - 번들링과 로더

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
