---
title: Vuejs Composition API vs Options API
tags: vue
---

## Composition API vs Options API

Composition API vs Options API 를 비교해보려고 한다.

Composition API은 최신에 나왔으며 Options API보다 더 편하게 사용가능하다

### 중첩의 제거

Option api에서는 다음과 같이 중첩이 생긴다. export default안에 data안에 return이 있다.

```vue
<script>
    export default {
        data() {
            return {
                a: 10
            };
        }
    }
</script>

<template>
    {{ a }}
</template>
```

Composition API는 다음과 같이 사용할 수 있다.   
export default의 객체로 내보내지 않아도 되며, 이를통해 중접이 사라진 걸 볼 수 있다.

```vue
<script setup>
    let b = 30;
</script>

<template>
    {{ b }}
</template>
```

### ref, reactive

a는 2way가 지원되며 input에 값을 넣을 때 마다 a가 바뀐다. a는 리액티브 하다.
b는 1way이며 input에 값을 넣을 때 b가 바뀌지 않는다. b는 리액티브 하지 않다.

```vue
<script>
    export default {
        data() {
            return {
                a: 10
            };
        }
    }
</script>

<script setup>
    let b = 30;
</script>

<template>
    <div>
        {{ a }}, {{ b }}
    </div>
    <div>
        a:<span v-text="a"></span><input v-model="a"><br>
        b:<span v-text="a"></span><input v-model="b">
    </div>
</template>
```

b도 리엑티브하게 바꾸려면 Vuejs 공식 홈페이지 API의 Reactivity: Core를 확인해 보자. 아래와 같이 수정하면 b도 리액티브 해진다.

```vue
<script setup>
    import { ref } from 'vue';

    let b = ref(30);
</script>
```

아래와 같이 menu를 만들고 ref로 감싸면 menu는 리액티브 하다. 하지만 이렇게 하는게 맞을까?

```vue
<script setup>
    import { ref } from 'vue';

    let b = ref(30);
    let menu = ref({
        id: 1,
        name: "아메리카노",
        price: 3000
    })
</script>

<template>
    <div>
        b:<span v-text="b"></span><input v-model="b"><br>
        price:<span v-text="menu.price"></span><input v-model="menu.price"><br>
    </div>
</template>
```

같은 결과를 보이지만 **객체**를 리엑티브하게 바꾸려면 공식 API 설명에 따라 reactive()를 써주는게 바람직하다

```vue
<script setup>
    import { reactive, ref } from 'vue';

    let b = ref(30);
    let menu = reactive({
        id: 1,
        name: "아메리카노",
        price: 3000
    })
</script>

<template>
    <div>
        b:<span v-text="b"></span><input v-model="b"><br>
        price:<span v-text="menu.price"></span><input v-model="menu.price"><br>
    </div>
</template>
```

### reactive 사용시 주의점

```vue
<script setup>
import { reactive } from 'vue';

let list = reactive([]);
</script>
```

위와같이 list 선언해두고 fetch 끝날 때 list = json.list 을 하면 list에 값이 담기긴 하나 화면에 안 뜬다. reactive가 대체되기 때문이다.   
list = reactive(json.list); 도 안 됐다.

아래 예제에선 model에 반응형 객체를 만들고 그 안에 list를 만들었다.   
이렇게 하면 fetch문으로 list를 받아왔을 때 reactive하게 View(Template)에 반영된다.

```vue
<script setup>
import { onMounted, reactive } from 'vue';

let model = reactive({
    newList:[],
    list:[]
})

async function load() {
    let res = await fetch("http://localhost:8080/menus");
    let json = await res.json();
    model.list = json.list;
}

onMounted(() =>{
    load();
})
</script>

<template>
    <div>
        <ul>
            <li v-for="m in model.list">
                <span v-text="m.name"></span>
            </li>
        </ul>
    </div>
</template>
```

### components

Options API는 components에 Header를 담아야 했다

```vue
<script>
    import Header from './components/Header.vue';

    export default {
        components:{
            Header
        }
    }
</script>

<template>
    <Header />
</template>
```

Composition API는 다음과 같다

```vue
<script setup>
import Header from './components/Header.vue';
</script>

<template>
    <Header />
</template>
```

### Lifecycle

Options API

```vue
<script>
    export default {
        mounted(){
            console.log("mounted o")
        }
    }
</script>
```

Composition API

```vue
<script setup>
import { onMounted } from 'vue';

onMounted(() => {
    console.log("mounted c")
});
</script>
```

### methods

Options API

```vue
<script>
    export default {
        methods:{
            clickHandler(){
                console.log("clicked");
            }
        }
    }
</script>

<template>
    <button @click="clickHandler">클릭</button>
</template>
```

Composition API

```vue
<script setup>
function clickHandler(){
    console.log("clicked");
}
</script>

<template>
    <button @click="clickHandler">클릭</button>
</template>
```

아래와 같이 람다를 써도 되긴하나 바람직하지 않다. 이유는 lambda 에서 확인

```vue
<script setup>
const clickHandler2 = () => {
    console.log("clicked");
}
</script>
```

## computed

computed 속성은 반응형 데이터를 기반으로 계산된 값을 생성할 수 있다.   
아래 예제에서 total은 의존하는 데이터(b)가 변경될 때마다 자동으로 다시 계산하고 업데이트한다.

total의 기능 : reactive한 b값에 2를 더한 total을 출력한다.   

ref로 생성된 변수는 내부적으로 객체로 래핑되어 .value 속성을 통해 실제 값을 가져올 수 있다.   
v-model.number는 문자열 더하기가 되서 썼다.

```vue
<script setup>
import { computed, ref } from 'vue';

let b = ref(30);
let total = computed(() => b.value + 2);
</script>


<template>
    <div>
        total(b+2) : <span v-text="total"></span><br>
        b : <input v-model.number="b">
    </div>
</template>
```

list를 받아와 View에 보여주며 모든 list의 price total을 계산하는 예제   

[map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 메소드는 배열의 각 요소에 대해 주어진 함수를 호출하고, 해당 함수의 반환 값을 가지고 새로운 배열을 생성합니다.   
[reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) 메소드는 배열의 요소를 하나씩 순회하며 주어진 함수를 호출하여 값을 축적하는 작업을 수행합니다.   

```vue
<script setup>
import { computed, onMounted, reactive } from 'vue';

let model = reactive({
    newList: [],
    list: []
})

let total = computed(() => model.list.map((m) => m.price).reduce((p, c) => p + c, 0));

async function load() {
    let res = await fetch("http://localhost:8080/menus");
    let json = await res.json();
    model.list = json.list;
}

onMounted(() => {
    load();
})
</script>


<template>
    <div>
        <ul>
            <li v-for="m in model.list">
                <span v-text="m.name"></span>
            </li>
        </ul>
    </div>

    <div>
        total : <span v-text="total"></span><br>
    </div>
</template>
```

추가 예제

list 옆에있는 del 버튼을 누르면 해당 아이템만 삭제하는 기능.

[findIndex](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) 메소드는 배열에서 주어진 조건을 만족하는 첫 번째 요소의 인덱스를 반환하는 배열 메소드다.

```vue
<script setup>
import { computed, onMounted, reactive } from 'vue';

let model = reactive({
    newList: [],
    list: []
})

let total = computed(() => model.list.map((m) => m.price).reduce((p, c) => p + c, 0));

async function load() {
    let res = await fetch("http://localhost:8080/menus");
    let json = await res.json();
    model.list = json.list;
}

// --- Del 기능 ---

function menuDelHandler(id) {
    let idx = model.list.findIndex(m => m.id == id);
    model.list.splice(idx, 1);
    // 여기서 dom 사용은 지양하자
}

onMounted(() => {
    load();
})
</script>


<template>
    <div>
        <ul>
            <li v-for="m in model.list">
                <span v-text="m.name"></span><input type="button" value="del" @click="menuDelHandler(m.id)" />
            </li>
        </ul>
    </div>

    <div>
        total : <span v-text="total"></span><br>
    </div>
</template>
```

추가 예제 2 (watch 사용)

computed는 계산된 값이 같으면 동작하지 않지만, watch는 그냥 함수다 리액티브한 데이터를 보고있다가 바뀌면 실행된다.

filter를 구현해 검색 기능을 추가했다. 메뉴의 이름 m.name에 text 박스에 쓴 텍스트가 포함되어 있다면 필터링 된다.

[filter](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 메소드는 배열에서 주어진 조건을 만족하는 모든 요소로 구성된 새로운 배열을 생성하는 배열 메소드입니다.   
[includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) 메소드는 배열이 특정 요소를 포함하는지 여부를 확인하는 배열 메소드입니다. 주어진 값이 배열에 포함되어 있는 경우 true를 반환하고, 그렇지 않은 경우 false를 반환합니다.

```vue
<script setup>
import { computed, onMounted, reactive, watch, ref } from 'vue';

let model = reactive({
    newList: [],
    list: []
})

let total = computed(() => model.list.map((m) => m.price).reduce((p, c) => p + c, 0));

let query = ref("");

async function load() {
    let res = await fetch("http://localhost:8080/menus");
    let json = await res.json();
    model.list = json.list;
}

function menuDelHandler(id) {
    let idx = model.list.findIndex(m => m.id == id);
    model.list.splice(idx, 1);
}

// --- filter 기능 ---

watch(query, () => {
    model.list = model.list.filter(m => m.name.includes(query.value));
});

onMounted(() => {
    load();
})
</script>

<template>
    <div>
        search : <input type="text" v-model="query">
    </div>

    <div>
        <ul>
            <li v-for="m in model.list">
                <span v-text="m.name"></span><input type="button" value="del" @click="menuDelHandler(m.id)" />
            </li>
        </ul>
    </div>

    <div>
        total : <span v-text="total"></span><br>
    </div>
</template>
```

## shallowRef 와 triggerRef

shallowRef와 triggerRef는 Vue 3 Composition API에서 제공되는 리액티브(reactive) 객체를 다루기 위한 함수

shallowRef 함수는 단일 값 또는 객체를 리액티브하게 감싸는 역할을 한다.   
일반적인 ref와 달리, shallowRef는 객체의 프로퍼티까지는 리액티브로 만들지 않는다. 즉, 객체 내부의 변경은 리액티브로 추적되지 않는다.



```vue
<script setup>
import { computed, onMounted, reactive, watch, ref, shallowRef, triggerRef } from 'vue';

let model = reactive({
    newList: [],
    list: []
})

let total = computed(() => model.list.map((m) => m.price).reduce((p, c) => p + c, 0));

let query = ref("");

// --- shallowRef ---

let aa = shallowRef({name:'okay'});

async function load() {
    let res = await fetch("http://localhost:8080/menus");
    let json = await res.json();
    model.list = json.list;
}

function menuDelHandler(id) {
    let idx = model.list.findIndex(m => m.id == id);
    model.list.splice(idx, 1);
}

function inputHandler(){
    triggerRef(aa);
}

watch(query, () => {
    model.list = model.list.filter(m => m.name.includes(query.value));
});

onMounted(() => {
    load();
})
</script>

<template>
    <div>
        search : <input type="text" v-model="query">
    </div>

    <div>
        <ul>
            <li v-for="m in model.list">
                <span v-text="m.name"></span><input type="button" value="del" @click="menuDelHandler(m.id)" />
            </li>
        </ul>
    </div>

    <div>
        total : <span v-text="total"></span><br>
        {{ aa.name }}<input type="text" v-model="aa.name" @input="inputHandler">
    </div>
</template>
```

컴포넌트간 데이터 교환?

과거방식

아래는 NewMenu.vue 이다

```vue
<script>
export default{
    props:['list']
}
</script>
```

아래는 App.vue 이다

```vue
<template>
	<Newlist :list="model.newList" />
</template>
```

App.vue에서 model.newList를 NewMenu.vue의 data로 넘겼다

현재방식

Newmenu.vue 만 아래와 같이 바꾸면 된다

```vue
<script setup>
    let props = defineProps({
        list: []
    });
</script>
```

그 외에도 다음과 같이 값을 보내면

```vue
<Newlist :list="model.newList" title="추천메뉴" :aa="aa.name"/>
```

아래와 같이 받을 수 있다.

```vue
<script setup>
    let props = defineProps({
        list: [],
        title: "",
        aa: ""
    });
</script>
```

## 모달 만들 때 배경 검정색

```html
<template>
    <div class="screen">
        <section>
            <h1>title</h1>
            <div>
                <button>OK</button>
                <button>Cancel</button>
            </div>
        </section>
    </div>
</template>
```

아래와 같이 배경을 주면 button도 투명도가 먹는다. button의 opacity를 1로 해도 투명도가 먹는다.

```css
.screen{
    background-color: black;
    opacity: 0.7;

    position: fixed;
    left: 0;
    top: 0;
    width: 100vw;
    height: 100vh;
}

button{
    opacity: 1;
}
```

background-color: rgba 로 하면 버튼의 배경은 투명해지지 않는다

```css
.screen{
    background-color: rgba(0, 0, 0, 0.8);

    position: fixed;
    left: 0;
    top: 0;
    width: 100vw;
    height: 100vh;
}
```

## 모달 띄우기

### app.vue

```vue
<script setup>
import { ref } from 'vue';
import Modal from './components/Modal.vue';

let showModal = ref(false)

function showHandler() {
    showModal.value = true;
}

</script>

<template>
    <div>
        <button @click="showHandler">show 모달</button>
    </div>
    <Modal title="공지사항" :show="showModal">
        <div>
            안녕하세요
        </div>
    </Modal>
</template>
```

### modal.vue

```vue
<script setup>
let props = defineProps({
    title: "",
    show: false
})

function okHandler() {
    props.show = false;
}
</script>

<template>
    <div class="screen" :class="{ 'd-none': !show }">
        <section>
            <h1>{{ title }}</h1>
            <div class="content">
                <slot></slot>
            </div>
            <div class="commands">
                <button @click="okHandler">OK</button>
                <button>Cancel</button>
            </div>
        </section>
    </div>
</template>
```

okHandler()로는 props.show가 바뀌지 않는다.
Set operation on key "show" failed: target is readonly.
생각해보면 올바른 코드로 우리를 이끌고 있다
modal이 아닌 app.vue에서 ok를 눌렀는지 cancel를 눌렀는지 알아야할 필요가있다

emit을 사용하여 Modal.vue에서 App.vue의 @ok 이벤트를 발생시킬 수 있다.
아래 예제는 ok 이벤트를 발생시키며 단순 텍스트로 "데이터"도 전달 했다

```vue
<script setup>
import { ref } from 'vue';
import Modal from './components/Modal.vue';

let showModal = ref(false)

function showHandler() {
    showModal.value = true;
}

function dlgHandler(a) {
    console.log(a); // 콘솔에 "데이터"가 찍힌다
    showModal.value = false;
}
</script>

<template>
    <div>
        <button @click="showHandler">show 모달</button>
    </div>
    <Modal title="공지사항" :show="showModal" @ok="dlgHandler">
        <div>
            안녕하세요
        </div>
    </Modal>
</template>
```


```vue
<script setup>
let props = defineProps({
    title: "",
    show: false
})
</script>

<template>
    <div class="screen" :class="{ 'd-none': !show }">
        <section>
            <h1>{{ title }}</h1>
            <div class="content">
                <slot></slot>
            </div>
            <div class="commands">
                <button @click="$emit('ok', '데이터')">OK</button>
                <button>Cancel</button>
            </div>
        </section>
    </div>
</template>

<style scoped>
.d-none {
    display: none !important;
}

.screen {
    background-color: rgba(0, 0, 0, 0.8);

    position: fixed;
    left: 0;
    top: 0;
    width: 100vw;
    height: 100vh;

    display: flex;
    align-items: center;

    justify-content: center;
}

section {
    background-color: white;
    display: inline-block;

    border-radius: .7em;
}

section>h1 {
    font-size: 14px;
    padding: 0px 10px;
}

section>.content {
    border-top: 1px solid black;
    border-bottom: 1px solid black;
    padding: 10px 20px;
}

section>.commands {
    padding: 10px 10px;

    display: flex;
    justify-content: center;
}
</style>
```

## 모달 애니메이션 추가

### App.vue

```vue
<script setup>
import { ref } from 'vue';
import Modal from './components/Modal.vue';

let showModal = ref(false)

function showHandler() {
    showModal.value = true;
}

function dlgHandler(a) {
    console.log(a); // 콘솔에 "데이터"가 찍힌다
    showModal.value = false;
}
</script>

<template>
    <div>
        <button @click="showHandler">show 모달</button>
    </div>
    <Modal title="공지사항" :show="showModal" @ok="dlgHandler">
        <div>
            안녕하세요
        </div>
    </Modal>
</template>
```

### Modal.vue

```vue
<script setup>
let props = defineProps({
    title: "",
    show: false
})
</script>

<template>
    <div class="screen" :class="{ 'd-none': !show }">
        <section :class="{ 'show-effect': show }">
            <h1>{{ title }}</h1>
            <div class="content">
                <slot></slot>
            </div>
            <div class="commands">
                <button @click="$emit('ok', '데이터')">OK</button>
                <button>Cancel</button>
            </div>
        </section>
    </div>
</template>

<style scoped>

@keyframes show-effect {
    from{
        transform: translateY(-300px);
    }

    to{
        transform: translateY(-200px);
    }
    
}

.d-none {
    display: none !important;
}

.screen {
    background-color: rgba(0, 0, 0, 0.8);

    position: fixed;
    left: 0;
    top: 0;
    width: 100vw;
    height: 100vh;

    display: flex;
    align-items: center;

    justify-content: center;
}

section {
    background-color: white;
    display: inline-block;

    border-radius: .7em;
}

.show-effect{
    animation: show-effect 1s forwards;
}

section>h1 {
    font-size: 14px;
    padding: 0px 10px;
}

section>.content {
    border-top: 1px solid black;
    border-bottom: 1px solid black;
    padding: 10px 20px;
}

section>.commands {
    padding: 10px 10px;

    display: flex;
    justify-content: center;
}
</style>
```

## Vuejs Transition을 이용한 애니메이션

Transition으로 효과가 나타나길 바라는 부분을 감싸고 v-if로 보였다 안보였다 할 때 애니메이션 효과를 줄 수 있다.

```vue
<script setup>
import { ref } from 'vue'

let showRcmdMenu = ref(false);

function showHandler() {
    showRcmdMenu.value = !showRcmdMenu.value;
}
</script>

<template>
	<button type="submit" @click.prevent="showHandler">보기</button>
	
    <Transition>
        <div v-if="showRcmdMenu">
            내용
        </div>
    </Transition>
</template>

<style scoped>
.v-enter-active,
.v-leave-active {
    transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
    opacity: 0;
}
</style>
```

[공식문서](https://vuejs.org/guide/built-ins/transition.html)

## 페이지 이동간 Transition

Vue Router를 이용해야 한다.

1

```vue
<script setup>
import Header from './Header.vue';
import Footer from './Footer.vue';
import Aside from './Aside.vue';
</script>

<template>
    <Header />
    <Aside />

    <router-view v-slot="{ Component }">
        <transition name="fade">
            <component :is="Component" />
        </transition>
    </router-view>

    <Footer />
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>
```

2

```vue
<script setup>
import Header from './Header.vue';
import Footer from './Footer.vue';
import Aside from './Aside.vue';
</script>

<template>
    <Header />
    <Aside />

    <Transition name="fade">
        <router-view></router-view>
    </Transition>

    <Footer />
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>
```

두 가지 방법 모두 가능하다