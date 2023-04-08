---
title: Spring
---

`spring` 함수는 자주 변경되는 값에 더 잘 작동하는 `tweened`의 대안입니다.

이 예에는 두 개의 저장소가 있습니다. 하나는 원의 좌표를 나타내고 다른 하나는 크기를 나타냅니다. 스프링으로 변환해 보겠습니다.

```html
<script>
	import { spring } from 'svelte/motion';

	let coords = spring({ x: 50, y: 50 });
	let size = spring(10);
</script>
```

두 스프링 모두 기본 `stiffness` 및 `damping` 값을 가지고 있으며 스프링의 탄력성을 제어합니다. 자체 초기 값을 지정할 수 있습니다.

```js
let coords = spring({ x: 50, y: 50 }, {
	stiffness: 0.1,
	damping: 0.25
});
```

마우스를 움직이고 슬라이더를 드래그하여 스프링의 동작에 어떤 영향을 미치는지 느껴보십시오. 스프링이 움직이는 동안 값을 조정할 수 있습니다.

자세한 내용은 [API 참조](/docs#run-time-svelte-motion-spring)를 참조하세요.
