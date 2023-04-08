---
question: How do I document my components?
---

Svelte Language Server를 사용하는 편집기에서는 특별히 형식이 지정된 주석을 사용하여 구성 요소, 기능 및 내보내기를 문서화할 수 있습니다.

````sv
<script>
	/** What should we call the user? */
	export let name = 'world';
</script>

<!--
@component
Here's some documentation for this component.
It will show up on hover.

- You can use markdown here.
- You can also use code blocks here.
- Usage:
  ```tsx
  <main name="Arethra">
  ```
-->
<main>
	<h1>
		Hello, {name}
	</h1>
</main>
````

참고: 구성 요소를 설명하는 HTML 주석에는 `@component`가 필요합니다.
