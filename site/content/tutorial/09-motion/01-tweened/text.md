---
title: Tweened
---

값을 설정하고 DOM 업데이트를 자동으로 보는 것은 멋집니다. 더 멋진 것이 무엇인지 아십니까? 이러한 값을 *트위닝*합니다. Svelte에는 애니메이션을 사용하여 변경 사항을 전달하는 매끄러운 사용자 인터페이스를 구축하는 데 도움이 되는 도구가 포함되어 있습니다.

`progress` 저장소를 `tweened` 값으로 변경하여 시작하겠습니다.

```html
<script>
	import { tweened } from 'svelte/motion';

	const progress = tweened(0);
</script>
```

버튼을 클릭하면 진행률 표시줄이 새 값으로 애니메이션됩니다. 그래도 약간 로봇적이고 만족스럽지 않습니다. 여유 기능을 추가해야 합니다.

```html
<script>
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	const progress = tweened(0, {
		duration: 400,
		easing: cubicOut
	});
</script>
```

> `svelte/easing` 모듈에는 [Penner easing equations](https://web.archive.org/web/20190805215728/http://robertpenner.com/easing/)가 포함되어 있거나 `p`와 `t`가 모두 0과 1 사이의 값인 `p => t` 함수를 제공할 수 있습니다.

`tweened`에 사용할 수 있는 전체 옵션 세트:

* `delay` — 트윈 시작 전 밀리초
* `duration` — 트윈의 기간(밀리초) 또는 `(from, to) => milliseconds` 함수를 사용하여 (예를 들어) 값의 더 큰 변화에 대해 더 긴 트윈을 지정할 수 있습니다.
* `easing` — `p => t` 함수
* `interpolate` — 임의의 값 사이를 보간하기 위한 사용자 정의 `(from, to) => t => value` 함수. 기본적으로 Svelte는 숫자, 날짜, 동일한 모양의 배열 및 개체(숫자 및 날짜 또는 기타 유효한 배열 및 개체만 포함하는 경우) 사이를 보간합니다. 예를 들어 색상 문자열 또는 변환 행렬을 보간하려면 사용자 정의 보간기를 제공하십시오.

또한 이러한 옵션을 `progress.set` 및 `progress.update`에 두 번째 인수로 전달할 수 있으며, 이 경우 기본값을 재정의합니다. `set` 및 `update` 메서드는 모두 트윈이 완료될 때 해결되는 약속을 반환합니다.
