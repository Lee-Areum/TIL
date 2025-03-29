https://ytlive.tistory.com/357

### Command 패턴

- 행동 패턴(Behavioral Pattern) 중 하나로 객체가 자신의 역할을 깔끔하게 수행할 수 있도록 도움
- 기능을 캡슐화 해서 작업(행동)을 규격화 하는 것

### 왜 사용하는가?

어떤 객체 A에서 객체 B의 메서드를 실행할 경우 객체 B에 대해 의존성이 발생한다.

B의 기능이 수정될 경우 A도 수정이 필요할 수 있다.

하지만 커맨트 패턴을 사용하면 B에 대한 의존성을 제거할 수 있다.

### 예시

만약 그림판 기능을 만든다고 생각해보겠다.

그림을 그리는 사용자 User 클래스, 그림판 Board 클래스, 연필 Pencil 클래스가 있다고 한다.

이 코드를 작성하면 아래와 같을 것이다.

```jsx
public class Pencil {
	public void drawPencil() {
		System.out.println("----");
	}
}
```

```jsx
public class Board {
	private Pencil pencil;
	
	public Board(Pencil pencil){
		this.pencil = pencil;
	}
	
	public void clickButton(){
		pencil.drawPencil();
	}
}
```

```jsx
public class User {
	public static void main(String args[]){
		Pencil pencil = new Pencil();
		Board board = new Board(pencil);
		board.draw();
	}
}
```

이 코드를 실행하면 연필 선이 그려질 것이다.

그런데 여기서 지우개 기능이 추가되면 어떻게 될까

새로운 Eraser 클래스를 생성한다.

```jsx
public class Eraser {
	public void eraseLine(){
		System.out.println("--Erase--\n\n\n\n\n\n\n\n\n\n");
	}
}
```

```jsx
public class Pencil {
	public void drawPencil() {
		System.out.println("----");
	}
}
```

```jsx
public class Board {
	private static String[] MODE = {"pencil", "eraser"};

	private Pencil pencil;
	private Eraser eraser;
	private String mode;
	
	// 생성자의 파라미터가 추가되었다.
	public Board(Pencil pencil, Eraser eraser){
		this.pencil = pencil;
		this.eraser = eraser;
	}
	
	// 모드가 추가되었다.
	public void setMode(int index){
		this.mode = MODE[index];
	}
	
	public void clickButton(){
		switch(this.mode){
			case "pencil": 
				pencil.drawPencil();
				break;
			case "eraser":
				eraser.eraseLine();
				break;
		}
}
```

```jsx
public class User {
	public static void main(String args[]){
		Pencil pencil = new Pencil();
		Eraser eraser = new Eraser ();
		Board board = new Board(pencil, eraser);
		
		// 선그리기
		board.setMode(0);
		board.clickButton();
		
		// 선지우기
		board.setMode(1);
		board.clickButton();
	}
}
```

이렇게 기능이 추가될 때마다 객체가 추가되고 Board 클래스와 User 클래스가 수정될 것이다.

### 커맨드 패턴 적용하기

![image.png](attachment:f33141e3-7e24-4b7e-962d-930c389100d0:image.png)

먼저 Board의 기능을 캡슐화 하여 클래스화 한다.

그리고 Command 인터페이스에서 해당 클래스를 호출하도록 한다.

![image.png](attachment:1b9ac2fe-3733-454e-b72a-dcb42ded1096:image.png)

```jsx

public interface Command {
	public void run();
}
```

```jsx
public class Pencil {
	public void drawPencil() {
		System.out.println("----");
	}
}
```

```jsx
public class DrawOnCommand implements Command {
	private Eraser eraser;
	
	// eraser 기능
	public DrawOnCommand(Eraser eraser){
		this.eraser = eraser;
	}
	
	@Override
	public void run(){
		eraser.drawPencil();
	}
}
```

```jsx
public class Eraser {
	public void eraseLine(){
		System.out.println("--Erase--\n\n\n\n\n\n\n\n\n\n");
	}
}
```

```jsx
public class EraseOnCommand implements Command {
	private Eraser eraser;
	
	// eraser 기능
	public EraseOnCommand(Eraser eraser){
		this.eraser = eraser;
	}
	
	@Override
	public void run(){
		eraser.eraseLine();
	}
}
```

이렇게 기능들을 클래스로 정의하고 Board의 clickButton 함수에서는 Command 인터페이스의 run 메서드를 호출하도록 한다.

```jsx
public class Board {
	private Command command;
	
	public void setCommand(Command command){
		this.command = command;
	}
	
	public void clickButton(){
		command.run();
	}
}
```

```jsx
public class User {
	public static void main(String args[]){
		Pencil pencil = new Pencil();
		Eraser eraser = new Eraser ();
		
    Command drawOnCommand= new DrawOnCommand(pencil);
    Command eraseOnCommand= new EraseOnCommand(eraser);
		
		Board board = new Board();
		
		// 선그리기
		board.setCommand(drawOnCommand);
		board.clickButton();
		
		// 선지우기
		board.setCommand(eraseOnCommand);
		board.clickButton();
	}
}
```

기존 코드에서 Command, DrawOnCommand, EraseOnCommand 클래스가 추가되었지만

Board 클래스의 구조는 간단해지고 기능이 추가되어도 Board 클래스가 추가되지 않아도 된다.

Before

```jsx
public class Board {
	private static String[] MODE = {"pencil", "eraser"};

	private Pencil pencil;
	private Eraser eraser;
	private String mode;
	
	// 생성자의 파라미터가 추가되었다.
	public Board(Pencil pencil, Eraser eraser){
		this.pencil = pencil;
		this.eraser = eraser;
	}
	
	// 모드가 추가되었다.
	public void setMode(int index){
		this.mode = MODE[index];
	}
	
	public void clickButton(){
		switch(this.mode){
			case "pencil": 
				pencil.drawPencil();
				break;
			case "eraser":
				eraser.eraseLine();
				break;
		}
}
```

After

```jsx
public class Board {
	private Command command;
	
	public void setCommand(Command command){
		this.command = command;
	}
	
	public void clickButton(){
		command.run();
	}
}
```

![image.png](attachment:7b44fa91-d11f-49b5-9b40-bd93e1d0da0a:image.png)

### Command Pattern의 구조

![image.png](attachment:f33141e3-7e24-4b7e-962d-930c389100d0:image.png)

- Command : execute() 메서드를 선언하여 Receiver에서 수행할 연산을 구체화 하는 방법을 정의하는 인터페이스
    - 예시의 Command 클래스
- ConcreteCommand : Command 인터페이스를 구현하고 이를 통해 Receiver 클래스의 함수를 호출하는 클래스
    - 예시의 EraseOnCommand, DrawOnCommand
- Invoker : 사용자의 요청을 Command 객체로 변환하여, 이 객체를 저장하고 실행함
    - 예시의 clickButton 함수
- Receiver : 요청을 수행하는데 필요한 실제 작업을 구현한 클래스
    - 예시의 Eraser, Pencil 클래스
