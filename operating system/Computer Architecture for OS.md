# OS를 위한 Computer Architecture Basic 👀

### Computer Architecture
![1주차-컴퓨터구조](https://user-images.githubusercontent.com/31653025/92484485-b3f2ce00-f224-11ea-8f0e-be4439214765.png)

### Mode bit
> Mode bit을 통해 하드웨어적으로 두 가지 모드의 operation을 지원한다
```
 1 - 사용자모드(User Mode) : 사용자 프로그램 수행
 0 - 커널모드(Kernel Mode), 시스템모드(System Mode) : OS 코드 수행
```

* Interrupt나 Exception 발생시 하드웨어가 mode bit을 0으로 바꿈
* 사용자 프로그램에게 CPU를 넘기기 전에 mode bit을 1로 세팅

### Timer
> 타이머는 Time Sharing을 구현하기 위해 사용됨
* 타이머는 매 클럭 틱 때마다 감소하여 타이머 값이 0이 되면 타이머 인터럽트 발생
* 정해진 시간이 흐른 뒤 **운영체제에게 제어권이 넘어가도록** 인터럽트를 발생시킴 
    
    👉 여기서 발생시키는 인터럽트는 당연히 **하드웨어 인터럽트**
* CPU를 특정 프로그램이 독점하는 것으로부터 보호
* 타이머는 현재 시간을 계산하기 위해서도 사용

### Device Controller
> 해당 I/O 장치유형을 관리하는 일종의 작은 CPU
* 제어 정보를 위해 control register, status register를 가짐
* local buffer를 가짐
* I/O는 device와 local buffer사이에서 일어난다
```
device controller(장치 제어기) : 각 장치를 통제하는 일종의 작은 CPU ---> hardware
device driver(장치 구동기) : OS 코드 중 각 장치별 처리루틴 ---> software
```

### Interrupt 🎯
> 마이크로프로세서(CPU)가 프로그램을 실행하고 있을 때, 입출력 하드웨어 등의 장치나 또는 예외상황이 발생하여 처리가 필요한 경우에 마이크로프로세서(CPU)에게알려 처리할 수 있도록 하는 것을 말한다. (위키백과 참고)
* 인터럽트 당한 시점의 레지스터와 프로그램 카운터에 있는 주소값을 저장 후 CPU의 제어를 [인터럽트 처리루틴](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%9F%BD%ED%8A%B8)에 넘긴다
```
- Interrupt(H/W Interrupt) : 하드웨어가 발생시킨 인터럽트
        ⏩ I/O 인터럽트, 타이머 인터럽트 등
- Trap(S/W Interrupt) : 소프트웨어가 발생시킨 인터럽트 👉 트랩이라 부르기도 하고 인터럽트의 넓은의미로 인터럽트라고도 부른다
        ⏩ Exception : 프로그램이 오류를 범한 경우
        ⏩ System call : 프로그램이 커널함수를 호출하는 경우
```

* 인터럽트 관련 용어
```
- 인터럽트 벡터 : 해당 인터럽트의 처리 루틴 주소를 가지고 있음
- 인터럽트 처리 루틴(핸들러) : 해당 인터럽트를 처리하는 커널 함수
```

### System Call 🎯
> 운영체제의 커널이 제공하는 서비스에 대해, 응용 프로그램 요청에 따라 커널에 접근하기 위한 인터페이스이다. 보통 C나 C++같은 고급언어로 작성된 프로그램들은 직접 시스템 호출을 사용할 수 없기 때문에 고급 API를 통해 시스템 콜에 접근하게 하는 방법이다. (위키백과 참고)

* 사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출하는 것 👉 큰 의미에서는 trap을 발생시키는 것이고, 자세히 말하면 System call이라 한다

### DMA(Direct Memory Access)
> 특정 하드웨어 하위 시스템이 CPU와 독립적으로 메인 시스템 메모리에 접근할 수 있게 해주는 컴퓨터 시스템의 기능
* CPU의 중재없이 device controller가 device의 buffer storage의 내용을 메모리에 block단위로 직접 전송
* 바이트 단위가 아닌 블럭단위로 인터럽트 발생시킴 👉 **CPU에게 인터럽트 발생 수를 줄임(오버헤드 감소)**

### references
* 운영체제 9판 Silberschatz , Peter B. Galvin , Greg Gagne 지음
* 이화여자대학교 반효경 교수 강의 참고
* 고려대학교 최린 교수 강의 참고
* [인터럽트 위키백과](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%9F%BD%ED%8A%B8)
* [시스템 콜 위키백과](https://ko.wikipedia.org/wiki/%EC%8B%9C%EC%8A%A4%ED%85%9C_%ED%98%B8%EC%B6%9C)
