---
title: Named slots
---

이전 예제에는 구성 요소의 직계 자식을 렌더링하는 *기본 슬롯*이 포함되어 있습니다. 이 `<ContactCard>`와 같이 배치에 대한 더 많은 제어가 필요한 경우가 있습니다. 이러한 경우 *명명된 슬롯*을 사용할 수 있습니다.

`ContactCard.svelte`에서 각 슬롯에 `name` 속성을 추가합니다.

```html
<article class="contact-card">
	<h2>
		<slot name="name">
			<span class="missing">Unknown name</span>
		</slot>
	</h2>

	<div class="address">
		<slot name="address">
			<span class="missing">Unknown address</span>
		</slot>
	</div>

	<div class="email">
		<slot name="email">
			<span class="missing">Unknown email</span>
		</slot>
	</div>
</article>
```

그런 다음 `<ContactCard>` 구성요소 내부에 해당 `slot="..."` 속성이 있는 요소를 추가합니다.

```html
<ContactCard>
	<span slot="name">
		P. Sherman
	</span>

	<span slot="address">
		42 Wallaby Way<br>
		Sydney
	</span>
</ContactCard>
```