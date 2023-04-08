---
title: Else-if blocks
---

`else if`와 함께 여러 조건을 '연결'할 수 있습니다.

```html
{#if x > 10}
	<p>{x} is greater than 10</p>
{:else if 5 > x}
	<p>{x} is less than 5</p>
{:else}
	<p>{x} is between 5 and 10</p>
{/if}
```