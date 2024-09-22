## ref와 reactive
### ref(Reactive Reference)
```
<template>
  <div class="name">{{text}}</div>
</template>

<script>
  import {ref} from "vue";

export default {
  const text = ref("Name");
}
</script>
```
  - .value 속성에 새값을 할당할 수 있음 / .value로 변화하는 값을 즉시 사용할 수 있고 즉시 변화시킬 수 있음
  - 주로 원시타입을 사용, Object 타입도 사용가능 (Arrya, Object...)
  - Object 타입으로 ref를 할당하면 reactive()로 내부 깊숙히 반응함.  

</br>

  - ref는 스크립트 내부에서는 래핑(.value로 사용)되고 template 내부에서는 언래핑(ref변수명을 그대로 사용)됨.  
    
</br>

  - 컴포넌트 레퍼런스 제공할 수 있음

</br>
    
### reactive
```
<template>
  <div class="name">{{text.name}}</div>
</template>

<script>
  import {reactuve} from "vue";

export default {
  const text = reactuve({
    name : 'test'
  });
}
</script>
```
  - reactive 함수를 사용하면 객체의 프로퍼티 변경시 Vue가 자동으로 변경 사항을 감지하고 뷰를 업데이트함.  
이전 버전의 observable 함수와 같은 기능
    
</br>

  - ref와 달리 원시타입 사용할 수 없음. object 타입만 사용가능, 함수를 넣을 수도 있음.
  - ref와 달리 .value 사용하지 않아도 됨.
  - object 타입을 사용할 경우 ref는 재할당하면 반응형을 유지하지만 reactive는 재할당시 반응형을 잃어버림
    
    ```
    let refObj = ref({ count: 0 });
    let reactObj = reactive({ count: 0 })

    const newObj = {name : 'test'};

    refObj.value = { count: 1 };
    reactObj = reactive({ count: 1 })

    console.log(refObj); //refImpl
    console.log(reactObj); // 반응성 연결이 끊어짐   
    ```

</br>

-----

</br>


## watch 와 computed
### computed
```
const now = computed(() => Date.now())
```
- computed는 종속대상을 caching하는 특성이 있음 -> 종속하는 대상이 변경되지 않으면 다시 실행하여 계산하지 않음.
- 기본적으로 읽기 전용, 새값을 할당하려고하면 에러 / 수정이 필요하면 getter, setter를 사용
    ```
    <script setup>
      import { ref, computed } from 'vue'
      
      const firstName = ref('John')
      const lastName = ref('Doe')
      
      const fullName = computed({
        // getter
        get() {
          return firstName.value + ' ' + lastName.value
        },
        // setter
        set(newValue) {
          // 참고: 분해 할당 문법을 사용함.
          [firstName.value, lastName.value] = newValue.split(' ')
        }
      })
    </script>
    ```

### watch
```
const x = ref(0)
const y = ref(0)

// 단일 ref
watch(x, (newX) => {
  console.log(`x는 ${newX}입니다`)
})

// getter
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`x와 y의 합은: ${sum}입니다`)
  }
)

// 여러 소스의 배열
watch([x, () => y.value], ([newX, newY]) => {
  console.log(`x는 ${newX}이고 y는 ${newY}입니다`)
})
```
- 상태 변화에 따른 부수효과를 처리할 때 사용
- ex) 데이터 패칭, DOM 조작, API 호출, toast 등 부수 효과
