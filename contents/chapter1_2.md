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

### 연습문제 1.9 plus(a, b) 함수 생성 과정을 치환 모형으로 표현하기
> 다음 두 함수는 주어진 인수를 1 증가하는 함수 inc와 1 감소하는 함수 dec를 이용해서 두 양의 정수의 덧셈을 구현한다.
> `plus(4, 5)`를 평가할 때 각 함수가 생성하는 과정을 치환 모형으로 표현하라. 이 과정들은 반복적인가, 아니면 재귀적인가?

```js
// 재귀적
function plus(a, b) {
  return a === 0 ? b : inc(plus(dec(a), b));
}

// 과정 - 계산이 끝날 때까지 지연된 계산들을 저장함
plus(4, 5);
inc(plus(dec(4), 5));
inc(plus(3, 5));
inc(inc(plus(dec(3), 5)));
inc(inc(plus(2, 5)));
inc(inc(inc(plus(dec(2), 5))));
inc(inc(inc(plus(1, 5))));
inc(inc(inc(inc(plus(dec(1), 5)))));
inc(inc(inc(inc(plus(0, 5)))));
inc(inc(inc(inc(5))));
inc(inc(inc(6)));
inc(inc(7));
inc(8);
9;
```
```js
// 반복적
function plus(a, b) {
  return a === 0 ? b : plus(dec(a), inc(b));
}

// 과정 - 상태가 매 실행마다 담겨있음. 지연된 계산이 없음
plus(4, 5)
plus(dec(4), inc(5));
plus(3, 6);
plus(dec(3), inc(6));
plus(2, 7);
plus(dec(2), inc(7));
plus(1, 8);
plus(dec(1), inc(8));
plus(0, 9);
9;
```

### 연습문제 1.10 애커만 함수
> 다음 함수는 애커만 함수라고 부르는 수학 함수를 계산한다.

```js
function A(x, y) {
  return y === 0 
    ? 0
    : x === 0
    ? 2 * y
    : y === 1
    ? 2
    : A(x - 1, A(x, y - 1));    
}
```
> 다음 문장들은 각각 어떤 값으로 평가되는가?

```js
// 1번 문제 - 결과: 1024
// - 2^10: 아래 g(n) 함수 참고
A(1, 10); 


// 2번 문제 - 결과: 65,536
// - 2^(2^(2^(2))) 아래 h(n) 함수 참고
A(2, 4); 

// 전개 과정
4 === 0 // y는 0 아님
  ? 0
  : 2 === 0 // x는 0 아님
    ? 2 * y
    : 4 === 1 // y는 1 아님
      ? 2
      : A(2 - 1, A(2, 4 - 1)); // 실행

A(2 - 1, A(2, 4 - 1));
A(1, A(2, 3));
A(1, A(2 - 1, A(2, 3 - 1)));
A(1, A(1, A(2, 2)));
A(1, A(1, A(2 - 1, A(2, 2 - 1))));
A(1, A(1, A(1, A(2, 1))));
A(1, A(1, A(1, 2)));
A(1, A(1, A(1 - 1, A(1, 2 - 1))));
A(1, A(1, A(0, A(1, 1))));
A(1, A(1, A(0, 2)));
A(1, A(1, 4));
A(1, A(1 - 1, A(1, 4 - 1)));
A(1, A(0, A(1, 3)));
A(1, A(0, A(1 - 1, A(1, 3 - 1))));
A(1, A(0, A(0, A(1, 2))));
A(1, A(0, A(0, A(1 - 1, A(1, 2 - 1)))));
A(1, A(0, A(0, A(0, A(1, 1)))));
A(1, A(0, A(0, A(0, 2))));
A(1, A(0, A(0, 4)));
A(1, A(0, 8));
A(1, 16);
A(1 - 1, A(1, 16 - 1));
A(0, A(1, 15));
A(0, A(1 - 1, A(1, 15 - 1)));
A(0, A(0, A(1, 14)));
A(0, A(1 - 1, A(1, 14 - 1)));
A(0, A(0, A(1, 13)));
A(0, A(0, A(1 - 1, A(1, 13 - 1))));
A(0, A(0, A(0, A(1, 12))));
// ... 계속 중첩 계산되면서 결과값이 크게 증가함
```


> 양의 정수 n에 대해 f, g, h가 계산하는 함수를 각각 간결한 수식으로 표현하라.
```js
function A(x, y) {
  return y === 0 ? 0 : 
    x === 0 ? 2 * y : 
    y === 1 ? 2 : 
    A(x - 1, A(x, y - 1));    
}

// 수식: 2 * n
// A 함수는 x가 0일 경우 항상 2 * y를 반환함
// y가 0일 경우 2 * 0은 0이므로 y === 0 ? 0에 일치함
function f(n) {
  return A(0, n);
}

// 수식: 2^n
// y가 0이면 0, y가 1이면 2, 그 외에는 `A(1 - 1, A(1, y - 1))` 반환
// A(0, A(1, y - 1))에서 A(1, y - 1) 부분은 x가 1이라서 2의 n제곱 형태로 값이 늘어남
// - `A(1, 1) = 2`
// - `A(1, 2) = A(0, A(1, 1)) = A(0, 2) = 4`
// - `A(1, 3) = A(0, A(1, 2)) = A(0, 4) = 8`
// - `A(1, 4) = A(0, A(1, 3)) = A(0, 8) = 16`
function g(n) {
  return A(1, n);
}

// 수식: 2^(2^(2^(...))) (n만큼 2를 중첩)
// y가 0이면 0, y가 1이면 2, 그 외에는 `A(2 - 1, A(2, y - 1))` 반환
// A(1, A(2, y - 1))에서 A(1, y)는 2^n을 반환하고 A(2, y - 1)는 다시 A(1, A(2, y - 1))를 반환하므로
// A(1, A(2, y - 1))는 2^(2^(2^(...))) 형태가 된다.
function h(n) {
  return A(2, n);
}
```

## 1.2.2 트리 재귀
- 피보나치 수열을 계산하는 두가지 방법

### 1. 트리 재귀적 과정
```js
function fib(n) {
  return n === 0
    ? 0
    : n=== 1
    ? 1 
    : fib(n - 1) + fib(n - 2);
}
```
- fib가 한번 호출될 때마다 또 다른 두 fib 호출이 발생한다. 
- n을 줄여가며 같은 작업을 반복한다. 중복 계산하는 말단 노드(`fib(1)`, `fib(0)`) 개수는 `fib(n + 1)`이다.
- `fib(n)`은 `ϕⁿ/ √5`에 가장 가까운 정수인데 `ϕ² = ϕ + 1`이다. 
  - n이 올라갈 때 마다 `fib(n + 1)` 만큼이나 계산을 한다는 건 `fib(n²)`과 유사한 만큼 계산을 더 하는 거고, 지수적으로 계산이 증가한다는 의미이다.



### 2. 반복적 과정
```js
function fib(n) {
  return fib_iter(1, 0, n);
}

function fib_iter(a, b, count) {
  return count === 0
    ? b
    : fib_iter(a + b, a, count -1);
}

// 이해하기 쉽게 변수명 변경
function fib_iter(next, curr, remaining) {
  return remaining === 0
    ? curr
    : fib_iter(curr + next, next, remaining - 1);
}
```
- 필요한 단계의 수는 n에 선형으로 비례한다.                                     


### 연습문제 1.11

> 만일 `n < 3`이면 `f(n) = n`이고 만일 `n >= 3`이면 `f(n) = f(n - 1) + 2f(n - 2) + 3f(n - 3)`으로 정의되는 함수 f가 있다.  
> 재귀적 과정으로 f를 계산하는 자바스크립트 함수를 작성하라.   
> 반복적 과정으로 f를 계산하는 자바스크립트 함수를 작성하라.

#### 재귀적 과정
```js
function recursive(n) {
  return n < 3 
    ? n
    : recursive(n - 1) + (2 * recursive(n - 2)) + (3 * recursive(n - 3));
}
```

#### 반복적 과정
```js
function iterative(n) {
  return n < 3
    ? n 
    : iter(2, 1, 0, n - 2); //  f(2)까지 계산해서 시작하므로 count에서 2를 제외한다.
}

function iter(a, b, c, n) {
  return n === 0
    ? a
    : iter(a + (2 * b) + (3 * c), a, b, n - 1);
}
```
- a, b, c 각 인자를 `f(n - 1)`, `2f(n - 2)`, `3f(n - 3)`로 사용한다. 
  - 반복할 때 마다 `n - 1`에 해당하는 a에 새로운 계산 값을 넣고 나머지는 이전 값(b, c 자리)으로 옮긴다.


### 연습문제 1.12
> 파스칼의 삼각형을 재귀적 과정으로 계산하는 함수를 작성하라

```js
function pascals(x, y) {
  if (x > y) return 0;
  
  if (x === 1 || x === y) {
    return 1;
  }
  
  return pascals(x - 1, y - 1) + pascals(x, y - 1);
}
```

### 연습문제 1.13
> `Fib(n)`이 `ϕⁿ/ √5`에 가장 가까운 정수임을 증명하라  
> 힌트: 귀납법과 피보나치 수열의 정의를 이용해서 `Fib(n) = ϕⁿ- ψⁿ/ √5`을 먼저 증명해볼 것

<img src="../img/q-1_13.jpg" width="480"/>