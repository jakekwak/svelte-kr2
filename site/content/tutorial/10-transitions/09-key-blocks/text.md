---
title: Key blocks
---

키 블록은 식의 값이 변경될 때 내용을 파괴하고 다시 만듭니다.

```html
{#key value}
	<div transition:fade>{value}</div>
{/key}
```

이는 요소가 DOM에 들어가거나 나올 때만이 아니라 값이 변경될 때마다 전환을 재생하려는 경우에 유용합니다.

`number`에 따라 `<span>` 요소를 키 블록에 래핑합니다. 이렇게 하면
증가 버튼을 누를 때마다 애니메이션이 재생됩니다.
