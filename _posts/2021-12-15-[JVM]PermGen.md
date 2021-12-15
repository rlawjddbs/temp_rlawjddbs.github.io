---
title: PermGen
updated: 2021-12-15 11:22
category: Java
---
# [JVM] PermGen
**[note]** : JDK 8부터 `Permanent Heap(Permgen)` 또는 `Permanent Area` 로 불리던 영역이 제거 되었다

> Perm 영역은 보통 **`Class`의 Meta 정보**나 **`Method`의 Meta 정보**, **`Static` 변수와 `상수` 정보**들이 **저장되는 공간**으로 흔히 `메타데이터 저장 영역`이라고도 한다. 이 영역은 Java 8 부터는 `Native` 영역으로 이동하여 `Metaspace` 영역으로 변경되었다. (다만, 기존 Perm 영역에 존재하던 `Static Object`는 Heap 영역으로 옮겨져서 `GC`의 대상이 최대한 될 수 있도록 하였다)

| |Java 7|Java 8|
|------|---|---|
|Class 메타 데이터|저장|저장|
|Method 메타 데이터|저장|저장|
|Static Object 변수, 상수|저장|Heap 영역으로 이동|
|메모리 튜닝|Heap, Perm 영역 튜닝|Heap 튜닝, Native 영역은 OS가 동적 조정|
|메모리 옵션|`-XX:PermSize`  `-XX:MaxPermSize`|`-XX:MetaspaceSize`  `-XX:MaxMetaspaceSize`|

    
Java 7 까지의 HotSpot[^1] 구조
```shell
<----- Java Heap ----->             <--- Native Memory --->
+------+----+----+-----+-----------+--------+--------------+
| Eden | S0 | S1 | Old | Permanent | C Heap | Thread Stack |
+------+----+----+-----+-----------+--------+--------------+
                        <--------->
                       Permanent Heap
S0: Survivor 0
S1: Survivor 1
```

Java 8 HotSpot JVM 구조
```shell
<----- Java Heap -----> <--------- Native Memory --------->
+------+----+----+-----+-----------+--------+--------------+
| Eden | S0 | S1 | Old | Metaspace | C Heap | Thread Stack |
+------+----+----+-----+-----------+--------+--------------+
```

[What's New in JDK 8](https://www.oracle.com/java/technologies/javase/8-whats-new.html)문서를 보면 HotSpot 항목에서 다음 문장을 찾을 수 있다.
> - Removal of PermGen.

## Java 7 버전에서 Java Heap과 Perm 영역의 역할
- Java Heap
	- PermGen에 있는 클래스의 인스턴스 저장
	- `-Xms`(min), `-Xmx`(max)로 사이즈 조정.
- PermGen
	- 클래스와 메소드의 메타데이터 저장.
	- 상수 풀 정보.
	- JVM, JIT 관련 데이터.
	- `-XX:PermSize`(min), `-XX:MaxPermSize`(max)로 사이즈 조정.



---

[^1]: 오라클이 소유한 두 종류의 JVM 중 하나로, 오라클은 썬마이크로시스템즈에서 개발된 `HotSpot`과 또 BAE 시스템에서 개발한 `JRockit`을 소유하고 있다.