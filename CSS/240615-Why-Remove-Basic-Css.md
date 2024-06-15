## ❓ 오늘의 궁금증

Vanilla Js 수업을 듣다가 궁금한 점이 발생했다. 공부하는 부분이 

브라우저에서 기본적으로 제공해주는 CSS가 있으므로 아래의 코드를 통해 없애주는 것이 좋다는 데, 기본적으로 제공해주는 CSS가 있는 이유가 뭘까?

css 방식
```css
/* http://meyerweb.com/eric/tools/css/reset/
v2.0 | 20110126
License: none (public domain)
*/

html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
margin: 0;
padding: 0;
border: 0;
font-size: 100%;
font: inherit;
vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure,
footer, header, hgroup, menu, nav, section {
display: block;
}
body {
line-height: 1;
}
ol, ul {
list-style: none;
}
blockquote, q {
quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
content: '';
content: none;
}
table {
border-collapse: collapse;
border-spacing: 0;
}
```

React npm ```styled-reset``` 방식
```javascript
import * as React from 'react'
import { Reset } from 'styled-reset'

const App = () => (
  <React.Fragment>
    <Reset />
    <div>Hi, I'm an app!</div>
  </React.Fragment>
)
```

### 궁금증 해결
CSS의 적용되는 절차를 확인하면 알 수 있는 문제였다.
1. 웹 개발자가 지정한 스타일을 먼저 적용
2. 사용자가 지정한 스타일
3. 브라우저가 지정한 기본 스타일 적용
 
 1 -> 2 -> 3 순서대로 CSS는 적용된다. 

브라우저마다, 취하는 CSS 틀이 있을 것이다. 만약 웨일에서는 
```button```이 가로, 세로 32px, 20px이 기본으로 하고, 크롬에서는 ```button```이 가로, 세로 22px, 30px으로 기본으로 한다고 치면, 우리가 ```button```의 속성을 따로 설정하지 않았을 때 브라우저가 설정된 코드의 button 속성이 적용되는 것이다.
그래서 위의 코드를 적용하게 되면 1번 웹 개발자가 지정한 스타일 먼저 적용으로 기본 스타일을 해제 할 수 있다.
