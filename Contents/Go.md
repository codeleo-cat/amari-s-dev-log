![Gopher|200](https://i.namu.wiki/i/c_LoHiKr-ly7DrmAmeF-oOJdD_iwCh4SiMJEoCpnDmKv9zGA63OSWO0Rfw1gOPOnmG1h_tDl0oNA3_jyTV-zFqH982iXsCWsD8qwh3EsA6jlDkCq8C4BvXhosazktuNUNt4PPXRTv5GhQE3Ps9b52g.svg)

Go 언어의 특징은 컴파일 언어이지만 [컴파일러](https://namu.wiki/w/%EC%BB%B4%ED%8C%8C%EC%9D%BC%EB%9F%AC "컴파일러")의 컴파일 속도가 매우 빨라 [인터프리터](https://namu.wiki/w/%EC%9D%B8%ED%84%B0%ED%94%84%EB%A6%AC%ED%84%B0 "인터프리터") 언어처럼 쓸 수 있다.
코드 역시 간결하면서도 컴파일 언어답게 높은 성능을 낼 수 있다는 점.
“Go is not revolutionary, but its strength is in combining the right features"

==컴파일 언어 VS 인터프리터 언어==
**컴파일 언어**는 프로그램을 실행하기 전에 전체 코드를 컴파일러라는 도구를 통해 **기계어로 번역**한 후 실행하는 방식을 사용합니다. 대표적인 컴파일 언어로는 C, C++, 그리고 Go
프로그램이 실행될 때 이미 기계어로 변환되어 있기 때문에 실행 속도가 빠르고, 오류가 미리 체크되어 안정성이 높다.
그러나 코드 수정 후에는 다시 컴파일을 해야 하는 번거로움.

VS

**인터프리터 언어**는 프로그램을 실행할 때마다 코드 한 줄씩 해석하여 실행하는 방식을 사용합니다. 대표적인 인터프리터 언어로는 Python, JavaScript, Ruby.
이 방식은 코드 수정 후 즉시 실행이 가능하여 개발 과정에서의 유연성이 높다

Java는 이 두 언어적 특성을 모두 가진 hybrid 언어로 분류됨.

Go 언어는 **멀티스레드**를 지원하지만, 좀 더 정확하게 말하자면, Go는 **고루틴(goroutines)** 이라는 개념을 사용합니다. 고루틴은 Go 언어의 경량 쓰레드로, 멀티스레딩의 일종이지만, 기존의 OS 수준의 스레드보다 훨씬 가볍고 효율적입니다.

Docker, Kubernetes를 작성하는 데 사용된 언어.
Google, Dropbox 기능 상당수, 트위치, SpaceX

==Web FrameWork==
- Gin
- echo
- Fiber (보다 미니멀함. 생태계가 그렇게 넓지는 않음. node js의 express 느낌)