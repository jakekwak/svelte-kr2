---
title: Readable stores
---

참조가 있는 사람이 모든 상점을 쓸 수 있는 것은 아닙니다. 예를 들어 마우스 위치나 사용자의 지리적 위치를 나타내는 상점이 있을 수 있으며 '외부'에서 이러한 값을 설정할 수 있다는 것은 말이 되지 않습니다. 이러한 경우를 위해 *읽을 수 있는* 저장소가 있습니다.

`stores.js` 탭을 클릭하세요. `reader`의 첫 번째 인수는 초기 값이며, 아직 없는 경우 `null` 또는 `undefined`일 수 있습니다. 두 번째 인수는 `set` 콜백을 받고 `stop` 함수를 반환하는 `start` 함수입니다. `start` 함수는 상점이 첫 구독자를 확보할 때 호출됩니다. 마지막 구독자가 구독을 취소하면 `stop`이 호출됩니다.

```js
export const time = readable(new Date(), function start(set) {
	const interval = setInterval(() => {
		set(new Date());
	}, 1000);

	return function stop() {
		clearInterval(interval);
	};
});
```
