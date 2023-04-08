---
title: beforeUpdate and afterUpdate
---

`beforeUpdate` 함수는 DOM이 업데이트되기 직전에 작업이 수행되도록 예약합니다. `afterUpdate`는 DOM이 데이터와 동기화되면 코드를 실행하는 데 사용되는 대응 항목입니다.

함께 요소의 스크롤 위치를 업데이트하는 것과 같이 순전히 상태 기반 방식으로 달성하기 어려운 작업을 명령적으로 수행하는 데 유용합니다.

이 [Eliza](https://en.wikipedia.org/wiki/ELIZA) 챗봇은 채팅창을 계속 스크롤해야 하기 때문에 사용하기 불편합니다. 문제를 해결해 보겠습니다.

```js
let div;
let autoscroll;

beforeUpdate(() => {
	autoscroll = div && (div.offsetHeight + div.scrollTop) > (div.scrollHeight - 20);
});

afterUpdate(() => {
	if (autoscroll) div.scrollTo(0, div.scrollHeight);
});
```

`beforeUpdate`는 구성 요소가 마운트되기 전에 먼저 실행되므로 해당 속성을 읽기 전에 `div`의 존재를 확인해야 합니다.
