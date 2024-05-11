# Template Syntax

- Template Syntax
    - DOM을 기본 구성 요소 인스턴스 데이터에 선언적으로 바인딩할 수 있는 HTML 기반 템플릿 구문을 사용한다.

- Template Syntax 종류
    1. Text Interpolation
    2. Raw HTML
    3. Attribute Bindings
    4. JavaScript Expressions

- Text Interpolation
    
    `<p>Message: {{ msg }}</p>`
    
    - 데이터 바인딩의 가장 기본적인 형태
    - 이중 중괄호 구문(콧수염 구문)을 사용한다.
    - 콧수염 구문은 해당 구성 요소 인스턴스의 msg 속성 값으로 대체된다.
    - msg 속성이 변경될 때마다 업데이트된다.

- Raw HTML
    
    ```jsx
    <div v-html="rawHtml"></div>
    
    const rawHtml = ref('<span style="color:red">This should be red.</span>')
    
    위의 코드는
    
    <div>
      rawHtml
    </div>
    
    이며 아래와 같다.
    
    <div>
      <span style="color:red">This should be red.</span>
    </div>
    ```
    
    - 콧수염 구문은 데이터를 일반 텍스트로 해석하기 때문에 실제 HTML을 출력하려면 `v-html`을 사용해야한다.

- Attribute Bindings
    
    ```jsx
    <div v-bind: id="dynamicId"></div>
    
    const dynamicId = ref('my-id')
    
    마찬가지로 위의 코드는 아래와 같다.
    
    <div id="my-id"></div>
    ```
    
    - 콧수염 구문은 HTML 속성 내에서 사용할 수 없기 때문에 `v-bind`를 사용한다.
    - HTML의 id 속성 값을 Vue의 dynamicId 속성과 동기화 되도록 한다.
    - 바인딩 값이 null이나 undefined인 경우 렌더링 요소가 제거된다.

- JavaScript Expressions
    
    ```jsx
    {{ number + 1 }}
    {{ ok ? 'YES' : 'NO' }}
    {{ message.split('').reverse().join('') }}
    <div v-bind: id="`list-${id}`"></div>
    ```
    
    - Vue는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원한다.
    - Vue 템플릿에서 JavaScript 표현식을 사용할 수 있는 위치
        - 콧수염 구문 내부
        - 모든 directive의 속성 값 (`v-`로 시작하는 특수 속성)

- Expressions 주의사항
    - 각 바인딩에는 하나의 단일 표현식만 포함될 수 있다.
        - 표현식은 값으로 평가할 수 있는 코드 조각을 뜻한다. (return 뒤에 사용할 수 있는 코드)
        - 작동하지 않는 경우
            
            ```jsx
            표현식이 아닌 선언식인 경우
            {{ const number = 1 }}
            
            흐름제어도 작동하지 않는다. 삼항 표현식을 사용
            {{ if (ok) {return message } }}
            ```
            

- Directive
    - ‘`v-`’ 접두사가 있는 특수 속성

- Directive 특징
    - Directive의 속성 값은 단일 JavaScript 표현식이어야 한다. (`v-for`, `v-on` 제외)
    - 표현식 값이 변경될 때 DOM에 반응적으로 업데이트를 적용한다.
    - 예시
        - `v-if`는 seen 표현식 값의 T / F를 기반으로 <p>요소를 제거 / 삽입
            
            `<p v-if="seen">Hi There</p>`
            
            ![Untitled](/images/Template%20Syntax%208a8e704030c34e19b9018f53c78ab628/Untitled.png)
            

- Directive - Arguments
    - 일부 directive는 directibe 뒤에 콜론(`:`)으로 표시되는 인자를 사용할 수 있다.
    - 아래의 href는 HTML <a> 요소의 href 속성 값을 myUrl 값에 바인딩 하도록하는 `v-bind` 인자이다.
        
        ```jsx
        <a v-bind:href="myUrl">Link</a>
        ```
        
    - 아래의 click은 수신할 이벤트의 이름을 작성하는 `v-on`의 인자이다.
        
        ```jsx
        <button v-on:click="doSomething">Button</button>
        ```
        

- Directive - Modifiers
    - `.`(dot)로 표시되는 특수 접미사로, directive가 특별한 방식으로 바인딩 되어야 함을 나타낸다.
    - 예를 들어 `.prevent`는 발생한 이벤트에서 `event.preventDefault()`를 호출하도록 `v-on`에 지시하는 modifier이다.
        
        ```jsx
        <form v-on:submit.prevent="onSubmit">...</form>
        ```