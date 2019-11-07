---
title: macOS에서 Tomcat 서버가 Run on Server 되지 않는 이슈
updated: 2019-05-20 21:52
category: Java
---

<a href="https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/assets/imgs/190520/404error.png" target="_new">![](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/assets/common/imgs/190520/404error.png)</a>


```terminal
sudo lsof -i : "8080"
```
터미널을 실행한 후 위 코드를 입력한 뒤 비밀번호까지 입력하면 현재 8080번 포트를 사용중인 프로세스를 확인할 수 있음.

<a href="https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/assets/imgs/190520/lsof-i8080.png" target="_new">![](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/assets/common/imgs/190520/lsof-i8080.png)</a>

같은 포트를 사용중인 프로세스들. 이클립스를 제외한 모든 프로세스의 PID를 Kill 해줘야 한다.

```terminal
sudo kill -9 :"Kill할 프로세스 PID"
```
sudo lsof -i :"535" 를 입력하면 PID가 535인 프로세스를 Kill 할 수 있다.


### 2019-05-31 추가
- 매번 프로세스를 종료하는 것이 번거로운 점
- 집을 제외한 다른 장소에서는 프로세스를 종료하지 않고도 서버가 가동된다는 점

정확한 원인을 알 수 없지만 집에서는 Tomcat WAS 서버에서 IPv6로 IP Address를 반환하는 이슈를 발견,
따라서 톰캣 실행 시 전달되는 JVM의 환경 변수에 다음 설정 값을 추가해보았다.

<a href="https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/assets/imgs/190520/run_configurations.png" target="_new">![](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/assets/common/imgs/190520/run_configurations.png)</a>

```terminal
"-Djava.net.preferIPv4Stack=true"
```

이후로는 8080 포트를 사용하는 프로세스를 강제 종료하지 않고도 톰캣 서버 구동 시 IPv4의 형태로 IP 주소가 반환되는 것을 확인할 수 있었다.
