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

---



### 프로그램의 평가와 실행 과정

- 자바스크립트는 **실행가능한 코드**(Executable code)를 만나면 그 코드를 평가(Evaluation)해서 실행 문맥(Execution Context)으로 만든다.



#### 실행 가능한 코드

- 전역 코드
  - 전역 객체 Window 아래에 정의된 함수
- 함수 코드
  - 함수
- eval 코드
  - eval 함수
  -  렉시컬 환경이 아닌 별도의 동적 환경에서 실행



#### 실행 문맥의 구성

- 실행 문맥(Execution Context)은 실행 가능한 코드가 실제로 실행되고 관리되는 영역	
- 필요한 모든 정보를 컴포넌트 여러 개가 나누어 관리하도록 만들어짐
  - 렉시컬 환경 (Lexical Environment) 컴포넌트
  - 변수 환경 (Variable Environment) 컴포넌트
  - 디스 바인딩(This Binding) 컴포넌트



#### 렉시컬 환경 컴포넌트의 구성

- 자바스크립트 엔진이 자바스크립트 코드를 실행하기 위해 자원을 모아 둔 곳
- 함수 또는 블록의 유효범위 안에 있는 식별자와 그 그 결괏값(식별자가 가리키는 값)을 쌍으로 바인드해서 저장
- **환경 레코드**와 **외부 렉시컬 환경 참조 **컴포넌트로 구성되어 있다.
  - 환경 레코드 :  유효 범위 안에 포함된 식별자를 기록하고 실행하는 영역
    - 선언적 환경 레코드 : 함수와 변수, catch문의 식별자와 실행 결과가 저장
    - 객체 환경 레코드 : 실행 문맥 외부에 저장된 객체의 참조에서 데이터를 읽거나 쓴다.
  - 외부 렉시컬 환경 참조 :  유효 범위 너머의 유효 범위도 검색 가능, 함수 를 둘러싸고 있는 코드가 속한 렉시컬 환경 컴포넌트의 참조가 저장



#### 전역 환경과 전역 객체의 생성

- 자바스크립트 인터프리터는 새로운 웹 페이지를 읽어 들인 후 렉시컬 환경 타입의 전역 환경을 생성
- 그리고 전역 객체를 생성한 다음 전역 환경의 객체 환경 레코드에 전역 객체의 참조를 대입
- 최상위 레벨(함수 바깥에 있는 코드)의 this는 전역객체를 가리킨다.

```js
this === window // true
```





#### 프로그램의 평가와 전역 변수

- 전역 환경과 전역 객체를 생성한 후에는 자바스크립트 프로그램을 읽어들인다.
- 그 이후에는 프로그램을 평가하며, 최상위 레벨에 var문으로 작성한 전역 변수는 전역 환경의 환경레코드(객체 환경 레코드)의 프로퍼티로 추가된다.

```js
var a = { x: 1, y: 2};
console.log(window.a); // { x: 1, y: 2}

function norm(x) { ... };
console.log(window.norm); // norm(x)
```



- 즉,  전역 변수의 실체는 전역 객체의 프로퍼티 (또는 전역 객체의 실행 문맥에 들어있는 환경 레코드의 프로퍼티) 이다.

- 마찬가지로 함수 안에 선언된 지역변수와 중첩 함수의 참조 또한 그 함수가 속한 실행 환경의 환경 레코드의 프로퍼티이다.
- 호이스팅(끌어올림) : 최상위 레벨에 선언된 함수와 변수는 프로그램을 평가하는 단계에 이미 객체 환경 레코드에 추가된 상태이기 때문에 코드의 어느 위치에 작성해도 전체 프로그램이 참조도 할 수 있다.





#### 프로그램의 실행과 실행 문맥

- 프로그램이 평가된 다음에는 프로그램이 실행되는데, 프로그램은 **실행 문맥**안에서 실행된다.
- 실행문맥은 **스택**이라는 구조로 관리된다. (호출 스택, call stack)
  - 스택 : 일종의 자료 구조로 데이터를 아래서부터 쌓아 올려서 마지막으로 추가한 데이터를 먼저 꺼낸다. (후입선출(LIFO, Last In First Out))
    - push : 스택의 가장 윗 부분에 데이터를 쌓는 행위
    - pop : 스택의 가장 윗 부분에서 데이터를 빼내는 행위
- 함수를 실행하면 그 함수를 실행하기 위한 실행 문맥을 스택에 push하고,  작업을 끝내고 함수를 호출한 부분으로 제어권이 돌아오면 스택에서 pop한다.
  - 함수가 특정 함수의 내부에 정의된 중첩 함수라면 중첩 함수의 실행 문맥을 새로 만들어서 스택에 push한다.
  - 재귀 호출한 함수는 전혀 다른 함수로 스택에 push 되고, return문이 샐행되어 제어권이 호출한 코드로 돌아가면 스택에서 pop 된다.



#### 자바스크립트는 싱글 스레드

- 스레드 : 프로그램의 처리 흐름
  - 싱글 스레드 : 프로그램 한 개의 처리 흐름으로 프로그램을 순차적으로 실행하는 방식
  - 멀티 스레드 : 프로그램 여러 개의 처리 흐름으로 동시에 작업을 여러 개 병렬로 실행하는 방식

- 자바스크립트에서는 작업을 싱글 스레드로 처리한다. 호출 스택에 쌓인 실행 문맥(함수 또는 코드)을 위에ㅅ부터 아래로 차례차례 실행해 나간다.
  - 이벤트 처리와 같은 비동기 처리도 이러한 방식으로 대기 행렬을 만들어서 차레로 실행한다.
- 이 때 웹 브라우저의 API인 Web Workers를 사용하면 특정 작업을 백그라운드에 있는 다른 스레드에서 실행 할 수 있다. (멀티스레드 처리 가능)



#### this 값

- 함수가 호출되어 실행되는 시점에 this값이 결정된다. 
- 이 때의 this값은 **함수가 호출되었을 때 그 함수가 속해 있던 객체의 참조** 이며 실행 문맥의 디스 바인딩 컴포넌트가 참조하는 객체이다.

```js
var tom = {
	name: "Tom",
	sayHello: function(){
		console.log("Hello! " + this.name)
    }
}

tom.sayHello(); // Hello! Tom
```

- 함수를 tom.sayHello라는 이름으로 참조해서 실행
- tom.sayHello가 속한 객체는 Tom이다.
- 즉 sayHello 메서드가 호출되는 실행 문맥의 디스 바인딩 컴포넌트가 가리키는 객체는 tom 이다. 
- 따라서 this값은 tom을 가리키고 this.name 값이 "Tom"이 된다.

```js
var huck = { name: "Huck"};
huck.sayHello = tom.sayHello;
huck.sayHello(); // Hello! Huck
```

- 함수를 huck.sayHello라는 이름으로 참조해서 실행
- huck.sayHello가 속한 객체는 huck이고 this값이 huck 객체를 가리킨다.

- 함수는 객체에 묶여 있지 않다. ( 객체가 함수를 참조할 뿐이다.)





#### 다양한 상황에서 this가 가리키는 객체

- 호출될 때의 상황에 따라 this가 가리키는 객체가 바뀐다.

1.  최상위 레벨 코드의 this
   - 최상위 레벨 코드의 this는 전역 객체를 가리킨다.

```js
console.log(this); // Window
```



2. 이벤트 처리기 안에 있는 this
   - 이벤트 처리기(이벤트 리스너) 안에 있는 this는 이벤트가 발생한 요소 객체를 가리킨다.

```html
<div>
  <button id="button">버튼</button>
</div>
```

```js
function Person(name) {
  this.name = name;
};

Person.prototype.sayHello = function() {
  console.log("Hello!" + this.name);
}

var person = new Person("Tom");
var button = document.getElementById("button");
button.addEventListener("click", person.sayHello, false);
// Hello!
```

- 위 코드에서  버튼을 클릭하면 person.sayHello가 실행된다.
- person.sayHello의 this가 person을 가리킬 거라 기대하지만 실제로는 button 요소 객체를 가리킨다.
- button 객체의 name 프로퍼티 값은 빈 문자열이므로 `Hello!` 만 콘솔에 출력된다.

- 위에서 this가 원하는 객체를 가리키도록 설정하는 방법 

  - bind 메서드 사용
    - this가 person을 가리킨다.

  ```js
  button.addEventListener("click", person.sayHello.bind(person), false);
  // Hello! Tom
  ```

  - 익명함수 안에서 실행
    - 이벤트 리스너로 익명함수를 넘기고 익명함수 안에서 메서드를 호출하면 그 메서드의 this가 메서드를 참조하는 객체를 가리킨다.

  ```js
  button.addEventListener("click", function(e) {
    person.sayHello();
  }, false);
  // Hello! Tom
  ```





3. 생성자 함수 안에 있는 this
   - 사용자가 정의한 생성자 함수 안에 있는 this는 그 생성자로 생성한 객체를 가리킨다.



4. 생성자의 prototype 메서드 안에 있는 this
   - 생성자의 prototype  메서드 안에 있는 this는 그 생성자로 생성한 객체를 가리킨다.



5. 직접 호출한 함수 안에 있는 this
   - 함수를 최상위 레벨의 코드에서 호출하면 함수 안에 있는 this가 전역 객체를 가리킨다.
   - f(); 코드 앞에 객체가 없으므로 디스 바인딩 컴포넌트가 전역 객체를 가리킨다.

```js
function f() { console.log(this); }
f();  // Window
```



6. apply와 call메서드로 호출한 함수 안에 있는 this

- 함수 객체의 apply와 call 메서드를 사용하면 함수를 호출할 때 this가 가리키는 객체를 바꿀 수 있다.
  - apply와 call의 동작은 본질적으로 같다. 차이점은 apply는 함수에 배열인 인수를 넘기고 call은 쉼표로 구분한 값의 목록을 인수로 넘긴다.

```js
function say(greetings, honorifics) {
  console.log(greetings + " " + honorifics + this.name);
}

var tom = { name: "Tom Sawyer"};
var becky = { name: "Becky Thatcher"};

// apply
say.apply(tom, ["Hello!", "Mr."]); // Hello! Mr.Tom Sawyer
say.apply(becky, ["Hi!", "Ms."]); // Hi! Ms.Becky Thatcher

// call
say.call(tom, "Hello!", "Mr."); // Hello! Mr.Tom Sawyer
say.call(becky, "Hi!", "Ms."); // Hi! Ms.Becky Thatcher
```



#### 식별자 결정 : 유효 범위 체인

- 자바스크립트는 어휘적 유효 범위를 가진다. 그래서 변수를 선언하면 그 안쪽에 있는 코드 전체가 그 변수르 사용할 수 있는 유효 범위가 된다.
  - 전역 변수의 유효 범위는 코드 전체, 함수 안에 있는 지역 변수의 범위는 함수 안 코드 전체
- 식별자 결정 : 변수 x 가 어디서 선언된 변수인지 결정하는 작업
- 자바스크립트 식별자 결정 규칙 : 좀 더 안 쪽 코드에 선언된 변수를 사용한다.



```js
var a = "A";
function f() {
  var b = "B";
  function g() {
    var c = "C";
    console.log( a + b + c );
  }
  g();
}
f(); // ABC
```

- 속박 변수 : 일반적으로 함수의 인수와 지역변수를 말한다.
  - ex)  변수 c 는 함수 g 안에서 선언된 속박 변수
- 자유 변수 : 속박 변수 외의 변수 
  - ex) 변수 b, a 는 함수 g 바깥에서 선언된 자유 변수



- 스코프 체인 : 현재의 유효 범위 안에 없는 식별자를 찾을 때 밖ㅌ쪽 범위로 호출자의 렉시컬 환경에 속한 외부 렉시컬 환경의 참조를 찾아가는 논리적인 연결방식  ( = 외부 렉시컬 환경 체인 = 유효 범위 체인 )





####  가비지 컬렉션

- 객체를 생성하면 메모리 공간이 동적으로 확보된다. 
- 사용하지 않는 객체의 메모리 영역은 가비지 컬렉터가 자동으로 해제한다.
  - 사용하지 않는 객체 : 다른 객체의 프로퍼티와 변수가 참조하지 않는 객체



---



### 클로저

- 클로저(closure) 
  - 자기 자신이 정의된 환경에서 함수 안에 있는 자유 변수의 식별자 결정을 실행하는 함수와 그 기능을 구현한 자료 구조의 모음
  - 렉시컬 스코프를 바탕으로 형성된 스코프를 기억하는 함수이다. 자유 변수를 참조한 함수가 리턴된다. 리턴된 함수를 사용하면 자유변수는 값이 누적된다.

```js
var a = "A";
function f() {
  var b = "B";
  function g() {
    var c = "C";
    console.log( a + b + c );
  }
  g();
}
f(); // ABC
```



- 이 코드에서 중첩 함수 g가 정의된 렉시컬 환경은 함수 g를 둘러싼 바깥 영역 전체, 이 안에서 함수 g의 자유 변수 a와 b의 식별자 결정을 한다.
  - 함수 f를 호출할 때 함수f의 렉시컬 환경 컴포넌트가 생성된다.
  - 그 후에 함수g의 함수 선언문을 평가해서 함수 객체를 생성한다.
    - 이 함수 객체의 렉시컬 환경 컴포넌트에는 함수 g의 코드, 함수 f의 렉시컬 컴포넌트 참조 (변수 b가 들어 있음), 전역 객체의 참조(변수a가 들어있음)가 저장된다.
    - 함수g를 호출해서 실행하면 그 시점에 함수 g의 렉시컬 환경 컴포넌트를 생성한다.
      - 동시에 함수 g의 실행 문맥의 외부 렉시컬 환경참조를 체인처럼 거슬러 올라가 자유 변수 a와 b값을 참조한다.



#### 클로저의 성질

```js
function makeCounter(){
  var count = 0;
  return f;
  function f() {
    return count++;
  }
}

var counter = makeCounter();
console.log(counter()); // 0
console.log(counter()); // 1
console.log(counter()); // 2

var counter2 = makeCounter(); // 별도의 카운터, 각 클로저는 서로 다른 내부 상태를 저장
console.log(counter2()); // 0
```

- 외부 함수 makeCounter()는 중첩 함수 f의 참조를 반환한다.
  - f의 함수 객체를 전역변수 count가 참조한다.
- 중첩 함수 f는 외부 함수 makeCounter의 지역변수 count를 참조한다.
  - 함수 makeCounter의 렉시컬 환경 컴포넌트를 f의 함수 객체가 참조한다.

- 그 결과 함수 makeCounter의 렉시컬 환경 컴포넌트를 전역 변수 counter가 f의 함수 객체로 간접적으로 참조하게 되므로 가비지 컬렉션의 대상이 되지 않는다.
- 따라서 makeCounter 실행이 끝나서 호출자에게 제어권이 넘어가도 makeCounter의 렉시컬 환경 컴포넌트가 메모리에서 지워지지 않게 된다.
  - 지역변수 count는 함수 makeCounter가 속한 렉시컬 환경 컴포넌트에 있는 선언적 환경 레코드의 프로퍼티이므로 메모리에서 지워지지 않는다. 



##### 캡슐화

- 객체지향 프로그래밍에서 객체의 프로퍼티를 외부로부터 은폐하는 행위
  - 클로저 : 캡슐화된 객체

- 위 코드에서 변수 count는 클로저의 내부상태로서 저장된다. 또한 count는 지역 변수 이기 때문에 함수 바깥에서 읽거나 쓸 수 없다.
- 클로저는 마치 객체와 같다. 
  - 지역변수 count가 객체의 프로퍼티에 해당하고 함수 f가 객체의 메서드에 해당한다.



##### 익명함수를 반환

```js
function makeCounter(){
  var count = 0;
  return function () {
    return count++;
  }
}

var counter = makeCounter();
console.log(counter()); // 0
console.log(counter()); // 1
console.log(counter()); // 2
```



#### 클로저를 응용한 예제



```js
function Person(name, age){
  var _name = name;
  var _age = age;
  return {
    getName: function() { 
      return _name;
    },
    getAge: function() {
      return _age;
    },
    setAge: function(x) {
      _age = x;
    }
  };
}

var person = Person("tom", 18);
console.log(person.getName()); // tom
console.log(person.getAge()); // 18

person.setAge(20);
console.log(person.getAge()); // 20

```



```js
function makeMultiplier(x) {
  return function(y){
    return x * y;
  };
}

var multi2 = makeMultiplier(2);
console.log(multi2(3)); // 6

var multi10 = makeMultiplier(10);
console.log(multi10(3)); // 30

// multi2의 렉시컬 환경에서는 x는 2, multi10의 렉시컬 환경에서는 x가 10이다.
```



```js
function calc () {
  var store = 0; // 자유변수 
  var add = function(a, b){
    store += (a + b);
  };
  var print = function() {
    console.log('store:', store)
  }
  return {
    add: add,
    print: print
  }
}

var c = calc();
c.add(10, 20);
c.add(10, 20);
c.print() // store: 60
```





---



### 이름 공간

#### 전역 이름 공간의 위험

- 전역변수와 전역 함수를 전역객체에 선언하면 '전역 유효 범위를 오염시킨다'고 한다.
- 전역 유효 범위가 오염되면 변수 이름과 함수 이름이 겹칠 수 있다.



#### 객체를 이름 공간으로 활용하기

- 이름 공간(name space) : 변수 이름과 함수 이름을 한 곳에 모아 두어 이름 충돌을 미리 방지하고, 변수와 함수를 쉽게 가져다 쓸 수 있게 만든 메커니즘
- 객체를 이름 공간으로 활용하면 객체를 값으로 가지는 전역 변수를 하나 생성하고, 그 객체에 프로그램 전체에서 사용하는 모든 변수와 함수를 프로퍼티로 정의한다.

```js
var myApp = myApp || {};
// myApp이 정의 되어있을 때는 그것을 사용하고 그렇지 않으면 빈 객체를 myApp에 할당

myApp.name = "Tom";
myApp.showName = function(){};

// 부분 이름 공간
myApp.view = {};
myApp.controls = {};
```



#### 함수를 이름 공간으로 활용하기

- 함수 안에서 선언된 변수의 유효 범위는 함수 내부이다.  
- 변수를 함수 안에서는 읽거나 쓸 수 있지만 바깥에서는 일거나 쓸 수 없다.
- 이 성질을 활용하여 함수를 이름공간으로 활용할 수 있다.


```js
var x = "global x";
(function() {
  var x = 'local x';
  var y = 'local y';
})();

console.log(x); // global x
console.log(y); // ReferenceError: y is not defined
```

- 즉시 실행함수 내부에서 선언한 변수 x와 y는 이 함수의 지역 변수 이므로 전역 변수와 이름이 충돌하지 않는다.
- 따라서 일시적인 처리를 수행 하고자 할 때 그 내용물을 즉시 실행 함수에 작성하면 전역 유효 공간을 오염시키지 않고 실행 할 수 있다.

```js
(function(){
  // 이곳에 프로그램을 작성한다.
})();
```



#### 모듈 패턴

```js
var Module = Module || {};
(function(_Module)) {
  var name = "NoName";
  function getName() { // private 함수
    return name;
  }
  _Module.showName = function(){ // public 함수
    console.log(getName());
  };
  _Module.setName = function(x){ // public 함수
    name = x;
  }
})(Module);
Module.setName("Tom");
Module.showName(); // Tom
// 클로저 생성
```



---



### 콜백 함수



#### 콜백 함수 

- 자바스크립트의 함수는 일급 객체이며 다른 함수에 인수로 넘겨질 수 있다.  다른 함수에 인수로 넘겨지는 함수를 가리켜 콜백 함수라고 부른다.

```js
f(g, ..,);
...

function f (callback, ...){
  ...
  callback();
  ...
}
```

- 함수 f의 인수로 넘겨진 함수 g가 콜백함수 이다. 
- 호출한 함수 f 안에서 특정 콜백 함수를 실행시켜 그 콜백 함수에 제어권을 부여할 수 있다.
- 콜백 함수는 함수를 호출할 때 무언가 새로운 일이 생기거나 그 함수의 실행이 끝나면 지정한 콜백 함수를 실행해 주도록 함수에 요청해야 할 때 사용한다.
- 이때  콜백 함수의 주체는 어디까지나 함수를 호출한 호출자이다. 호출자가 목적에 따라 어떤 콜백 함수를 사용할 것인지 정한다. 호출된 함수를 콜백 함수를 실행하지만 그 콜백 함수가 작업하는 내용에는 관여하지 않는다.

```js
function foo(callback){  // 함수를 받는 함수 : 받은 함수에 숫자를 넣어주는 역할을 한다.
  callback(10);
}

foo(function(num){  // foo가 먼저 실행이 되고 foo안에 넘겨받은 함수를 실행
  console.log('foo로부터 받은 number', num);  
})

// foo로부터 받은 number 10
```





#### 이벤트 처리기

- 이벤트 처리기는  특정 이벤트가 발생했을 때 실행하도록 등록하는 함수이다.

```js
button.onclick = function() {...};
                             
button.addEventListener("click", function() { ... }, false);
```



#### 타이머

- 타이머 메서드(setTimeout, setInterval)에 첫 번째 인수로 넘기는 함수가 콜백 함수이다.

```js
setInterval(function() {...}, 2000);
```



#### 콜백 함수 예제

```js
function step1(){
  setTimeout(function(){
    console.log('i am step1');
  }, 2000)
}

function step2(){
  setTimeout(function(){
    console.log('i am step2');
  }, 1000)
}

step1();
step2();

// i am step2
// i am step1
```

- 위 코드에서 step1이 먼저 출력되고, 그 다음에 step2가 출력려면 콜백 함수를 이용한다.

```js
function step1(step2){
  setTimeout(function(){
    console.log('i am step1');
    step2();
  }, 2000)
}

function step2(){
  setTimeout(function(){
    console.log('i am step2');
  }, 1000)
}

step1(step2);

// i am step1
// i am step2
```








> 책 < 모던 자바스크립트 입문 - 이소 히로시 >과  js스터디에서 배운 것을 정리하였습니다. 