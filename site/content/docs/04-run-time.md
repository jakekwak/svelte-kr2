---
title: Run time
---


### `svelte`

`svelte` 패키지는 [수명 주기 기능](/tutorial/onmount) 및 [context API](/tutorial/context-api)를 노출합니다.

#### `onMount`

```js
onMount(callback: () => void)
```
```js
onMount(callback: () => () => void)
```

---

'onMount' 함수는 컴포넌트가 DOM에 마운트되자마자 실행되도록 콜백을 예약합니다. 구성 요소의 초기화 중에 호출해야 합니다(그러나 구성 요소 *내부*에 있을 필요는 없습니다. 외부 모듈에서 호출할 수 있습니다).

`onMount`는 [서버측 구성요소](/docs#run-time-server-side-component-api) 내에서 실행되지 않습니다.

```sv
<script>
	import { onMount } from 'svelte';

	onMount(() => {
		console.log('the component has mounted');
	});
</script>
```

---

함수가 `onMount`에서 반환되면 구성 요소가 마운트 해제될 때 호출됩니다.

```sv
<script>
	import { onMount } from 'svelte';

	onMount(() => {
		const interval = setInterval(() => {
			console.log('beep');
		}, 1000);

		return () => clearInterval(interval);
	});
</script>
```

> 이 동작은 `onMount`에 전달된 함수가 *동기적으로* 값을 반환할 때만 작동합니다. `async` 함수는 항상 `Promise`를 반환하므로 *동기적으로* 함수를 반환할 수 없습니다.

#### `beforeUpdate`

```js
beforeUpdate(callback: () => void)
```

---

상태 변경 후 구성 요소가 업데이트되기 직전에 실행되도록 콜백을 예약합니다.

> 콜백이 처음 실행되는 시간은 초기 `onMount` 이전입니다.

```sv
<script>
	import { beforeUpdate } from 'svelte';

	beforeUpdate(() => {
		console.log('the component is about to update');
	});
</script>
```

#### `afterUpdate`

```js
afterUpdate(callback: () => void)
```

---

구성 요소가 업데이트된 직후 콜백이 실행되도록 예약합니다.

> 콜백이 처음 실행되는 시간은 초기 `onMount` 이후입니다.

```sv
<script>
	import { afterUpdate } from 'svelte';

	afterUpdate(() => {
		console.log('the component just updated');
	});
</script>
```

#### `onDestroy`

```js
onDestroy(callback: () => void)
```

---

구성 요소가 마운트 해제되기 직전에 실행되도록 콜백을 예약합니다.

`onMount`, `beforeUpdate`, `afterUpdate` 및 `onDestroy` 중에서 서버 측 구성 요소 내부에서 실행되는 유일한 것입니다.

```sv
<script>
	import { onDestroy } from 'svelte';

	onDestroy(() => {
		console.log('the component is being destroyed');
	});
</script>
```

#### `tick`

```js
promise: Promise = tick()
```

---

보류 중인 상태 변경이 적용되면 해결되는 약속을 반환하거나, 변경 사항이 없는 경우 다음 마이크로태스크에서 해결되는 약속을 반환합니다.

```sv
<script>
	import { beforeUpdate, tick } from 'svelte';

	beforeUpdate(async () => {
		console.log('the component is about to update');
		await tick();
		console.log('the component just updated');
	});
</script>
```

#### `setContext`

```js
setContext(key: any, context: any)
```

---

임의의 `context` 개체를 현재 구성 요소 및 지정된 `key`와 연결하고 해당 개체를 반환합니다. 그런 다음 'getContext'를 사용하여 구성 요소(슬롯된 콘텐츠 포함)의 자식에서 컨텍스트를 사용할 수 있습니다.

수명 주기 함수와 마찬가지로 구성 요소 초기화 중에 호출해야 합니다.

```sv
<script>
	import { setContext } from 'svelte';

	setContext('answer', 42);
</script>
```

> 컨텍스트는 본질적으로 반응적이지 않습니다. 컨텍스트에서 반응적 값이 필요한 경우 저장소를 컨텍스트로 전달할 수 있으며 이는 반응 *합니다*.

#### `getContext`

```js
context: any = getContext(key: any)
```

---

지정된 `키`를 사용하여 가장 가까운 상위 구성 요소에 속하는 컨텍스트를 검색합니다. 구성 요소 초기화 중에 호출해야 합니다.

```sv
<script>
	import { getContext } from 'svelte';

	const answer = getContext('answer');
</script>
```

#### `hasContext`

```js
hasContext: boolean = hasContext(key: any)
```

---

상위 구성 요소의 컨텍스트에서 지정된 '키'가 설정되었는지 여부를 확인합니다. 구성 요소 초기화 중에 호출해야 합니다.

```sv
<script>
	import { hasContext } from 'svelte';

	if (hasContext('answer')) {
		// do something
	}
</script>
```

#### `getAllContexts`

```js
contexts: Map<any, any> = getAllContexts()
```

---

가장 가까운 부모 구성 요소에 속하는 전체 컨텍스트 맵을 검색합니다. 구성 요소 초기화 중에 호출해야 합니다. 예를 들어 프로그래밍 방식으로 구성 요소를 만들고 기존 컨텍스트를 전달하려는 경우에 유용합니다.

```sv
<script>
	import { getAllContexts } from 'svelte';

	const contexts = getAllContexts();
</script>
```

#### `createEventDispatcher`

```js
dispatch: ((name: string, detail?: any, options?: DispatchOptions) => boolean) = createEventDispatcher();
```

---

[구성 요소 이벤트](/docs#template-syntax-component-directives-on-eventname)를 전달하는 데 사용할 수 있는 이벤트 전달자를 만듭니다. 이벤트 디스패처는 `name`과 `detail`의 두 인수를 사용할 수 있는 함수입니다.

`createEventDispatcher`로 생성된 구성 요소 이벤트는 [CustomEvent](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent)를 생성합니다. 이러한 이벤트는 [버블링](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#Event_bubbling_and_capture)하지 않습니다. `detail` 인수는 [CustomEvent.detail](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/detail) 속성에 해당하며 모든 유형의 데이터를 포함할 수 있습니다.

```sv
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
</script>

<button on:click="{() => dispatch('notify', 'detail value')}">Fire Event</button>
```

---

자식 구성 요소에서 전달된 이벤트는 부모 구성 요소에서 수신할 수 있습니다. 이벤트가 발송되었을 때 제공된 모든 데이터는 이벤트 객체의 `detail` 속성에서 사용할 수 있습니다.

```sv
<script>
	function callbackFunction(event) {
		console.log(`Notify fired! Detail: ${event.detail}`)
	}
</script>

<Child on:notify="{callbackFunction}"/>
```

---

디스패치 함수에 세 번째 매개변수를 전달하여 이벤트를 취소할 수 있습니다. 이 함수는 이벤트가 'event.preventDefault()'로 취소되면 'false'를 반환하고 그렇지 않으면 'true'를 반환합니다.

```sv
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function notify() {
		const shouldContinue = dispatch('notify', 'detail value', { cancelable: true });
		if (shouldContinue) {
			// no one called preventDefault
		} else {
			// a listener called preventDefault
		}
	}
</script>
```

### `svelte/store`

`svelte/store` 모듈은 [readable](/docs#run-time-svelte-store-readable), [writable](/docs#run-time-svelte-store-writable) 및 [derived](/docs#run-time-svelte-store-derived) 저장소를 만드는 기능을 내보냅니다.

구성 요소에서 [reactive `$store` 구문](/docs#component-format-script-4-prefix-stores-with-$-to-access-their-values)을 사용하기 위해 이러한 함수를 사용할 *필요*가 없음을 명심하십시오. `.subscribe`, unsubscribe 및 (선택적으로) `.set`을 올바르게 구현하는 모든 개체는 유효한 저장소이며 특수 구문 및 Svelte의 내장 [`derived` 저장소](/docs#run-time-svelte-store-derived)와 함께 작동합니다.

이렇게 하면 Svelte에서 사용하기 위해 거의 모든 다른 반응 상태 처리 라이브러리를 래핑할 수 있습니다. [스토어 계약](/docs#component-format-script-4-prefix-stores-with-$-to-access-their-values-store-contract)에 대해 자세히 읽어보고 올바른 구현이 어떤 것인지 확인하세요.

#### `writable`

```js
store = writable(value?: any)
```
```js
store = writable(value?: any, start?: (set: (value: any) => void) => () => void)
```

---

'외부' 구성 요소에서 설정할 수 있는 값이 있는 저장소를 만드는 기능입니다. 추가 `set` 및 `update` 메서드를 사용하여 객체로 생성됩니다.

`set`은 설정할 값인 하나의 인수를 취하는 메서드입니다. 저장 값이 아직 같지 않은 경우 저장 값은 인수 값으로 설정됩니다.

`update`는 콜백인 하나의 인수를 취하는 메소드입니다. 콜백은 기존 매장 값을 인수로 사용하고 매장에 설정할 새 값을 반환합니다.

```js
import { writable } from 'svelte/store';

const count = writable(0);

count.subscribe(value => {
	console.log(value);
}); // logs '0'

count.set(1); // logs '1'

count.update(n => n + 1); // logs '2'
```

---

함수가 두 번째 인수로 전달되면 가입자 수가 0에서 1로 증가할 때 호출됩니다(그러나 1에서 2 등은 아님). 해당 함수는 상점의 값을 변경하는 `set` 함수로 전달됩니다. 구독자 수가 1에서 0이 될 때 호출되는 `stop` 함수를 반환해야 합니다.

```js
import { writable } from 'svelte/store';

const count = writable(0, () => {
	console.log('got a subscriber');
	return () => console.log('no more subscribers');
});

count.set(1); // does nothing

const unsubscribe = count.subscribe(value => {
	console.log(value);
}); // logs 'got a subscriber', then '1'

unsubscribe(); // logs 'no more subscribers'
```

예를 들어 페이지를 새로 고칠 때와 같이 파괴되면 `writable` 값이 손실됩니다. 그러나 값을 예를 들어 `localStorage`에 동기화하는 고유한 논리를 작성할 수 있습니다.

#### `readable`

```js
store = readable(value?: any, start?: (set: (value: any) => void) => () => void)
```

---

'외부'에서 값을 설정할 수 없는 저장소를 생성합니다. 첫 번째 인수는 저장소의 초기 값이고 `readable`에 대한 두 번째 인수는 `writable`에 대한 두 번째 인수와 같습니다.

```js
import { readable } from 'svelte/store';

const time = readable(null, set => {
	set(new Date());

	const interval = setInterval(() => {
		set(new Date());
	}, 1000);

	return () => clearInterval(interval);
});
```

#### `derived`

```js
store = derived(a, callback: (a: any) => any)
```
```js
store = derived(a, callback: (a: any, set: (value: any) => void) => void | () => void, initial_value: any)
```
```js
store = derived([a, ...b], callback: ([a: any, ...b: any[]]) => any)
```
```js
store = derived([a, ...b], callback: ([a: any, ...b: any[]], set: (value: any) => void) => void | () => void, initial_value: any)
```

---

하나 이상의 다른 상점에서 상점을 파생시킵니다. 콜백은 첫 번째 구독자가 구독할 때 처음 실행된 다음 저장소 종속성이 변경될 때마다 실행됩니다.

가장 간단한 버전에서 `derived`는 단일 저장소를 사용하고 콜백은 파생된 값을 반환합니다.

```js
import { derived } from 'svelte/store';

const doubled = derived(a, $a => $a * 2);
```

---

콜백은 두 번째 인수 `set`을 수락하고 적절할 때 호출하여 비동기적으로 값을 설정할 수 있습니다.

이 경우 `derived`에 세 번째 인수를 전달할 수도 있습니다. 이는 `set`이 처음 호출되기 전 파생 저장소의 초기 값입니다.

```js
import { derived } from 'svelte/store';

const delayed = derived(a, ($a, set) => {
	setTimeout(() => set($a), 1000);
}, 'one moment...');
```

---

콜백에서 함수를 반환하면 a) 콜백이 다시 실행되거나 b) 마지막 구독자가 구독을 취소할 때 호출됩니다.

```js
import { derived } from 'svelte/store';

const tick = derived(frequency, ($frequency, set) => {
	const interval = setInterval(() => {
	  set(Date.now());
	}, 1000 / $frequency);

	return () => {
		clearInterval(interval);
	};
}, 'one moment...');
```

---

콜백에서 함수를 반환하면 a) 콜백이 다시 실행되거나 b) 마지막 구독자가 구독을 취소할 때 호출됩니다.

```js
import { derived } from 'svelte/store';

const summed = derived([a, b], ([$a, $b]) => $a + $b);

const delayed = derived([a, b], ([$a, $b], set) => {
	setTimeout(() => set($a + $b), 1000);
});
```

#### `get`

```js
value: any = get(store)
```

---

일반적으로 스토어를 구독하고 시간 경과에 따라 변경되는 값을 사용하여 스토어의 가치를 읽어야 합니다. 경우에 따라 구독하지 않은 상점의 가치를 검색해야 할 수도 있습니다. 'get'을 사용하면 그렇게 할 수 있습니다.

> 이것은 구독을 생성하고 값을 읽은 다음 구독을 취소하는 방식으로 작동합니다. 따라서 핫 코드 경로에서는 권장되지 않습니다.

```js
import { get } from 'svelte/store';

const value = get(store);
```


### `svelte/motion`

`svelte/motion` 모듈은 값이 즉시가 아니라 `set` 및 `update` 후에 시간이 지남에 따라 변경되는 쓰기 가능한 저장소를 생성하기 위해 `tweened` 및 `spring`의 두 함수를 내보냅니다.

#### `tweened`

```js
store = tweened(value: any, options)
```

트위닝된 저장소는 고정된 기간 동안 값을 업데이트합니다. 다음 옵션을 사용할 수 있습니다.

* `delay` (`number`, 기본값 0) — 시작 전 밀리초
* `duration` (`number` | `function`, 기본값 400) — 트윈이 지속되는 밀리초
* `easing`(`function`, 기본값 `t => t`) — [easing 함수](/docs#run-time-svelte-easing)
* `interpolate` (`function`) — 아래 참조

`store.set` 및 `store.update`는 인스턴스화 시 전달된 옵션을 재정의하는 두 번째 `options` 인수를 허용할 수 있습니다.

두 함수 모두 트윈이 완료되면 해결되는 Promise를 반환합니다. 트윈이 중단되면 약속이 해결되지 않습니다.

---

기본적으로 Svelte는 두 개의 숫자, 두 개의 배열 또는 두 개의 개체 사이를 보간합니다(배열과 개체가 동일한 '모양'이고 해당 '리프' 속성도 숫자인 경우).

```sv
<script>
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	const size = tweened(1, {
		duration: 300,
		easing: cubicOut
	});

	function handleClick() {
		// this is equivalent to size.update(n => n + 1)
		$size += 1;
	}
</script>

<button
	on:click={handleClick}
	style="transform: scale({$size}); transform-origin: 0 0"
>embiggen</button>
```

---

초기 값이 `undefined` 또는 `null`이면 첫 번째 값 변경이 즉시 적용됩니다. 이는 소품을 기반으로 하는 값을 트위닝하고 구성 요소가 처음 렌더링될 때 모션을 원하지 않는 경우에 유용합니다.

```js
const size = tweened(undefined, {
	duration: 300,
	easing: cubicOut
});

$: $size = big ? 100 : 10;
```

---

`interpolate` 옵션을 사용하면 *임의의* 임의 값 사이를 트위닝할 수 있습니다. `(a, b) => t => value` 함수여야 합니다. 여기서 `a`는 시작 값, `b`는 대상 값, `t`는 0과 1 사이의 숫자, `value`는 결과입니다. 예를 들어 [d3-interpolate](https://github.com/d3/d3-interpolate) 패키지를 사용하여 두 색상 사이를 부드럽게 보간할 수 있습니다.

```sv
<script>
	import { interpolateLab } from 'd3-interpolate';
	import { tweened } from 'svelte/motion';

	const colors = [
		'rgb(255, 62, 0)',
		'rgb(64, 179, 255)',
		'rgb(103, 103, 120)'
	];

	const color = tweened(colors[0], {
		duration: 800,
		interpolate: interpolateLab
	});
</script>

{#each colors as c}
	<button
		style="background-color: {c}; color: white; border: none;"
		on:click="{e => color.set(c)}"
	>{c}</button>
{/each}

<h1 style="color: {$color}">{$color}</h1>
```

#### `spring`

```js
store = spring(value: any, options)
```

`spring` 저장소는 `stiffness` 및 `damping` 매개변수에 따라 대상 값으로 점진적으로 변경됩니다. `tweened` 저장소는 고정된 기간 동안 값을 변경하는 반면, `spring` 저장소는 기존 속도에 의해 결정된 기간 동안 변경되므로 많은 상황에서 보다 자연스러운 동작이 가능합니다. 다음 옵션을 사용할 수 있습니다.

* `stiffness`(`number`, 기본값 `0.15`) — 0과 1 사이의 값이며 높을수록 '더 단단한' 스프링을 의미합니다.
* `damping` (`number`, default `0.8`) — 0과 1 사이의 값으로, 낮을수록 '더 탄력 있는' 스프링을 의미합니다.
* `precision` (`number`, default `0.01`) — 스프링이 '정착'된 것으로 간주되는 임계값을 결정합니다. 여기서 낮을수록 더 정밀함을 의미합니다.

---

위의 모든 옵션은 스프링이 움직이는 동안 변경할 수 있으며 즉시 적용됩니다.

```js
const size = spring(100);
size.stiffness = 0.3;
size.damping = 0.4;
size.precision = 0.005;
```

---

[`tweened`](/docs#run-time-svelte-motion-tweened) 저장소와 마찬가지로 `set` 및 `update`는 스프링이 해결되면 해결되는 Promise를 반환합니다.

`set`과 `update`는 모두 `hard` 또는 `soft` 속성을 가진 객체인 두 번째 인수를 사용할 수 있습니다. `{ hard: true }`는 대상 값을 즉시 설정합니다. `{ soft: n }`은 안정되기 전에 `n`초 동안 기존 모멘텀을 유지합니다. `{ soft: true }`는 `{ soft: 0.5 }`와 같습니다.

```js
const coords = spring({ x: 50, y: 50 });
// updates the value immediately
coords.set({ x: 100, y: 200 }, { hard: true });
// preserves existing momentum for 1s
coords.update(
	(target_coords, coords) => {
		return { x: target_coords.x, y: coords.y };
	},
	{ soft: 1 }
);
```

[스프링 자습서에서 전체 예제를 참조하십시오.](/tutorial/spring)

```sv
<script>
	import { spring } from 'svelte/motion';

	const coords = spring({ x: 50, y: 50 }, {
		stiffness: 0.1,
		damping: 0.25
	});
</script>
```

---

초기 값이 `undefined` 또는 `null`인 경우 `tweened` 값과 마찬가지로 첫 번째 값 변경이 즉시 적용됩니다(위 참조).

```js
const size = spring();
$: $size = big ? 100 : 10;
```

### `svelte/transition`

`svelte/transition` 모듈은 `fade`, `blur`, `fly`, `slide`, `scale`, `draw` 및 `crossfade`의 7가지 기능을 내보냅니다. Svelte [`transitions`](/docs#template-syntax-element-directives-transition-fn)와 함께 사용하기 위한 것입니다.

#### `fade`

```sv
transition:fade={params}
```
```sv
in:fade={params}
```
```sv
out:fade={params}
```

---

요소의 불투명도를 `in` 전환의 경우 0에서 현재 불투명도까지, `out` 전환의 경우 현재 불투명도에서 0으로 애니메이션합니다.

`fade`는 다음 매개변수를 허용합니다.

* `delay` (`number`, 기본값 0) — 시작 전 밀리초
* `duration` (`number`, 기본값 400) — 전환이 지속되는 밀리초
* `easing`(`function`, 기본 `linear`) — [easing 함수](/docs#run-time-svelte-easing)

[전환 튜토리얼](/tutorial/transition)에서 `fade` 전환이 작동하는 것을 볼 수 있습니다.

```sv
<script>
	import { fade } from 'svelte/transition';
</script>

{#if condition}
	<div transition:fade="{{delay: 250, duration: 300}}">
		fades in and out
	</div>
{/if}
```

#### `blur`

```sv
transition:blur={params}
```
```sv
in:blur={params}
```
```sv
out:blur={params}
```

---

요소의 불투명도와 함께 `blur` 필터에 애니메이션을 적용합니다.

`blur`는 다음 매개변수를 허용합니다.

* `delay` (`number`, 기본값 0) — 시작 전 밀리초
* `duration` (`number`, 기본값 400) — 전환이 지속되는 밀리초
* `easing`(`function`, 기본 `cubicInOut`) — [easing 함수](/docs#run-time-svelte-easing)
* `opacity` (`number`, default 0) - 움직이게 할 불투명도 값입니다.
* `amount` (`number`, default 5) - 블러의 크기(픽셀 단위)

```sv
<script>
	import { blur } from 'svelte/transition';
</script>

{#if condition}
	<div transition:blur="{{amount: 10}}">
		fades in and out
	</div>
{/if}
```

#### `fly`

```sv
transition:fly={params}
```
```sv
in:fly={params}
```
```sv
out:fly={params}
```

---

요소의 x 및 y 위치와 불투명도에 애니메이션을 적용합니다. `in`은 요소의 현재(기본값) 값에서 매개변수로 전달된 제공된 값으로 전환합니다. `out`은 제공된 값에서 요소의 기본값으로 애니메이션을 전환합니다.

`fly`는 다음 매개변수를 허용합니다.

* `delay` (`number`, 기본값 0) — 시작 전 밀리초
* `duration` (`number`, 기본값 400) — 전환이 지속되는 밀리초
* `easing`(`function`, 기본 `cubicOut`) — [easing 함수](/docs#run-time-svelte-easing)
* `x` (`숫자`, 기본값 0) - 애니메이트할 x 오프셋
* `y` (`number`, default 0) - 애니메이트할 y 오프셋
* `opacity` (`number`, default 0) - 움직이게 할 불투명도 값입니다.

[전환 가이드](/tutorial/adding-parameters-to-transitions)에서 작동 중인 `fly` 전환을 볼 수 있습니다.

```sv
<script>
	import { fly } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
</script>

{#if condition}
	<div transition:fly="{{delay: 250, duration: 300, x: 100, y: 500, opacity: 0.5, easing: quintOut}}">
		flies in and out
	</div>
{/if}
```

#### `slide`

```sv
transition:slide={params}
```
```sv
in:slide={params}
```
```sv
out:slide={params}
```

---

요소를 안팎으로 슬라이드합니다.

`slide`는 다음 매개변수를 허용합니다.

* `delay` (`number`, 기본값 0) — 시작 전 밀리초
* `duration` (`number`, 기본값 400) — 전환이 지속되는 밀리초
* `easing`(`function`, 기본 `cubicOut`) — [easing 함수](/docs#run-time-svelte-easing)

```sv
<script>
	import { slide } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
</script>

{#if condition}
	<div transition:slide="{{delay: 250, duration: 300, easing: quintOut }}">
		slides in and out
	</div>
{/if}
```

#### `scale`

```sv
transition:scale={params}
```
```sv
in:scale={params}
```
```sv
out:scale={params}
```

---

요소의 불투명도와 배율에 애니메이션을 적용합니다. `in`은 요소의 현재(기본값) 값에서 매개변수로 전달된 제공된 값으로 전환합니다. `out`은 제공된 값에서 요소의 기본값으로 애니메이션을 전환합니다.

`scale`은 다음 매개변수를 허용합니다.

* `delay` (`number`, 기본값 0) — 시작 전 밀리초
* `duration` (`number`, 기본값 400) — 전환이 지속되는 밀리초
* `easing`(`function`, 기본 `cubicOut`) — [easing 함수](/docs#run-time-svelte-easing)
* `start` (`number`, default 0) - 애니메이트할 스케일 값
* `opacity` (`number`, default 0) - 움직이게 할 불투명도 값입니다.

```sv
<script>
	import { scale } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
</script>

{#if condition}
	<div transition:scale="{{duration: 500, delay: 500, opacity: 0.5, start: 0.5, easing: quintOut}}">
		scales in and out
	</div>
{/if}
```

#### `draw`

```sv
transition:draw={params}
```
```sv
in:draw={params}
```
```sv
out:draw={params}
```

---

튜브 안의 뱀처럼 SVG 요소의 스트로크에 애니메이션을 적용합니다. `in` 전환은 보이지 않는 경로에서 시작하여 시간이 지남에 따라 화면에 경로를 그립니다. `out` 전환은 보이는 상태에서 시작하여 점진적으로 경로를 지웁니다. `draw`는 `<path>` 및 `<polyline>`과 같은 `getTotalLength` 메서드가 있는 요소에서만 작동합니다.

`draw`는 다음 매개변수를 허용합니다.

* `delay` (`number`, 기본값 0) — 시작 전 밀리초
* `speed` (`number`, 기본 정의되지 않음) - 애니메이션의 속도, 아래 참조.
* `duration` (`number` | `function`, 기본값 800) — 전환이 지속되는 밀리초
* `easing`(`function`, 기본 `cubicInOut`) — [easing 함수](/docs#run-time-svelte-easing)

`speed` 매개변수는 경로의 길이에 상대적인 전환 기간을 설정하는 수단입니다. 경로의 길이에 적용되는 수식어: `duration = length / speed`. 속도가 1인 1000픽셀 경로의 지속 시간은 `1000ms`이며, 속도를 `0.5`로 설정하면 지속 시간이 두 배가 되고 `2`로 설정하면 절반이 됩니다.

```sv
<script>
	import { draw } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
</script>

<svg viewBox="0 0 5 5" xmlns="http://www.w3.org/2000/svg">
	{#if condition}
		<path transition:draw="{{duration: 5000, delay: 500, easing: quintOut}}"
					d="M2 1 h1 v1 h1 v1 h-1 v1 h-1 v-1 h-1 v-1 h1 z"
					fill="none"
					stroke="cornflowerblue"
					stroke-width="0.1px"
					stroke-linejoin="round"
		/>
	{/if}
</svg>

```


#### `crossfade`

`crossfade` 함수는 `send` 및 `receive`라는 [전환](/docs#template-syntax-element-directives-transition-fn) 쌍을 만듭니다. 요소가 `send`되면 `receive`되는 해당 요소를 찾고 해당 요소를 상대 위치로 변환하고 페이드 아웃하는 전환을 생성합니다. 요소가 '수신'되면 그 반대가 발생합니다. 대응 항목이 없으면 `fallback` 전환이 사용됩니다.

---

`crossfade`는 다음 매개변수를 허용합니다.

* `delay` (`number`, 기본값 0) — 시작 전 밀리초
* `duration` (`number` | `function`, 기본값 800) — 전환이 지속되는 밀리초
* `easing`(`function`, 기본 `cubicOut`) — [easing 함수](/docs#run-time-svelte-easing)
* `fallback` (`function`) — 수신되는 일치하는 요소가 없을 때 전송에 사용하고 전송되는 요소가 없을 때 수신에 사용할 대체 [전환](/docs#template-syntax-element-directives-transition-fn)입니다.

```sv
<script>
	import { crossfade } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';

	const [send, receive] = crossfade({
		duration:1500,
		easing: quintOut
	});
</script>

{#if condition}
	<h1 in:send={{key}} out:receive={{key}}>BIG ELEM</h1>
{:else}
	<small in:send={{key}} out:receive={{key}}>small elem</small>
{/if}
```


### `svelte/animate`

`svelte/animate` 모듈은 Svelte [애니메이션](/docs#template-syntax-element-directives-animate-fn)과 함께 사용할 함수 하나를 내보냅니다.

#### `flip`

```sv
animate:flip={params}
```

'flip' 기능은 요소의 시작 및 끝 위치를 계산하고 그 사이에 애니메이션을 적용하여 'x' 및 'y' 값을 변환합니다. 'flip'은 [First, Last, Invert, Play](https://aerotwist.com/blog/flip-your-animations/)의 약자입니다.

'flip'은 다음 매개변수를 허용합니다.

* `delay` (`number`, 기본값 0) — 시작 전 밀리초
* `duration` (`number` | `function`, default `d => Math.sqrt(d) * 120`) — 아래 참조
* `easing`(`function`, 기본 `cubicOut`) — [easing 함수](/docs#run-time-svelte-easing)


`duration`은 다음 중 하나로 제공될 수 있습니다.

- `숫자`(밀리초).
- `distance: number => duration: number` 함수는 요소가 이동할 거리를 픽셀 단위로 수신하고 기간을 밀리초 단위로 반환합니다. 이렇게 하면 각 요소가 이동한 거리에 상대적인 기간을 할당할 수 있습니다.

---

[애니메이션 자습서](/tutorial/animate)에서 전체 예제를 볼 수 있습니다.


```sv
<script>
	import { flip } from 'svelte/animate';
	import { quintOut } from 'svelte/easing';

	let list = [1, 2, 3];
</script>

{#each list as n (n)}
	<div animate:flip="{{delay: 250, duration: 250, easing: quintOut}}">
		{n}
	</div>
{/each}
```



### `svelte/easing`

여유 기능은 시간 경과에 따른 변화율을 지정하며 Svelte의 내장 전환 및 애니메이션은 물론 트위닝 및 스프링 유틸리티로 작업할 때 유용합니다. `svelte/easing`에는 31개의 명명된 내보내기, `linear` ease 및 10가지 easing 함수의 3가지 변형(`in`, `out` 및 `inOut`)이 포함되어 있습니다.

[예시 섹션](/examples)에서 [이즈 시각화 도구](/examples/easing)를 사용하여 다양한 이즈를 탐색할 수 있습니다.


| ease | in | out | inOut |
| --- | --- | --- | --- |
| **back** | `backIn` | `backOut` | `backInOut` |
| **bounce** | `bounceIn` | `bounceOut` | `bounceInOut` |
| **circ** | `circIn` | `circOut` | `circInOut` |
| **cubic** | `cubicIn` | `cubicOut` | `cubicInOut` |
| **elastic** | `elasticIn` | `elasticOut` | `elasticInOut` |
| **expo** | `expoIn` | `expoOut` | `expoInOut` |
| **quad** | `quadIn` | `quadOut` | `quadInOut` |
| **quart** | `quartIn` | `quartOut` | `quartInOut` |
| **quint** | `quintIn` | `quintOut` | `quintInOut` |
| **sine** | `sineIn` | `sineOut` | `sineInOut` |


### `svelte/register`

번들링 없이 Node.js에서 Svelte 구성요소를 렌더링하려면 `require('svelte/register')`를 사용하세요. 그런 다음 `require`를 사용하여 `.svelte` 파일을 포함할 수 있습니다.

```js
require('svelte/register');

const App = require('./App.svelte').default;

...

const { html, css, head } = App.render({ answer: 42 });
```

> 기본 JavaScript 모듈을 Node에서 인식하는 CommonJS 모듈로 변환하기 때문에 `.default`가 필요합니다. 구성 요소가 JavaScript 모듈을 가져오면 Node에서 로드하지 못하므로 대신 번들러를 사용해야 합니다.

컴파일 옵션을 설정하거나 사용자 정의 파일 확장자를 사용하려면 `register` 후크를 함수로 호출하십시오.

```js
require('svelte/register')({
  extensions: ['.customextension'], // defaults to ['.html', '.svelte']
	preserveComments: true
});
```


### Client-side component API

#### Creating a component

```js
const component = new Component(options)
```

클라이언트 측 구성요소, 즉 `generate: 'dom'`(또는 지정되지 않은 `generate` 옵션)으로 컴파일된 구성요소는 자바스크립트 클래스입니다.

```js
import App from './App.svelte';

const app = new App({
	target: document.body,
	props: {
		// assuming App.svelte contains something like
		// `export let answer`:
		answer: 42
	}
});
```

다음 초기화 옵션을 제공할 수 있습니다.

| option | default | description |
| --- | --- | --- |
| `target` | **none** | 렌더링할 `HTMLElement` 또는 `ShadowRoot`. 이 옵션은 필수입니다
| `anchor` | `null` | 직전에 구성 요소를 렌더링하는 `target`의 자식
| `props` | `{}` | 구성 요소에 제공할 속성 개체
| `context` | `new Map()` | 구성 요소에 제공할 루트 수준 컨텍스트 키-값 쌍의 `map`
| `hydrate` | `false` | 아래 참조
| `intro` | `false` | `true`인 경우 후속 상태 변경을 기다리지 않고 초기 렌더링에서 전환을 재생합니다.

`target`의 기존 자식은 그대로 남습니다.


---

`hydrate` 옵션은 Svelte가 새 요소를 생성하는 대신 기존 DOM(일반적으로 서버 측 렌더링에서)을 업그레이드하도록 지시합니다. 구성요소가 [`hydratable: true` 옵션](/docs#compile-time-svelte-compile)으로 컴파일된 경우에만 작동합니다. `<head>` 요소의 하이드레이션은 서버 측 렌더링 코드가 `hydratable: true`로 컴파일된 경우에만 제대로 작동합니다. 이 코드는 `<head>`의 각 요소에 마커를 추가하여 구성 요소가 어떤 요소인지 알 수 있도록 합니다. 수화 중 제거를 담당합니다.

`target`의 자식은 일반적으로 홀로 남겨지는 반면 `hydrate: true`는 모든 자식을 제거합니다. 이러한 이유로 `anchor` 옵션은 `hydrate: true`와 함께 사용할 수 없습니다.

기존 DOM은 구성 요소와 일치할 필요가 없습니다. Svelte는 DOM이 진행됨에 따라 '수리'합니다.

```js
import App from './App.svelte';

const app = new App({
	target: document.querySelector('#server-rendered-html'),
	hydrate: true
});
```

#### `$set`

```js
component.$set(props)
```

---

프로그래밍 방식으로 인스턴스에 소품을 설정합니다. `component.$set({ x: 1 })`은 구성요소 `<script>` 블록 내부의 `x = 1`과 동일합니다.

이 메서드를 호출하면 다음 마이크로태스크에 대한 업데이트가 예약됩니다. DOM은 동기식으로 업데이트되지 *않습니다*. 씨

```js
component.$set({ answer: 42 });
```

#### `$on`

```js
component.$on(event, callback)
```

---

구성요소가 '이벤트'를 전달할 때마다 `callback` 함수가 호출되도록 합니다.

호출 시 이벤트 리스너를 제거하는 함수가 반환됩니다.

```js
const off = app.$on('selected', event => {
	console.log(event.detail.selection);
});

off();
```

#### `$destroy`

```js
component.$destroy()
```

DOM에서 구성 요소를 제거하고 `onDestroy` 핸들러를 트리거합니다.

#### Component props

```js
component.prop
```
```js
component.prop = value
```

---

구성 요소가 `접근자: true`로 컴파일되면 각 인스턴스에는 각 구성 요소의 소품에 해당하는 getter 및 setter가 있습니다. 값을 설정하면 `component.$set(...)`으로 인한 기본 비동기 업데이트가 아닌 *동기* 업데이트가 발생합니다.

사용자 정의 요소로 컴파일하지 않는 한 기본적으로 `접속자`는 `false`입니다.

```js
console.log(app.count);
app.count += 1;
```


### Custom element API

---

Svelte 구성 요소는 `customElement: true` 컴파일러 옵션을 사용하여 사용자 지정 요소(일명 웹 구성 요소)로 컴파일할 수도 있습니다. `<svelte:options>` [요소](/docs#template-syntax-svelte-options)를 사용하여 구성요소의 태그 이름을 지정해야 합니다.

```sv
<svelte:options tag="my-element" />

<script>
	export let name = 'world';
</script>

<h1>Hello {name}!</h1>
<slot></slot>
```

---

또는 `tag={null}`을 사용하여 맞춤 요소의 소비자가 이름을 지정해야 함을 나타냅니다.

```js
import MyElement from './MyElement.svelte';

customElements.define('my-element', MyElement);
```

---

맞춤 요소가 정의되면 일반 DOM 요소로 사용할 수 있습니다.

```js
document.body.innerHTML = `
	<my-element>
		<p>This is some slotted content</p>
	</my-element>
`;
```

---

기본적으로 맞춤 요소는 `accessors: true`로 컴파일됩니다. 즉, 모든 [props](/docs#template-syntax-attributes-and-props)가 DOM 요소의 속성으로 노출되며 읽기/ 가능한 경우 속성으로 쓰기 가능).

이를 방지하려면 `<svelte:options>`에 `accessors={false}`를 추가하십시오.

```js
const el = document.querySelector('my-element');

// get the current value of the 'name' prop
console.log(el.name);

// set a new value, updating the shadow DOM
el.name = 'everybody';
```

사용자 정의 요소는 [대부분의 프레임워크](https://custom-elements-everywhere.com/)뿐만 아니라 바닐라 HTML 및 JavaScript와도 작동하므로 비 Svelte 앱에서 사용할 구성 요소를 패키징하는 유용한 방법이 될 수 있습니다. 그러나 알아야 할 몇 가지 중요한 차이점이 있습니다.

* 스타일은 단순히 *범위*가 아니라 *캡슐화*됩니다. 즉, `:global(...)` 수식어가 있는 스타일을 포함하여 구성 요소가 아닌 스타일(예: `global.css` 파일에 있을 수 있음)은 맞춤 요소에 적용되지 않습니다.
* 별도의 .css 파일로 추출되는 대신 스타일이 JavaScript 문자열로 구성 요소에 인라인됩니다.
* JavaScript가 로드될 때까지 Shadow DOM이 보이지 않기 때문에 사용자 정의 요소는 일반적으로 서버 측 렌더링에 적합하지 않습니다.
* Svelte에서 슬롯 콘텐츠는 *느리게* 렌더링됩니다. DOM에서는 *열심히* 렌더링합니다. 즉, 컴포넌트의 `<slot>` 요소가 `{#if ...}` 블록 안에 있어도 항상 생성됩니다. 마찬가지로 `{#each ...}` 블록에 `<slot>`을 포함해도 슬롯 콘텐츠가 여러 번 렌더링되지 않습니다.
* `let:` 지시문은 효과가 없습니다.
* 이전 브라우저를 지원하려면 Polyfill이 필요합니다.



### Server-side component API

```js
const result = Component.render(...)
```

---

클라이언트 측 구성 요소와 달리 서버 측 구성 요소는 렌더링 후 수명이 없습니다. 전체 작업은 일부 HTML 및 CSS를 만드는 것입니다. 이러한 이유로 API는 다소 다릅니다.

서버측 구성 요소는 선택적 소품으로 호출할 수 있는 `render` 메서드를 노출합니다. `head`, `html` 및 `css` 속성이 있는 개체를 반환합니다. 여기서 `head`는 발생한 모든 `<svelte:head>` 요소의 내용을 포함합니다.

[`svelte/register`](/docs#run-time-svelte-register)를 사용하여 Svelte 구성 요소를 노드로 직접 가져올 수 있습니다.

```js
require('svelte/register');

const App = require('./App.svelte').default;

const { head, html, css } = App.render({
	answer: 42
});
```

---

`.render()` 메서드는 다음 매개변수를 허용합니다.

| parameter | default | description |
| --- | --- | --- |
| `props` | `{}` | 구성 요소에 제공할 속성 개체
| `options` | `{}` | 옵션의 대상

`options` 개체는 다음 옵션을 사용합니다.

| option | default | description |
| --- | --- | --- |
| `context` | `new Map()` | 구성 요소에 제공할 루트 수준 컨텍스트 키-값 쌍의 `Map`

```js
const { head, html, css } = App.render(
	// props
	{ answer: 42 },
	// options
	{
		context: new Map([['context-key', 'context-value']])
	}
);
```
