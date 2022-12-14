---
layout: blog
title:  "Difference btwn. Type Predicate and Boolean"
date:   2022-12-14 00:00
category: gdsc-yonsei
---

TypeScript를 공부하다가 든 의문점이다.
```typescript
class A { a: Boolean };
class B { b: Boolean };

function isA(x: A | B): x is A {
    return (x as A).a !== undefined;
}

const t = new A();
if (isA(t)) console.log('t is a !!!');
```
위와 같은 type predicate라는 기능이 있는데, 결국 boolean 값을 반환하고 있지 않은가? 그냥 return이 boolean을 반환한다고 알려주면 될 텐데...

그럼에도 굳이 type predicate를 쓰는 이유는 아래 코드에서 알 수 있다.
```typescript
class A { a: Boolean };
class B { b: Boolean };

function isAGood(x: A | B): x is A {
    return (x as A).a !== undefined;
}

function isABad(x: A | B): Boolean {
    return (x as A).a !== undefined;
}

const t = new A();
if (isAGood(t)) t.a; // works well !!!
if (isABad(t)) t.a; // error
```
type predicate를 쓰더라도 기능은 동일하지만, 컴파일러 또는 ide가 에러를 잡을 수 있다는 장점이 생긴다!