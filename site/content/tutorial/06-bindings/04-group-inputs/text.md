---
title: Group inputs
---

동일한 값과 관련된 여러 입력이 있는 경우 `value` 속성과 함께 `bind:group`을 사용할 수 있습니다. 동일한 그룹의 무선 입력은 상호 배타적입니다. 동일한 그룹의 확인란 입력은 선택한 값의 배열을 형성합니다.

각 입력에 `bind:group`을 추가합니다.

```html
<input type=radio bind:group={scoops} name="scoops" value={1}>
```

이 경우 체크박스 입력을 `each` 블록으로 이동하여 코드를 더 간단하게 만들 수 있습니다. 먼저 `<script>` 블록에 `menu` 변수를 추가합니다...

```js
let menu = [
	'Cookies and cream',
	'Mint choc chip',
	'Raspberry ripple'
];
```

...그런 다음 두 번째 섹션을 교체합니다.

```html
<h2>Flavours</h2>

{#each menu as flavour}
	<label>
		<input type=checkbox bind:group={flavours} name="flavours" value={flavour}>
		{flavour}
	</label>
{/each}
```

이제 새롭고 흥미로운 방향으로 아이스크림 메뉴를 쉽게 확장할 수 있습니다.