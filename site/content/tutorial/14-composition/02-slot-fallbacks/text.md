---
title: Slot fallbacks
---

구성 요소는 `<slot>` 요소 안에 내용을 넣어 비어 있는 모든 슬롯에 대해 *대체*를 지정할 수 있습니다.

```html
<div class="box">
	<slot>
		<em>no content was provided</em>
	</slot>
</div>
```

이제 자식 없이 `<Box>` 인스턴스를 만들 수 있습니다.

```html
<Box>
	<h2>Hello!</h2>
	<p>This is a box. It can contain anything.</p>
</Box>

<Box/>
```
