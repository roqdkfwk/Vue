# Event Handling

- `v-on`
    - DOM 요소에 이벤트 리스너를 연결 및 수신

- `v-on` 구성
    
    ```jsx
    v-on:event="handler"
    ```
    
    - handler 종류
        1. Inline handlers : 이벤트가 트리거 될 때 실행 될 JavaScript 코드
        2. Method handlers : 컴포넌트에 정의된 메소드 이름
    1. `v-on` shorthand → ‘@’

- Inline Handlers
    - Inline Handlers는 주로 간단한 상황에 사용한다.
        
        ```jsx
        <!-- enevt-handling.html -->
        
        const count = ref(0)
        
        ==================================================
        
        <button v-on:click="count++">Add</button>
        <p>Count: {{ count }}</p>
        ```
        

- Method Handlers
    - Inline Handlers로는 불가능한 대부분의 상황에서 사용한다.
        
        ```jsx
        const name = ref('Alice')
        const myFunc = function (event) {
          console.log(event)
          console.log(event.target)
          console.log(`Hello ${name.value}!`)
        })
        
        ==================================================
        
        <button v-on:click="myFunc">Hello</button>
        
        ==================================================
        
        함수 내부에서 event.taret을 이는 클릭된 <button>요소 자체를 참조하게 된다.
        console.log(event.target)을 입력하면 <button>Hello</button>이 출력된다.
        event.target.tagName을 사용하면 "button"이 출력된다.
        event.target.textContent or event.target.innerText를 사용하면 Hello가 출력된다.
        ```
        

- Inline Handlers에서의 메소드 호출
    - 메소드 이름에 직접 바인딩하는 대신 Inline Handlers에서 메소드를 호출할 수도 있다.
    - 이렇게 하면 기본 이벤트 대신 사용자 지정 인자를 전달할 수 있다.
        
        ```jsx
        const greeting = function (message) {
          console.log(message)
        }
        
        ==================================================
        
        <button v-on:click="greeting('hello')">Say Hello</button>
        <button v-on:click="greeting('bye')">Say Bye</button>
        ```
        
- Inline Handlers에서의 event 인자에 접근하기
    - Inline Handlers에서 원래 DOM 이벤트에 접근하기
    - `$event` 변수를 사용하여 메소드에 전달
        
        ```jsx
        const warning = function (message, event) {
          console.log(message)
          console.log(event)
        }
        
        ==================================================
        
        <button v-on:click="warning('경고입니다.', $event)">Submit</button>
        ```
        

- Event Modifiers
    - Vue는 `v-on`에 대한 Event Modifiers를 제공해 `event.preventDefault()`와 같은 구문을 메소드에서 작성하지 않도록 한다.
    - stop, prevent, self 등 다양한 modifiers를 제공한다.
    - 메소드는 DOM 이벤트에 대한 처리보다는 데이터에 관한 논리를 작성하는 것에 집중한다.
        
        ```jsx
        <form v-on:submit.prevent="onSubmit">...</form>
        <a v-on:cllick.stop.prevent="onLink">...</a>
        ```
        

- Key Modifiers
    - Vue는 키보드 이벤트를 수신할 때 특정 키에 관한 별도 modifiers를 사용할 수 있다.
    - 예시) Key가 Enter일 때만 onSubmit 이벤트를 호출하기
        
        ```jsx
        <input v-on:keyup.enter="onSubmit">
        ```