# Axios

- Axios
    - 브라우저와 node.js에서 사용할 수 있는 Promise 기반 HTTP 클라이언트 라이브러리이다.
    - Vue에서 권고하는 HTTP 통신 라이브러리이다.
    - 특징
        - 브라우저를 위해 `XMLHttpRequests`를 생성한다.
        - node.js를 위해 http 요청을 생성한다.
        - Promise API를 지원한다.
        - 요청 및 응답 인터셉트
            - 요청을 보내기 전이나 서버로부터 응답을 받은 후에 데이터를 가공하거나 추가적인 작업을 수행할 수 있도록 인터셉터를 제공한다.
        - 요청 및 응답 데이터 변환
        - 요청 취소
        - JSON 데이터 자동 변환
            - 서버로부터 받은 응답 데이터를 자동으로 JSON 형태로 변환해준다. 이를 통해 데이터를 즉시 처리할 수 있다.
        - XSRF를 막기 위한 클라이언트 사이트 지원

- Axios 설치
    - npm 사용하기
        
        ```jsx
        $ npm install axios
        ```
        
    - unpkg CDN 사용하기
        
        ```jsx
        <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
        ```
        

- Axios  API
    - `axios(config)`
    - `axios(url[, config])`
    - `axios.request(config)`
    - `axios.get(url[, config])`
    - `axios.delete(url[, config])`
    - `axios.head(url[, config])`
    - `axios.options(url[, config])`
    - `axios.post(url[, data[, config]])`
    - `axios.put(url[, data[, config]])`
    - `axios.patch(url[, data[, config]])`