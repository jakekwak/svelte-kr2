---
title: Adding data
---

일부 정적 마크업만 렌더링하는 구성 요소는 그리 흥미롭지 않습니다. 일부 데이터를 추가해 보겠습니다.

먼저 구성 요소에 스크립트 태그를 추가하고 `name` 변수를 선언합니다.

```html
<script>
	let name = 'world';
</script>

<h1>Hello world!</h1>
```

그런 다음 마크업에서 `name`을 참조할 수 있습니다.

```html
<h1>Hello {name}!</h1>
```

중괄호 안에 원하는 JavaScript를 넣을 수 있습니다. 더 큰 소리로 인사하려면 `name`을 `name.toUpperCase()`로 변경해 보세요.