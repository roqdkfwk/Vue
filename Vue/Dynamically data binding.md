# Dynamically data binding

- `v-bind`
    - 하나 이상의 속성 또는 컴포넌트 데이터를 표현식에 동적으로 바인딩한다.

- `v-bind`의 사용
    - Attribute Bindings
    - Class and Style Bindings

- Attribute Bindings
    - HTML의 속성 값을 Vue의 상태 속성 값과 동기화 되도록 한다.
        
        ```jsx
        <!-- v-bind.html -->
        
        <img v-bind:src="imageSrc">
        <a v-bind:href="myUrl">Move to url</a>
        ```
        
    - v-bind shorthand → :
        
        ```jsx
        <img :src="imageSrc">
        <a :href="myUrl">Move to url</a>
        ```
        
    - Dynamic attribute name (동적 인자 이름)
        - 대괄호로 감싸서 directive argument에 JavaScript 표현식을 사용할 수도 있다.
        - JavaScript 표현식에 따라 동적으로 평가된 값이 최종 argument 값으로 사용된다.
            
            ```jsx
            <button :[key]="myValue"></button>
            ```
            

- Attibute Bindings 예시
    
    ```jsx
    <!-- v-bind.html -->
    
    <img :src="imageSrc">
    <a :href="myUrl">Move to url</a>
    <p :[dynamicattr]="dynamicValue">...</p>
    
    ==================================================
    
    const { createApp, ref } = Vue
    const app = createApp({
      setup() {
        const imageSrc = ref('https://picsum.photos/200')
        const myUrl = ref('https://www.google.co.kr/')
        const dynamicattr = ref('title')
        const dynamicValue = ref('Hello Vue.js')
        
        return { imageSrc, myUrl, dynamicattr, dynamicValue }
      }
    })
    app.mount('#app')
    
    ==================================================
    
    <div id="app" data-v-app>
      <img src="https://picsum.photos/200">
      <a href="https://www.google.co.kr/">Move to url</a>
      <p title="Hello Vue.js">Dynamic Attr</p>
    </div>
    ```
    

- Class and Style Bindings
    - 클래스와 스타일은 모두 속성이므로 `v-bind`를 사용하여 다른 속성과 마찬가지로 동적으로 문자열 값을 할당할 수 있다.
    - 그러나 단순히 문자열 연결을 사용하여 이러한 값을 생성하는 것은 번거롭고 오류가 발생하기가 쉽다.
    - Vue는 클래스 및 스타일과 함께 `v-bind`를 사용할 때 객체 또는 배열을 활용한 개선 사항을 제공한다.

- Class and Style Bindings가 가능한 경우
    - Binding HTML Classes
        - Binding to Object
        - Binding to Arrays
    - Binding Inline Styles
        - Binding to Object
        - Binding to Arrays

- Binding HTML Classes - Binding to Objects
    - 객체를 `:Class`에 전달하여 클래스를 동적으로 전환할 수 있다.
    - 예시) `isActive`의 T / F에 의해 active 클래스의 존재가 결정된다.
        
        ```jsx
        <!-- binding-html0classes.html -->
        
        const isActive = ref(false)
        
        ==================================================
        
        <div :class="{ active: isActive }">Text</div>
        ```
        
    - 예시) :class directive를 일반 클래스 속성과 함께 사용 가능
        
        ```jsx
        const isActive = ref(false)
        const hasInfo = ref(true)
        
        ==================================================
        
        <div class="static" :class="{ active: isActive, 'text-primary': hasInfo }">Text</div>
        
        ==================================================
        
        <div class="static text-primary">Text</div>
        ```
        

- Binding HTML Classes - Binding to Objects
    - 반드시 Inline 방식으로 작성하지 않아도 된다.
        
        ```jsx
        const isActive = ref(true)
        const hasInfo = ref(false)
        
        // ref는 반응 객체의 속성으로 액세스되거나 변경될 때 자동으로 unwrap
        const classObj = ref({
          active: isActive,
        	  'text-primary': hasInfo
        })
        
        ==================================================
        
        <div class="static" :class="classObj">Text</div>
        
        ==================================================
        
        위의 예제의 경우 <div>요소는 static active 두 개의 클래스가 적용되어
        `class = "static active"`와 같이 렌더링된다. 이러한 구조는 Vue의 반응형 시스템을
        활용하여 사용자 인터페이스의 다양한 상태 변화를 효과적으로 표현할 수 있게 해준다.
        ```
        

- Binding HTML Classes - Binding to Arrays
    - `:class`를 배열에 바인딩하여 클래스 목록을 적용할 수 있다.
    - 예시)
        
        ```jsx
        const activeClass = ref('active')
        const infoClass = ref('text-primary')
        
        ==================================================
        
        <div :class="[activeClass, infoClass]">Text</div>
        
        ==================================================
        
        <div class="active text-primary">Text</div>
        ```
        
    - 배열 구문 내에서 객체 구문 사용
    - 예시)
        
        ```jsx
        <div :class="[{active: isActive }, infoClass]">Text</div>
        ```
        

- Binding Inline Styles - Binding to Objects
    - `:style`은 JavaScript 객체 값에 대한 바인딩을 지원한다. (HTML style 속성에 해당)
    - 예시)
        
        ```jsx
        <!-- binding-inline-styles.html -->
        
        const activeColor = ref('crimson')
        const fontSize = ref(50)
        
        ==================================================
        
        <div :style="{color: activeColor, fontSize: fontSize + 'px'}">Text</div>
        
        ==================================================
        
        <div style="color: crimson; font-size: 50px;">Text</div>
        ```
        

- Binding Inline Styles - Binding to Objects
    - 실제 CSS에서 사용하는 것처럼 :`style`은 kebab-cased 키 문자열도 지원한다. (단, camelCase 작성을 권장)
    - 예시
        
        ```jsx
        const activeColor = ref('crimson')
        const fontSize = ref(50)
        
        ==================================================
        
        <div :style="{ 'font-size': fontSize + 'px'}">Text</div>
        
        ==================================================
        
        <div style="font-size: 50px;">Text</div>
        ```
        

- Binding Inline Styles - Binding to Objects
    - 템플릿을 더 깔끔하게 적성하려면 스타일 객체에 직접 바인딩하는것을 권장한다.
    - 예시
        
        ```jsx
        const activeColor = ref('crimson')
        const fontSize = ref(50)
        const styleObj = ref({
          color: activeColor,
          fontSize: fontSize.value + 'px'
        })
        
        ==================================================
        
        <div :style="styleObj">Text</div>
        
        ==================================================
        
        <div style="color: crimson; font-size: 50px;">Text</div>
        ```
        

- Binding Inline Styles - Binding to Arrays
    - 여러 스타일 객체의 배열에 `:style`을 바인딩할 수 있다.
    - 작성한 객체는 병합되어 동일한 요소에 적용된다.
    - 예시)
        
        ```jsx
        const styleObj2 = ref({
          color: 'blue',
          border: '1px solid black'
        })
        
        ==================================================
        
        <div :style="[styleObj, styleObj2]">Text</div>
        
        ==================================================
        
        <div style="color: blue; font-size: 50px; border: 1px solid black;">
        ```