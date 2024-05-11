# Navigation Guard

- Navigation Guard
    - Vue router를 통해 특정 URL에 접근할 때, 다른 URL로 redirect를 하거나 취소하여 네비게이션을 보호한다.
    - ex) 인증 정보가 없으면 특정 페이지에 접근하지 못하게 함

- Navigation Guard의 종류
    - Globally (전역 가드)
        - 어플리케이션의 전역에서 동작한다.
        - `index.js`에서 정의한다.
    - Per-route (라우터 가드)
        - 특정 `route`에서만 동작한다.
        - `index.js`의 각 routes에 정의된다.
    - In-component (컴포넌트 가드)
        - 특정 컴포넌트 내에서만 동작한다.
        - 컴포넌트 script에 정의된다.

- `router.beforeEach()`
    - 다른 URL로 이동하기 직전에 실행되는 함수이다.

- `router.beforeEach()` 구조
    
    ```jsx
    router.beforeEach((to, from) => {
    	...
    	return false
    })
    ```
    
    - `to` : 이동할 URL 정보가 담긴 Route 객체
    - `from` : 현재 URL 정보가 담긴 Route 객체
    - 선택적 반환 값
        - false
            
            ```jsx
            router.beforeEach((to, from) => {
            	...
            	return false
            })
            ```
            
            - 현재 네비게이션을 취소한다.
            - 브라우저 URL이 변경된 경우 (사용자가 수동으로 또는 뒤로 버튼을 통해) from 경로의 URL로 재설정한다.
        - Route Location
            
            ```jsx
            router.beforeEach((to, from) => {
            	...
            	return { name: 'About' }
            })
            ```
            
            - `router.push()`를 호출하는 것처럼 경로 위치를 전달하여 다른 위치로 redirect한다.

- `router.beforeEach()` 예시
    - 전역 가드 beforeEach 작성
    - `HomeView`에서 `UserView`로 이동 후 각 인자 값 출력 확인하기
        
        ```jsx
        index.js
        
        import UserView from '../views/UserView.vue'
        
        const router = createRouter({
        	routes: [
        		{
        			path: '/user/:id',
        			name: 'user',
        			component: UserView,
        		},
        		{ ... },
        		...
        	]
        })
        ```
        
    - `to`에는 이동할 URL인 user 라우트에 대한 정보가,
    - `from`에는 현재 URL인 home 라우트에 대한 정보가 들어 있다.
        
        ```jsx
        to >> {fullPath: '/user/1', hash: '', query: {...}, name: 'user', path: '/user/1', ...}
        from >> {fullPath: '/', path: '/', query: {...}, hash: '', name: 'home', ...}
        ```
        

- `router.beforeEach()` 활용
    - `LoginView` 컴포넌트 작성 및 라우트 등록
        
        
        ```jsx
        <!-- LoginView.vue -->
        
        <template>
        	<div>
        		<h1>Login View</h1>
        	</div>
        </template>
        ```
        
        ```jsx
        index.js
        
        {
        	path: '/login',
        	name: 'login',
        	component: LoginView
        }
        ```
        
        ```jsx
        App.vue
        
        <RouterLonk :to="{ name: 'login' }">Login</RouterLink>
        ```
        
    - 만약 로그인이 되어있지 않고, 이동하는 주소 이름이 login이 아니라면 login 페이지로 redirect
        
        ```jsx
        index.js
        
        router.beforeEach((to, from) => {
        	const isAuthenticated = false
        		
        	if(!isAuthenticated && to.name !== 'login') {
        		console.log('로그인이 필요합니다.')
        		return { name: 'login' }
        	}
        })
        ```
        
    - Login이 되어있지 않다면 페이지 진입을 막고 Login 페이지로 이동시키기
    - 어떤 `RouterLink`를 클릭해서 `LoginView` 컴포넌트만 볼 수 있다.

- `router.beforeEnter()`
    - route에 진입했을 때만 실행되는 함수
    - 매개변수, 쿼리 값이 변경될 때는 실행되지 않고 다른 경로에서 탐색할 때만 실행된다.

- `router.beforeEnter()` 구조
    
    ```jsx
    {
    	path: '/user/:id',
    	name: 'user',
    	component: UserView,
    	beforeEnter: (to, from) => {
    		...,
    		return false
    	}
    })
    ```
    
    - routes 객체에서 정의한다.
    - 함수의 `to`, `from`, 선택 반환 인자는 `beforeEach`와 동일하다.

- `router.beforeEnter()` 예시
    - 라우터 가드 beforeEnter를 작성한다.
    - `HomeView`에서 `UserView`로 이동 후 각 인자 값 출력을 확인한다.
        
        ```jsx
        index.js
        
        {
        	path: '/user/:id',
        	name: 'user',
        	component: UserView,
        	beforeEnter: (to, from) => {
        		console.log(to)
        		console.log(from)
        	}
        }
        ```
        
    - to에는 이동할 URL인 user 라우트에 대한 정보가, from에는 현재 URL인 home 라우트에 대한 정보가 들어있다.
    - 다른 경로에서 user라우트를 탐색했을 때 실행된 것이다.
        
        ```jsx
        to >> {fullPath: '/user/1', hash: '', query: {...}, name: 'user', path: '/user/1', ...}
        from >> {fullPath: '/', name: undefined, params: {...}, query: {...}, hash: '', ...}
        ```
        

- `router.beforeEnter()` 활용
    - 이미 로그인 한 상태라면 `LoginView` 진입을 막고 `HomeView`로 이동시키기 (전역 가드 활용 코드는 주석처리 후 진행)
        - 이미 로그인 한 상태라면 `HomeView`로 이동
        - 로그인 상태가 아니라면 `LoginView`로 이동
        
        ```jsx
        index.js
        
        const isAuthenticated = true
        
        const router = createRouter({
        	routes: [
        		{
        			path: '/login',
        			name: 'login',
        			component: LoginView,
        			beforeEnter: (to, from) => {
        				if (isAuthenticated === true) {
        						console.log('이미 로그인 상태입니다.')
        						return {name: 'home'}
        				}
        			}
        		},
        		{...},
        		...
        	]
        })
        ```