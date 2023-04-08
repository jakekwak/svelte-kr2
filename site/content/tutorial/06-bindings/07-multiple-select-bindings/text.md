---
title: Select multiple
---

select는 `multiple` 속성을 가질 수 있으며, 이 경우 단일 값을 선택하는 대신 배열을 채웁니다.

[이전 아이스크림 예](/tutorial/group-inputs)로 돌아가서 확인란을 `<select multiple>`으로 바꿀 수 있습니다.

```html
<h2>Flavours</h2>

<select multiple bind:value={flavours}>
	{#each menu as flavour}
		<option value={flavour}>
			{flavour}
		</option>
	{/each}
</select>
```

> 여러 옵션을 선택하려면 `control` 키(또는 MacOS의 경우 `command` 키)를 길게 누릅니다.
