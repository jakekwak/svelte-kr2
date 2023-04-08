---
title: Sharing code
---

지금까지 본 모든 예에서 `<script>` 블록에는 각 구성 요소 인스턴스가 초기화될 때 실행되는 코드가 포함되어 있습니다. 대부분의 구성 요소의 경우 이것이 필요한 전부입니다.

매우 가끔 개별 구성 요소 인스턴스 외부에서 일부 코드를 실행해야 합니다. 예를 들어 이 오디오 플레이어 5개를 모두 동시에 재생할 수 있습니다. 하나를 재생하면 다른 모든 것을 중지하는 것이 좋습니다.

`<script context="module">` 블록을 선언하면 됩니다. 내부에 포함된 코드는 구성 요소가 인스턴스화될 때가 아니라 모듈이 처음 평가될 때 한 번 실행됩니다. 이것을 `AudioPlayer.svelte`의 맨 위에 놓습니다.

```html
<script context="module">
	let current;
</script>
```

이제 상태 관리 없이 구성 요소가 서로 '대화'할 수 있습니다.

```js
function stopOthers() {
	if (current && current !== audio) current.pause();
	current = audio;
}
```
