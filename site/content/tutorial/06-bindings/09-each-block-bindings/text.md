---
title: Each block bindings
---

`each` 블록 내부의 속성에 바인딩할 수도 있습니다.

```html
{#each todos as todo}
	<div class:done={todo.done}>
		<input
			type=checkbox
			bind:checked={todo.done}
		>

		<input
			placeholder="What needs to be done?"
			bind:value={todo.text}
		>
	</div>
{/each}
```

> 이러한 `<input>` 요소와 상호 작용하면 배열이 변경됩니다. 변경할 수 없는 데이터로 작업하는 것을 선호하는 경우 이러한 바인딩을 피하고 대신 이벤트 핸들러를 사용해야 합니다.
