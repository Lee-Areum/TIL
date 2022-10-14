# android Package

### APK(Android Application Package)

- Android에 설치 가능하고 실행 가능한 앱 형식
- 컴파일된 코드와 리소스를 묶어서 키로 서명한 것

- apk 파일 하나를 통해 많은 디바이스와 호환 지원
    
    → APK 자체에 여러 개의 ABI(Android Binary Interface)를 포함함 
    
    → APK 파일의 크기가 커짐
    
- Google Play에서 다운로드 하는 APK는 100MB 이하여야 한다.
    - 이상이면 앱을 실행할 때 개발자가 직접 리소스를 호스팅하고 다운로드함

### AAB(Android App Bundle)

- Android용 게시 형식

- APK의 용량문제를 해결하기 위해 사용
- 사용자 기기에 맞게 최적화된 APK를 대신 만들어줌
    - 설치 파일은 동일한 APK
    - 개발자가 직접 파일을 만들지 않고 Google Play에서 최적화된 파일을 만들어줌
    - 필요한 파일만 다운 → 용량 줄어듬