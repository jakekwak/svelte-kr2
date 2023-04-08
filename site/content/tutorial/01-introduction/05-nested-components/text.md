---
title: Nested components
---

전체 앱을 단일 구성 요소에 넣는 것은 비실용적입니다. 대신 다른 파일에서 구성 요소를 가져온 다음 요소를 포함하는 것처럼 사용할 수 있습니다.

이제 오른쪽(상단 막대)에 있는 편집기에 `App.svelte` 및 `Nested.svelte`를 클릭할 수 있는 2개의 파일이 표시됩니다.

각 `.svelte` 파일에는 함께 속한 HTML, CSS 및 JavaScript를 캡슐화하는 재사용 가능한 자체 포함 코드 블록인 구성 요소가 있습니다.

파일(구성 요소) `Nested.svelte`를 앱으로 가져오는 `App.svelte`에 `<script>` 태그를 추가해 보겠습니다.

```html
<script>
	import Nested from './Nested.svelte';
</script>
```

그런 다음 앱 마크업에서 컴포넌트 `Nested`를 사용하십시오.

```html
<p>This is a paragraph.</p>
<Nested/>
```

`Nested.svelte`에 `<p>` 요소가 있더라도 `App.svelte`의 스타일이 누출되지 않는다는 점에 유의하세요.

또한 구성 요소 이름 `Nested`가 대문자로 표시되어 있습니다. 이 규칙은 사용자 정의 구성 요소와 일반 HTML 태그를 구별할 수 있도록 채택되었습니다.
