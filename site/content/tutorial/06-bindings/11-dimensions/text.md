---
title: Dimensions
---

모든 블록 수준 요소에는 `clientWidth`, `clientHeight`, `offsetWidth` 및 `offsetHeight` 바인딩이 있습니다.

```html
<div bind:clientWidth={w} bind:clientHeight={h}>
	<span style="font-size: {size}px">{text}</span>
</div>
```

이러한 바인딩은 읽기 전용입니다. `w` 및 `h` 값을 변경해도 아무런 영향이 없습니다.

> 요소는 [이 항목](http://www.backalleycoder.com/2013/03/18/cross-browser-event-based-element-resize-detection/)과 유사한 기술을 사용하여 측정됩니다. 약간의 오버헤드가 수반되므로 많은 수의 요소에 사용하지 않는 것이 좋습니다.
>
> `display: inline` 요소는 이 접근 방식으로 측정할 수 없습니다. 다른 요소(예: `<canvas>`)를 포함할 수 없는 요소도 마찬가지입니다. 이러한 경우 래퍼 요소를 대신 측정해야 합니다.