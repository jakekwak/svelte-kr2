---
title: The class directive
---

다른 속성과 마찬가지로 다음과 같이 JavaScript 속성으로 클래스를 지정할 수 있습니다.

```html
<button
	class="{current === 'foo' ? 'selected' : ''}"
	on:click="{() => current = 'foo'}"
>foo</button>
```

이것은 Svelte가 이를 단순화하기 위한 특수 지시문을 포함하는 UI 개발의 일반적인 패턴입니다.

```html
<button
	class:selected="{current === 'foo'}"
	on:click="{() => current = 'foo'}"
>foo</button>
```

`selected` 클래스는 표현식의 값이 참일 때마다 요소에 추가되고 거짓일 때 제거됩니다.
