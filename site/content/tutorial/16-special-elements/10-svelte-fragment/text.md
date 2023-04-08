---
title: <svelte:fragment>
---

`<svelte:fragment>` 요소를 사용하면 콘텐츠를 컨테이너 DOM 요소로 래핑하지 않고 명명된 슬롯에 배치할 수 있습니다. 이렇게 하면 문서의 흐름 레이아웃이 그대로 유지됩니다.

예제에서 `1em` 간격으로 상자에 플렉스 레이아웃을 적용한 방법을 확인하십시오.

```sv
<!-- Box.svelte -->
<div class="box">
	<slot name="header">No header was provided</slot>
	<p>Some content between header and footer</p>
	<slot name="footer"></slot>
</div>

<style>
	.box {		
		display: flex;
		flex-direction: column;
		gap: 1em;
	}
</style>
```

그러나 바닥글의 콘텐츠는 div로 래핑하면 새로운 흐름 레이아웃이 생성되기 때문에 이 리듬에 따라 간격이 지정되지 않습니다.

`App` 구성 요소에서 `<div slot="footer">`를 변경하여 이 문제를 해결할 수 있습니다. `<div>`를 `<svelte:fragment>`로 바꿉니다.

```sv
<svelte:fragment slot="footer">
	<p>All rights reserved.</p>
	<p>Copyright (c) 2019 Svelte Industries</p>
</svelte:fragment>
```
