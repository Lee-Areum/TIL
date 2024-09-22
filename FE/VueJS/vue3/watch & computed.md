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
