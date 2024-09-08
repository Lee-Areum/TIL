# 타입스크립트 기본 타입  
- 기본 타입 12개
  - string / number / boolean / object / array / tuple / enum / null / undefined / any / void / never

### 배열(array)
```
int arr: number[] = [1,2,3];
int arr: Array<number> = [1,2,3];
```

### 튜플(tubple)
- 배열의 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열의 형식
```
let arr: [string, number] = ['hi',10];
```
- 정의 하지 않은 타입, 인덱스로 접근할 경우 오류가 남.

## 이넘(enum)
- 특정 값(상수)들의 집합
```
enum Avengers {
  Capt,
  IronMan,
  Thor
}
let hero : Avengers = Avengers.Capt;
```
- 이넘은 인덱스 번호로도 접근 가능
- 이넘의 인덱스를 사용자 편의로 변경하여 사용할 수도 있음.
  ```
  enum Avengers {
    Capt = 2,
    IronMan,
    Thor
  }
  let hero : Avengers = Avengers[2]; // Capt
  let hero : Avengers = Avengers[4]; // Thor
  ```
## any
- 모든 타입에 사용할 수 있는 치트키 같은 타입
- 특정 데이터의 타입을 잘 모르거나 자바스크립트 프로젝트에 타입스크립트를 점진적으로 적용할 떄 사용
```
let str: any = 'hi';
let num: any = 10;
let arr: any = ['a', 2, true];
```
- 단, any 타입을 자주 사용하면 타입스크립트의 장점이 사라짐.

## Void
- 반환값이 없는 함수의 반환 타입
```
function printSomething():void{
  return;
}
```

### never
- 절대 발생하지 않는 값을 의미
- 함수가 반복문이나 에러 핸들링으로 함수의 끝에 절대 도달하지 않는 경우 사용
```
function neverEnd(): never{
  throw new Error('unexpected');
}
```
