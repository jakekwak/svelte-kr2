---
title: DOM events
---

이미 간략하게 살펴본 것처럼 `on:` 지시문을 사용하여 요소의 모든 이벤트를 수신할 수 있습니다.

```html
<div on:mousemove={handleMousemove}>
	The mouse position is {m.x} x {m.y}
</div>
```