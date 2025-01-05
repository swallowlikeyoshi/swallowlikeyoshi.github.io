---
title: "Visual Studio Code(VSC)에서 Arduino 개발환경 구축하기"
date: "2025-01-03"
thumbnail: "/assets/img/아두이노/image.png"
tags:
    - Arduino
---

# 개요
---

아두이노 IDE 버전 1.8 이하 사용시 VSC에서 동작하는 아두이노 익스텐션은 윈도우에 이미 설치된 아두이노 IDE를 VSC에서 편하게 쓸 수 있도록 연결해놓은 것에 불과했다. 아두이노 IDE가 2.x로 버전업하면서 내부적으로 arduino_cli.exe 파일을 이용한 방식으로 구조가 변경되었는데, 이로 인해 기존의 설치 방법을 이용하면 사용이 불가능하다.

> 참고: [아두이노 포럼](https://forum.arduino.cc/t/vs-code-and-user-setting-for-arduino-ide/1064852/6){:target="_blank"}


# 설치 방법
---

![](/assets/img/아두이노/image.png)

Arduino_cli.exe를 사용하면서 별다른 IDE 설치가 필요 없이 VSC의 익스텐션 설치만으로도 사용이 가능하다는 점이다. 애초에 익스텐션 설치 시 Arduino_cli.exe 파일이 함께 설치되기 때문이다. 그렇기에 기존 1.8 버전에서 하듯이 익스텐션 설정에서 추가로 패스를 설정하면 오류가 나게 된다. 설정에서 위 칸들을 비워놔야 익스텐션이 정상적으로 동작하게 된다.

![](/assets/img/아두이노/image%20(2).png)

파일 생성을 마치고 F1 키를 눌러 "Arduino: Initialize" 명령을 실행 후 자신의 보드를 선택하면 된다. 그러면 자동으로 .vscode 파일이 생성되고 기본 보드 설정값, 시리얼 포트 등을 저장하게 된다.

# 주의점
---

1. 

![](/assets/img/아두이노/image%20(1).png)

프로젝트의 루트 폴더명과 메인 .ino 파일의 이름이 동일해야만 Intellisense가 정상적으로 동작한다.

2. 

![](/assets/img/아두이노/image%20(3).png)

> [Warning] Output path is not specified. Unable to reuse previously compiled files. Build will be slower. See README. IntelliSense configuration updated. To manually rebuild your IntelliSense configuration run "Ctrl+Alt+I" 

위와 같은 에러가 뜨는 경우 "arduino.json" 파일에 "output" 옵션을 추가하고 마음에 드는 경로를 선택하고 해당 폴더를 만들어주면 된다. 컴파일 과정에서 이전의 컴파일된 파일들을 재사용하기 위해 빌드 출력물을 저장할 위치를 지정해달라는 오류다.


