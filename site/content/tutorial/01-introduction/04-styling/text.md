---
title: Styling
---

HTML에서와 마찬가지로 컴포넌트에 `<style>` 태그를 추가할 수 있습니다. `<p>` 요소에 몇 가지 스타일을 추가해 보겠습니다:

```html
<p>This is a paragraph.</p>

<style>
	p {
		color: purple;
		font-family: 'Comic Sans MS', cursive;
		font-size: 2em;
	}
</style>
```

중요한 것은 이러한 규칙이 *구성 요소로 범위가 지정된다는 것입니다*. 다음 단계에서 볼 수 있듯이 앱의 다른 위치에서 `<p>` 요소의 스타일을 실수로 변경하지 않습니다.
