---
title: onMount
---

모든 구성 요소에는 생성될 때 시작하여 파괴될 때 끝나는 *수명 주기*가 있습니다. 수명 주기 동안 중요한 순간에 코드를 실행할 수 있는 몇 가지 기능이 있습니다.

가장 자주 사용하게 될 것은 `onMount`로 구성 요소가 처음 DOM에 렌더링된 후에 실행됩니다. 렌더링된 후 `<canvas>` 요소와 상호 작용해야 할 때 [이전에](/tutorial/bind-this) 잠시 만났습니다.

네트워크를 통해 일부 데이터를 로드하는 `onMount` 핸들러를 추가합니다.

```html
<script>
	import { onMount } from 'svelte';

	let photos = [];

	onMount(async () => {
		const res = await fetch(`/tutorial/api/album`);
		photos = await res.json();
	});
</script>
```

> `<script>`의 최상위 수준보다는 `onMount`에 `fetch`를 넣는 것이 서버측 렌더링(SSR) 때문에 권장됩니다. `onDestroy`를 제외하고 수명 주기 함수는 SSR 중에 실행되지 않습니다. 즉, 구성 요소가 DOM에 마운트된 후 느리게 로드되어야 하는 데이터를 가져오는 것을 피할 수 있습니다.

콜백이 `setTimeout`이 아닌 구성 요소 인스턴스에 바인딩되도록 구성 요소가 초기화되는 동안 수명 주기 함수를 호출해야 합니다.

`onMount` 콜백이 함수를 반환하는 경우 구성 요소가 소멸될 때 해당 함수가 호출됩니다.