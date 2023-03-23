## Nuxt.js
vuejs + library

### import 경로

~ 보다는 @사용

### html 선택시

- 우선적으로 refs를 사용하여 이벤트 처리
- 다만 for과 같이 동적으로 생성하는 dom은 refs 사용불가하기 때문에
querySelector나 nextTick을 사용함
- refs > getElementById  > querySelector > jquery
