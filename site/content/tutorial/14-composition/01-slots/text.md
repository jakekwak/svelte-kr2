---
title: Slots
---

요소가 자식을 가질 수 있는 것처럼...

```html
<div>
	<p>I'm a child of the div</p>
</div>
```

...구성 요소도 마찬가지입니다. 구성 요소가 자식을 수락하려면 먼저 자식을 어디에 둘지 알아야 합니다. `<slot>` 요소로 이 작업을 수행합니다. 이것을 `Box.svelte` 안에 넣으십시오:

```html
<div class="box">
	<slot></slot>
</div>
```

이제 상자에 물건을 넣을 수 있습니다.

```html
<Box>
	<h2>Hello!</h2>
	<p>This is a box. It can contain anything.</p>
</Box>
```