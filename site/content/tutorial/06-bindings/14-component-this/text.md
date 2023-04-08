---
title: Binding to component instances
---

DOM 요소에 바인딩할 수 있는 것처럼 구성 요소 인스턴스 자체에 바인딩할 수 있습니다. 예를 들어 `<InputField>` 인스턴스를 DOM 요소를 바인딩할 때와 같은 방식으로 `field`라는 변수에 바인딩할 수 있습니다.

```html
<script>
	let field;
</script>

<InputField bind:this={field} />
```

이제 `field`를 사용하여 이 구성 요소와 프로그래밍 방식으로 상호 작용할 수 있습니다.

```html
<button on:click="{() => field.focus()}">
	Focus field
</button>
```

> 버튼이 처음 렌더링될 때 필드가 정의되지 않고 오류가 발생하므로 `{field.focus}`를 수행할 수 없습니다.
