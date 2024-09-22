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
