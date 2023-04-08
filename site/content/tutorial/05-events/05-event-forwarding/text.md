---
title: Event forwarding
---

DOM 이벤트와 달리 구성 요소 이벤트는 *버블링*하지 않습니다. 깊이 중첩된 일부 구성 요소에서 이벤트를 수신하려면 중간 구성 요소가 이벤트를 *전달(forward)*해야 합니다.

이 경우 [이전 장](/tutorial/component-events)에서와 동일한 `App.svelte` 및 `Inner.svelte`가 있지만 이제 `<Inner/>`를 포함하는 `Outer.svelte` 구성 요소가 있습니다.

이 문제를 해결할 수 있는 한 가지 방법은 `outer.svelte`에 `createEventDispatcher`를 추가하고 `message` 이벤트를 수신하고 이에 대한 핸들러를 만드는 것입니다.

```html
<script>
	import Inner from './Inner.svelte';
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function forward(event) {
		dispatch('message', event.detail);
	}
</script>

<Inner on:message={forward}/>
```

하지만 이것은 작성해야 할 코드가 많기 때문에 Svelte는 이에 상응하는 속기를 제공합니다. 값이 없는 `on:message` 이벤트 지시문은 '모든 `message` 이벤트 전달'을 의미합니다.

```html
<script>
	import Inner from './Inner.svelte';
</script>

<Inner on:message/>
```