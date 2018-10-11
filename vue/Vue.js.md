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

vue ui   // Using the GUI
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



### Vue 인스턴스

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
      console.log("updated"); // 업데이트할 내용이 없으면 출력되지 X
    }
  })
</script>
```



---



### Vue 컴포넌트

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





---

### Vue 컴포넌트 통신

#### 상-하위 컴포넌트 간 데이터 전달 방법 (Parent - Child 컴포넌트 통신)

##### 부모와 자식 컴포넌트 관계

- 구조상 상-하관계에 있는 컴포넌트의 통신
  - Parent(상) ---- `Props를 전달` ----> Child (하)
  - Child(하) ---- `Event를 발생` ----> Parent (상)



#### Props

- 모든 컴포넌트는 각 컴포넌트 자체의 스코프를 갖는다.
  - ex) 하위 컴포넌트는 상위 컴포넌트의 값을 바로 참조할 수 없다.

- 상위에서 하위로 값을 전달하려면 props 속성을 사용한다.

```js
Vue.component('child-component', {
  props: ['passedData'],
  template: '<p>{{passedData}}</p>' // 결과물
});
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js! from Parent Component'
  }
});
```

```html
<div id="app">
    <!-- props명 = "상위컴포넌트의 데이터" 의 구조 --> 
  <child-component v-bind:passed-data="message"></child-component>
</div>
```

- props 변수 명명할때 js에서  카멜 기법( ex. passedData )으로 하면 html에서는 케밥 기법 ( ex. passed-data ) 으로 해야한다.



#### 같은 레벨의 컴포넌트 간 통신

- 동일한 상위 컴포넌트를 가진 2개의 하위 컴포넌트 간의 통신은
  - **Child** --> **Parent** -->  (다시 2개의)  **Children** 

- Vue 기본구조상 **컴포넌트 간의 직접적인 통신**은 불가능하다. (컴포넌트는 자체 스코프를 가지므로 데이터는 다른 컴포넌트에서 직접적인 참조가 불가능)



---

### Event Bus

#### Event Bus - 컴포넌트 간 통신

- Non Parent - Child 컴포넌트 간의 통신을 위해 Event Bus를 활용할 수 있다.
  - Event Bus를 위해 새로운 Vue를 생성하여 Vue Root Instance가 위치한 파일에 등록

```js
// Vue Root Instance 전에 꼭 등록 (순서 중요)
export const eventBus = new Vue();
new Vue({
 // ...
})
```



- 부모-자식 관계를 유지하지 않아도 모든 컴포넌트의 데이터 전달이 바로 된다.

- 단점 : 컴포넌트 간의 관계가 불명확 
  -  이벤트를 던졌을 때의 관리, 데이터 관리를 더욱 용이하게 하기위해 VueX를 사용한다.
- 컴포넌트에서 이벤트 버스에 데이터를 보내고, 또 다른 컴포넌트에서 데이터를 받아온다. (이벤트 버스는 중간자 역할)



#### 이벤트 발생

- 이벤트를 발생시킬 컴포넌트에 `eventBus` import 후 `$emit` 으로 이벤트 발생

```js
import { eventBus } from '../../main';

eventBus.$emit('refresh', 10); // refresh라는 이벤트, 데이터 값은 10
```



#### 이벤트 수신

- 해당 이벤트를 받을 컴포넌트에도 동일하게 import 후 콜백으로 이벤트 수신

```js
import { eventBus } from '../../main';

// 등록 위치는 해당 컴포넌트의 created 메서드에 등록
created() {
  eventBus.$on('refresh', function(data){
    console.log(data); // 10
  });
}
```



---



### Vue 라우터

#### Vue Routers
- 일반적으로 화면이 전환될 때 전환되는 행위를 Route라고한다.
- Vue를 이용한 SPA를 제작할 때 유용한 라우팅 라이브러리
- Vue 코어 라이브러리 외에 Router라이브러리를 공식 지원하고 있다.

```
// 설치

npm install vue-router ( --save )
```



- Vue 라우터는  기본적으로 **RootUrl'/#/' {Router name}**의 구조로 되어있다.

```
example.com/#/user
```

- 위에서  `#` 태그 값을 제외하고 기본 URL 방식으로 요청 때마다 index.html을 받아 라우팅을 하려면 아래와 같이 mode에 history 속성을 추가한다.

```js
const router = new VueRouter({
  routes,
  mode: 'history'
})
```



#### 기본 라우터

##### html

```html
<div id="app">
  <h1>Hello Vue Router</h1>
  <p>
    <!-- 네비게이션을 위해 router-link 컴포넌트를 사용 -->
    <!-- 기본적으로 <router-link>는 <a>태그로 렌더링된다. --> 
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
    
  <!-- 현재 라우트에 맞는 컴포넌트가 렌더링된다. -->
  <router-view></router-view>
</div>
```



##### Javascript

```js
// 0. 모듈 시스템(ex. vue-cli)을 이용하고 있다면, Vue와 Vue라우터를 import 한다. 그리고 Vue.use (VueRouter) 를 호출한다.


// 1. 라우트 컴포넌트를 정의한다.
// 아래 내용들은 다른 파일로부터 가져올 수 있다.
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>'}

// 2. 라우트를 정의한다.
// 각 라우트는 반드시 컴포넌트와 매핑되어야 한다.
// "component"는 Vue.extend()를 통해 만들어진 
// 실제 컴포넌트 생성자이거나 컴포넌트 옵션 객체이다.
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

// 3. routes옵션과 함께 router 인스턴스를 만든다.
// 추가 옵션을 여기서 전달해야한다. 
const router = new VueRouter({
  routes  // routes: routes의 줄임
})

// 4. 루트 인스턴스를 만들고 mount한다.
// router와 router 옵션을 전체 앱에 주입한다.
const app = new Vue({
  router
}).$mount('#app')
```

- Go to Foo 를 클릭하면 
  - url 에 /foo가 추가된다. 
  - 해당하는 컴포넌트를 뿌려주므로 text가 변경된다.



#### Nested Routers (중첩된 라우트)

- 라우터로 화면 이동시 Nested Routers 를 이용하여 여러 개의 컴포넌트를 동시에 렌더링 할 수 있다.
- 렌더링되는 컴포넌트의 구조는 가장 큰 상위의 컴포넌트가 하위의 컴포넌트를 포함하는 (중첩된) `Parent - Child` 형태와 같다.

```html
<!-- localhost:5000 -->
<div id="app">
  <router-view></router-view>
</div>

<!-- localhost:5000/home -->
<!-- parent component -->
<div>
  <p>Main Component rendered</p>
  <!-- child component -->
  <app-header></app-header>
</div>
```



##### Nested Routers 예제

```html
<div id="app">
  <h1>Hello Vue Nested Router!</h1>
  <p>
    <router-link to="/login">Go to Login</router-link>
    <router-link to="/list">Go to List</router-link>
  </p>
  <router-view></router-view>
</div>
```

```js
var Login = {
  template: `
    <div>
      Login Section
      <router-view></router-view>
    </div>
  `,
};

var LoginForm = {
  template: `
    <form action="/" method="post">
      <div>
        <label for="account">Email :</label>
        <input type="email" id="account">
      </div>
      <div>
        <label for="password">Password : </label>
        <input type="password" id="password">
      </div>
    </form>
  `,
};

var List = {
  template:`
    <div>
      List Section
      <router-view></router-view>
    </div>
  `,
};

var ListItems = {
  template: `
    <ul>
      <li>1</li>
      <li>2</li>
      <li>3</li>
    </ul>
  `,
};

var routes = [
  {
    path: '/login',
    component: Login,
    children: [
      { path: '', component: LoginForm} // path : '' -> 상위와 같은경로
    ] // 중첩된 outlet에 컴포넌트를 렌더링하려면 children 옵션을 사용한다.
  },
  {
    path: '/list',
    component: List,
    children: [
      { path: '', component: ListItems }
    ]
  }
];

var router = new VueRouter({
  routes
});

var app = new Vue({
  router
}).$mount('#app');
```



#### 주의사항 - Vue Template Root Element

- Vue의 Template에는 최상위 태그가 1개만 있어야 렌더가 가능하다.
- 여러 개의 태그를 최상위 태그 레벨에 동시에 위치시킬 수 없다.

```js
var Foo = {
  // div 태그 안에 텍스트와 router-view 포함하여 정상동작
  template: `
    <div>
	  foo
	  <router-view></router-view>
    </div>
  `
};
```



#### Named Views (이름을 가지는 뷰)

- 라우터로 특정 URL로 이동시, 특정 URL에 해당하는 여러 개의 View(컴포넌트)를 동시에 렌더링 한다.
- 각 컴포넌트에 해당하는 `name` 속성과 `router-view` 지정 필요

```html
<div id="app">
  <router-view name="nestedHeader"></router-view>
  <router-view></router-view> <!-- default -->
</div>
```

```js
{
  path: '/home',
  // Named Router
  components: {
    nestedHeader: AppHeader,
    default: Body
  }
},
```



#### Nested Routes 와 Named Views 차이점

- Nested Routes : 특정 URL에서 1개의 컴포넌트(부모)안에 여러 개의 하위 컴포넌트(자식)를 갖는 것
- Named Views : 특정 URL에서 여러 개의 컴포넌트를 쪼개진 뷰 단위로 렌더링 하는 것 (중첩 X)



---



### Vue 템플릿

#### Vue Templates

Vue로 그리는 화면의 요소들, 함수, 데이터 속성은 모두 Templates 안에 포함된다.

- Vue는 DOM의 요소와 Vue인스턴스를 매핑할 수 있는 HTML Template 을 사용한다.
- Vue는 Template으로 렌더링할 때 Virtual DOM을 사용하여 DOM조작을 최소화하고 렌더링을 꼭 다시 해야만 하는 요소를 계산하여 성능 부하를 최소화한다.

- 원하면 render function을 직접 구현하여 사용할 수 있다.



#### Attributes 

- HTML Attributes를 Vue의 변수와 연결할때는 `v-bind` 를 이용한다.

```html
<div v-bind:id="dynamicId"></div>
```



#### Javascript Expressions

- ` {{  }} ` 안에 다음과 같이 Javascipt 표현식도 가능하다.
- 이 표현식은 Vue 인스턴스 범위 내에서 Javascript로 계산된다.
- 각 바인딩에 하나의 단일 표현식만 포함할 수 있다.

```html
<div>{{ number + 1 }}</div> 
<!-- O(지향함) -->

<div>{{ message.split('').reverse().join('') }}</div> 
<!-- O(허용함, 지양함) -->

<div>{{ if (ok) { return message } }}</div> 
<!-- X( 구문이나 조건문은 허용하지 않음, 대신에 삼항 연산자를 사용하기) -->
```



#### 지시자(Directives)

- HTML 컴파일러가 실행될 때 해당 DOM을 찾기 위해 사용되는 특별한 형태의 속성

- 접두사 `v-`로 시작하는 attribute로 스타일에서부터 이벤트까지 DOM을 다룰 수 있는 모든 범위에서 다양한 지시자를 제공한다.

- 일부 디렉티브는 `:`를 붙여 **전달 인자**를 받아 취급할 수 있다.



- 아래 코드에서 `v-if`디렉티브는 seen 표현의 진실성에 기반하여 <p>엘리먼트를 제거 또는 삽입한다.

```html
<p v-if="seen">Now you see me</p>
<!-- true이면 보이고, false이면 보이지 않음 -->
```



- `v-bind` 디렉티브 : 반응적으로 HTML 속성을 갱신하는데 사용된다.
- 아래 코드에서 href는 전달인자로, 엘리먼트의 href속성을 표현식 url의 값에 바인드하는 `v-bind` 디렉티브에게 알려준다. 
```html
<a v-bind:href="url"></a>
<!-- : 뒤에 선언한 href 인자를 받아 url 값이랑 매핑 -->
```



- `v-on` 디렉티브 : DOM 이벤트를 수신한다. 전달인자는 이벤트를 받을 이름이다.

- 아래 코드에서 `v-on` 지시자를 가진 버튼을 추가한 후 클릭 이벤트를 발생시키면 name값이 자동으로 업데이트 된다.

```html
<div id="example">
  <p>My framework is {{ name + '!' }}</p>
  <button v-on:click="changeName">Change</button>
  <!-- click이라는 이벤트를 받아 Vue에 넘겨준다. -->
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



#### Filter

- 화면에 표시되는 텍스트의 형식을 편하게 바꿀수 있도록 고안된 기능이며, `|`을 이용하여 여러 개의 필터를 적용할 수 있다.
- 중괄호 보간법 혹은 v-bind 표현법을 이용할 때 사용가능하다.

```html
<!-- message에 표시될 문자에 capitalize 필터를 적용하여 첫 글자를 대문자로 변경한다. -->

{{ message | capitalize }} <!-- 중괄호 보간법 -->

<div v-bind:id="rawId | formatId"></div> <!-- v-bind 표현법 -->
```

```js
new Vue({
  //...
  filters: { // filter 정의
    capitalize: function (value) {
      if(!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```



---



### Vue 데이터 바인딩

#### Data Binding

- DOM 기반 HTML Template에 Vue 데이터를 바인딩하는 방법은 아래와 같이 크게  3가지가 있다.
  - Interpolation ( 값 대입 )
  - Binding Expressions ( 값 연결 )
  - Directives ( 디렉티브 사용 )



#### Interpolation ( 값 대입 )

- Vue의 가장 기본적인 데이터 바인딩 체계는 Mustache `{{ }}`를 따른다.

```html
<span>Message: {{ msg }}</span>
<span>This will never change: {{* msg }}</span>  
<!-- {{* msg }} : 예전 문법, msg라는 값이 한 번 들어가면 바껴도 값이 변하지 않는다.   리액티브 시스템 적용X -->
<!-- 이제는 위 코드 대신 아래 코드를 사용한다. -->
 <span v-once>{{ msg }}</span> 
```



#### Binding Expression

- `{{ }}` 를 이용한 데이터 바인딩을 할 때 자바스크립트 표현식을 사용할 수 있다.

```html
<div>{{ number + 1 }}</div>
<div>{{ message.split('').reverse().join('') }} </div>
```

- Vue에 내장된 Filter를 `{{ }}` 안에 사용할 수 있다. 여러 개 필터 체인 가능

```js
{{ message | capitalize }}
{{ message | capitalize | upcapitalize }}
```





#### Directives

- Vue에서 제공하는 특별한  Attributes이며 `v-` 의 prefix(접두사)를 갖는다.
- 자바스크립트 표현식, filter 모두 적용된다.

```html
<!-- login의 결과에 따라 p가 존재 또는 미존재 -->
<p v-if="login">Hello!</p>

<!-- click = {{doSomething}} 와 같은 역할 -->
<a v-on:click="doSomething">
```



#### Class Binding

- CSS 스타일링을 위해서 class를 아래 방법으로 추가 가능하다.
  - `v-bind:class`

  - `class="{{ className }}"`  -> Vue2.4버전 부터는 제거된 문법

- 아래와 같이 `class`속성과  `v-bind:class ` 속성을 동시에 사용해도 된다.

```html
<div class="static" v-bind:class="{ 'class-a': isA, 'class-b': isB}"></div> // 주의 :  중괄호 한개
<script>
  data: {
    isA: true,
    isB: false
  }
<script>
```

- 위 코드의  결과값은 다음과 같다.

```html
<div class="static class-a"></div>
```



- 아래와 같이 Array구문도 사용할 수 있다.

```html
<div v-bind:class="[classA, classB]"></div>
<script>
    data: {
      classA: 'class-a',
      classB: 'class-b'  
    }
</script>
```










<br>



> 참고 링크
>
> [인프런 강의 - 누구나 다루기 쉬운 Vue.js 프론트 개발 – 3시간 안에 배우기](https://www.inflearn.com/course/vue-pwa-vue-js-%EA%B8%B0%EB%B3%B8/)
>
> [자바스크립트 프레임워크 소개 3 - Vue.js](https://meetup.toast.com/posts/99)
>
> [Vue.js 공식가이드](https://kr.vuejs.org/v2/guide/)
>
> [Vue Router](https://router.vuejs.org/kr/)
>
> [디자인패턴 - MVC, MVP, MVVM 비교](http://beomy.tistory.com/43)
>
> [MVC, MVP, MVVM, MVI](https://brunch.co.kr/@oemilk/113)

