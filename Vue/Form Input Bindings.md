# Form Input Bindings

- Form Input Binding
    - form을 처리할 때 사용자가 input에 입력하는 값을 실시간으로 JavaScript상태에 동기화해야하는 경우 (양방향 바인딩)
    - 양방향 바인딩 방법
        1. `v-bind`와 `v-on`을 함께 사용
        2. `v-model` 사용

- `v-bind`와 `v-on`을 함께 사용
    - `v-bind`를 사용하여 input 요소의 value 속성 값을 입력 값으로 사용
    - `v-on`을 사용하여 input 이벤트가 발생할 때마다 input 요소의 value값을 별도 반응형 변수에 저장하는 핸들러 호출
        
        ```jsx
        <!-- form-input-bindings.html -->
        
        const inputText1 = ref('')
        const onInput = function (event) {
          inputText1.value = event.currentTarget.value
        }
        
        ==================================================
        
        <p>{{ inputText1 }}</p>
        <input v-bind:value="inputText1" v-on:input="onInput">
        
        input 박스 안에 입력되는 값(=value)와 "inputText1"을 바인딩하고
        글을 입력하면 "onInput" 메소드가 실행된다.
        메소드가 실행되면 "inputText1"의 값(=inputText1.value)이 입력한 글로 갱신된다.
        ```
        

- `v-model`
    - form input 요소 또는 컴포넌트에서 양방향 바인딩을 만든다.
    - `v-model`을 사용하여 사용자 입력 데이터와 반응형 변수를 실시간으로 동기화
        
        ```jsx
        const inputText2 = ref('')
        
        ==================================================
        
        <p>{{ inputText2 }}</p>
        <input v-model="inputText2">
        ```
        

- Checkbox 활용
    - 단일 체크박스와 boolean 값 활용
        
        ```jsx
        <!-- v-model.html -->
        
        const checked = ref(false)
        
        ==================================================
        
        <input type="checkbox" id="checkbox" v-model="checked">
        <label for="checkbox">{{ checked }}</label>
        
        type="checkbox"는 사용자가 선택할 수 있는 체크박스를 생성한다.
        id="checkbox"는 HTML 요소에 고유한 식별자를 부여하며 <label>요소의 for 속성과
        연동되어 접근성을 향상시키는 데 사용된다.
        ```
        
        ![Untitled](/images/Form%20Input%20Bindings/Untitled.png)
        
    - 여러 체크박스와 배열 활용
        - 해당 배열에는 현재 선택된 체크박스의 값이 포함된다.
            
            ```jsx
            const checkedNames = ref([])
            
            ==================================================
            
            <div>Checked names: {{ checkedNames }}</div>
            
            <input type="checkbox" id="alice" value="Alice" v-model="checkedNames">
            <label for="alice">Alice</label>
            
            <input type="checkbox" id="bella" value="Bella" v-model="checkedNames">
            <label for="bella">Bella</label>
            ```
            
            ![Untitled](/images/Form%20Input%20Bindings/Untitled%201.png)
            

- Select 활용
    - select에서 `v-model` 표현식의 초기 값이 어떤 option과도 일치하지 않는 경우 select 요소는 “선택되지 않은(unselected)” 상태로 렌더링된다.
        
        ```jsx
        const selected = ref('')
        
        ==================================================
        
        <div>Selected: {{ selected }}</div>
        
        <select v-model="selected">
          <option disabled value="">Please select one</option>
          <option>Alice</option>
          <option>Bella</option>
          <option>Cathy</option>
        </select>
        ```
        
        ![Untitled](/images/Form%20Input%20Bindings/Untitled%202.png)