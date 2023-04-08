---
title: In and out
---

`transition` 지시어 대신에 요소는 `in` 또는 `out` 지시어 또는 둘 다를 함께 가질 수 있습니다. `fly`와 함께 `fade` 가져오기...

```js
import { fade, fly } from 'svelte/transition';
```

...그런 다음 `transition` 지시문을 별도의 `in` 및 `out` 지시문으로 바꿉니다.

```html
<p in:fly="{{ y: 200, duration: 2000 }}" out:fade>
	Flies in, fades out
</p>
```

이 경우 전환이 반전되지 *않습니다*.