---
title: The @debug tag
---

경우에 따라 데이터가 앱을 통과할 때 데이터를 검사하는 것이 유용합니다.

한 가지 방법은 마크업 내부에 `console.log(...)`를 사용하는 것입니다. 그러나 실행을 일시 중지하려면 검사하려는 값의 쉼표로 구분된 목록과 함께 `{@debug ...}` 태그를 사용할 수 있습니다.

```html
{@debug user}

<h1>Hello {user.firstname}!</h1>
```

이제 devtools를 열고 `<input>` 요소와 상호 작용하기 시작하면 `user` 값이 변경될 때 디버거를 트리거합니다.
