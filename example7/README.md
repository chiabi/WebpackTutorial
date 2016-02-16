# 예제 7 - 코딩 시작하기

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

흥미롭게도, [핫 모듈 교체가 작동하려면](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html#what-is-needed-to-use-it) 다음과 같은 코드를 포함해야 합니다:

```javascript
if (module.hot) {
  module.hot.accept()
}
```

모듈 또는 모듈의 부모입니다.
