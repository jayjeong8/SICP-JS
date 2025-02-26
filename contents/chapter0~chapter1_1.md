# 시작
### 추천사
> 내가 생각하기에 사람이 잘하는 것 중 하나는 대상에 이름을 붙이는 것이다. 우리는 강력한 연상 능력 또는 연관 기억 능력을 갖추었다.

### SICP 제 1판(1984) 서문
> 초보자는 자기 바이올린 소리가 끔찍하다고 말할 것이다. (...) 그들은 컴퓨터 프로그램들이 특정한 목적에는 훌륭하지만, 유연하지 않다고 말한다. 바이올린도 타자기도 유연하지 않다-우리가 사용법을 익히기 전에는.

# 1장 함수를 이용한 추상화
> 인간 지성론 43p
> - 다수의 단순 관념을 하나의 복합 관념으로 조합
> - 두 관념을 가져와서 관계있는 관념으로 연결
> - 하나의 관념을 실제 존재에서 분리해서 추상화

## 1.1 프로그래밍의 기본 요소
> 아이디어(생각)을 조직화하기 위한 세가지 메커니즘
> - 원시 표현식 / 조합 수단 / 추상화 수단

## 1.1.1 표현식
> 수를 나타내는 표현식들을 연산자로 조합할 수 있다. 그 결과는 연산자들에 해당하는 원시 함수를 해당 수들에 적용하는 하나의 복합 표현식이다.
> 연산자 조합도 중첩할 수 있다. ex_ (3*5) + (10-6);

> 💡 시작~1.1.1
> - 자바스크립트로 컴퓨터 영혼을 불러내는 마법사가 되어보자
> - 우리가 하려는 건 결국 생각을 구현하는 것. 구현하기 위해 생각 속 요소들에 **이름을 붙이고**, 다른 요소와 **조합하고, 묶고, 추상화한다.**

## 1.1.2 이름 붙이기와 환경
> 복합적인 연산의 결과를 간단한 이름으로 지칭할 수 있다는 점에서 상수 선언은 우리의 언어에서 가장 단순한 추상화 수단이다.

> 상호작용을 통해 점진적으로 만들어 나갈 수 있으므로 (...)
> 자바스크립트 프로그램들이 흔히 많은 수의 비교적 간단한 함수들로 구성되는 이유이다.

- 값에 이름을 붙인다. 값을 엮어 단순한 기능을 만든다. 단순한 기능을 엮어 복잡한 프로그램을 만든다.

## 1.1.3 연산자 조합의 평가
> `(2 + 4 * 6) * (3 + 12);`  
> 일반적으로 재귀는 이처럼 트리 형태의 위계구조로 조직화된 객체들을 다루는 데 대단히 강력한 기법이다.
> 실제로 이처럼 "값들을위로 올려보내는" 형태의 평가 규칙은 트리 누산이라고 부르는 좀 더 일반적인 과정의 한 사례이다.

> 키워드를 포함한 문장을 구문형이라고 부르는데, 각각의 구문형마다 고유한 평가 규칙이 있다.

- 작성한 코드는 트리 구조로 만들 수 있다. 키워드(ex_const)로 구문형을 구분하고 키워드에 맞는 트리 구조를 만들어서 분석할 수 있다. 

> 💡 단순한 숫자 계산식을 트리 구조로 표현한 예시가 새로웠다. 각 요소를 구조적인 관점에서 생각해보게 되었다.

## 1.1.4 복합 함수
> 함수 선언: 복합 연산에 이름을 붙여서 그 연산을 하나의 단위로 지칭한 것

> 옮긴이 주석 - 다른 프로그래밍 언어들(특히 절차적 언어)에서는 "인수로 함수를 호출한다"라는 표현이 흔히 쓰이지만, 이 책에서는 "함수를 인수에 적용한다(apply)"라는 표현도 사용한다.

> square를 다른 함수를 정의하는 구축 요소(building block)로 사용할 수도 있다. 주어진 두 수의 제곱합을 산출하는 함수 sum_of_squares를 선언하는 것은 간단한 문제이다. 더 나아가서, sum_of_squares 자체를 또 다른 함수의 구축 요소로 사용할 수 있다.

- 연산은 이름을 붙여서 구축 요소로 사용할 수 있다. 이 구축 요소는 더 큰 연산의 구축 요소로 사용할 수 있다.

## 1.1.5 함수 적용의 치환 모형
### 인수 우선 평가
> 해석기는 먼저 함수 적용의 요소들을 평가한 후 함수를 인수들에 적용한다. 
- 인수에 적용할 함수 표현식을 먼저 평가한 뒤 인수 표현식에서 인수를 구하고, 함수를 인수에 적용한다. 
- 자바스크립트 평가 방식

### 정상 순서 평가
> 인수의 값이 실제로 필요해질 때까지 인수 표현식의 평가를 미루는 평가 모형도 가능하다. 인수 표현식들을 매개변수들에 대입해 놓기만 하고, 연산자들과 원시 함수들만 관여하는 표현식을 평가할 때가 되면 비로소 그 인수 표현식들을 평가한다.
- 먼저 완전히 전개한 후 축약
- 치환 모형을 벗어난 함수들에 대해서는 훨씬 복잡하다.

## 1.1.6 조건부 표현식과 술어
> 조건부 표현식은 하나의 술어로 시작한다. 술어는 값이 참 아니면 거짓인 표현식이다. 서로 구별되는 두 가지 boolean 값이다. 술어 다음에는 물음표와 귀결 표현식이 오고 그 다음에 콜론과 대안 표현식으로 끝난다. 술어가 참으로 평가되면 해석기는 귀결 표현식을 평가해서 그 값을 조건부 표현식 값으로 돌려준다. 거짓으로 평가되면 대안 표현식을 평가해서 조건부 표현식 값으로 돌려준다.
- 술어: predicate, 서술어, 속성, 동작, 상태

> 표현식1 && 표현식2
> 이 구문형은 표현식1 ? 표현식2 : false의 문법적 설탕이다.

> 표현식1 || 표현식2
> 이 구문형은 표현식1 ? true : 표현식2d의 문법적 설탕이다.

> &&와 ||는 연산자가 아니라 구문형임을 주의하자. 우변에 오는 표현식이 항상 평가되지는 않는다.
> 구문형 우선순위는 비교 연산자(>, <)보다 낮다.  
 
### 연습문제 1.3 - 세개의 수를 받고 셋 중 가장 작은 것을 제외한 두 수의 제곱들을 합한 결과를 돌려주는 함수를 선언하라.
```js
// Math.min()이 없다고 가정하고 작성
function func(a, b, c){
  const x = a > b ? a : b;
  const y = x===a ? (b > c ? b : c) : (a > c ? a : c);
  
  return x ** 2 + y ** 2;
}

// 클로드가 작성한 버전
function func(a, b, c) {
  let min = a;
  if (b < min) min = b;
  if (c < min) min = c;
  
  return a*a + b*b + c*c - min*min;
}
```

### 연습문제 1.4 - 함수의 작동 방식 서술 
```js
function plus(a, b) { return a + b };
function minus(a, b) { return a - b };
// (a + (b의 절대값))을 구하는 함수
function aPlusAbsB(a,b) {
  return (b >= 0 ? plus : minus)(a,b);
}
``` 
```js
aPlusAbsB(1, 2);
2 >= 0 // true
true ? plus : minus;
plus(1, 2);
1 + 2;
3;
```

```js
aPlusAbsB(1, -2);
-2 >= 0 // false
false ? plus : minus;
minus(1, -2);
1 - -2;
3;
```

### 연습문제 1.5 - 해석기가 인수 우선 평가를 사용할 때와 정상 순서 평가를 사용할 때 아래 예제가 어떤 식으로 평가되는지를 각각 서술하라. 
```js
function p() { return p(); }

function test(x, y){
  return x === 0 ? 0 : y;
}

test(0,p()); // 평가할 문장
```

```js
// 인수 우선 평가
test(0,p()); 

// 인수 표현식을 먼저 평가하고 그 결과로 얻은 함수를 인수들에 적용한다.
0
p()
p()
p() // p()를 실행하면 다시 p()가 실행되고 무한 재귀
```

```js
// 정상 순서 평가
test(0,p()); 

// 인수의 값이 실제로 필요해질 때까지 인수 표현식의 평가를 미룬다.
0 === 0 // true
true ? 0 : p(); // 술어가 참으로 평가되면 해석기는 귀결 표현식을 평가해서 그 값을 조건부 표현식 값으로 돌려준다.
0; // 결과는 0
```

## 1.1.7 예제: 뉴턴 방법으로 제곱근 구하기
> 수학 함수와 컴퓨터 함수의 이러한 차이점은 사물의 속성을 서술하는 것과 뭔가를 하는 방법을 서술하는 것의 차이를 반영한다. 
> 이를 선언적 지식과 명령적 지식의 구분이라고 말하기도 한다. 
> 수학에서는 주로 선언적 서술(이것을 무엇인가?)에 관심을 두지만 컴퓨터 과학에서는 주로 명력적 서술(어떻게 하는가?)에 관심을 둔다.

### 연습문제 1.6 - sqrt_iter 함수를 삼항 연산자 대신 conditional 함수로 변경했을 때 어떤 일이 생기는지 설명하라.
#### 예시
```js
function sqrt_iter(guess, x) {
  return is_good_enough(guess, x)
    ? guess
    : sqrt_iter(improve(guess, x), x);
}

function is_good_enough(guess, x) {
  return abs(square(guess) - x) < 0.001;
}

function improve(guess, x) {
  return average(guess, x / guess);
}

function average(x, y) {
  return (x + y) / 2;
}
```
```js
function conditional(predicate, thenClause, elseClause) {
  return predicate ? thenClause : elseClause;
}

function sqrt_iter(guess, x) {
  return conditional(
    is_good_enough(guess, x),
    guess,
    sqrt_iter(improve(guess, x), x)
  )
}
```
#### 정답
1. 삼항연산자는 predicate가 참이 아닐 때만 elseClause를 실행한다.   
2. `conditional`처럼 함수로 만든 경우 자바스크립트는 인수를 먼저 평가하기 때문에 인수를 먼저 실행한다.   
3. 그러므로 `sqrt_iter(improve(guess, x), x)`가 함수 본문보다 먼저 실행된다.  
4. `sqrt_iter` 안에는 다시 `sqrt_iter(improve(guess, x), x)`를 인수로 가진 `conditional` 함수가 있다.  
5. 그러므로 끝없이 `sqrt_iter(improve(guess, x), x)`를 실행하며 **무한 재귀에 빠진다.**

> 💡 연습문제 1.5에서 자바스크립트가 인수 우선 평가로 동작하는걸 배웠다. 그러나 conditional 함수는 삼항 연산자에 이름을 붙여 추상화했을 뿐이므로 동일하게 동작할 거라고 생각했다.
> 정상 순서 평가인 언어였다면 삼항연산자와 conditional 함수는 동일하게 동작한다. 단순한 기능을 만들 때도 언어가 어떻게 동작하는지 알아야 의도한대로 기능을 구현할 수 있다.

### 연습문제 1.7 - is_good_enough 판정 방식이 아주 작은 수와 아주 큰 수에 대해 부적합한 이유를 설명하고 사례를 제시하라.
```js
function is_good_enough(guess, x) {
  return abs(square(guess) - x) < 0.001;
}
```
- 부적합한 이유: 오차 범위가 0.001로 고정되어 있다.
  - 작은 값 예시: x = 0.0000001 일 때 제곱근은 0.0000001보다 작아야하지만 중간 과정에서 0.001보다 작은 값이 나오면 `is_good_enough`를 통과한다.
  - 큰 값 예시: x = 1000000000000 정도로 클 때, 중간 과정에서 나온 값이 충분히 좋은(is_good_enough) 값이지만 오차 허용 범위가 너무 작기 때문에 `is_good_enough`를 통과하지 못한다.

#### 절대 오차에서 상대 오차를 계산하는 방식으로 변경
```js
function is_good_enough(guess, x) {
  return (abs(square(guess) - x) / x) < 0.001; // 평균값에서 x를 나눠서 작은 수는 큰 수로, 큰 수는 작은 수로 만들어 0.001과 비교한다.
}
```

### 연습문제 1.8 - 세제곱근 근삿값 구하는 뉴턴 방법을 함수로 구현하라.
```js
function cbrt_iter(guess, x){
  return is_good_enough(guess, x) 
  ? guess 
  : cbrt_iter(improve(guess, x), x);
}

function is_good_enough(guess, x) {
  return (abs(cube(guess) - x) / x) < 0.001;
}

function improve(guess, x) {
  return ((x / (guess ** 2)) + (2 * guess)) / 3;
}
```

## 1.1.8 블랙박스 추상으로서의 함수
- 문제는 다수의 부분 문제로 분해할 수 있다. 프로그램은 부분 문제에 대응하는 함수들의 군집이다. 
- 분해 전략 - 각 함수를 모듈로 사용해서 다른 함수에서 사용할 수 있는 형태로 분해한다. 
- 사용하는 함수 쪽에서는 내부 구현을 알 필요 없이 블랙박스 취급할 수 있어야 한다. (ex_`is_good_enough`에서는 `square` 구현을 몰라도 제곱값을 구한다는 return값 정보만 가지고 사용한다.) 

> 사용자는 함수가 어떻게 구현되었는지 몰라도 함수를 사용할 수 있어야 한다.

> 💡 함수 이름은 이름만 보고도 매개변수가 뭔지 알 수 있도록 명확해야 한다.   
> 함수 사용자는 `square(x)`에서 square라는 이름만 보고도 x가 뭔지 알 수 있다.  
> 나는 읽기 쉬운 이름 짓기에 진심인 편인데, 요즘은 IDE에서 함수에서 cmd+b만 누르면 사용한 함수 코드 위치로 빠르게 건너갈 수 있고 마우스 커서만 올려도 정보를 다 알려주니까 오히려 매개변수 이름 짓는데 집중하기도 했던 것 같다.  
> "함수 이름만 보고도 어떤 일을 하는지 알 수 있게 이름 짓기"를 첫번째 규칙으로 삼아야겠다.

#### 지역 이름
- 매개변수는 함수에 묶인(바인딩) 값이다. 함수 매개변수 이름에는 함수 scope에서만 통하는 이름(지역 이름)을 사용해야 한다.
- 자유 이름(함수 이름)은 이름이 변하면 함수를 사용하는 쪽의 의미에 영향을 미친다.

> `is_good_enough`의 올바른 작동은 `abs`라는 이름이 주어진 수의 절댓값을 계산하는 함수를 지칭한다는 사실에 의존한다.  
> 예를 들어 `abs` 함수 본문을 `math_cos` 함수 본문으로 대체하면 `is_good_enough` 함수가 이전과는 다른 결과를 내게 된다.

#### 내부 선언과 블록 구조
- 연습문제 1.6 첫번째 예시를 아래와 같이 변경
  - 사용자가 사용할 함수가 sqrt 뿐이라면 나머지 함수는 보조 함수다. 대형 시스템을 다수의 프로그래머가 함께 구축할 때 비슷한 보조 함수를 만들어 혼란을 줄 수 있으므로 개선한다.
  - 블록 안에 선언된 이름은 블록 내부로 한정되므로 매개변수를 단순하게 변경
  
```js
function sqrt(x) {
  function is_good_enough(guess) {
    return abs(square(guess) - x) < 0.001;
  }
  function improve(guess){
    return average(guess, x / guess);
  }
  function sqrt_iter(guess) {
    return is_good_enough(guess)
      ? guess
      : sqrt_iter(improve(guess));
  }
  return sqrt_iter(1);
}
```

> 💡이 책에서는 큰 프로그램을 나누는 목적으로 블록 구조를 적극적으로 활용한다고 하는데, 개인적으로는 위 블록구조보다 1.6 예시 코드가 좋은 것 같다. 
>  - 중첩이 적어서 읽기 편하다.
>  - 현대 자바스크립트에서 구분된 파일, export, private를 활용할 수 있다. 
>  - 내가 정리한 1.6 예시 코드에서 선언 순서는 리팩터링 책을 참고해서 작성했다. 이렇게 큰 함수(사용이 먼저되는 함수)가 먼저 선언되었을 때 코드 흐름을 읽기 좋은 것 같다. 위 블록 구조를 뒤집어서 return문이 먼저 오게 할 수도 있을 것 같다.

```js
// 사용이 먼저되는 함수가 위쪽으로 오도록 작성한다면?
function sqrt(x) {
  return sqrt_iter(1);

  function sqrt_iter(guess) {
    return is_good_enough(guess)
            ? guess
            : sqrt_iter(improve(guess));
  }
  function is_good_enough(guess) {
    return abs(square(guess) - x) < 0.001;
  }
  function improve(guess){
    return average(guess, x / guess);
  }
}
```