# Vue.js

- Vue란?
    - 사용자 인터페이스를 구축하기 위한 JavaScript 프레임워크

- Vue를 학습하는 이유
    1. 쉬운 학습 곡선 및 간편한 문법
        - 새로운 개발자들도 빠르게 학습할 수 있다.
    2. **반응성 시스템**
        - **데이터 변경에 따라 자동으로 화면이 업데이트** 되는 기능을 제공한다.
    3. **모듈화 및 유연한 구조**
        - 어플리케이션을 **컴포넌트 조각**으로 나눌 수 있다.
        - 코드의 재사용성을 높이고 유지보수를 용이하게 한다.

- SSAFY에서의 Vue
    - Vue는 React나 Angular 대비 간결하고 직관적인 문법을 가지고 있어 초기 학습이 상대적으로 원활하다.
    - 거대하고 활발한 커뮤니티를 가지고 있어 풍부한 문서, 튜토리얼, 예제 및 다양한 리소스를 공유 받을 수 있다.

- Vue의 2가지 핵심 기능
    1. 선언적 렌더링 (Declarative Rendering)
        - HTML을 확장하는 템플릿 구문을 사용하여 HTML이 JavaScripg 데이터를 기반으로 어떻게 보이는지 설명할 수 있다.
    2. **반응형 (Reactivity)**
        - **JavaScript 상태 변경 사항을 자동으로 추적**하고 변경 사항이 발생할 때 DOM을 효율적으로 업데이트한다.

- Vue를 사용하는 방법
    - CDN 방식
    - NPM 방식 (CDN 방식 이후에 진행)

- 첫 번째 Vue 작성하기
    - CDN 및 Application instance 작성
    - Application instance
        - 모든 Vue 어플리케이션을 createApp 함수로 새 Application instance를 생성하는 것으로 시작
    - `app.mount()`
        - 컨테이너 요소에 어플리케이션 인스턴스를 탑재(연결)
        - 각 앱 인스턴스에 대해 `mount()`는 한 번만 호출할 수 있다.
    - ref 함수
     **: 반응형 상태(데이터)를 선언하는 함수(Declaring Reactive State)**
        - 인자를 받아 .value 속성이 있는 ref 객체로 래핑하여 반환한다.
        - **ref로 선언된 변수의 값이 변경되면, 해당 값을 사용하는 템플릿에서 자동으로 업데이트한다.**
        - 인자는 어떠한 타입도 가능하다.
        - 템플릿의 참조에 접근하려면 `setup()` 함수에서 선언 및 반환 필요하다.
        - 템플릿에서 ref를 사용할 때는 .value를 작성할 필요 없다.

- Vue 기본 구조
    - `createApp()`에 전달되는 객체는 Vue 컴포넌트이다.
    - 컴포넌트의 상태는 `setup()` 함수 내에서 선언되어야 하며 객체를 반환해야 한다.

- 템플릿 렌더링
    - 반환된 객체의 속성은 템플릿에서 사용할 수 있다.
    - Mustache syntax(콧수염 구문)를 사용하여 메시지 값을 기반으로 동적 텍스트를 렌더링한다.
    - 콘텐츠는 식별자나 경로에만 국한되지 않으며 유효한 JavaScript 표현식을 사용할 수 있다.
- Event Listeners in Vue
    - ‘`v-on`’ directive를 사용하여 DOM 이벤트를 수신할 수 있다.
    - 함수 내에서 refs를 변경하여 구성 요소 상태를 업데이트한다.
        
        ```jsx
        <!-- event-listener.html -->
        
        <div id="app">
          <button v-on:click="increment">{{ count }}</button>
        </div>
        
        const { createApp, ref } = Vue
        
        const app = createApp({
          setup() {
            const count = ref(0)
            const increment = function () {
              count.value++
            }
            
            return { count, increment }
          }
        })
        ```