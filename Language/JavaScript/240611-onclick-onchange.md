## **❓ 오늘의 궁금증**

### **onClick과 onChage가 무엇이 다른가?**

특히 이 `<button>` 에 달려 있는 이벤트들이 헷갈린다. onClick과 onChange 무엇이 다를까 알아보자

### **`onclick`**

사용자가 클릭했을 때의 시점

```jsx
// html 방식
<button onclick="myFunction()">Click me</button>

// react 방식<button onClick={myFunction}>Click me</button>
```

### **`onchange`**

onchange는 말 그대로, 변경이 되었을 때 감지되는 것으로, click도 하나의 변경으로 볼 수 있는 거 아니야? 라는 생각에 헷갈려 했었는데, click 은 마우스 왼쪽을 누른 순간이고, onchange는 내용이 변경되었을 경우다. input에 글자를 한 글자 한 글자씩 추가로 적을 때마다 상태(글자)는 달라진다. 즉 `change`된 상태로 onchange를 사용하면 된다.

```jsx
// html
<input onchange="handleChange()"/>

// react
<input onChange={handleChange}>
```
<br>

## **➕ 궁금증**

### **onClick / onclick & onchange / onChange?**

그중에 나는 왜 검색을 해보면, 어디는 onclick이고 onClick일까? 왜 어떤건 다 소문자고 어떤 건 카멜 케이스일까? 궁금증이 유발되었다.

위에 예시를 통해서 알 수 있는 데, React에서는 카멜 케이스를 사용해야한다. 그래서 onclick이던, onchange이던 onClick, onChange를 각각 써야 한다.

프론트에 대해 모르는 백엔드 개발자가 JavaScript를 하니까 이런 기본적인 것도 모르는 것 같다. 웹은 하나라고 생각한다. 백엔드 개발자가 백엔드 파트만 아는 건 개인적으로 안된다고 생각이 든다.