# Vue.js 

Evan You가 만들고 2014년에 릴리즈한 View에 최적화된 자바스크립트 프레임워크



#### MVC 패턴 (전통적인 웹개발 패턴)

- `M(Model)` :  데이터와 데이터를 처리하는 부분 
- `V(View) `: 화면에 보여지는 UI부분
- `C(Contoller)` : 사용자의 입력을 받고 처리하는 부분

Logic들이 커지고 복잡해지므로 M-V 사이의 의존성이 강해지고,  유지 보수가 어려워졌다.



따라서 Backend로직과 Client의 마크업 & 데이터 표현단을 분리하게 되었다.



#### MVVM 패턴  (Model-View-ViewModel)

앞단의 화면 관련된 로직과  뒷단의 DB데이터 처리 및 서버 로직을 별도로 분리하고, 뒷단에서 넘어온 데이터를 Model에 담에 View로 넘겨주는 준다.

- `M(Model)` : 데이터와 그 데이터를 처리하는 부분
- `V(View)` : 화면에 보여지는 UI부분 (사용자의 입력을 받는다.)
  - DOM
- `VM(ViewModel)` :  보여줄 페이지의 데이터만 가져오는 것 (변화된 데이터) 
  - DOM Listeners
  - Data Bindings
  - **Vue** 는 ViewModel 레이어에 해당

먼저, View의 변화를 ViewModel에서 감지해서 Model에 신호를 보낸다. -> Model에서 변화된 데이터에 대해 다시 ViewModel을 거쳐 View로 보낸다.  -> 화면이 Reactive하게 반응이 일어난다.  



---



#### 특징

- 데이터 바인딩과 화면 단위를 **컴포넌트(Components) 형태**로 제공
- Angular에서 지원하는 2way data bindings을 동일하게 제공
- Component간 통신은 React의 1way Data Flow 와 유사 (부모 -> 자식)
- Virtual DOM 을 이용한 렌더링 방식 (React와 유사)

- 다른 프론트엔드 프레임워크와 비교했을 때 훨씬 가볍고 빠르다.




#### 구동환경

- 대부분의 모던브라우저 지원, 인터넷 익스플로러는 9버전부터 지원



---



#### Install

```
npm install -g @vue/cli

yarn global add @vue/cli
```

#### Create a project

```
vue create my-project

// Using the GUI
vue ui 
```



---



### 템플릿

- Vue.js의 데이터를 외부로 보여주기 위해 HTML 기반의 템플릿 사용을 권장한다. (JSX 사용도 가능)
- Mustaches라고 불리는 `{{ }}`  태그 안에 `data` 객체의 프로퍼티 값을 출력

```html
<div id="example">
  <p>My framework is {{ name + '!' }}</p>
</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    name: 'Vue.js'
  }
});
```

- 위의  예제를 실행하면 'My framework is Vue.js!'가 출력



---



### 뷰 인스턴스

- Vue.js라이브러리를 로딩 후  `Vue` 생성자로 인스턴스를 생성한다.
  - Vue라는 기존 객체에 화면에서 사용할 옵션(데이터, 속성, 메서드 등)을 포함하여 화면의 단위를 생성한다.

```js
var vm = new Vue({
  // options
})
```



- Vue 생성자 함수의 인스턴스를 생성할 때 뷰와 데이터를 연결하기 위한 options를 설정할 수 있다.
  - 뷰 관련 옵션 : `el` , `template`

  - 데이터 관련 옵션 : `data`, `methods`, `computed`

  - 컴포넌트 관련 옵션  : `components`

  - 생명 주기 훅 : `created`, `mounted`, `updated`, `destroyed`

```js
// vm : ViewModel의 관행상의 약어
var vm = new Vue({
  template: ..., 
    // 화면에 나타나는 한 개의 요소(ex. div, button 등의 태그) 
    // + 뒷단에서 모델의 데이터를 앞단으로 연결
  el:...,
    // 화면이 그려지는 시작점 , element의 약자 (특정 태그)
  methods: {
    // 클릭이나 http요청과 같은 메소드
  },
  created: {
   // 라이프 사이클훅
  }
})
```




- 각 options 으로 미리 정의한 vue객체를 extend를 사용하여  확장하면 재사용이 가능하다.  ( 하지만 아래 방법보다는 template에서 custon element 로 작성하는 것을 권장 )

```js
var MyComponent = Vue.extend({
  // template, el, methods 와 같은 options 정의
  template: `<p>Hello {{ message }}</p>`,
  data : {
    messagae: 'Vue'
  }  
  // ...
})

// 위에서 정의한 내용(template, data, ...)을 기본으로 하는 컴포넌트 생성
var myComponentInstance = new MyComponent()
```



---



###  Vue Instance 라이프 싸이클

- 객체가 생성, 변경, 소멸될 때 각각의 단계에서 어떤 작업을 할 지 분기를 시켜주는 기능

#### Vue Instance 라이프 싸이클 초기화

- 데이터 관찰
  - 뷰단에서 데이터가 변화했을 때 감지
- 템플릿 컴파일
  - 앞단의 화면에서 기본적인 html이 아니라 뷰가 가지고 있는 데이터들을 html에 넘겨주어 html이 뷰의 내용까지 같이 반영한 웹 문서가 되도록 변형해 주는 것
- DOM에 객체 연결
  - 기본적인 DOM이아닌 Vue.js에 어떤 특성, 옵션을 가지고 있는 객체를 DOM에 연결 
- 데이터 변경시 DOM 업데이트
  - Reactive하게 화면을 자연스럽게 재 갱신



```js
// 위의 초기화 작업 외에도 개발자가 의도하는 커스텀 로직을 단계별로 추가 가능
var vm = new Vue({
  data: {
    a: 1
  },
  created: function(){ 
    console.log('a is: ' + this.a) // this는 vm을 가리킴
  }
})
```

- 위와 같이 초기화 메서드로 커스텀 로직을 수행하기 때문에 Vue에서는 따로 Contoller를 가지지 않는다.




#### 라이프 싸이클(생명 주기) 훅

- `beforeCreate` ~ `created`  : 데이터 및 이벤트 초기화
- `created` ~ `beforeMount` : 뷰 생성
- `mounted` ~ `updated` : 데이터 바인딩, 데이터 변경 주시 및 뷰 업데이트
- `destroyed` : 자식 컴포넌트, 이벤트 리스너 해제



```html
<div id="app">
  {{ message }}
</div>
<script src="https://unpkg.com/vue@2.4.4/dist/vue.js"></script>
<script>
  new Vue({
    el: '#app',
    data: {
      message: 'Hello Vue.js!'
    },
    beforeCreate: function(){
      console.log("beforeCreate");
    },
    created: function(){
      console.log("created");
    },
    mounted: function(){
      console.log("mounted");
      this.message = '1'; // 업데이트할 내용
    },
    updated: function(){
      console.log("updated"); // 업데이트할 내용이 없으면 출력되지 X,=
    }
  })
</script>
```



---



### 뷰 컴포넌트

- 화면에 비춰지는 뷰의 단위를 쪼개어 재활용이 가능한 형태로 관리하는 것

```html
<div id="app">
    <button>Parent Component</button>
    <my-component></my-component>
</div>

<script src="https://unpkg.com/vue@2.4.4/dist/vue.js"></script>
<script>
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
});
new Vue({
  el: '#app'
})
</script>
```



#### 전역 컴포넌트

- 컴포넌트를 뷰 인스턴스에 등록해서 사용할 때 다음과 같이 global하게 등록할 수 있다.

```js
Vue.component('my-component', {
  //...
})
```



#### 지역 컴포넌트

- local하게 등록하는 방법은 다음과 같다.

```js
var cmp = {
  data: function() {
    return {
      // ...
    };
  }
  template: '<hr>',
  methods: {}
}
// 아래 Vue인스턴스에서만 활용할 수 있는 로컬(지역) 컴포넌트 등록
new Vue({
  components: {
    // 태그명(<my-cmp></my-cmp>) : 컴포넌트의 내용
    'my-cmp' : cmp
  }
})
```






















#### 지시자(Directives)

- HTML 컴파일러가 실행될 때 해당 DOM을 찾기 위해 사용되는 특별한 형태의 속성
- 접두사 `v-`로 시작하며 스타일에서부터 이벤트까지 DOM을 다룰 수 있는 모든 범위에서 다양한 지시자를 제공한다.

```html
<div id="example">
  <p>My framework is {{ name + '!' }}</p>
  <button v-on:click="changeName">Change</button>
</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    name: 'Vue.js'
  },
  methods: {
    changeName: function() {
	  this.name = this.name.toLowerCase(); // Vue.js --> vue.js
	}
  }
});
```

- `v-on` 지시자를 가진 버튼을 추가한 후 클릭 이벤트를 발생시키면 name값이 자동으로 업데이트 된다.





> 참고 링크
>
> [인프런 강의 - 누구나 다루기 쉬운 Vue.js 프론트 개발 – 3시간 안에 배우기](https://www.inflearn.com/course/vue-pwa-vue-js-%EA%B8%B0%EB%B3%B8/)
>
> [자바스크립트 프레임워크 소개 3 - Vue.js](https://meetup.toast.com/posts/99)
> [Vue.js 공식가이드](https://kr.vuejs.org/v2/guide/)
>
> [디자인패턴 - MVC, MVP, MVVM 비교](http://beomy.tistory.com/43)
> [MVC, MVP, MVVM, MVI](https://brunch.co.kr/@oemilk/113)

