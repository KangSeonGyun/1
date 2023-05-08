---
title: Vuejs Composition API vs Options API
tags: 
---

Composition API vs Options API 둘 중에 하나를 선택해서 사용해야 한다.

Composition API은 최신에 나왔으며 Options API보다 더 편하게 사용가능하다


option api에서는 다음과 같이 중첩이 생긴다 

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

Composition API는 다음과 같이 사용할 수 있다

```vue
<script setup>
    let b = 30;
</script>

<template>
    {{ b }}
</template>
```

a는 2way가 지원되며 input에 값을 넣을 때 마다 a가 바뀐다
a는 리액티브 하다
b는 1way이며 input에 값을 넣을 때 b가 바뀌지 않는다
b는 리액티브 하지 않다

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

b도 리엑티브하게 바꾸려면 Vuejs 공식 홈페이지 API의 Reactivity: Core를 확인해 보자
아래와 같이 수정하면 b도 리액티브 해진다.

```vue
<script setup>
    import { ref } from 'vue';

    let b = ref(30);
</script>
```

아래와 같이 menu를 만들고 ref로 감싸면 menu는 리액티브 하다
하지만 이게 정상일까?

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

위와 같은 결과를 보이지만
객체를 리엑티브하게 바꾸려면 공식 API 설명에 따라 reactive()를 써주는게 바람직하다

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

생명주기도 다르게 작성할 수 있다
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

Composition API는 export default의 객체로 내보내지 않아도 되며, 이를통해 중접이 사라진 걸 볼 수 있다.

```vue
<script setup>
import { onMounted } from 'vue';

onMounted(() => {
    console.log("mounted c")
});
</script>
```

methods 차이
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

아래와 같이 람다를 써도 되긴하나 바람직하지 않다.
이유는 lambda 에서 확인

```vue
<script setup>
const clickHandler2 = () => {
    console.log("clicked");
}
</script>
```

let list = reactive([]);를 선언해두고 fetch 끝날 때 
list = json.list 로 하면 list에 값이 담기긴 하나 화면에 안 뜬다
reactive가 대체되기 때문
list = reactive(json.list); 도 안 된다.

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

아래 예제는 b 값을 입력받고 b값에 2를 더한 total을 출력한다
b.value를 왜쓰지?
computed가 정확히 뭐지?
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

list를 출력하는 예제와 total을 호출하는 예제를 합쳐 모든 메뉴의 price를 계산하는 방법
map을 왜 만들고 어떻게 만든거지?
reduce 란?

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

이제 del 버튼을 누르면 리스트를 삭제할 수 있다

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

filter를 사용해 검색 기능을 추가했다 메뉴의 이름 m.name에 text 박스에 쓴 텍스트가 포함되어 있다면 필터링 된다.

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

watch(query, () => {
    model.list = model.list.filter(m => m.name.includes(query.value));
});
// watch는 그냥 함수다 리액티브한 데이터를 보고있다가 바뀌면 실행

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

shallowRef 와 triggerRef

```vue
<script setup>
import { computed, onMounted, reactive, watch, ref, shallowRef, triggerRef } from 'vue';

let model = reactive({
    newList: [],
    list: []
})

let total = computed(() => model.list.map((m) => m.price).reduce((p, c) => p + c, 0));

let query = ref("");

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
```vue