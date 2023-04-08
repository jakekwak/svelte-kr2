---
title: The animate directive
---

[이전 장](/tutorial/deferred-transitions)에서는 지연 전환을 사용하여 요소가 한 할일 목록에서 다른 목록으로 이동할 때 움직이는 것처럼 보이는 효과를 만들었습니다.

환상을 완성하려면 전환되지 *않는* 요소에 모션을 적용해야 합니다. 이를 위해 `animate` 디렉티브를 사용합니다.

먼저 `svelte/animate`에서 `flip` 함수를 가져옵니다. flip은 ['First, Last, Invert, Play'](https://aerotwist.com/blog/flip-your-animations/)를 의미합니다.

```js
import { flip } from 'svelte/animate';
```

그런 다음 `<label>` 요소에 추가합니다.

```html
<label
	in:receive="{{key: todo.id}}"
	out:send="{{key: todo.id}}"
	animate:flip
>
```

이 경우 움직임이 약간 느리므로 `duration` 매개변수를 추가할 수 있습니다.

```html
<label
	in:receive="{{key: todo.id}}"
	out:send="{{key: todo.id}}"
	animate:flip="{{duration: 200}}"
>
```

> `duration`은 `d => milliseconds` 함수일 수도 있습니다. 여기서 `d`는 요소가 이동해야 하는 픽셀 수입니다.

모든 전환 및 애니메이션은 JavaScript가 아닌 CSS로 적용되므로 기본 스레드를 차단(또는 차단)하지 않습니다.