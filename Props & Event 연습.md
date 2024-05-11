# Props & Event 연습

```jsx
<!-- App.vue -->

<template>
  <div>
    <ParentPage />
  *</div>*
</template>

<script setup>
import ParentPage from './components/ParentPage.vue';

</script>

<style scoped>

</style>
```

```jsx
<!-- ParentPage.vue -->

<template>
    <div>
        <h1>ParentPage</h1>
        <ChildPage :ChildList="children" v-on:giveEmit="giveMoney"/>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import ChildPage from './ChildPage.vue'

const children = ref([
    {name: "첫째", age: 30, money: 10000},
    {name: "둘째", age: 25, money: 10000},
    {name: "셋째", age: 20, money: 10000},
])

const giveMoney = function(child) {
    const receiveChild = children.value.find(c => c.name === child.name)
    if (receiveChild){
        receiveChild.money += 1000
    }
}
</script>

<style scoped>
</style>
```

```jsx
<!-- ChildPage.vue -->

<template>
    <div>
        <p v-for="child in ChildList">
            <h3>자식 페이지입니다.</h3>
            <p>이름 : {{ child.name }}</p>
            <p>나이 : {{ child.age }}</p>
            <p>용돈 : {{ child.money }}</p>
            <button v-on:click="givefunc(child)">용돈 추가</button>
        </p>
    </div>
</template>

<script setup>
defineProps({
    ChildList: Array
})

const emit = defineEmits(['giveEmit'])

const givefunc = function (child) {
    emit('giveEmit', child)
}
</script>

<style scoped>
</style>
```

- 동작 순서
    - `ParentPage`에서 `ChildPage`에게 `ChildList`라는 이름으로 Array타입의 `children`를 전달한다.
    - `ChildPage`에서는 `ParentPage`에서 받아온 `ChildList`를 순회하면서 `child`의 `name`, `age`, `money` 정보를 출력한다
    - `<button>`태그의 `click` 이벤트를 감지하면 `givefunc()`라는 함수가 실행된다.
    - `givefunc()`함수는 `giveEmit`이라는 이름의 이벤트를 발생시키며 `ParentPage`로 `child` 객체를 전달한다.
    - `ParentPage`에서는 `ChildPage`에서 발생한 `giveEmit`이라는 이벤트를 감지하면 `giveMoney()`라는 함수를 실행시킨다.
    - `giveMoney()` 함수는 인자로 받은 `child`와 이름이 일치하는 객체를 찾아 그 객체의 `money`값을 1000만큼 증가시킨다.