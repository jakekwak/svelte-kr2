---
title: Accessibility warnings
---

접근성(a11y로 줄임)이 항상 올바른 것은 아니지만 Svelte는 액세스할 수 없는 마크업을 작성하는 경우 컴파일 타임에 경고하여 도움을 줍니다. 그러나 많은 접근성 문제는 다른 자동화 도구를 사용하고 응용 프로그램을 수동으로 테스트해야만 런타임에 식별할 수 있음을 명심하십시오.

다음은 Svelte가 수행할 접근성 검사 목록입니다.

---

### `a11y-accesskey`

요소에 'accesskey'를 적용하지 않습니다. 액세스 키는 웹 개발자가 요소에 키보드 단축키를 할당할 수 있도록 하는 HTML 속성입니다. 스크린 리더와 키보드만 사용하는 사용자가 사용하는 키보드 단축키와 키보드 명령 간의 불일치로 인해 접근성 문제가 발생합니다. 합병증을 피하려면 액세스 키를 사용하지 않아야 합니다.

```sv
<!-- A11y: Avoid using accesskey -->
<div accessKey='z'></div>
```

---

### `a11y-aria-attributes`

예약된 특정 DOM 요소는 ARIA 역할, 상태 및 속성을 지원하지 않습니다. 이는 종종 `meta`, `html`, `script`, `style`과 같이 보이지 않기 때문입니다. 이 규칙은 이러한 DOM 요소가 `aria-*` 소품을 포함하지 않도록 강제합니다.

```sv
<!-- A11y: <meta> should not have aria-* attributes -->
<meta aria-hidden="false">
```

---

### `a11y-autofocus`

요소에 `autofocus`이 사용되지 않도록 합니다. 자동 초점 요소는 시력이 있는 사용자와 시력이 없는 사용자 모두에게 사용성 문제를 일으킬 수 있습니다.

```sv
<!-- A11y: Avoid using autofocus -->
<input autofocus>
```

---

### `a11y-click-events-have-key-events`

`on:click` 시행에는 `onKeyUp`, `onKeyDown`, `onKeyPress` 중 하나 이상이 수반됩니다. 마우스를 사용할 수 없는 신체 장애가 있는 사용자, AT 호환성 및 스크린리더 사용자에게 키보드 코딩은 중요합니다.

이는 대화형 또는 숨겨진 요소에는 적용되지 않습니다.

```sv
<!-- A11y: on:click 이벤트가 있는 가시적이고 비대화형 요소에는 on:keydown, on:keyup 또는 on:keypress 이벤트가 수반되어야 합니다. -->
<div on:click={() => {}} />
```

---

### `a11y-distracting-elements`

주의를 산만하게 하는 요소가 사용되지 않도록 합니다. 시각적으로 주의를 산만하게 할 수 있는 요소는 시각 장애가 있는 사용자에게 접근성 문제를 일으킬 수 있습니다. 이러한 요소는 사용되지 않을 가능성이 높으므로 피해야 합니다.

다음 요소는 시각적으로 주의를 산만하게 합니다: `<marquee>` 및 `<blink>`.

```sv
<!-- A11y: Avoid <marquee> elements -->
<marquee />
```

---

### `a11y-hidden`

특정 DOM 요소는 스크린 리더 탐색에 유용하며 숨겨서는 안 됩니다.

```sv
<!-- A11y: <h2> element should not be hidden -->
<h2 aria-hidden="true">invisible header</h2>
```

---

### `a11y-img-redundant-alt`

img alt 속성에 image, picture 또는 photo라는 단어가 포함되어 있지 않습니다. 스크린 리더는 이미 `img` 요소를 이미지로 알려줍니다. _image_, _photo_ 및/또는 _picture_와 같은 단어를 사용할 필요가 없습니다.

```sv
<img src="foo" alt="Foo eating a sandwich." />

<!-- aria-hidden, won't be announced by screen reader -->
<img src="bar" aria-hidden="true" alt="Picture of me taking a photo of an image" />

<!-- A11y: Screen readers already announce <img> elements as an image. -->
<img src="foo" alt="Photo of foo being weird." />

<!-- A11y: Screen readers already announce <img> elements as an image. -->
<img src="bar" alt="Image of me at a bar!" />

<!-- A11y: Screen readers already announce <img> elements as an image. -->
<img src="foo" alt="Picture of baz fixing a bug." />
```

---

### `a11y-incorrect-aria-attribute-type`

올바른 유형의 값만 aria 속성에 사용되도록 합니다. 예를 들어 `aria-hidden`은 부울만 받아야 합니다.

```sv
<!-- A11y: The value of 'aria-hidden' must be exactly one of true or false -->
<div aria-hidden="yes"/>
```

---

### `a11y-invalid-attribute`

접근성에 중요한 속성이 유효한 값을 갖도록 강제합니다. 예를 들어 `href`는 비어 있거나 `'#'` 또는 `javascript:`가 아니어야 합니다.

```sv
<!-- A11y: '' is not a valid href attribute -->
<a href=''>invalid</a>
```

---

### `a11y-label-has-associated-control`

레이블 태그에 텍스트 레이블과 연결된 컨트롤이 있는지 확인합니다.

레이블을 컨트롤과 연결하는 데 지원되는 두 가지 방법이 있습니다.

- 컨트롤을 레이블 태그로 래핑합니다.
- 레이블에 `for`를 추가하고 페이지의 입력 ID를 할당합니다.

```sv
<label for="id">B</label>

<label>C <input type="text" /></label>

<!-- A11y: A form label must be associated with a control. -->
<label>A</label>
```

---

### `a11y-media-has-caption`

미디어에 캡션을 제공하는 것은 청각 장애인 사용자가 따라가는 데 필수적입니다. 캡션은 대화, 음향 효과, 관련 음악 신호 및 기타 관련 오디오 정보의 전사 또는 번역이어야 합니다. 이것은 접근성에 중요할 뿐만 아니라 미디어를 사용할 수 없는 경우 모든 사용자에게 유용할 수 있습니다(이미지를 로드할 수 없을 때 이미지의 `alt` 텍스트와 유사).

캡션에는 해당 미디어를 이해하는 데 중요하고 관련된 모든 정보가 포함되어야 합니다. 이는 캡션이 미디어 콘텐츠 대화의 1:1 매핑이 아님을 의미할 수 있습니다. 그러나 `muted` 속성이 있는 동영상 구성요소에는 캡션이 필요하지 않습니다.

```sv
<video><track kind="captions"/></video>

<audio muted></audio>

<!-- A11y: Media elements must have a <track kind=\"captions\"> -->
<video></video>

<!-- A11y: Media elements must have a <track kind=\"captions\"> -->
<video><track /></video>
```

---

### `a11y-misplaced-role`

예약된 특정 DOM 요소는 ARIA 역할, 상태 및 속성을 지원하지 않습니다. 이는 종종 `meta`, `html`, `script`, `style`과 같이 보이지 않기 때문입니다. 이 규칙은 이러한 DOM 요소가 `role` 소품을 포함하지 않도록 강제합니다.

```sv
<!-- A11y: <meta> should not have role attribute -->
<meta role="tooltip">
```

---

### `a11y-misplaced-scope`

범위 속성은 `<th>` 요소에만 사용해야 합니다.

```sv
<!-- A11y: The scope attribute should only be used with <th> elements -->
<div scope="row" />
```

---

### `a11y-missing-attribute`

접근성에 필요한 속성이 요소에 존재하도록 강제합니다. 여기에는 다음 검사가 포함됩니다.

- `<a>`에는 href가 있어야 합니다([단편 정의 태그](https://github.com/sveltejs/svelte/issues/4697)가 아닌 경우).
- `<area>`에는 alt, aria-label 또는 aria-labelledby가 있어야 합니다.
- `<html>`에는 lang이 있어야 합니다.
- `<iframe>`에는 제목이 있어야 합니다.
- `<img>`에는 alt가 있어야 합니다.
- `<object>`에는 제목, aria-label 또는 aria-labelledby가 있어야 합니다.
- `<input type="image">`에는 alt, aria-label 또는 aria-labelledby가 있어야 합니다.

```sv
<!-- A11y: <input type=\"image\"> element should have an alt, aria-label or aria-labelledby attribute -->
<input type="image">

<!-- A11y: <html> element should have a lang attribute -->
<html></html>

<!-- A11y: <a> element should have an href attribute -->
<a>text</a>
```

---

### `a11y-missing-content`

제목 요소(`h1`, `h2` 등)와 앵커에 콘텐츠가 있고 스크린 리더에서 콘텐츠에 액세스할 수 있도록 강제합니다.

```sv
<!-- A11y: <a> element should have child content -->
<a href='/foo'></a>

<!-- A11y: <h1> element should have child content -->
<h1></h1>
```

---

### `a11y-mouse-events-have-key-events`

`on:mouseover` 및 `on:mouseout`에는 각각 `on:focus` 및 `on:blur`가 수반되도록 합니다. 이렇게 하면 이러한 마우스 이벤트에 의해 트리거되는 모든 기능에 키보드 사용자도 액세스할 수 있습니다.

```sv
<!-- A11y: on:mouseover must be accompanied by on:focus -->
<div on:mouseover={handleMouseover} />

<!-- A11y: on:mouseout must be accompanied by on:blur -->
<div on:mouseout={handleMouseout} />
```

---

### `a11y-no-redundant-roles`

일부 HTML 요소에는 기본 ARIA 역할이 있습니다. 이러한 요소에 브라우저에서 이미 설정한 [아무런 효과도 없고](https://www.w3.org/TR/using-aria/#aria-does-nothing) 중복되는 ARIA 역할을 부여합니다.

```sv
<!-- A11y: Redundant role 'button' -->
<button role="button" />

<!-- A11y: Redundant role 'img' -->
<img role="img" src="foo.jpg" />
```

---

### `a11y-no-interactive-element-to-noninteractive-role`

[WAI-ARIA](https://www.w3.org/TR/wai-aria-1.1/#usage_intro) 역할은 대화형 요소를 비대화형 요소로 변환하는 데 사용해서는 안 됩니다. 비대화형 ARIA 역할에는 `article`, `banner`, `complementary`, `img`, `listitem`, `main`, `region` 및 `tooltip`이 포함됩니다.

```sv
<!-- A11y: <textarea> cannot have role 'listitem' -->
<textarea role="listitem" />
```

---

### `a11y-no-noninteractive-tabindex`

Tab 키 탐색은 상호 작용할 수 있는 페이지의 요소로 제한되어야 합니다.

```sv
<!-- A11y: noninteractive element cannot have positive tabIndex value -->
<div tabindex='0' />
```

---

### `a11y-positive-tabindex`

긍정적인 `tabindex` 속성 값을 사용하지 마세요. 이렇게 하면 예상되는 탭 순서에서 요소가 이동되어 키보드 사용자에게 혼란스러운 경험이 됩니다.

```sv
<!-- A11y: avoid tabindex values above zero -->
<div tabindex='1'/>
```

---

### `a11y-role-has-required-aria-props`

ARIA 역할이 있는 요소에는 해당 역할에 필요한 모든 속성이 있어야 합니다.

```sv
<!-- A11y: A11y: Elements with the ARIA role "checkbox" must have the following attributes defined: "aria-checked" -->
<span role="checkbox" aria-labelledby="foo" tabindex="0"></span>
```

---

### `a11y-structure`

특정 DOM 요소가 올바른 구조를 갖도록 합니다.

```sv
<!-- A11y: <figcaption> must be an immediate child of <figure> -->
<div>
	<figcaption>Image caption</figcaption>
</div>
```

---

### `a11y-unknown-aria-attribute`

알려진 ARIA 속성만 사용되도록 합니다. 이는 [WAI-ARIA 상태 및 속성 사양](https://www.w3.org/WAI/PF/aria-1.1/states_and_properties)을 기반으로 합니다.

```sv
<!-- A11y: Unknown aria attribute 'aria-labeledby' (did you mean 'labelledby'?) -->
<input type="image" aria-labeledby="foo">
```

---

### `a11y-unknown-role`

ARIA 역할이 있는 요소는 유효한 비추상 ARIA 역할을 사용해야 합니다. 역할 정의에 대한 참조는 [WAI-ARIA](https://www.w3.org/TR/wai-aria/#role_definitions) 사이트에서 찾을 수 있습니다.

```sv
<!-- A11y: Unknown role 'toooltip' (did you mean 'tooltip'?) -->
<div role="toooltip"></div>
```
