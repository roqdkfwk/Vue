# Passing Props

- Props
    - 부모 컴포넌트 → 자식 컴포넌트
    - 데이터를 전달하는데 사용되는 속성이다.
- One-Way Data Flow
    - 모든 props는 자식 속성과 부모 속성 사이에 **하향식 단방향 바인딩**을 형성한다.
- Props 특징
    - 부모 속성이 업데이트되면 자식으로 흐르지만 그 반대는 안된다.
    - 즉, 자식 컴포넌트 내부에서 props를 변경하려고 시도해서는 안되며 불가능하다.
    - 부모 컴포넌트가 업데이트 될 때마다 자식 컴포넌트의 모든 props가 최신 값으로 업데이트된다.
- 단방향인 이유
    - 하위 컴포넌트가 실수로 상위 컴포넌트의 상태를 변경하여 앱에서의 데이터 흐름을 이해하기 어렵게 만드는 것을 방지하기 위함이다.
- 사전 준비
    - Vue 프로젝트 생성
    - 초기 생성된 컴포넌트 모두 삭제
    - src/assets 내부 파일 모두 삭제
    - main.js 해당 코드 삭제
- `App` → `Parent` → `ParentChild` 컴포넌트 관계 작성
    - `App` 컴포넌트 작성
        
        ```java
        <!-- App.vue -->
        
        <template>
        	<div>
        		<Parent />
        	</div>
        </template>
        
        <script setup>
        import Parent from '@/components/Parent.vue'
        </script>
        ```
        
    - `Parent` 컴포넌트 작성
        
        ```jsx
        <!-- Parent.vue -->
        
        <template>
        	<div>
        		<ParentChild />
        	</div>
        </template>
        
        <script setup>
        import ParentChild from '@/components/ParentChild.vue'
        </script>
        ```
        
    - `ParentChild` 컴포넌트 작성
        
        ```jsx
        <!-- ParentChild.vue -->
        
        <template>
        	<div>ParentChild입니다.</div>
        </template>
        
        <script setup>
        </script>
        ```
        
- Props 선언
    - 부모 컴포넌트에서 보낸 props를 사용하기 위해서는 자식 컴포넌트에서 명시적인  props 선언이 필요하다.
- Props 작성
    - 부모 컴포넌트 `Parent`에서 자식 컴포넌트 `ParentChild`에 보낼 props 작성하기.
        
        ```jsx
        <!-- Parent.vue -->
        
        <template>
        	<div>
        		<ParentChild my-msg="message" />
        	</div>
        </template>
        
        my-msg : prop 이름
        message : prop 값
        ```
        
- Props를 선언하는 방법
    - 문자열 배열을 사용한 선언
    - 객체를 사용한 선언

- 문자열 배열을 사용한 Props 선언
    - `defineProps()`를 사용하여 props를 선언

```jsx
<!-- ParentChild.vue -->

<script setup>
	defineProps(['myMsg'])
<script>
```

- 객체를 사용한 Props 선언
    - 객체 선언 문법의 각 객체 속성의 키는 props의 이름이 되며, 객체 속성의 값은 값이 될 데이터의 타입에 해당하는 생성자 함수 `(Number, String, …)`이어야 한다.
    - **객체 선언 문법 사용이 권장**된다.
        
        ```jsx
        <!-- ParentChild.vue -->
        
        <script setup>
        	defineProps({
        		myMsg: String
        	})
        </script>
        ```
        
- prop 데이터 사용
    - 템플릿에서 반응형 변수와 같은 방식으로 활용한다.
        
        ```jsx
        <!-- ParentChild.vue -->
        
        <div>
        	<p>{{ myMsg }}</p>
        </div>
        ```
        
    - props를 객체로 반환하므로 필요한 경우 JavaScript에서 접근이 가능하다.
        
        ```jsx
        <script setup>
        	const props = defineProps({ myMsg: String })
        	console.log(props)    // {myMsg: 'message'}
        	console.log(props.myMsg)    // 'message'
        </script>
        ```
        
- 한 단계 더 prop 내려보내기
    - `ParentChild` 컴포넌트를 부모로 갖는 `ParentGrandChild` 컴포넌트를 생성 및 등록한다.
    
    ```jsx
    <!-- ParentGrandChild.vue -->
    
    <template>
    	<div>ParentGrandChild입니다.</div>
    <template>
    
    <script setup>
    </script>
    ```
    
    ```jsx
    <!-- ParentChild.vue -->
    
    <template>
    	<div>
    		<p>{{ myMsg }}</p>
    		<ParentGrandChild />
    	</div>
    </template>
    
    <script setup>
    import ParentGrandChild from '@/components/ParentGrandChild.vue'
    
    defineProps({
    	myMsg: String
    })
    </script
    ```
    
    - `ParentChild` 컴포넌트에서 `Parent`로부터 받은 prop인 `myMsg`를 `ParentGrandChild`에게 전달한다.
    
    ```jsx
    <!-- ParentChild.vue -->
    
    <template>
    	<div>
    		<p>{{ myMsg }}</p>
    		<ParentGrandChild :my-msg="myMsg"/>
    	</div>
    </template>
    ```
    
    ```jsx
    <!-- ParentGrandChild.vue -->
    
    <template>
    	<div>
    		<p>{{ myMsg }}</p>
    	</div>
    </template>
    
    <script setup>
    defineProps({
    	myMsg: String
    })
    </script>
    ```
    
    - 출력 결과를 확인한다.
    - `ParentGrandChild`가 받아서 출력하는 prop은 `Parent`에 정의되어있는 prop이며 `Parent`가 prop을 변경하는 경우 이를 전달받고 있는 `ParentChild`, `ParentGrandChild`에서도 모두 업데이트가 된다.
- Props Name Casing
    - 선언 및 템플릿 참조 시 → `camelCase`
        
        ```jsx
        <p>{{ myMsg }}</P>
        ```
        
        ```jsx
        defineProps({
        	myMsg: String
        })
        ```
        
    - 자식 컴포넌트로 전달 시 → kebab-case
        
        ```jsx
        <ParentChild my-msg="message" />
        ```
        
- Static props & Dynamic props
    - 지금까지 작성한 것은 Static(정적) props이다.
    - v-bind를 사용하여 **동적으로 할당된 props**를 사용할 수 있다.
    1. Dynamic props 정의
        
        ```jsx
        <!-- Parent.vue --> 
        
        import { ref } from 'vue'
        
        const name = ref('Alice')
        ```
        
        ```jsx
        <!-- Parent.vue --> 
        
        <ParentChild my-msg="message" :dynamic-props="name" />
        ```
        
    2. Dynamic props 선언 및 출력
        
        ```jsx
        <!-- ParentChild.vue -->
        
        defineProps({
        	myMsg: String,
        	dynamicProps: String
        })
        ```
        
        ```jsx
        <!-- ParentChild.vue -->
        
        <p>{{ dynamicProps }}</p>
        ```
        
    3. Dynamic props 출력 확인
        
        ![Untitled](Passing%20Props%200a4e67e9e83c4271bf330ae36788bdbc/Untitled.png)