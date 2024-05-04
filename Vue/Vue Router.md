# Vue Router

- Vue 프로젝트 구조 변화
    - `App.vue` 코드 변화
    - `router` 폴더 생성
    - `views` 폴더 생성
    
    ![Untitled](./images/Vue%20Router/Untitled.png  )
    
- `RouterLink`
    - 페이지를 다시 로드하지 않고 URL을 변경하고 URL 생성 및 관련 로직을 처리한다.
    - **HTML의 a 태그를 렌더링한다.**
- `RouterView`
    - **URL에 해당하는 컴포넌트를 표시**한다.
    - 어디에나 배치하여 레이아웃에 맞출 수 있다.
        
        ```jsx
        <!-- App.vue -->
        
        <template>
          <header>
        	<nav>
         	  <RouterLink to="/">Home</RouterLink>
        	  <RouterLink to="/about">About</RouterLink>
        	<nav>
          </header>
        	
          <RouterView />
        </template>
        ```
        

- `router/index.js`
    - 라우팅에 관련된 정보 및 설정이 작성되는 곳이다.
    - `router`**에 URL과 컴포넌트를 매핑**한다.
- `views`
    - `RouterView` 위치에 렌더링 할 컴포넌트를 배치한다.
    - 기존 `components` 폴더와 기능적으로 다른 것은 없으며 단순 분류의 의미로 구성된다.
- 라우팅 기본
    - `index.js`에 라우터 관련 설정을 작성한다. (주소, 이름, 컴포넌트)
    - `RouterLink`의 ‘to’ 속성으로 `index.js`에서 정의한 주소 속성 값(path)을 사용한다.
        
        ```jsx
        router/index.js
        
        const router = createRouter({
          routes: [
        	{
         	  path: '/',
        	  name: 'home',
        	  component: HomeView    // router에 URL과 컴포넌트를 매핑
        	},
        	{ ... }, 
        	...
          ]
        })
        ```
        
        ```jsx
        <!-- App.vue -->
        
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/About">About</RouterLink>
        ```
        

- Named Routes
    - 경로에 이름을 지정하는 라우팅이다.
- Named Routes 예시
    - name 속성 값에 경로에 대한 이름을 지정한다.
    - 경로에 연결하려면 `RouterLink`에 `v-bind`를 사용해 ‘to’ prop 객체로 전달이 가능하다.
        
        ```jsx
        index.js
        
        const router = createRouter({
          routes: [
            {
        	  path: '/',
        	  name: 'home',
        	  component: HomeView
        	},
        	{ ... },
        	...
          ]
        })
        ```
        
        ```jsx
        <!-- App.vue -->
        
        <RouterLink :to="{ name: 'home' }">Home</RouterLink>
        <RouterLink :to="{ name: 'about' }">About</RouterLink>
        ```
        
- Named Routes의 장점
    - 하드 코딩 된 URL을 사용하지 않아도 된다.
    - URL 입력 시 오타를 방지한다.

- 매개변수를 사용한 동적 경로 매칭
    - 주어진 패턴 경로를 동일한 컴포넌트에 매핑해야 하는 경우에 활용한다.
    - 예를 들어, 모든 사용자의 ID를 활용하여 프로필 페이지 URL을 설계한다면
        
        ```jsx
        user/1
        user/2
        user/3
        처럼 일정한 패턴의 URL 작성을 반복해야 한다.
        ```
        
- 매개변수를 사용한 동적 경로 매칭 활용
    - `UserView` 컴포넌트 작성
        
        ```jsx
        <!-- UserView.vue -->
        
        <template>
          <div>
        	<h1>UserView</h1>
        </div>
        </template>
        ```
        
    - `UserView` 컴포넌트 라우트 등록
    - 매개변수는 콜론`(:)`으로 표기
        
        ```jsx
        index.js
        
        import UserView from '../views/UserView.vue'
        
        const router = createRouter({
          routes: [
        	{
          	  path: '/user/:id',
        	  name: 'user',
        	  component: UserView
        	},
        	{ ... },
        	...
          ]
        })
        ```
        
    
    - 라우트의 매개변수는 컴포넌트에서 `$route.params`로 참조 가능하다.
        
        ```jsx
        <!-- UserView.vue -->
        
        <template>
          <div>
        	<h1>UserView</h1>
        	<h2>{{ $route.params.id }}번 User 페이지</h2>
          </div>
        </template>
        ```
        
    
    - 하지만 다음과 같이 Composition API 방식으로 작성하는 것이 권장된다.
        
        ```jsx
        <!-- UserView.vue -->
        
        import { ref } from 'vue'
        import { useRoute } from 'vue-router'
        
        const route = useRoute()
        const userId = ref(route.params.id)
        
        <template>
          <div>
        	<h1>UserView</h1>
        	<h2>{{ userId }}번 User 페이지</h2>
          </div>
        </template>
        ```
        
- 프로그래밍 방식 네비게이션
    - `router`의 인스턴스 메소드를 사용해 `RouterLink`로 a 태그를 만드는 것처럼 프로그래밍으로 네비게이션 관련 작업을 수행할 수 있다.
        - 다른 위치로 이동하기 → `router.push()`
        - 현재 위치 바꾸기 → `router.replace()`
- `router.push()`
    - 다른 URL로 이동하는 메소드이다.
    - 새 항목을 history stack에 `push`하므로 사용자가 브라우저 뒤로 가기 버튼을 클릭하면 이전 URL로 이동이 가능하다.
    - `RouterLink`를 클릭했을 때 내부적으로 호출되는 메소드이므로 `RouterLink`를 클릭하는 것은 `router.push()`를 호출하는 것과 같다.
    
    ![Untitled](./images/Vue%20Router/Untitled%201.png)
    
- `router.push()` 활용
    - UserView 컴포넌트에서 HomeView 컴포넌트로 이동하는 버튼 만들기
        
        ```jsx
        <!-- UserView.vue -->
        
        import { useRoute, useRouter } from 'vue-router'
        
        const router = useRouter()
        
        const goHome = function() {
          router.push({ name: 'home' })
        }
        
        <button v-on:click="goHome">홈으로</button>
        ```
        
- `router.push()`인자 활용 참고
    
    ```jsx
    router.push('/users/alice')
    router.push({ path: '/users/alice' })
    router.push({ name: 'user', params: {username: 'alice' } })
    router.push({ path: '/register', query: { plan: 'private' } })
    ```
    
- `router.replace()`
    - 현재의 위치를 바꾸는 메소드이다.
    - `push`메소드와 달리 history stack에 새로운 항목을 push하지 않고 다른 URL로 이동한다. → 이동 전 URL로 뒤로 가기가 불가능
    
    ![Untitled](./images/Vue%20Router/Untitled%202.png)
