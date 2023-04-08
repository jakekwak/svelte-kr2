---
title: <svelte:element>
---

렌더링할 DOM 요소의 종류를 미리 알지 못하는 경우가 있습니다. `<svelte:element>`는 여기서 유용합니다. 일련의 `if` 블록 대신...

```html
{#if selected === 'h1'}
	<h1>I'm a h1 tag</h1>
{:else if selected === 'h3'}
	<h3>I'm a h3 tag</h3>
{:else if selected === 'p'}
	<p>I'm a p tag</p>
{/if}
```

...하나의 동적 구성 요소를 가질 수 있습니다.

```html
<svelte:element this={selected}>I'm a {selected} tag</svelte:element>
```

`this` 값은 임의의 문자열이거나 거짓 값일 수 있습니다 — 거짓이면 요소가 렌더링되지 않습니다.