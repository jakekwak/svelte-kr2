---
title: This
---

읽기 전용 `this` 바인딩은 모든 요소(및 구성 요소)에 적용되며 렌더링된 요소에 대한 참조를 얻을 수 있습니다. 예를 들어 `<canvas>` 요소에 대한 참조를 얻을 수 있습니다.

```html
<canvas
	bind:this={canvas}
	width={32}
	height={32}
></canvas>
```

구성 요소가 마운트될 때까지 `canvas`의 값은 `undefined`이므로 논리를 `onMount` [lifecycle function](/tutorial/onmount)에 넣습니다.
