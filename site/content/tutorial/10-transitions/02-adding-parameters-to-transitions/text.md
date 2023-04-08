---
title: Adding parameters
---

전환 함수는 매개변수를 받을 수 있습니다. `fade` 전환을 `fly`로 교체...

```html
<script>
	import { fly } from 'svelte/transition';
	let visible = true;
</script>
```

...몇 가지 옵션과 함께 `<p>`에 적용합니다.

```html
<p transition:fly="{{ y: 200, duration: 2000 }}">
	Flies in and out
</p>
```

전환은 *되돌릴 수 있음*에 유의하십시오. 전환이 진행되는 동안 확인란을 전환하면 시작이나 끝이 아닌 현재 지점에서 전환됩니다.