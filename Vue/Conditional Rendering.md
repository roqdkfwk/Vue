# Conditional Rendering

- `v-if`
    - 표현식 값의 T / F를 기반으로 요소를 조건부로 렌더링
- `v-if` 예시
    - `‘v-else’` directive를 사용하여 `v-if`에 대한 else 블록을 나타낼 수 있다.
        
        ```jsx
        const isSeen = ref(true)
        ```
        
        ```jsx
        <p v-if="isSeen">true일 때 보여요</p>
        <p v-else>false일 때 보여요</p>
        <button @click="isSeen = !isSeen">토글</button>
        ```
        
        ![Untitled](/images/Conditional%20Rendering/Untitled.png)        
        
    - `‘v-else-if’` directive를 사용하여 `v-if`에 대한 else if 블록을 나타낼 수 있다.
        
        ```jsx
        const name = ref('Cathy')
        ```
        
        ```jsx
        <div v-if="name === 'Alice'">Alice</div>
        <div v-else-if="name === 'Bella'">Bella</div>
        <div v-else-if="name === 'Cathy'">Cathy</div>
        <div v-else>아무도 아닙니다.</div>
        ```
        
- 여러 요소에 대한 `v-if` 적용
    - `v-if`는 directive이기 때문에 단일 요소에만 연결이 가능하다.
    - 이 경우 template 요소에 `v-if`를 사용하여 하나 이상의 요소에 대해 적용할 수 있다.
    (`v-else`, `v-else-if` 모두 적용가능)
    
    ```jsx
    <template v-if="name === 'Cathy'">
    	<div>Cathy</div>
    	<div>30살</div>
    </template>
    ```
    
- HTML `<template>` element
    - 페이지가 로드될 때 렌더링 되지 않지만 JavaScript를 사용하여 나중에 문서에서 사용할 수 있도록 하는 HTML을 보유하기 위한 메커니즘
    - “보이지 않는 wrapper 역할”
- `v-show`
    - 표현식 값의 T / F를 기반으로 요소의 가시성(visibility)을 전환
- `v-show` 예시
    - `v-show` 요소는 항상 렌더링 되어 DOM에 남아있다.
    - CSS display 속성만 전환하기 때문이다.
    
    ```jsx
    const isShow = ref(false)
    ```
    
    ```jsx
    <div v-show="isShow">v-show</div>
    ```
    
- `v-if` vs `v-show`
    - `v-if` (Cheap initial load, expensive toggle)
        - 초기 조건이 false인 경우 아무 작업도 수행하지 않는다.
        - 토글 비용이 높다.
    - `v-show` (Expensive initial load, cheap toggle)
        - 초기 조건에 관계없이 항상 렌더링된다.
        - 초기 렌더링 비용이 더 높다.
    - 무언가를 매우 자주 전환해야 하는 경우에는 `v-show`를, 실행 중에 조건이 변경되지 않는 경우에는 `v-if`를 권장한다.