# Computed Property

- `computed()` : 계산된 속성을 정의하는 함수. 미리 계산된 속성을 사용하여 템플릿에서 표현식을 단순하게 하고 불필요한 반복 연산을 줄인다.
- `computed()` 기본 예시(1 / 2)
    - 할 일이 남았는지 여부에 따라 다른 메세지를 출력하기
        
        ```jsx
        const todos = ref([
        		{ text: 'Vue 실습' },
        		{ text: '자격증 공부' },
        		{ text: 'TIL 작성' }
        ])
        ```
        
        ```jsx
        <h2>남은 할 일</h2>
        <p>{{ todos.length > 0 ? '아직 남았다' : '퇴근' }}</p>
        ```
        
    - 템플릿이 복잡해지며 todos에 따라 계산을 수행하게 된다.
    - 만약 이 계산을 템플릿에 여러 번 사용하는 경우에는 반복이 발생한다.
- `computed()` 기본 예시(2 / 2)
    - `computed()` 적용
    - 반응성 데이터를 포함하는 복잡한 로직의 경우 `computed()`를 활용하여 미리 값을 계산
        
        ```jsx
        const { createApp, ref, computed } = Vue
        
        const restOfTodos = computed(() => {
        		return todos.value.length > 0 ? '아직 남았다' : '퇴근'
        })
        ```
        
        ```jsx
        <h2>남은 할 일</h2>
        <p>{{ restOfTodos }} </p>
        ```
        
- `computed()` 특징
    - 반환되는 값은 computed ref이며 일반 refs와 유사하게 계산된 결과를 `.value`로 참조할 수 있다. (템플릿에서는 `.value`는 생략가능)
    - `computed()` 속성은 의존된 반응형 데이터를 자동으로 추적한다.
    - 의존하는 데이터가 변경될 때만 재평가한다.
        - `restOfTodos`의 계산은 `todos`에 의존하고 있다.
        - 따라서 `todos`가 변경될 때만 `restOfTodos`가 업데이트 된다.
    
    ```jsx
    const restOfTodos = computed(() => {
    		return todos.value.length > 0 ? '아직 남았다' : '퇴근'
    })
    ```
    
- `computed()`와 동일한 로직을 처리할 수 있는 method
    - `computed()` 속성 대신 method로도 동일한 기능을 정의할 수 있다.
    - 두 가지 접근 방식은 실제로 완전히 동일하다.
    
    ```jsx
    const getRestOfTodos = function() {
    		return todos.value.length > 0 ? '아직 남았다' : '퇴근'
    }
    ```
    
    ```jsx
    <p>{{ getRestOfTodos() }}</p>
    ```
    
- `computed()`와 method의 차이
    - `computed()` 속성은 **의존된 반응형 데이터를 기반으로 캐시(cached)된다**.
    - 의존하는 데이터가 변경된 경우에만 재평가된다.
    - 즉, 의존된 반응형 데이터가 변경되지 않는 한 이미 계산된 결과에 대한 여러 참조는 다시 평가할 필요 없이 이전에 계산된 결과를 즉시 반환한다.
    - 반면, method 호출은 다시 렌더링이 발생할 때마다 항상 함수를 실행한다.
- Cache(캐시)
    - 데이터나 결과를 일시적으로 저장해두는 임시 저장소이다.
    - 이후에 같은 데이터나 결과를 다시 계산하지 않고 빠르게 접근할 수 있도록 한다.
- Cache 예시
    - “웹 페이지의 캐시 데이터”
        - 페이지 일부 데이터를 브라우저 캐시에 저장 후 같은 페이지에 다시 요청 시 모든 데이터를 다시 응답 받는 것이 아닌 캐시 된 데이터를 사용하여 더 빠르게 웹 페이지를 렌더링한다.
- compute와 method의 적절한 사용처
    - computed
        - **의존하는 데이터에 따라 결과가 바뀌는 계산된 속성을 만들 때** 유용하다.
        - 동일한 의존성을 가진 여러 곳에서 사용할 때 계산 결과를 캐싱하여 중복 계산을 방지한다.
    - method
        - **단순히 특정 동작을 수행하는 함수를 정의할 때** 사용한다.
        - 데이터에 의존하는지 여부와 관계없이 항상 동일한 결과를 반환하는 함수이다.
- compute와 method 정리
    - compute
        - 의존된 데이터가 변경되면 자동으로 업데이트된다.
    - method
        - 호출해야만 실행된다.
    - 무조건 compute만 사용하는 것이 아니라 사용 목적과 상황에 맞게 compute와 method를 적절히 조합하여 사용한다.