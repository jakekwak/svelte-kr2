---
title: The transition directive
---

요소를 DOM 안팎으로 우아하게 전환하여 보다 매력적인 사용자 인터페이스를 만들 수 있습니다. Svelte는 `transition` 지시문을 사용하여 이를 매우 쉽게 만듭니다.

먼저 `svelte/transition`에서 `fade` 기능을 가져옵니다...

```html
<script>
	import { fade } from 'svelte/transition';
	let visible = true;
</script>
```

...그런 다음 `<p>` 요소에 추가합니다.

```html
<p transition:fade>Fades in and out</p>
```