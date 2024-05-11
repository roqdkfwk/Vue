# Pinia 실습

- Pinia를 활용한 Todo 프로젝트 구현
    - Todo CRUD
    - Todo 개수 계산
        - 전체 Todo
        - 완료된 Todo
        - 미완료된 Todo

- 컴포넌트 구성
    - `App`
        - `TodoForm`
        - `TodoList`
            - `TodoListItem`

- 사전 준비
    - `TodoListItem`컴포넌트 작성
        
        ```jsx
        <!-- TodoListItem.vue -->
        
        <template>
          <div>TodoListItem</div>
        </template>
        ```
        
    - `TodoList`컴포넌트 작성
    - `TodoListItem`컴포넌트 등록
        
        ```jsx
        <!-- TodoList.vue -->
        
        <template>
          <div>
            <TodoListItem />
          </div>
        </template>
        
        <script setup>
        import TodoListItem from '@/components/TodoListItem.vue'
        </script>
        ```
        
    - `TodoForm`컴포넌트 작성
        
        ```jsx
        <!-- TodoForm.vue -->
        
        <template>
          <div>TodoForm</div>
        </template>
        ```
        
    - `App`컴포넌트에 `TodoList`, `TodoForm`컴포넌트 등록
        
        ```jsx
        <!-- App.vue -->
        
        <template>
          <div>
            <h1>Todo Project</h1>
            <TodoList />
            <TodoForm />
          </div>
        </template>
        
        <script setup>
        import TodoForm from '@/components/TodoForm.vue'
        import TodoList from '@/components/TodoList.vue'
        </script>
        ```
        

- Todo 조회
    - store에 임시 todos 목록 상태를 정의
        
        ```jsx
        // stores/counter.js
        
        import { ref, computed } from 'vue'
        import { defineStore } from 'pinia'
        
        export const useCounterStore = defineStore('counter', () => {
          let id = 0
          const todos = ref([
            { id: id++, text: '할 일1', isDone: false },
            { id: id++, text: '할 일2', isDone: false },
          ])
          
          return { todos }
        })
        
        'counter'는 저장소의 고유 이름이자 식별자
        이 이름을 사용하여 어플리케이션 내 다른 부분에서 이 'counter'저장소를 참조하고
        접근할 수 있게 된다.
        ```
        
    - store의 todos 상태를 참조
    - 하위 컴포넌트인 `TodoListItem`을 반복하면서 개별 todo를 props로 전달한다.
        
        ```jsx
        <!-- TodoList.vue -->
        
        import { useCounterStore } from '@/stores/counter'
        import TodoListItem from '@/components/TodoListItem.vue'
        
        const store = useCounterStore()
        
        ==================================================
        
        <template>
          <div>
            <TodoListItem
              v-for="todo in store.todos"
              :key="todo.id"
              :todo="todo"
            />
          </div>
        </template>
        ```
        
    - props 정의 후 데이터 출력 확인
        
        ```jsx
        <!-- TodoListItem.vue -->
        
        <template>
          <div>{{ todo.text }}</div>
        </template>
        
        <script setup>
        defineProps({
          todo: Object
        })
        </script>
        ```
        
- Todo 생성
    - todos 목록에 todo를 생성 및 추가하는 `addTodo()` 액션 정의
    - `TodoForm`에서 실시간으로 입력되는 사용자 데이터를 양방향 바인딩하여 반응형 변수로 할당한다.
    - submit 이벤트가 발생했을 때 사용자 입력 텍스트를 인자로 전달하여 store에 정의한 `addTodo()` 액션 메소드를 호출한다.
    - form 요소를 선택하여 todo 입력 후 input 데이터를 초기화 할 수 있도록 처리한다.
        
        ```jsx
        // stores/counter.js
        
        const addTodo = function(todoText) {
          todos.value.push({
            id: id++,
            text: todoText,
            isDone: false
          })
        }
        
        return { todos, addTodo }
        
        ==================================================
        
        <!-- TodoForm.vue -->
        
        <template>
          <div>
            <form v-on:submit.prevent="createTodo(todoText)" ref="formElem">
              <input type="text" v-model="todoText">
              <input type="submit">
            </form>
          </div>
        </template>
        
        ==================================================
        
        import { ref } from 'vue'
        import { useCounterStore } from '@/stores/counter'
        
        const store = useCounterStore()
        const todoText = ref('')
        const formElem = ref(null)
        const createTodo = function(todoText) {
          store.addTodo(todoText)
          formElem.value.reset()
        }
        
        <form>은 사용자가 새 할 일을 입력할 수 있는 입력 필드와 제출 버튼을 포함한다.
        `v-model` 지시자를 사용하여 `todoText` 데이터 속성과 입력 필드를 양방향으로
        바인딩한다.
        폼 제출 이벤트(`v-on:submit.prevent`)는 페이지 리로드를 방지하며 
        `createTodo` 함수를 호출한다.
        `formElem`은 폼 요소를 참조하기 위한 반응형 참조로, 제출 후 폼을 초기화하기 위해
        사용된다.
        `createTodo`함수는 `store.addTodo`를 호출하여 새로운 할 일을 추가하고,
        이후 `formElem.value.reset()`을 통해 입력 필드를 초기화한다.
        ```
        
- Todo 삭제
    - todos 목록에서 특정 todo를 삭제하는 `deleteTodo()` 액션 정의
    - 각 todo에 삭제 버튼을 작성
    - 버튼을 클릭하면 선택된 todo의 id를 인자로 전달해 `deleteTodo()` 메소드를 호출한다.
    - 전달 받은 todo의 id값을 활용해 선택된 todo의 인덱스를 구한다.
    - 특정 인덱스 todo를 삭제 후 todos 배열을 재설정한다.
        
        ```jsx
        // stores/counter.js
        
        const deleteTodo = function(todoId) {
          const index = todos.value.findIndex((todo) => todo.id === todoId)
          todos.value.splice(index, 1)
        }
        
        return {todos, addTodo, deleteTodo }
        
        ==================================================
        
        <!-- TodoListItem.vue -->
        
        import { useCounterStore } from '@/stores/counter'
        
        const store = useCounterStore()
        
        ==================================================
        
        <div>
          <span>{{ todo.text }}</span>
          <button v-on:click="store.deleteTodo(todo.id)">Delete</button>
        </div>
        ```
        
- Todo 수정
    - 각 todo 상태의 isDone 속성을 변경하여 todo의 완료 유무 처리하기
    - 완료된 todo에는 취소선 스타일을 적용한다.
    - todos 목록에서 특정 todo의 isDone 속성을 변경하는 `updateTodo()`액션을 정의한다.
    - 전달 받은 todo의 id값을 활용해 선택된 todo와 동일 todo를 목록에서 검색한다.
    - 일치하는 todo 데이터의 isDone 속성 값을 반대로 재할당 후 새로운 todo 목록을 반환한다.
    - todo 객체의 isDone 속성 값에 따라 스타일 바인딩을 적용한다.
        
        ```jsx
        // stores/counter.js
        
        const updateTodo = function(todoId) {
          todos.value = todos.value.map((todo) => {
            if(todo.id === todoId) {
              todo.isDone = !todo.isDone
            }
            return todo
          })
        }
        
        return {todos, addTodo, deleteTodo, updateToto}
        
        ==================================================
        
        <!-- TodoListItem.vue -->
        
        <div>
          <span v-on:click="store.updateTodo(todo.id)">
            {{ todo.text }}
          </span>
          <button v-on:click="store.deleteTodo(todo.id)">Delete</buttion>
          <span v-on:click="store.updateTodo(todo.id)" :class="{'is-done': todo.isDone}">
            {{ todo.text }}
          </span>
        </div>
        
        <style scoped>
          .is-done {
            text-decoration: line-through;
          }
        </style>
        ```
        
- Todo 개수 계산
    - todos 배열의 길이 값을 반환하는 함수 `doneTodosCount()` 작성(getters)
    - `App`컴포넌트에서 `doneTodosCount()` getter를 참조한다.
        
        ```jsx
        // stores/counter.js
        
        const doneTodosCount = computed(() => {
          return todos.value.filter((todo) => todo.isDone)
        })
        
        return { todos, addTodo, ..., doneTodosCount }
        
        ==================================================
        
        <!-- App.vue -->
        
        import { useCounterStore } from '@/stores/counter'
        
        const store = useCounterStore()
        
        ==================================================
        
        <div>
          <h1>Todo Project</h1>
          <h2>완료된 Todo : {{ store.doneTodosCount }}</h2>
          <TodoList />
          <TodoForm />
        </div>
        ```
        

- Local Storage
    - 브라우저 내에 key-value 쌍을 저장하는 웹 스토리지 객체

- Local Storage 특징
    - 페이지를 새로 고침하고 브라우저를 다시 실행해도 데이터가 유지된다.
    - 쿠키와 다르게 네트워크 요청 시 서버로 전송되지 않는다.
    - 여러 탭이나 창 간에 데이터를 공유할 수 있다.

- Local Storage 사용 목적
    - 웹 어플리케이션에서 사용자 설정, 상태 정보, 캐시 데이터 등을 클라이언트 측에서 보관하여 웹사이트의 성능을 향상시키고 사용자 경험을 개선하기 위함