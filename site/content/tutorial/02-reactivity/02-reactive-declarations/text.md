---
title: Declarations
---

Svelte의 반응성은 이전 섹션에 표시된 것처럼 DOM을 애플리케이션의 변수와 동기화 상태로 유지할 뿐만 아니라 반응 선언을 사용하여 변수를 서로 동기화 상태로 유지할 수도 있습니다. 그들은 다음과 같이 보입니다:

```js
let count = 0;
$: doubled = count * 2;
```

> 조금 낯설게 보이더라도 걱정하지 마세요. 그것은 [유효한](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label)(틀에 얽매이지 않는 경우) JavaScript입니다. Svelte는 '참조된 값이 변경될 때마다 이 코드를 다시 실행'하는 의미로 해석합니다. 익숙해지면 되돌릴 수 없습니다.

마크업에서 `doubled`를 사용합시다:

```html
<p>{count} doubled is {doubled}</p>
```

물론 대신 마크업에 `{count * 2}`를 작성할 수 있습니다. 반응형 값을 사용할 필요가 없습니다. 반응형 값은 여러 번 참조해야 하거나 *다른* 반응형 값에 의존하는 값이 있을 때 특히 유용합니다.
