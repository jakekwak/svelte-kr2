---
title: Slot props
---

이 앱에는 마우스가 현재 위에 있는지 추적하는 `<Hoverable>` 구성 요소가 있습니다. 슬롯 콘텐츠를 업데이트할 수 있도록 해당 데이터를 상위 구성 요소로 *다시* 전달해야 합니다.

이를 위해 *슬롯 소품*을 사용합니다. `Hoverable.svelte`에서 `hovering` 값을 슬롯에 전달합니다.

```html
<div on:mouseenter={enter} on:mouseleave={leave}>
	<slot hovering={hovering}></slot>
</div>
```

> 원하는 경우 `{hovering}` 축약형을 사용할 수도 있습니다.

그런 다음 `<Hoverable>` 구성 요소의 내용에 `hovering`을 노출하기 위해 `let` 지시문을 사용합니다.

```html
<Hoverable let:hovering={hovering}>
	<div class:active={hovering}>
		{#if hovering}
			<p>I am being hovered upon.</p>
		{:else}
			<p>Hover over me!</p>
		{/if}
	</div>
</Hoverable>
```

원하는 경우 변수의 이름을 바꿀 수 있습니다. 상위 구성 요소에서 `active`라고 부르겠습니다.

```html
<Hoverable let:hovering={active}>
	<div class:active>
		{#if active}
			<p>I am being hovered upon.</p>
		{:else}
			<p>Hover over me!</p>
		{/if}
	</div>
</Hoverable>
```

원하는 만큼 이러한 구성 요소를 가질 수 있으며 슬롯이 있는 소품은 선언된 구성 요소에 대해 로컬로 유지됩니다.

> 명명된 슬롯에는 소품도 있을 수 있습니다. 컴포넌트 자체가 아닌 `slot="..."` 속성이 있는 요소에 `let` 지시문을 사용하십시오.
