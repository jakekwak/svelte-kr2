---
title: Custom CSS transitions
---

`svelte/transition` 모듈에는 몇 가지 기본 제공 전환이 있지만 자신만의 전환을 만드는 것은 매우 쉽습니다. 예를 들어, 이것은 `fade` 전환의 소스입니다.

```js
function fade(node, {
	delay = 0,
	duration = 400
}) {
	const o = +getComputedStyle(node).opacity;

	return {
		delay,
		duration,
		css: t => `opacity: ${t * o}`
	};
}
```

이 함수는 두 개의 인수(전환이 적용되는 노드 및 전달된 모든 매개변수)를 사용하고 다음 속성을 가질 수 있는 전환 개체를 반환합니다.

* `delay` — 전환이 시작되기 전 밀리초
* `duration` — 전환 시간(밀리초)
* `easing` — `p => t` 여유 함수([tweening](/tutorial/tweened) 장 참조)
* `css` — `(t, u) => css` 함수, 여기서 `u === 1 - t`
* `tick` — 노드에 영향을 미치는 `(t, u) => {...}` 함수

't' 값은 인트로 시작 또는 아웃트로 종료 시 '0'이고 인트로 종료 또는 아웃트로 시작 시 '1'입니다.

대부분의 경우 `tick` 속성이 *아닌* `css` 속성을 반환해야 합니다. 가능한 경우 버벅거림을 방지하기 위해 CSS 애니메이션이 기본 스레드에서 실행되기 때문입니다. Svelte는 전환을 '시뮬레이션'하고 CSS 애니메이션을 구성한 다음 실행합니다.

예를 들어 `fade` 전환은 다음과 같은 CSS 애니메이션을 생성합니다.

```css
0% { opacity: 0 }
10% { opacity: 0.1 }
20% { opacity: 0.2 }
/* ... */
100% { opacity: 1 }
```

하지만 우리는 훨씬 더 창의적일 수 있습니다. 진정으로 불필요한 것을 만들어 봅시다.

```html
<script>
	import { fade } from 'svelte/transition';
	import { elasticOut } from 'svelte/easing';

	let visible = true;

	function spin(node, { duration }) {
		return {
			duration,
			css: t => {
				const eased = elasticOut(t);

				return `
					transform: scale(${eased}) rotate(${eased * 1080}deg);
					color: hsl(
						${Math.trunc(t * 360)},
						${Math.min(100, 1000 - 1000 * t)}%,
						${Math.min(50, 500 - 500 * t)}%
					);`
			}
		};
	}
</script>
```

기억하십시오: 큰 힘에는 큰 책임이 따릅니다.
