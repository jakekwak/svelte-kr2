---
title: Adding parameters
---

전환 및 애니메이션과 마찬가지로 작업은 인수를 취할 수 있으며 작업 함수는 해당 요소와 함께 호출됩니다.

여기에서는 사용자가 주어진 시간 동안 버튼을 누르고 있을 때마다 같은 이름의 이벤트를 발생시키는 `longpress` 작업을 사용하고 있습니다. 지금 `longpress.js` 파일로 전환하면 500ms로 하드코딩된 것을 볼 수 있습니다.

액션 함수를 변경하여 `duration`을 두 번째 인수로 받아들이고 `duration`을 `setTimeout` 호출에 전달할 수 있습니다.

```js
export function longpress(node, duration) {
	// ...

	const handleMousedown = () => {
		timer = setTimeout(() => {
			node.dispatchEvent(
				new CustomEvent('longpress')
			);
		}, duration);
	};

	// ...
}
```

`App.svelte`로 돌아가서 `duration` 값을 작업에 전달할 수 있습니다.

```html
<button use:longpress={duration}
```

이것은 *거의* 작동합니다. 이제 이벤트는 2초 후에만 시작됩니다. 그러나 지속 시간을 아래로 슬라이드해도 여전히 2초가 걸립니다.

이를 변경하기 위해 `longpress.js`에 `update` 메서드를 추가할 수 있습니다. 인수가 변경될 때마다 호출됩니다.

```js
return {
	update(newDuration) {
		duration = newDuration;
	},
	// ...
};
```

> 작업에 여러 인수를 전달해야 하는 경우 `use:longpress={{duration, spiciness}}`와 같이 단일 개체로 결합합니다.