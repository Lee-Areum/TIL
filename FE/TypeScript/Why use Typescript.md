- TS는 MS에 의해 개발/관리되는 오픈소스 프로그래밍 언어
- 대규모 애플리케이션 개발하는 데 JS는 불편함.

  ## TypeScript를 사용하는 이유
  - 에러의 사전 방지
  - 코드 가이드 및 자동완성(개발 생산성 향성)
 
    

-------
# TypeScript 공식문서


## Javascript는 예기치 않은 동작을 유발함
- Javascript는 빠르게 사용될 수 있고 짧은 줄의 어플리케이션을 작성하기 위해 만들어진 도구다.
- javascript는 여러 문제점을 가지고 있다.
  - 동일 연산자(==)는 예기치 않은 동작을 유발한다.
     ```
    if ("" == 0){
       // 참
    }
    if (1<x<3) {
       // 어떤 x 값이던 참
    }
    ```    
  - 존재하지 않는 프로퍼티의 접근을 허용함
    ```
    const obj = {width:10, height : 15 };
    const area = obj.width * obj.heigth; //NaN을 출력
    ```
  => 대부분의 프로그래밍 언어는 이런 종류의 오류가 발생하면 오류를 표출하고 일부는 컴파일 중에 오류를 표출함.  
    규모가 큰 수천 줄의 어플리케이션을 개발할 때 이런 오류는 심각한 문제임.

  ## TypeScript 정적 타입 검사자
  - **정적 검사** : 프로그램을 실행시키지 않으면서 코드의 오류를 검출하는 것
      - typescript는 프로그램을 실행시키기 전에 값의 종류를 기반으로 프로그램의 오류를 찾음
        ```
        // @errors:> 2551
        const obj = { width: 10, height: 15 };
        const area = obj.width * obj.heigth;
        ```
  - **타입이 있는 JavaScript의 상위 집합**
  - TypeScript는 JS의 구문이 허용되는 JavaScript의 상위 집합 언어  
    => JavaScript 코드를 TypeScript 파일에 넣어도 잘 작동함
    
    ![image](https://github.com/user-attachments/assets/694bb439-ac30-466d-8ce8-f250b906789b)


  - TypeScript는 다른 종류의 값들을 사용할 수 있는 방법이 추가된, 타입이 있는 상위 집합
    ```
    console.log(4/[]);
     // JS : NaN를 출력 (NaN : 정의되지 않은 값 또는 표현할 수 없는 값)
     // TS : @errors: 2363
    ```

    ## TS의 특징
    - 런타임 특성(Runtime Behavior)
      TypeScript는 JavaScript의 런타임 특성을 가진 프로그래밍 언어  
      코드에 타입오류가 있음을 검출해도 JS 코드를 TS로 변환하여 같은 방식으로 실행시킬 것을 보장함
      
    - 삭제된 타입(Erased Types)
      TS 컴파일러가 코드 검사를 마치면 타입을 삭제해서 TS가 추론한 타입에 따라 프로그램의 특성을 변화시키지 않음.
      TS는 JS 프로그램과 같은 표준 라이브러리를 사용, 별도 프레임워크를 공부할 필요 없음.

    => TS는 JS와 구문과 런타임 특성을 공유함.
