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