# Single-File Components

- Component
    - 재사용 가능한 코드 블록

- Component 특징
    - UI를 독립적이고 재사용 가능한 일부분으로 분할하고 각 부분을 개별적으로 다룰 수 있다.
    - 그러면 앱은 자연스럽게 중첩된 Component의 트리로 구성된다.
        
        ![Untitled](/images/Single-File%20Components%20b86a3291379442728554833e2669a767/Untitled.png)
        
- Single-File Components (SFC)
    - 컴포넌트의 템플릿, 로직 및 스타일을 하나의 파일로 묶어낸 특수한 파일 형식(*.vue 파일)

- SFC 파일 예시
    - Vue SFC는 HTML, CSS 및 JavaScript 3개를 하나로 합친 것이다.
    - `<template>`, `<script>` 및 `<style>` 블록은 하나의 파일에서 컴포넌트의 뷰, 로직 및 스타일을캡슐화하고 배치한다.
        
        ```jsx
        <!-- MyComponent.vue -->
        
        <template>
        	<div class="greeting"> << msg >> </div>
        </template>
        
        <script setup>
        import { ref } from 'vue'
        const msg = ref('Hello World!')
        </script>
        
        <style scoped>
        	.greeting {
        		color: red;
        }
        </style>
        ```
        
- SFC 문법 개요
    - 각 *.vue 파일은 세 가지 유형의 최상위 언어 블록 `<template>`, `<script>`, `<style>`으로 구성된다.
    - 언어 블록의 작성 순서는 상관없으나 일반적으로 `template` → `script` → `style` 순서로 작성한다.
        
        ```jsx
        <template>
        	<div class="greeting"> << msg >> </div>
        </template>
        
        <script setup>
        import { ref } from 'vue'
        const msg = ref('Hello World!')
        </script>
        
        <style scoped>
        	.greeting {
        		color: red:
        	}
        </style>
        ```
        

- 언어 블록
    - `<template>` : 각 *.vue 파일은 최상위 `<template>`블록을 하나만 포함할 수 있다.
    - `<script setup>` : 각 *.vue 파일은 하나의 `<script setup>` 블록만 포함할 수 있다. (일반 `<script>`는 제외)
    - 컴포넌트의 `setup()` 함수로 사용되며 컴포넌트의 각 인스턴스에 대해 실행된다.
    - `<style scoped>` : *.vue 파일은 여러 `<style>` 태그가 포함될 수 있다.
    - `scoped`가 지정되면 CSS는 현재 컴포넌트에만 적용된다.