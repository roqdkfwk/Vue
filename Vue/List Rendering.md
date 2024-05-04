# List Rendering

- `v-for`
    - 소스 데이터(Array, Object, number, String, Iterable)를 기반으로 요소 또는 템플릿 블록을 여러 번 렌더링
- `v-for` 구조
    - `v-for`는 alias in expression 형식의 특수 구문을 사용하여 반복되는 현재 요소에 대한 별칭(alias)을 제공한다.
        
        ```jsx
        <div v-for="item in items">{{ item.text }}</div>
        ```
        
    - 인덱스(객체에서는 키)에 대한 별칭을 지정할 수 있다.
        
        ```jsx
        <div v-for="(item, index) in items"></div>
        <div v-for="value in object"></div>
        <div v-for="(value, key) in object"></div>
        <div v-for="(value, key, index) in object"></div>
        ```
        
- `v-for` 예시
    - 배열 반복
        
        ```jsx
        const my Arr = ref([
          { name: 'Alice', age: 20},
          { name: "Bella', age: 21}
        ])
        ```
        
        ```jsx
        <div v-for="(item, index) in myArr">{{ index }} / {{ item }}</div>
        ```
        
    - 객체 반복
        
        ```jsx
        const myObj = ref({
          name: 'Cathy',
          age: 30
        })
        ```
        
        ```jsx
        <div v-for="(value, key, index) in myObj">
          {{ index }} / {{ key }} / {{ value }}
        </div>
        ```
        
- 여러 요소에 대한 `v-for` 적용
    - template 요소에 `v-for`를 사용하여 하나 이상의 요소에 대해 반복 렌더링 할 수 있다.
        
        ```jsx
        <ul>
          <template v-for="item in myArr">
            <li>{{ item.name }}</li>
            <li>{{ item.age }}</li>
            <hr>
          </template>
        </ul>
        ```
        
- `v-for` with key
    - 반드시 `v-for`와 key를 함께 사용한다.
    - 내부 컴포넌트의 상태를 일관되게 유지한다.
    - 데이터의 예측 가능한 행동을 유지한다. (Vue 내부 동작 관련)
    - key는 반드시 각 요소에 대한 고유한 값을 나타낼 수 있는 식별자이다.
    
    ```jsx
    let id = 0
    
    const items = ref([
      { id: id++, name: 'Alice' },
      { id: id++, name: 'Bella' }
    ])
    ```
    
    ```jsx
    <div v-for="item in items" : key="item.id">
      <!-- content -->
    </div>
    ```
    
- `v-for` with `v-if`
    - 동일 요소에 `v-for`와 `v-if`를 함께 사용하지 않는다.
    - 동일한 요소에서 `v-if`가 `v-for`보다 우선순위가 더 높기 때문이다.
    - `v-if` 조건은 `v-for` 범위의 변수에 접근할 수 없다.
- `v-for`와 `v-if`문제 상황 1
    - `todo` 데이터 중 이미 처리한`(isComplete === true) todo`만 출력하기
        
        ```jsx
        let id = 0
        
        const todos = ref([
          { id: id++, name: '복습', isComplete: true },
          { id: id++, name: '예습', isComplete: false },
          { id: id++, name: '식사', isComplete: true },
          { id: id++, name: '노래', isComplete: false },
        ])
        ```
        
        ```jsx
        <ul>
          <li v-for="todo in todos" v-if="todo.isComplete === true" :key="todo.id">
            {{ todo.name }}
          </li>
        </ul>
        ```
        
- `v-for`와 `v-if` 해결법 1
    - `computed()`를 활용해 필터링 된 목록을 반환하여 반복하도록 설정한다.
        
        ```jsx
        const completeTodos = computed(() => {
          return todos.value.filter((todo) => todo.isComplete)
        })
        ```
        
        ```jsx
        <ul>
          <li v-for="todo in completeTodos" :key="todo.id">
            {{ todo.name }}
          </li>
        </ul>
        ```
        
- `v-for`와 `v-if`문제 상황 2
    - `v-if`가 더 높은 우선순위를 가지므로 `v-for`의 `todo` 요소를 `v-if`에서 사용할 수 없다.
        
        ```jsx
        <ul>
          <li v-for="todo in todos" v-if="!todo.isComplete" :key="todo.id">
            {{ todo.name }}
          </li>
        </ul>
        ```
        
    - `v-for`와 `v-if` 해결법 2
        
        ```jsx
        <ul>
        <template v-for="todo in todos" :key="todo.id">
          <li v-if="!todo.isComplete">
            {{ todo.name }}
          </li>
        </template>
        </ul>
        ```
