## .bat 파일
배치 파일을 만드는 확장자

## 배치 파일
사용자의 개입없이 일정시간 혹은 조건으로 동작하는 프로그램
- 주로 사용자가 적은 시간에 데이터를 백업하거나 다른 프로그램으로 전송하는 등의 일을 한다.

## bat 명령어
- @ECHO OFF  
bat 파일의 시작
내부 명령어를 출력하지 않고 결과만 출력

- ECHO  
print 기능
pause를 같이 사용해야 출력후 정지함

- @REM  
주석

- SET /a  
수식 할당
%변수%를 사용하지 않고 변수를 사용할 수 있음

- SET /p  
사용자가 입력한 값을 할당

- for /f \[option] $$parameter in \[filename] do \[command]  
for문을 돌면서 filename의 값을 option을 기준으로 잘라서 parameter에 넣음

- xcopy a b
  a의 폴더 상태 그대로 b로 복사

- \> : 덮어쓰기

- SETLOCAL enabledelayed expension  
설정한 환경변수가 라인별로 적용되도록 하는 기능
사용하지 않으면 처음 세팅한 값으로 고정됨
=> %변수%는 처음 설정한 값
=> !변수!는 변경된 값

