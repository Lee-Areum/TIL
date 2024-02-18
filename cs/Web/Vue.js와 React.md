### 개발 CLI
- Vue.js : vue-cli
- React : create-react-app

<br>

**명령 줄 인터페이스(CLI, Command line Interface)** - 개발환경 설정도구  
텍스트 터미널을 통해 사용자와 컴퓨터가 상호 작용하는 방식  
사용자가 컴퓨터 키보드 등을 통해 문자열의 형태로 입력하며, 컴퓨터로부터 출력 역시 문자열의 형태로 주어짐  

cli를 통해 프로젝트 생성시 설정할 수 있는 옵션들을 함께 셋업하고 빠르게 프로젝트를 구성할 수 있음  

<br>

### CSS 파일 존재 유무
- Vue.js : 없음, style이 실제 컴포넌트 파일 안에서 정의  
(css 파일 별도 생성 가능)
- React : 파일이 존재, 해당 타일을 통해 style 적용

<br>

### 데이터 변이
- Vue.js : 반드시 데이터 객체를 생성한 이후 data를 업데이트 할 수 있음
- React : state 객체를 만들고 업데이트에 조금 더 작업이 필요

```
// name : kim 값을 lee로 변경
Vue.js : this.name = 'lee'
React : this.setState({name : 'lee'})
```
Vue에서는 data를 업데이트할 때마다 setState를 알아서 결합한다.
