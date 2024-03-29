---
layout: blog
title:  "Asynchronous Javascript & Promise"
date:   2022-11-06 00:00
category: gdsc-yonsei
---

# What is 'Asynchronous Javascript'?

## Synchronous

> The word 'synchronous' means the browser steps through the program one line at a time, in the order we wrote it.

이러한 synchronous 처리는 UX를 중요시하는 웹에선 적절하지 않다. 계산이 많이 필요한 작업이 있더라도 다른 완료 가능한 작업부터 먼저 완료하여야 한다. 따라서 다음 코드는 예상과 다르게 동작한다. 그리고 이 특이한 동작 방식이 asynchronous 방식이다.

## Asynchronous

```javascript
console.log('1');
setTimeout(function() {
    console.log('2');
}, 3000);
console.log('3');
// Output: 1 -> 3 -> wait 3sec -> 2
```

javascript를 이용해 간단한 어플리케이션을 만들 때 event handler(listner 등)를 많이 사용하는데, 이 event listner의 역할이 바로 asynchronous operation의 완료를 caller에게 notify하는 것이다. 이 또한 javascript에서의 synchronous 처리 방식 중 하나이다.

하지만 코드를 synchronous하게 동작하도록 하는 방법은 몇 가지 더 존재한다.

# Callback

> A callback is a function that's passed into another function, with the expectation that the callback will be called at the appropriate time.

아까의 코드를 callback을 이용하여 synchronous하게 동작하도록 할 수 있다.

```javascript
function step1to2(callback) {
    console.log('1');
    setTimeout(function() {
        console.log('2');
        callback(step3);
    }, 3000);    
}

function step3() {
    console.log('3');
}

step1to2(step3);

// Output: 1 -> wait 3sec -> 2 -> 3
```

그리고 event handler의 사용 방식을 보면 event handler도 callback의 일종인 것을 알 수 있다. 그러나 순서를 지켜주어야 하는 연산이 많은 경우엔 callback 내부에서 callback을 호출하는 일을 반복하게 되고 이는 코드를 읽거나 디버그하기 어렵게 만든다(callback hell). 따라서 조금 더 읽기 편한 asynchronous 처리 문법이 필요하고 그래서 도입된 것이 promise이다.

# Promise

> A Promise is an object representing the eventual completion or failure of an asynchronous operation.

다음과 같은 방식으로 사용할 수 있다.

```javascript
doSomething().then(successCallback, failureCallback);
```

Promise 문법의 몇 가지 규칙은 다음과 같다.
- Callbacks added with then() will never be invoked before the completion of the current run of the JavaScript event loop.
- These callbacks will be invoked even if they were added after the success or failure of the asynchronous operation that the promise represents.
- Multiple callbacks may be added by calling then() several times. They will be invoked one after another, in the order in which they were inserted.

이는 promise 객체 그 자체와 then에 사용된 callback들이 javascript event loop의 message queue에 들어간다는 뜻이다. 이제 callback 방식과 promise 방식을 비교해봄으로써 promise를 왜 사용하는지 알 수 있다. 먼저 callback 방식이다.

```javascript
step1(function (result1) {
    step2(result1, function(result2) {
        step3(result2, function(result3) {
            console.log(`Final result: ${result3}`);
        }, failureCallback);
    }, failureCallback);
}, failureCallback);
```

그리고 promise 방식이다.

```javascript
step1().then(function (result1) {
    return step2(result1);
}).then(function (result2) {
    return step3(result2);
}).then(function (result3) {
    console.log(`Final result: ${result3}`);
}).catch(failureCallback); // short for then(null, failureCallback)
```

여기에 arrow functions 방식을 적용하면 경우에 따라 더 읽기에 편하다.

```javascript
// () => x is short for () => { return x; }
step1()
    .then((result1) => step2(result1))
    .then((result2) => step3(result2))
    .then((result3) => {
        console.log(`Final result: ${result3}`);
    })
    .catch(failureCallback);
```

# Reference
https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop