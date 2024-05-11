# Component Events

- `$emit()`
    - **자식 컴포넌트가 이벤트를 발생시켜 부모 컴포넌트로 데이터를 전달하는 역할의 메소드**
    - `‘$’` 표기는 Vue 인스턴스나 컴포넌트 내에서 제공되는 전역 속성이나 메소드를 식별하기 위한 접두어이다.

- `$emit()` 메소드 구조
    - `$emit(event, …args)`
    - `event` → 커스텀 이벤트 이름
    - `args` → 추가인자

- 이벤트 발신 및 수신
    - `$emit()`을 사용하여 템플릿 표현식에서 직접 사용자 정의 이벤트를 발신하면 부모에서는 `v-on`을 사용하여 수신할 수 있다.
        
        ```jsx
        <!-- Child -->
        <template>
          <button v-on:click="$emit('someEvent')">클릭</button>
        </template>
        
        ==================================================
        
        <!-- Parent -->
        <template>
          <ParentChild v-on:some-event="someCallback" :dynamic-props="name" />
        </template>
        
        <script>
        export default {
          methods: {
            someCallback() {
              console.log("ParentChild가 발신한 이벤트를 수신했습니다.");
            }
          }
        }
        </script>
        ```
        

- 이벤트 발신 및 수신하기
    - `ParentChild`에서 `someEvent`라는 이름의 사용자 정의 이벤트를 발신
    - `ParentChild`의 부모 `Parent`는 `v-on`을 사용하여 발신된 이벤트를 수신
    - 수신 후 처리할 로직 및 콜백함수 호출
        
        ```jsx
        <!-- ParentChild.vue -->
        
        <button v-on:click="$emit('someEvent')">클릭</button>
        
        ==================================================
        
        <!-- Parent.vue -->
        
        <ParentChild v-on:some-event="someCallback" my-msg="message" v-bind:dynamic-props="name" />
        
        const someCallback = function() {
          console.log("ParentChild가 발신한 이벤트를 수신했습니다.')
        }
        ```
        
    - 이벤트 수신 결과

- `$emit()`이벤트 선언
    - `defineEmits()`를 사용하여 명시적으로 발신할 이벤트를 선언할 수 있다.
    - script에서 `$emit()` 메소드에 접근할 수 없기 때문에 `defineEmits()`는 `$emit()` 대신 사용할 수 있는 동등한 함수를 반환한다.
        
        ```jsx
        <script setup>
        const emit = defineEmits(['someEvent', 'myFocus'])
        
        const buttonClick = function() {
          emit('someEvent')
        }
        </script>
        ```
        

- 이벤트 선언하기
    - 이벤트 선언 방식으로 추가 버튼 작성 및 결과 확인
        
        ```jsx
        <!-- ParentChild.vue -->
        
        <script setup>
        const emit = defineEmits(['someEvent', 'myFocus'])
        
        const buttonClick = function() {
          emit('someEvent')
        }
        </script>
        
        <button v-on:click="buttonClick">클릭</button>
        ```
        

- 이벤트 인자
    - 이벤트 발신 시 추가 인자를 전달하여 값을 제공할 수 있다.

- 이벤트 인자 전달하기
    - `ParentChild`에서 이벤트를 발신하여 `Parent`로 추가 인자 전달하기
        
        ```jsx
        <!-- ParentChild.vue -->
        
        const emit = defineEmits(['someEvent', 'emitArgs'])
        
        const emitArgs = function () {
          emit('emitArgs', 1, 2, 3)
        }
        
        <button v-on:click="emitArgs">추가 인자 전달</button>
        ```
        
    - `ParentChild`에서 발신한 이벤트를 `Parent`에서 수신
        
        ```jsx
        <!-- Parent.vue -->
        
        <ParentChild
          v-on:some-event="someCallback"
          v-on:emit-args="getNumbers"
          my-msg="message"
          v-bind:dynamic-props="name"
        />
        
        const getNumbers = function(...args) {
          console.log(args)
          console.log('ParentChild가 전달한 추가인자 ${args}를 수신했습니다.')
        }
        ```
        
    - 추가 인자 전달 확인

- Event Name Casing
    - 선언 및 발신 시 → camelCase
        
        ```jsx
        <button v-on:click="$emit('someEvent')">클릭</button>
        
        const emit = defineEmits(['someEvent'])
        
        emit('someEvent')
        ```
        
    - 부모 컴포넌트에서 수신 시 → kebab-case
        
        ```jsx
        <ParentChild v-on:some-event="..." />
        ```
        

- emit 이벤트 실습 및 구현하기
    - 최하단 컴포넌트 `ParentGrandChild`에서 `Parent` 컴포넌트의 `name` 변수 변경 요청하기
    - `App` → `Parent` → `ParentChild` → `ParentGrandChild` 순서
    - `ParentGrandChild`에서 이름 변경을 요청하는 이벤트 발신
        
        ```jsx
        <!-- ParentGrandChild.vue -->
        
        const emit = defineEmits(['updateName'])
        
        const updateName = function() {
        	emit('updateName')
        }
        
        <!-- '이름 변경' 버튼을 클릭하면 updateName을 ParentChild로 발신한다. -->
        <button v-on:click="updateName">이름 변경</button>
        ```
        
    - 이벤트 수신 후 이름 변경을 요청하는 이벤트 발신
        
        ```jsx
        <!-- ParentChild.vue -->
        
        const emit = defineEmits(['updateName'])
        
        const updateName = function() {
        	emit('updateName')
        }
        
        <!-- ParentGrandChild에서 발신된 update-name을 수신하면 updateName를 실행한다. -->
        <ParentGrandChild v-bind:my-msg="myMsg" v-on:update-name="updateName" />
        ```
        
    - 이벤트 수신 후 이름 변경 메소드 호출
        
        ```jsx
        <!-- Parent.vue -->
        
        <!-- ParentChild에서 발신된 update-name을 수신하면 updateName을 실행하고 -->
        <ParentChild v-on:update-name="updateName" />
        
        <!-- updateName이 실행되면 name이 'updatedName'으로 바뀐다. -->
        const updateName = function() {
        	name.value = 'updatedName'
        }
        ```
        
    - 해당 변수를 prop으로 받는 모든 곳에서 자동으로 업데이트
    - 버튼 클릭 후 결과 확인