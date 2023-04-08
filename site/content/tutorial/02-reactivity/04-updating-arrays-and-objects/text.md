---
title: Updating arrays and objects
---

Svelte의 반응성은 할당에 의해 트리거됩니다. 배열이나 객체를 변경하는 메서드는 자체적으로 업데이트를 트리거하지 않습니다.

이 예에서 "Add a number" 버튼을 클릭하면 배열에 숫자를 추가하지만 `sum`의 재계산을 트리거하지 않는 `addNumber` 함수가 호출됩니다.

이를 수정하는 한 가지 방법은 자신에게 `nubmers`를 할당하여 컴파일러에게 변경되었음을 알리는 것입니다.

```js
function addNumber() {
	numbers.push(numbers.length + 1);
	numbers = numbers;
}
```

ES6 확산 구문을 사용하여 더 간결하게 작성할 수도 있습니다.

```js
function addNumber() {
	numbers = [...numbers, numbers.length + 1];
}
```

`pop`, `shift` 및 `splice`와 같은 배열 방법과 `Map.set`, `Set.add` 등과 같은 객체 방법에도 동일한 규칙이 적용됩니다.

배열 및 객체의 *속성*에 할당 — 예: `obj.foo += 1` 또는 `array[i] = x` — 값 자체에 대한 할당과 동일한 방식으로 작동합니다.

```js
function addNumber() {
	numbers[numbers.length] = numbers.length + 1;
}
```

그러나 이와 같은 참조에 대한 간접 할당은...

```js
const foo = obj.foo;
foo.bar = 'baz';
```

또는

```js
function quox(thing) {
	thing.foo.bar = 'baz';
}
quox(obj);
```

...`obj = obj`로 추적하지 않는 한 `obj.foo.bar`에서 반응성을 트리거하지 않습니다.

간단한 경험 법칙: 업데이트된 변수는 할당의 왼쪽에 직접 나타나야 합니다.