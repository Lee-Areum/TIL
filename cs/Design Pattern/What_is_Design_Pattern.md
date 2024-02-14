## Design Pattern
일종의 설계 기법, 설계 방법

- 목적 : 재사용성, 호환성, 유지 보수성  
- 추후 재사용, 호환, 유지 보수시 발생하는 문제해결을 예방하기 위해 만들어 둔 아이디어

<br>   

### 원칙
#### SOLID (객체지향 설계 원칙)
1. **S**ingle Responsibility Principle  
하나의 클래스는 하나의 역할을 가져야 한다

2. **O**pen-Close Principle  
확장(상속)에는 열려있고 수정에는 닫혀있어야 한다.

3. **L**iskov Substitution Principle  
자식이 부모의 자리에 항상 교체될 수 있어야 한다.

4. **I**nterface Segregation Principle  
인터페이스가 잘 분리되어 클래스가 꼭 필요한 인터페이스만 구현하도록 해야한다.

5. **D**ependency Inversion Property  
상위 모듈이 하위 모듈에 의존하면 안된다  
상위, 하위 모두 추상화에 의존하며, 추상화는 세부 사항에 의존하면 안된다.

<br>

### 분류
1. 생성 패턴(Creational) : 객체의 생성 방식 결정
```
ex) DBConnection을 관리하는 interface를 하나만 만들 수 있도록 제한하여, 불필요한 연결을 막음
```
<br>

2. 구조 패턴(Structural) : 객체간의 관계를 조직
```
ex) 2개의 인터페이스가 서로 호환이 되지 않을 때, 둘을 연결해주기 위해 새로운 클래스를 만들어서 연결시킬 수 있도록 함
```  
<br>

3. 행위 패턴(Behavioral) : 객체의 행위를 조직, 관리, 연합
```
ex) 하위 클래스에서 구현해야 하는 함수 및 알고리즘을 미리 선언하여, 상속시 이를 필수로 구현하도록 함
```  
