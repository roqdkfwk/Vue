# Pinia

- pinia 구성 요소
    - store
    - state
    - getters
    - actions
    - plugin

- store
    - 중앙 저장소이다.
    - 모든 컴포넌트가 공유하는 상태, 기능 등이 작성된다.
        
        ```jsx
        <!-- stores/counter.js -->
        
        import { ref, computed } from 'vue'
        import { defineStore } from 'pinia'
        
        export const useCounterStore = defineStore('counter', () => {
        
          <!-- state -->
          const count = ref(0)
          
          <!-- getters -->
          const doubleCount = computed(() => count.value * 2)
          
          <!-- actions -->
          const increment = function() {
            count.value++
          }
          
          return { count, doubleCount, increment }
        })
        ```
        

- State
    - 반응형 데이터(상태)이다.
    - `ref()` === state
    - store의 인스턴스로 state에 접근하여 직접 읽고 쓸 수 있다.
    - 만약 store에 state를 정의하지 않았다면 컴포넌트에서 새로 추가할 수 없다.
        
        ```jsx
        <!-- App.vue -->
        
        import { useCounterStore } from '@/stores/counter'
        
        const store = useCounterStore()
        
        // state 참조 및 변경
        console.log(store.count)
        const newNumber = store.count + 1
        
        ==================================================
        
        <template>
          <div>
            <p>state : {{ store.count }}</p>
          </div>
        </template>
        ```
        

- Getters
    - 계산된 값
    - `computed()` === Getters
    - store의 모든 getters를 state처럼 직접 접근 할 수 있다.
        
        ```jsx
        <!-- App.vue -->
        
        // getters 참조
        console.log(store.doubleCount)
        
        ==================================================
        
        <template>
          <div>
            <p>getters : {{ store.doubleCount }}</p>
          </div>
        </template>
        ```
        

- Actions
    - 메소드
    - `function()` === actions
    - store의 모든 actions를 직접 접근 및 호출할 수 있다.
    - getters와 달리 state 조작, 비동기, API 호출이나 다른 로직을 진행할 수 있다.
        
        ```jsx
        <!-- App.vue -->
        
        // actions 호출
        store.increment()
        
        ==================================================
        
        <template>
          <div>
            <button v-on:click="store.increment()">+++</button>
          </div>
        </template>
        ```
        

- plugin
    - 어플리케이션의 상태 관리에 필요한 추가 기능을 제공하거나 확장하는 도구나 모듈이다.
    - 어플리케이션의 상태 관리를 더욱 간편하고 유연하게 만들어주며 패키지 매니저로 설치 이후 별도 설정을 통해 추가된다.

- Pinia 구성 요소 종합
    - Pinia는 store라는 저장소를 가진다.
    - store는 state, getters, actions으로 이루어지며 각각 `ref()`, `computed()`, `function()`과 동일하다.