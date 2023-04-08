---
title: Auto-subscriptions
---

이전 예제의 앱은 작동하지만 미묘한 버그가 있습니다. 스토어가 구독되지만 구독 취소되지는 않습니다. 구성 요소가 인스턴스화되고 여러 번 제거되면 *메모리 누수*가 발생합니다.

`App.svelte`에서 `unsubscribe`를 선언하여 시작합니다.

```js
const unsubscribe = count.subscribe(value => {
	countValue = value;
});
```
> `subscribe` 메서드를 호출하면 `unsubscribe` 함수가 반환됩니다.

이제 `unsubscribe`를 선언했지만 예를 들어 `onDestroy` [수명 주기 후크](/tutorial/ondestroy)를 통해 호출해야 합니다.

```html
<script>
	import { onDestroy } from 'svelte';
	import { count } from './stores.js';
	import Incrementer from './Incrementer.svelte';
	import Decrementer from './Decrementer.svelte';
	import Resetter from './Resetter.svelte';

	let countValue;

	const unsubscribe = count.subscribe(value => {
		countValue = value;
	});

	onDestroy(unsubscribe);
</script>

<h1>The count is {countValue}</h1>
```

그러나 특히 구성 요소가 여러 저장소에 가입하는 경우 약간의 상용구가 나오기 시작합니다. 대신, Svelte는 트릭을 사용합니다. 상점 이름 앞에 `$`를 붙여 상점 값을 참조할 수 있습니다.

```html
<script>
	import { count } from './stores.js';
	import Incrementer from './Incrementer.svelte';
	import Decrementer from './Decrementer.svelte';
	import Resetter from './Resetter.svelte';
</script>

<h1>The count is {$count}</h1>
```

> 자동 구독은 구성 요소의 최상위 범위에서 선언(또는 가져오기)된 저장소 변수에서만 작동합니다.

마크업 내에서 `$count`를 사용하는 것으로 제한되지 않습니다. 이벤트 핸들러나 반응 선언과 같은 `<script>`에서도 사용할 수 있습니다.

> `$`로 시작하는 모든 이름은 상점 값을 참조하는 것으로 간주됩니다. 이것은 사실상 예약된 문자입니다 — Svelte는 `$` 접두어로 자신의 변수를 선언하는 것을 방지합니다.
