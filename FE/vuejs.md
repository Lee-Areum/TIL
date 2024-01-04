### $nextTick()

데이터 갱신 후 UI까지 완료하고 수행

async await와 함께 사용 가능

</br>

## Ref 사용방법

자식 엘리먼트에 접근하는 기능

- 접근 대상에 ref 속성 명시
- 사용 대상을 this.refs로 접근

ref 속성은 컴포넌트가 랜더링 된 이후 적용 → 반응형으로 구성하기 위한 computed나 template에서 사용하면 안됨
- ref를 사용해서 자식의 데이터나 함수에 접근할 수 있음


</br>

## VueJS 생명주기

### 생성단계 : beforeCreate() ⇒ data() ⇒ create()

beforeCreate() : 아직 컴포넌트가 DOM에 추가되기 전 → Dom에 접근하면 에러

created() : data()변수와 events 메서드가 활성화됨
메서드나 변수를 접근해도 에러가 발생하지 않지만 템플릿과 가상 DOM에는 접근할 수 없다.

### 장착 단계 : beforeMount() ⇒ mounted()

**beforeMount()** : <template> 태그가 실행된 후 실행

**mounted()** : 템플릿과 렌더링 된 돔에 접근할 수 있는 단계

- 자식 컴포넌트가 부모 컴포넌트보다 먼저 Mounted가 실행됨

### 수정 단계 : beforeUpdate() ⇒ update()

**beforeUpdate()** : Dom이 제 렌더링 되고 패치되기 직전에 실행
재 렌더링 전의 “새 상태의 데이터”를 얻을 수 있음 
여기서 값을 변경해도 재 랜더링 되지 않음

**update()** : 제 랜더링이 일어난 후 실행
DOM 업데이트가 완료된 상태, 연산과 기능이 가능
여기서 값을 변경하면 무한루프에 빠질 수 있음

### 소멸 단계 : beforeDestroy() ⇒ destroyed()

**beforeDestroy()** : 소멸(뷰 컴포넌트 제거) 직전에 호출

이벤트와 같은 부분을 제거

**destroyed()** : 소멸된 후에 호출

vue의 모든 디렉티브(v-)가 바인딩 헤제, 모든 이벤트 리스터가 제거, 모든 하위 Vue 컴포넌트도 삭제

![img.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/00bc3837-8fa7-44fb-b571-e1e2f35d779b/img.png)

</br>


## vue export default 속성들

### computed

- 대상의 변경이 일어나야 호출

템플릿의 데이터 표현을 더 직관적이고 간결하게 도와주는 속성

템플릿에서 사용할만한 복잡한 로직을 정의

- 컴퓨티드 속성은 인자를 받지 않음
- HTTP통신과 같이 컴퓨팅 리소스가 많이 필요한 로직을 정의하지 않아야함
- getter, setter 사용가능
- 원래는 set이 되지 않지만 set()을 작성하면 set처럼 사용할 수 있음

→ Computed는 계산되어 있는 결과를 그대로 반환한다.

⇒ 캐시를 사용하지 않아도 되는 경우 : method
캐시를 사용해야 하는 경우 : computed

### watch

1, 데이터를 업데이트 할 때 비동기처리나 무거운 처리(많은 처리)를 실행하고 싶은 경우

1. 감시할 데이터를 지정하고 그 데이터가 바뀌면 선언한 함수를 실행하라는 방식
computed속성은 계산해야 하는 목표 데이터를 정의하는 방식

### method

렌더링이 일어날 대마다 항사 함수 실행

### $nextTick()

모든 데이터의 업데이트나 랜더링이 끝난 후 DOM에 접근하는 함수

```jsx
created: function(){
	var self = this;

	for(var i = 0;i<100;i++){
		this._data.list.push(i);
	}

	this.$nextTick(function(){
		var dom = document.getElementById('item-0');
		dom.style.backgroundColor = 'red';
	});
}
```
