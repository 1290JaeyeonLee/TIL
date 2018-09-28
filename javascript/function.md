# 함수

### 함수 정의 하기

#### 함수를 정의하는 방법

```js
// 1. 함수 선언문
function square(x) {
    return x * x;
}

// 2. 함수 리터럴
var square = function(x) {
    return x * x;
}

// 3. Function 생성자로 정의
var square = new Function("x", "return x * x");

// 4. 화살표 함수 표현식으로 정의
var square = x => x * x;
```

- 함수 선언문으로 정의된 함수는 호이스팅 가능, 나머지는 불가능



#### 중첩 함수 (Nested Function)

- 특정 함수의 내부에 선언된 함수
- 외부 함수의 바깥에서는 읽을 수 없다.
- 자신을 둘러싼 외부함수의 인수와 지역변수에 접근할 수 있다.
  - (외부 함수의 인수인 `x`사용 )

```js
function norm(x) {
  var sum2 = sumSquare();
  return Math.sqrt(sum2);

  function sumSquare(){
    sum = 0;
    for(var i = 0; i < x.length; i++) {
      sum += x[i] * x[i];
    }
    return sum;
  }
}

var arr = [100, 4, 5, 7];
var num = norm(arr);
console.log(num); //100.44899203078147
```





---



### 함수 호출하기

```js
// 1. 함수 호출 -> 함수의 참조가 저장된 변수 뒤에 ()를 붙여서 호출
var s = square(5);

// 2. 메서드 호출
obj.m = function() {...};
obj.m();

// 3. 생성자 호출 -> 함수 또는 메서드를 호출할 때 지정된 변수앞에 new 키워드 추가
var obj = new Object();

// 4. call, apply를 사용한 간접 호출

```



#### 즉시 실행 함수

- 함수 정의식을 그룹 연산자인 ()로 묶으면 괄호안의 함수 정의식이 평가되어 함수 값으로 바뀐다.

```js
(function(){
  console.log("즉시실행함수")
})(); // 즉시실행함수

(function(){
  console.log("즉시실행함수")
}()); // 즉시실행함수
```



```js
(function fact(n){
  if(n<=1) {
    return 1;
  }
  return n * fact(n-1);
})(5); // 120
```



---



### 함수의 인수

#### 인수의 생략

- 함수 정의식에 작성된 인자 갯수보다 인수를 적게 전달해서 함수를 실행하면 인수에서 생략된 인자는 undefined가 됩니다. 

```js
function f(x, y){
  console.log("x = " + x + ", y = " + y);
}

f(2); // x = 2, y = undefined
```

```js
function multiply(a, b) {
  b = b || 1; // b의 초기값을 1로 설정
  return a * b;
}

multiply(2, 3); // 6
multiply(2); // 2
```



#### 가변 길이 인수 목록 (Arguments 객체)

- 모든 함수에서 사용할 수 있는 지역변수로 arguments 변수가 있다.
- 함수에 인수를 n개 넘겨서 호출하면 인수 값이 arguments에 저장된다.

```js
arguments[0] // 첫 번째 인수 값
arguments[1] // 두 번째 인수 값
...
arguments[n - 1] // n 번째 인수 값

```

- Arguments 객체는 프로퍼티로 length와 callee를 갖고 있다.

```js
arguments.length // 인수 갯수
arguments.callee // 현재 실행되고 있는 함수의 참조 (익명함수의 재귀 호출이 가능하다)
```

```js
function f(x, y) {
  arguments[1] = 3;
  console.log("x = " + x + ", y = " + y);
}
f(2, 0);  // x = 2, y = 3
// 함수의 인자 y 값이 바뀐다
```



- arguments 변수를 활용하면 인수 개수가 일정하지 않은 가변 인수 함수를 정의할 수 있다.

```js
function myConcat(seperator){
  var s = "";
  console.log(seperator)
  for(var i = 1; i < arguments.length; i++){
    s += arguments[i];
    if(i < arguments.length - 1) {
      console.log(seperator); // /
      s += seperator
    }
  }
  return s;
}

console.log(myConcat("/", "apple", "orange", "peach"));
// apple/orange/peach;
```



- arguments[]는 유사 배열 객체이지만 다음과 같은 방법으로 배열 객체로 변환할 수 있다.

```js
var args = Array.prototype.slice.call(arguments);

var args = Array.from(arguments);

var args = [...arguments];
```



```js
function myConcat(separator) {
  var args = Array.prototype.slice.call(arguments, 1);
  return args.join(separator);
}
myConcat("/", "apple", "grape"); 
// 'apple/grape'
```



---

### 재귀 함수

- 재귀호출(recursive call) : 함수가 자기 자신을 호출하는 행위
- 재귀 호출로 문제를 간단히 해결할 수 있을 때만 사용한다.
  - 호출된 횟수만큼 메모리 소비량이 늘어나기 때문

```js
function fact(n) {
  if(n <= 1) {
    return 1;
  } // 재귀호출은 반드시 멈춰야 한다.
  return n * fact(n-1);
}

fact(5); //120
```



#### 퀵 정렬 

- 배열을 오름차순으로 정렬
- 평균 실행시간이 O(nlogn)으로 가장 빠른 정렬 알고리즘 중 하나

1.  적당한 기준 값 (pivot)을 고른다.  (pivot 값의 크기가  pivot값 이상인 요소의 개수와 pivot값 이하인 요소의 갯수가 같도록 한다.)
2. 배열 앞부분에는 pivot값 이상인 요소를 이동시키고, 배열 뒷 부분은 p값 이하인 요소를 이동시킨다.
3. 배열 앞 부분의 길이가 2 이상이면 그 부분을 대상으로 퀵 정렬을 한다.
4. 배열 뒷 부분의 길이가 2 이상이면 그 부분을 대상으로 퀵 정렬을 한다.
5.  이처럼 퀵 정렬은 배열을 두개로 나눈 뒤에 나눈 부분을 대상으로 퀵 정렬을 재귀적으로 반복한다.

```js
function quickSort(x, first, last) {
  var p = x[ Math.floor((first + last) / 2 ) ];
  for(var i = first, j = last; ; i++, j--) {
    while(x[i] < p) i++;
    // 왼쪽부터 차례대로 p 이상의 요소를 검색
    while(p < x[j]) j--;
    // 오른쪽부터 차례대로 p이하의 요소를 검색
    if(i >= j) break;
    // i와 j가 교차하면 다음으로 이동
    var w = x[i];
    x[i] = x[j];
    x[j] = w;
    // 발견하면 x[i]와 x[j]를 교환
  }
  if(first < i - 1) quickSort(x, first, i - 1); // 왼쪽에 두개 이상 남아있으면 왼쪽 다시 정렬
  if(j + 1 < last) quickSort(x, j + 1, last); // 오른쪽에 두개 이상 남아있으면 오른쪽 다시 정렬
}

var a = [7, 2, 5, 1, 8, 9, 3];
quickSort(a, 0, a.length - 1);
console.log(a); //[ 1, 2, 3, 5, 7, 8, 9 ]
```











> 책 < 모던 자바스크립트 입문 - 이소 히로시 > 를 보고 정리한  내용입니다.