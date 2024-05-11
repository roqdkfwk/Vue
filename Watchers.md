# Watchers

- `watch()`
    - 반응형 데이터를 감시하고, 감시하는 데이터가 변경되면 콜백 함수를 호출한다.
        
        ```jsx
        watch(variable, (newValue, oldValue) => {
        	do something
        }
        ```
        
        - variable : 감시하는 변수
        - newValue : 감시하는 변수가 변화된 값, 콜백 함수의 첫 번째 인자
        - oldValue : 콜백 함수의 두 번째 인자

- watch 예시 (1/4)
    - 감시하는 변수에 변화가 생겼을 때 기본 동작 확인하기
        
        ```jsx
        <!-- watcher.html -->
        
        <button v-on:click="count++> Add </button>
        <p>Count: {{ count }} </p>
        ```
        
        ```jsx
        const count = ref(0)
        
        const countWatch = watch(count, (newValue, oldValue) => {
        	console.log(`newValue: ${newValue}, oldValue: ${oldValue}`)
        })
        
        출력값
        newValue : 1, oldValue : 0
        newValue : 2, oldValue : 1
        newValue : 3, oldValue : 2
        ```
        
    - 감시하는 변수에 변화가 생겼을 때 연관 데이터 업데이트 하기
        
        ```jsx
        <input v-model="message">
        <p> Message length: {{ messageLength }} </p>
        ```
        
        ```jsx
        const message = ref('')
        const messageLength = ref(0)
        
        const messageWatch = watch(message, (newValue, oldValue) => {
        	messageLength.value = newValue.length
        })
        
        출력값
        hello!
        Message length : 6
        ```
        

- computed와 watchers
    
    ![Untitled](/images/Watchers%20dbb99f93e148429cab441ede2a94529d/Untitled.png)