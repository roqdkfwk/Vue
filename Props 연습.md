# Props 연습

- Passring Props 연습
    
    
    ```jsx
    <!-- App.vue -->
    
    <template>
      <div>
        <h1>쇼핑 애플리케이션</h1>
        <ProductList v-bind: receives="send" />
        <ProductList v-bind: 전달변수명="변수명"
      </div>
    </template>
    
    <script setup>
    import { ref } from 'vue'
    import ProductList from './components/ProductList.vue';
    
    let id = 0
    const 변수명
    const send = ref([
      { id: id++, name: '사과', price: 1000 },
      { id: id++, name: '바나나', price: 1500 },
      { id: id++, name: '딸기', price: 2000 },
      { id: id++, name: '포도', price: 3000 },
      { id: id++, name: '복숭아', price: 2000 },
      { id: id++, name: '수박', price: 5000 }
    ])
    </script>
    
    ```
    
    ```jsx
    <!-- ProductList.vue -->
    
    <template>
        <div>
            <ul>
    	          <li v-for="product in 전달된변수명" : key="product.id">
                <li v-for="product in receives" :key="product.id">
                    {{ product.name }} - {{ product.price }}원
                </li>
            </ul>
        </div>
    </template>
    
    <script setup>
    import { defineProps } from 'vue'
    
    defineProps({
        전달된변수명: 변수타입
        receives: Array
    })
    </script>
    
    <style scoped>
    
    </style>
    ```