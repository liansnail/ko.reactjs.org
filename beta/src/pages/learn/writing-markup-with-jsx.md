---
title: JSX로 마크업(Markup) 작성하기
---

<Intro>

JSX는 자바스크립트(JavaScript) 파일 내에서 유사-HTML 마크업을 작성하게끔 해주는 자바스크립트(JavaScript) 확장 문법입니다. 컴포넌트를 작성하는 다른 방법들도 있지만 대부분의 React 개발자들은 JSX의 간결함을 선호하고 대부분의 코드 베이스(codebase)에서 JSX를 사용합니다.

</Intro>

<YouWillLearn>

* 왜 React가 마크업과 렌더링(rendering) 로직을 섞었는지에 대해 배웁니다.
* HTML과 JSX가 어떻게 다른지에 대해 배웁니다.
* JSX로 정보를 어떻게 화면에 보여줄지에 대해 배웁니다.

</YouWillLearn>

## JSX: JavaScript에 마크업(Markup)을 더하다 {/*jsx-putting-markup-into-javascript*/}

웹(Web)은 HTML, CSS 그리고 JavaScript로 만들어져 왔습니다. 오랜 시간 동안 웹(Web) 개발자들은 주로 파일을 나누어서 내용은 HTML에 디자인은 CSS로 로직은 자바스크립트(JavaScript)에 두었습니다! 페이지의 로직은 별도의 자바스크립트(JavaScript)에 있는 반면 내용은 HTML에 있었습니다:

![독립된 파일로 존재하는 HTML과 JavaScript](/images/docs/illustrations/i_html_js.svg)

하지만 웹(Web)이 더 상호적(interactive)으로 되면서 점점 로직이 내용을 결정했습니다. 자바스크립트(JavaScript)가 HTML 역할을 하게 됐습니다! 이것이 왜 **React에서, 렌더링(rendering) 로직과 마크업이 하나의 컴포넌트라는 공간에 공존하는 이유입니다!**

![마크업이 흩뿌려진 JavaScript 함수](/images/docs/illustrations/i_jsx.svg)

버튼의 렌더링(rendering) 로직과 마크업을 같이 두는 것은 매 수정마다 조화를 이루어 잘 동작하고 있다는 확신을 줍니다. 반대로 말하자면 버튼의 마크업과 사이드 바의 마크업같이 관련이 없는 정보들은 독립적으로 존재하여 각각 더 안전하게 수정할 수 있게 해줍니다.

각 React 컴포넌트는 React가 브라우저에 표현할(render) 마크업을 가질 수 있는 자바스크립트(JavaScript) 함수입니다. 그러한 마크업을 표현하기 위해서 React 컴포넌트는 JSX라는 확장 문법을 사용합니다. JSX는 HTML과 많이 닮았지만 더 엄격하고 동적인 정보를 화면에 보여줄 수 있습니다. JSX를 이해할 수 있는 가장 좋은 방법은 HTML을 JSX로 변환해 보는 것입니다.

<Note>

[JSX와 React는 두 개의 분리된 것이고](/blog/2020/09/22/introducing-the-new-jsx-transform.html#whats-a-jsx-transform) 여러분은 이 둘을 각각 독립적으로 사용_가능_합니다.

</Note>

## HTML에서 JSX로 변환하기 {/*converting-html-to-jsx*/}

여러분에게 어떤 (완전히 유효한) HTML이 있다고 가정해 봅시다:

```html
<h1>Hedy Lamarr's Todos</h1>
<img 
  src="https://i.imgur.com/yXOvdOSs.jpg" 
  alt="Hedy Lamarr" 
  class="photo"
>
<ul>
    <li>Invent new traffic lights
    <li>Rehearse a movie scene
    <li>Improve the spectrum technology
</ul>
```

그리고 여러분은 이 HTML을 여러분의 컴포넌트에 넣기를 원합니다:

```js
export default function TodoList() {
  return (
    // ???
  )
}
```

만약 여러분이 그대로 복사하고 붙여 넣는다면 제대로 동작하지 않을 겁니다:


<Sandpack>

```js
export default function TodoList() {
  return (
    // This doesn't quite work!
    <h1>Hedy Lamarr's Todos</h1>
    <img 
      src="https://i.imgur.com/yXOvdOSs.jpg" 
      alt="Hedy Lamarr" 
      class="photo"
    >
    <ul>
      <li>Invent new traffic lights
      <li>Rehearse a movie scene
      <li>Improve the spectrum technology
    </ul>
  );
}
```

```css
img { height: 90px }
```

</Sandpack>

JSX는 HTML보다 더 엄격하고 규칙이 조금 더 있기 때문입니다! 상위의 에러 메시지를 읽어보시면 어떻게 마크업을 고쳐야 할지에 대한 가이드를 해줄 겁니다. 하위의 가이드를 따라도 좋습니다.

<Note>

대부분의 경우, 화면상의 React 에러 메시지는 여러분이 문제가 어디에서 발생됐는지 찾게 도와줄 겁니다. 막힐 경우 에러 메시지를 읽어보세요!

</Note>

## JSX 작성 규칙 {/*the-rules-of-jsx*/}

### 1. 하나의 루트(root) 엘리먼트(Element)를 반환하세요 {/*1-return-a-single-root-element*/}

하나의 컴포넌트에서 두 개 이상의 엘리먼트(element)를 반환하기 위해서는 **하나의 부모 태그로 감싸주세요**.

예를 들어 `<div>`를 사용할 수 있습니다:

```js {1,11}
<div>
  <h1>Hedy Lamarr's Todos</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
  >
  <ul>
    ...
  </ul>
</div>
```


`<div>`를 마크업에 추가로 덧붙이고 싶지 않으면, `<>`과 `</>`를 대신 작성하시면 됩니다:

```js {1,11}
<>
  <h1>Hedy Lamarr's Todos</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
  >
  <ul>
    ...
  </ul>
</>
```

이 빈 태그는 *[React fragment](TODO)*로 불립니다. React fragments는 브라우저 HTML 트리에 흔적을 남기지 않고 HTML 엘리먼트(Element)들을 그룹화해줍니다.

<DeepDive title="왜 두 개 이상의 JSX 태그는 감싸져야 하나요?">

JSX는 HTML과 유사해 보이지만 내부적으로 자바스크립트(JavaScript) 객체로 변형됩니다. 하나의 배열(array)로 감싸지 않는다면 두 객체(Object)를 하나의 함수에서 반환할 수 없습니다. 이것이 왜 태그나 fragment로 감싸지 않고서는 두 개의 JSX 태그를 반환할 수 없는지를 설명해 줍니다.


</DeepDive>

### 2. 모든 태그를 닫으세요 {/*2-close-all-the-tags*/}

JSX에서 태그들은 빠짐없이 모두 닫혀있어야 합니다: `<img>` 같이 자체 닫기 태그(self-closing tag)는 반드시 `<img />`로 되어야 하고 `<li>oranges` 같이 감싸는 태그는 반드시 `<li>oranges</li>` 이렇게 작성되어야 합니다.

Hedy Lamarr 이미지 태그와 리스트 태그들은 이렇게 닫혀있어야 합니다:

```js {2-6,8-10}
<>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
   />
  <ul>
    <li>Invent new traffic lights</li>
    <li>Rehearse a movie scene</li>
    <li>Improve the spectrum technology</li>
  </ul>
</>
```

### 3. <s>모든</s> 대부분의 것을 카멜 케이스(camelCase)으로 작성하세요! {/*3-camelcase-salls-most-of-the-things*/}

JSX가 자바스크립트(JavaScript)로 바뀌고 JSX에 있던 어트리뷰트(attribute)들은 자바스크립트 객체(object)의 키가 됩니다. 여러분의 컴포넌트에서 어트리뷰트(attribute)들을 변수처럼 읽어드릴 일이 많을 겁니다. 하지만 자바스크립트(JavaScript)는 변수명에 제약이 있습니다. 예를 들어 변수명은 대시('-')를  포함할 수 없고 `class` 같은 단어는 가질 수 없습니다.

이것이 React의 많은 HTML과 SVG 어트리뷰트(attribute)가 카멜 케이스(camelCase)로 쓰인 이유입니다. 예를 들어서 `stroke-width` 대신에 `strokeWidth`를 사용합니다. `class`는 예약어(reserved word)이기 때문에 React에서는  [해당되는 DOM 프로퍼티(property)](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)의 이름을 따서 `className`으로 대신 작성합니다:

```js {4}
<img 
  src="https://i.imgur.com/yXOvdOSs.jpg" 
  alt="Hedy Lamarr" 
  className="photo"
/>
```

여러분은 [React DOM 엘리먼트(Element)의 모든 어트리뷰트(attribute)를](TODO) 확인할 수 있습니다. 만에 하나 여러분이 잘못 작성했더라도 걱정하지 마세요-React가 [브라우저 콘솔](https://developer.mozilla.org/docs/Tools/Browser_Console)에 교정을 담은 메시지를 보여줄 겁니다.

<Gotcha>

역사적인 이유로 HTML 내에서 [`aria-*`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA) 와 [`data-*`](https://developer.mozilla.org/docs/Learn/HTML/Howto/Use_data_attributes) 어트리뷰트(attribute)들은 대시('-')와 함께 쓰여집니다.

</Gotcha>

### Pro-tip: JSX 변환 도구를 사용하세요 {/*pro-tip-use-a-jsx-converter*/}

기존 마크업의 모든 어트리뷰트(attribute)들을 변환하는 것은 지루한 작업이 될 수 있습니다! 기존 HTML, SVG를 JSX로 바꾸기 위해 [변환 도구](https://transform.tools/html-to-jsx) 사용을 추천합니다. 변환 도구는 실제로 꽤 유용하지만 그래도 여러분이 JSX를 문제없이 작성하기 위해서 JSX 뒤에 무슨 일이 일어나는지 이해하는 것은 가치 있는 일입니다.

이것이 여러분의 최종 결과물입니다:

<Sandpack>

```js
export default function TodoList() {
  return (
    <>
      <h1>Hedy Lamarr's Todos</h1>
      <img 
        src="https://i.imgur.com/yXOvdOSs.jpg" 
        alt="Hedy Lamarr" 
        className="photo" 
      />
      <ul>
        <li>Invent new traffic lights</li>
        <li>Rehearse a movie scene</li>
        <li>Improve the spectrum technology</li>
      </ul>
    </>
  );
}
```

```css
img { height: 90px }
```

</Sandpack>

<Recap>

이제 여러분은 JSX가 컴포넌트에서 왜 존재하고 어떻게 사용하는지 알았습니다:

* 렌더링(rendering) 로직과 마크업은 서로 관계가 있기 때문에 React 컴포넌트는 이 둘을 하나로 그룹화했습니다.
* JSX는 몇 가지의 차이점이 있지만 HTML과 비슷합니다. 필요에 의해 [변환 도구](https://transform.tools/html-to-jsx)를 사용할 수 있습니다.
* 에러 메시지는 여러분이 마크업을 고칠 수 있도록 올바른 방향을 제시할 겁니다.

</Recap>



<Challenges>

### HTML을 JSX로 변환하세요 {/*convert-some-html-to-jsx*/}

HTML이 컴포넌트에 붙여 넣어졌지만 올바른 형식의 JSX가 아닙니다. 고쳐보세요:

<Sandpack>

```js
export default function Bio() {
  return (
    <div class="intro">
      <h1>Welcome to my website!</h1>
    </div>
    <p class="summary">
      You can find my thoughts here.
      <br><br>
      <b>And <i>pictures</b></i> of scientists!
    </p>
  );
}
```

```css
.intro {
  background-image: linear-gradient(to left, violet, indigo, blue, green, yellow, orange, red);
  background-clip: text;
  color: transparent;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.summary {
  padding: 20px;
  border: 10px solid gold;
}
```

</Sandpack>

직접 변환을 하거나 변환 도구를 사용할지는 여러분한테 달렸습니다!

<Solution>

<Sandpack>

```js
export default function Bio() {
  return (
    <div>
      <div className="intro">
        <h1>Welcome to my website!</h1>
      </div>
      <p className="summary">
        You can find my thoughts here.
        <br /><br />
        <b>And <i>pictures</i></b> of scientists!
      </p>
    </div>
  );
}
```

```css
.intro {
  background-image: linear-gradient(to left, violet, indigo, blue, green, yellow, orange, red);
  background-clip: text;
  color: transparent;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.summary {
  padding: 20px;
  border: 10px solid gold;
}
```

</Sandpack>

</Solution>

</Challenges>