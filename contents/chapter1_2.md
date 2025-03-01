## 1.2 함수와 과정(함수가 생성하는)
> 전문 사진작가가 되려면 장면을 어떻게 바라봐야 할지, 다양한 노출 설정과 후처리 방법으로 사진을 인화했을 때 각 영역이 얼마나 어둡게 나올 것인지 등을 예측하는 방법을 배워야 한다.
> 그런 것들을 예측할 수 있어야 그로부터 거꾸로 추론해서 프레임, 조명, 노출, 인화 과정을 계획해서 원하는 효과를 얻을 수 있다.   
> 
> 전문적인 프로그래머가 되려면 다양한 종류의 함수가 생성하는 과정들을 시각화할 수 있어야 한다. 
> 원하는 행동을 보이는 프로그램을 안정적으로 구축하는 방법은 그런 능력을 갖추고 나서야 비로소 배울 수 있다.

## 1.2.1 선형 재귀와 반복
`n!` 계승(factorial)을 계산하는 두가지 방법

### 1. 선형 재귀적 과정
```js
function factorial(n) {
  return n === 1 ? 1 : n * factorial(n - 1);
}
```
```js
// 실행 과정 시각화
factorial(6)
6 * factorial(5)
6 * (5 * factorial(4))
6 * (5 * (4 * factorial(3)))
6 * (5 * (4 * (3 * factorial(2))))
6 * (5 * (4 * (3 * (2 * factorial(1)))))
6 * (5 * (4 * (3 * (2 * 1))))  // factorial(1) = 1 반환
6 * (5 * (4 * (3 * (2 * 1))))
6 * (5 * (4 * (3 * 2)))
6 * (5 * (4 * 6))
6 * (5 * 24)
6 * 120
720
```
- 입력 값이 크면 스택 오버플로우가 발생할 수 있다.

### 2. 선형 반복적 과정
```js
function factorial(n) {
  return fact_iter(1, 1, n);
}

function fact_iter(product, counter, max_count) {
  return counter > max_count
    ? product
    : fact_iter(counter * product, counter + 1, max_count);
}
```
```js
// 실행 과정 시각화
factorial(6)
fact_iter(1, 1, 6)   
fact_iter(1, 2, 6)   
fact_iter(2, 3, 6)   
fact_iter(6, 4, 6)   
fact_iter(24, 5, 6)  
fact_iter(120, 6, 6) 
fact_iter(720, 7, 6) 
720
```
- 선형 반복적 과정처럼 재귀적 함수로 서술되었지만 **반복적 과정을 고정된 크기 공간에서 실행**하는 구현을 꼬리 재귀적 구현이라고 부른다. 
- 꼬리 재귀는 오래전부터 알려진 컴파일러 최적화 요령 중 하나이다. **중간 결과를 누적하기 때문에 호출 스택을 쌓지 않고 다음 호출을 진행**한다.
- C, 자바, 파이썬 같은 프로그래밍 언어에서는 **함수가 소비하는 메모리와 함수 호출 횟수가 비례**해서 do, repeat, until, for, while 같은 **특별한 '루프 구조'로 반복적 과정을 서술**할 수 있다.

```
💡 자바스크립트에서는 ECMAScript 6(ES2015)에서 언어 명세에 꼬리 재귀 최적화를 포함했다.   
그러나 브라우저 중 Safari(webkit)만 꼬리 재귀 최적화를 구현하여 지원한다.

꼬리 재귀 최적화를 지원하지 않았을 때는 아래 두가지 이점을 얻을 수 있다.  
1. 스택 추적(stack trace) 가능
- 디버깅할 때 호출 경로와 중간 호출에 대한 정보를 알 수 있다. 
- 일반 재귀일 때 오류 메시지 
    `Error at factorial(1)
    at factorial(2)
    at factorial(3)
    at factorial(4)
    at factorial(5)`
- 꼬리 재귀 최적화 했을 때 오류 메시지
    `Error at factorial(1)`
2. 스택 깊이 제한 우회에 대한 안전 장치
- 자바스크립트는 무한 루프나 과도한 재귀를 방지하기 위한 스택 깊이 제한이 있다. 
- 꼬리 재귀는 무한 루프나 과도한 재귀에 걸리지 않고 무한히 실행할 수 있어서 CPU와 메모리 자원을 고갈시킬 수 있다.  

자바스크립트에서 꼬리 재귀 없이 반복적 과정을 구현할 수 있는 방법을 알아봤다. (아래 3, 4번)
```

### 3. 선형 반복적 과정을 while로 구현
```js
function factorial(n) {
  let product = 1;
  let counter = 1;

  while (counter <= n) {
    product *= counter;
    counter += 1;
  }

  return product;
}
```
- 명령형 프로그래밍. 상태 변경을 통해 계산

### 4. trampoline 패턴을 이용해서 구현
- 튀어오르기(함수 실행)와 착지하기(결과로 다른 함수 반환)를 반복하는 패턴
```js
// 함수 실행과 반환을 반복할 함수
function trampoline(fn) {
  return function(...args) {
    let result = fn(...args);
    while (typeof result === "function") {
      result = result();
    }
    return result;
  };
}

// trampoline으로 실행할 factorial 함수
function factorial(n, acc = 1) {
  if (n <= 1) return acc;
  return () => factorial(n - 1, n * acc);
}

const trampolinedFactorial = trampoline(factorial);
```

```js
// 실행 과정 시각화
trampolinedFactorial(6) 
→ factorial(6, 1)
→ () => factorial(5, 6) 반환
→ 이 함수 실행 → () => factorial(4, 30) 반환
→ 이 함수 실행 → () => factorial(3, 120) 반환
→ 이 함수 실행 → () => factorial(2, 360) 반환
→ 이 함수 실행 → () => factorial(1, 720) 반환
→ 이 함수 실행 → 720 반환
```
- trampoline으로 실행한 함수가 함수를 결과로 반환하면 다시 함수를 실행해서 함수가 아닌 결과가 나올 때 까지 반복한다.
- 함수형 프로그래밍. 상태를 누적(acc) 인자로 전달해서 계산

```
💡 trampoline 패턴은 자바스크립트에서도 함수형 방식으로 꼬리 재귀를 구현할 수 있지만 나머지 방식보다 가독성은 떨어져 보인다.
최적화가 중요하고 side effect가 없어야 하는 큰 단위 작업에서 유용하게 쓸 수 있을 것 같다.
```

### 연습문제 1.9
### 연습문제 1.10