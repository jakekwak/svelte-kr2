---
title: Component format
---

---

구성 요소는 Svelte 애플리케이션의 빌딩 블록입니다. 그들은 HTML의 상위 집합을 사용하여 `.svelte` 파일에 기록됩니다.

스크립트, 스타일 및 마크업의 세 섹션은 모두 선택 사항입니다.

```sv
<script>
	// logic goes here
</script>

<!-- markup (zero or more items) goes here -->

<style>
	/* styles go here */
</style>
```

### &lt;script&gt;

`<script>` 블록에는 구성 요소 인스턴스가 생성될 때 실행되는 JavaScript가 포함되어 있습니다. 최상위 수준에서 선언된(또는 가져온) 변수는 구성 요소의 마크업에서 '볼 수' 있습니다. 네 가지 추가 규칙이 있습니다.

#### 1. `export` creates a component prop

---

Svelte는 'export' 키워드를 사용하여 변수 선언을 *property* 또는 *prop*로 표시합니다. 이는 구성 요소의 소비자가 액세스할 수 있음을 의미합니다(자세한 내용은 [attributes and props](/docs#template-syntax-attributes-and-props) 섹션 참조).

```sv
<script>
	export let foo;

	// Values that are passed in as props
	// are immediately available
	console.log({ foo });
</script>
```

---

소품에 대한 기본 초기 값을 지정할 수 있습니다. 구성 요소를 인스턴스화할 때 구성 요소의 소비자가 구성 요소에 소품을 지정하지 않은 경우(또는 초기 값이 `undefined`인 경우) 사용됩니다. 소비자가 소품을 제거할 때마다 해당 값은 초기 값이 아닌 `undefined`으로 설정됩니다.

개발 모드에서([컴파일러 옵션](/docs#compile-time-svelte-compile) 참조) 기본 초기 값이 제공되지 않고 소비자가 값을 지정하지 않으면 경고가 인쇄됩니다. 이 경고를 억제하려면 `undefined`인 경우에도 기본 초기 값이 지정되어 있는지 확인하십시오.

```sv
<script>
	export let bar = 'optional default initial value';
	export let baz = undefined;
</script>
```

---

`const`, `class` 또는 `function`을 내보내면 구성 요소 외부에서 읽기 전용입니다. 그러나 아래와 같이 함수는 유효한 prop 값입니다.

```sv
<script>
	// these are readonly
	export const thisIs = 'readonly';

	export function greet(name) {
		alert(`hello ${name}!`);
	}

	// this is a prop
	export let format = n => n.toFixed(2);
</script>
```

읽기 전용 소품은 [`bind:this` 구문](/docs#template-syntax-component-directives-bind-this)을 사용하여 구성 요소에 연결된 요소의 속성으로 액세스할 수 있습니다.

---

예약어를 소품 이름으로 사용할 수 있습니다.

```sv
<script>
	let className;

	// creates a `class` property, even
	// though it is a reserved word
	export { className as class };
</script>
```

#### 2. Assignments are 'reactive'

---

구성 요소 상태를 변경하고 다시 렌더링을 트리거하려면 로컬에서 선언된 변수에 할당하기만 하면 됩니다.

업데이트 표현식(`count += 1`)과 속성 할당(`obj.x = y`)은 동일한 효과를 가집니다.

```sv
<script>
	let count = 0;

	function handleClick () {
		// calling this function will trigger an
		// update if the markup references `count`
		count = count + 1;
	}
</script>
```

---

Svelte의 반응성은 할당을 기반으로 하기 때문에 `.push()` 및 `.splice()`와 같은 배열 메서드를 사용하면 업데이트가 자동으로 트리거되지 않습니다. 업데이트를 트리거하려면 후속 할당이 필요합니다. 이에 대한 자세한 내용은 [튜토리얼](/tutorial/updating-arrays-and-objects)에서도 확인할 수 있습니다.

```sv
<script>
	let arr = [0, 1];

	function handleClick () {
		// this method call does not trigger an update
		arr.push(2);
		// this assignment will trigger an update
		// if the markup references `arr`
		arr = arr
	}
</script>
```

---

Svelte의 `<script>` 블록은 구성 요소가 생성될 때만 실행되므로 `<script>` 블록 내의 할당은 prop이 업데이트될 때 자동으로 다시 실행되지 않습니다. 소품에 대한 변경 사항을 추적하려면 다음 섹션의 다음 예제를 참조하십시오.

```sv
<script>
	export let person;
	// this will only set `name` on component creation
	// it will not update when `person` does
	let { name } = person;
</script>
```

#### 3. `$:` marks a statement as reactive

---

모든 최상위 문(예: 블록이나 함수 내부가 아님)은 `$:` [JS 레이블 구문](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label)을 접두사를 붙여 반응형으로 만들 수 있습니다. 반응형 문은 의존하는 값이 변경될 때마다 다른 스크립트 코드 이후와 구성 요소 마크업이 렌더링되기 전에 실행됩니다.

```sv
<script>
	export let title;
	export let person;

	// this will update `document.title` whenever
	// the `title` prop changes
	$: document.title = title;

	$: {
		console.log(`multiple statements can be combined`);
		console.log(`the current title is ${title}`);
	}

	// this will update `name` when 'person' changes
	$: ({ name } = person);

	// don't do this. it will run before the previous line
	let name2 = name;
</script>
```

---

`$:` 블록 내에 직접 나타나는 값만 반응문의 종속성이 됩니다. 예를 들어 아래 코드에서 `x`가 변경될 때만 `total`은 업데이트되지만 `y`가 변경될 때는 업데이트되지 않습니다.

```sv
<script>
	let x = 0;
	let y = 0;

	function yPlusAValue(value) {
		return value + y;
	}

	$: total = yPlusAValue(x);
</script>

Total: {total}
<button on:click={() => x++}>
	Increment X
</button>

<button on:click={() => y++}>
	Increment Y
</button>
```

---
반응형 블록은 컴파일 시간에 간단한 정적 분석을 통해 정렬되며, 컴파일러가 보는 모든 변수는 블록 자체에서 할당되고 사용되는 변수이며 블록에서 호출하는 함수가 아닙니다. 이는 다음 예에서 `x`가 업데이트될 때 `yDependent`가 업데이트되지 않음을 의미합니다.

```sv
<script>
	let x = 0;
	let y = 0;

	const setY = (value) => {
		y = value;
	}

	$: yDependent = y;
	$: setY(x);
</script>
```

`$: yDependent = y` 행을 `$: setY(x)` 아래로 이동하면 `x`가 업데이트될 때 `yDependent`도 업데이트됩니다.

---

명령문이 선언되지 않은 변수에 대한 할당으로만 구성된 경우 Svelte는 사용자를 대신하여 `let` 선언을 삽입합니다.

```sv
<script>
	export let num;

	// we don't need to declare `squared` and `cubed`
	// — Svelte does it for us
	$: squared = num * num;
	$: cubed = squared * num;
</script>
```

#### 4. Prefix stores with `$` to access their values

---

*스토어*는 간단한 *스토어 계약*을 통해 값에 대한 반응적 액세스를 허용하는 개체입니다. [`svelte/store` 모듈](/docs#run-time-svelte-store)에는 이 계약을 이행하는 최소 저장소 구현이 포함되어 있습니다.

스토어에 대한 참조가 있을 때마다 `$` 문자를 접두어로 지정하여 구성 요소 내부의 값에 액세스할 수 있습니다. 이로 인해 Svelte는 접두사 변수를 선언하고 구성 요소 초기화 시 저장소를 구독하고 적절한 경우 구독을 취소합니다.

`$` 접두사가 붙은 변수에 할당하려면 변수가 쓰기 가능한 저장소여야 하며 저장소의 `.set` 메소드에 대한 호출이 발생합니다.

저장소는 예를 들어 `if` 블록이나 함수 내부가 아니라 구성 요소의 최상위 수준에서 선언되어야 합니다.

지역 변수(스토어 값을 나타내지 않음)는 `$` 접두사가 *없어야* 합니다.

```sv
<script>
	import { writable } from 'svelte/store';

	const count = writable(0);
	console.log($count); // logs 0

	count.set(1);
	console.log($count); // logs 1

	$count = 2;
	console.log($count); // logs 2
</script>
```

##### Store contract

```js
store = { subscribe: (subscription: (value: any) => void) => (() => void), set?: (value: any) => void }
```

*스토어 계약*을 구현하여 [`svelte/store`](/docs#run-time-svelte-store)에 의존하지 않고 나만의 스토어를 만들 수 있습니다.

1. 스토어는 구독 기능을 인수로 수락해야 하는 `.subscribe` 메서드를 포함해야 합니다. 이 구독 기능은 `.subscribe`를 호출할 때 상점의 현재 값으로 즉시 동기적으로 호출되어야 합니다. 상점의 모든 활성 구독 기능은 나중에 상점의 값이 변경될 때마다 동기적으로 호출되어야 합니다.
2. `.subscribe` 메서드는 구독 취소 기능을 반환해야 합니다. 구독 취소 기능을 호출하면 구독이 중지되어야 하며 상점에서 해당 구독 기능을 다시 호출해서는 안 됩니다.
3. 상점은 *선택적으로* `.set` 메소드를 포함할 수 있습니다. 이 메소드는 상점의 새 값을 인수로 수락해야 하며 상점의 모든 활성 구독 기능을 동기식으로 호출합니다. 이러한 저장소를 *쓰기 가능한 저장소*라고 합니다.

RxJS Observables와의 상호 운용성을 위해 `.subscribe` 메서드는 구독 취소 함수를 직접 반환하는 대신 `.unsubscribe` 메서드로 객체를 반환할 수도 있습니다. 그러나 `.subscribe`가 구독을 동기적으로 호출하지 않는 한(Observable 사양에 필요하지 않음) Svelte는 스토어 값을 `undefined`로 볼 수 있습니다.


### &lt;script context="module"&gt;

---

`context="module"` 속성이 있는 `<script>` 태그는 각 구성 요소 인스턴스가 아니라 모듈이 처음 평가될 때 한 번 실행됩니다. 이 블록에 선언된 값은 일반 `<script>`(및 구성 요소 마크업)에서 액세스할 수 있지만 그 반대는 아닙니다.

이 블록에서 바인딩을 `export`할 수 있으며 컴파일된 모듈의 내보내기가 됩니다.

기본 내보내기는 구성요소 자체이므로 `export default`를 할 수 없습니다.

> `module` 스크립트에 정의된 변수는 반응형이 아닙니다. 다시 할당해도 변수 자체가 업데이트되더라도 다시 렌더링되지 않습니다. 여러 구성요소 간에 공유되는 값의 경우 [store](/docs#run-time-svelte-store)를 사용하는 것이 좋습니다.

```sv
<script context="module">
	let totalComponents = 0;

	// this allows an importer to do e.g.
	// `import Example, { alertTotal } from './Example.svelte'`
	export function alertTotal() {
		alert(totalComponents);
	}
</script>

<script>
	totalComponents += 1;
	console.log(`total number of times this component has been created: ${totalComponents}`);
</script>
```


### &lt;style&gt;

---

`<style>` 블록 내부의 CSS는 해당 구성 요소로 범위가 지정됩니다.

이는 구성 요소 스타일의 해시(예: `svelte-123xyz`)를 기반으로 영향을 받는 요소에 클래스를 추가하여 작동합니다.

```sv
<style>
	p {
		/* this will only affect <p> elements in this component */
		color: burlywood;
	}
</style>
```

---

전역적으로 선택기에 스타일을 적용하려면 `:global(...)` 한정자를 사용하세요.

```sv
<style>
	:global(body) {
		/* this will apply to <body> */
		margin: 0;
	}

	div :global(strong) {
		/* this will apply to all <strong> elements, in any
			 component, that are inside <div> elements belonging
			 to this component */
		color: goldenrod;
	}

	p:global(.red) {
		/* this will apply to all <p> elements belonging to this
			 component with a class of red, even if class="red" does
			 not initially appear in the markup, and is instead
			 added at runtime. This is useful when the class
			 of the element is dynamically applied, for instance
			 when updating the element's classList property directly. */
	}
</style>
```

---

전역적으로 액세스할 수 있는 @keyframe을 만들려면 키프레임 이름 앞에 `-global-`을 추가해야 합니다.

`-global-` 부분은 컴파일 시 제거되며 키프레임은 코드의 다른 위치에서 `my-animation-name`만 사용하여 참조됩니다.

```html
<style>
	@keyframes -global-my-animation-name {...}
</style>
```

---

구성 요소당 최상위 수준 `<style>` 태그는 1개만 있어야 합니다.

그러나 `<style>` 태그가 다른 요소나 논리 블록 내에 중첩될 수 있습니다.

이 경우 `<style>` 태그는 DOM에 있는 그대로 삽입되며 `<style>` 태그에서 범위 지정이나 처리가 수행되지 않습니다.

```html
<div>
	<style>
		/* this style tag will be inserted as-is */
		div {
			/* this will apply to all `<div>` elements in the DOM */
			color: red;
		}
	</style>
</div>
```
