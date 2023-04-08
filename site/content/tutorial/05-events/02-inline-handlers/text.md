---
title: Inline handlers
---

이벤트 핸들러를 인라인으로 선언할 수도 있습니다.

```html
<div on:mousemove="{e => m = { x: e.clientX, y: e.clientY }}">
	The mouse position is {m.x} x {m.y}
</div>
```

따옴표는 선택 사항이지만 일부 환경에서 구문 강조 표시에 유용합니다.

> 일부 프레임워크에서는 특히 루프 내부에서 성능상의 이유로 인라인 이벤트 핸들러를 사용하지 않는 것이 좋습니다. 이 조언은 Svelte에는 적용되지 않습니다. 어떤 형식을 선택하든 컴파일러는 항상 올바른 작업을 수행합니다.