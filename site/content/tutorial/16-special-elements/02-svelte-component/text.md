---
title: <svelte:component>
---

구성 요소는 `<svelte:component>`를 사용하여 해당 범주를 모두 변경할 수 있습니다. 일련의 `if` 블록 대신...

```html
{#if selected.color === 'red'}
	<RedThing/>
{:else if selected.color === 'green'}
	<GreenThing/>
{:else if selected.color === 'blue'}
	<BlueThing/>
{/if}
```

...단일 동적 구성 요소를 가질 수 있습니다.

```html
<svelte:component this={selected.component}/>
```

`this` 값은 구성 요소 생성자 또는 거짓 값일 수 있습니다 — 거짓이면 구성 요소가 렌더링되지 않습니다.