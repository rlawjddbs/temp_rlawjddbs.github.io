---
title: macOS에서 이클립스 스프링 포트가 충돌할 때
updated: 2019-05-20 21:52
---

![](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/assets/common/imgs/190520/404error.png)


```terminal
sudo lsof -i : "8080"
```
터미널을 실행한 후 위 코드를 입력한 뒤 비밀번호까지 입력하면 현재 8080번 포트를 사용중인 프로세스를 확인할 수 있음.

![](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/assets/common/imgs/190520/lsof-i8080.png)

같은 포트를 사용중인 프로세스들. 이클립스를 제외한 모든 프로세스의 PID를 Kill 해줘야 한다.

```terminal
sudo lsof -i :"Kill할 프로세스 PID"
```
sudo lsof -i :"535" 를 입력하면 PID가 535인 프로세스를 Kill 할 수 있다.


