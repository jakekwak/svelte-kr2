---
title: Transition events
---

전환이 시작되고 끝나는 시기를 아는 것이 유용할 수 있습니다. Svelte는 다른 DOM 이벤트처럼 수신할 수 있는 이벤트를 전달합니다.

```html
<p
	transition:fly="{{ y: 200, duration: 2000 }}"
	on:introstart="{() => status = 'intro started'}"
	on:outrostart="{() => status = 'outro started'}"
	on:introend="{() => status = 'intro ended'}"
	on:outroend="{() => status = 'outro ended'}"
>
	Flies in and out
</p>
```