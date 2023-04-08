---
title: Checking for slot content
---

경우에 따라 부모가 특정 슬롯에 대한 콘텐츠를 전달하는지 여부에 따라 구성 요소의 일부를 제어할 수 있습니다. 아마도 해당 슬롯 주변에 래퍼가 있고 슬롯이 비어 있으면 렌더링하고 싶지 않을 것입니다. 또는 슬롯이 있는 경우에만 클래스를 적용하고 싶을 수도 있습니다. 특수 `$$slots` 변수의 속성을 확인하여 이를 수행할 수 있습니다.

`$$slots`는 키가 상위 구성요소에 의해 전달된 슬롯의 이름인 객체입니다. 부모가 슬롯을 비워두면 `$$slots`에는 해당 슬롯에 대한 항목이 없습니다.

이 예에서 `<Project>`의 두 인스턴스 모두 주석이 있는 경우에도 주석과 알림 점에 대한 컨테이너를 렌더링합니다. 부모 `<App>`이 `comments` 슬롯에 대한 콘텐츠를 전달할 때만 이러한 요소를 렌더링하도록 `$$slots`를 사용하려고 합니다.

`Project.svelte`에서 `<article>`의 `class:has-discussion` 지시문을 업데이트합니다.

```html
<article class:has-discussion={$$slots.comments}>
```

다음으로 `comments` 슬롯과 `<div>`를 `$$slots`를 확인하는 `if` 블록으로 래핑합니다.

```html
{#if $$slots.comments}
	<div class="discussion">
		<h3>Comments</h3>
		<slot name="comments"></slot>
	</div>
{/if}
```

이제 `<App>`이 `comments` 슬롯을 비워두면 댓글 컨테이너와 알림 점이 렌더링되지 않습니다.
