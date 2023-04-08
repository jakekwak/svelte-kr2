---
title: Await blocks
---

대부분의 웹 애플리케이션은 어느 시점에서 비동기 데이터를 처리해야 합니다. Svelte를 사용하면 마크업에서 직접 [promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)의 값을 *await*하기 쉽습니다:

```html
{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
```

> 가장 최근의 `promise`만 고려하므로 경쟁 조건에 대해 걱정할 필요가 없습니다.

약속이 거부될 수 없다는 것을 알고 있다면 `catch` 블록을 생략할 수 있습니다. Promise가 해결될 때까지 아무 것도 표시하지 않으려면 첫 번째 블록을 생략할 수도 있습니다.

```html
{#await promise then number}
	<p>the number is {number}</p>
{/await}
```
