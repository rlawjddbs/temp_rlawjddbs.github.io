---
title: PermGen
updated: 2021-12-15 11:22
category: Java
---
# [JVM] PermGen

[출처: 기계인간 John Grib - JDK 8에서 Perm 영역은 왜 삭제됐을까](https://johngrib.github.io/wiki/java8-why-permgen-removed/)

**[note]** : JDK 8부터 `Permanent Heap(PermGen)` 또는 `Permanent Area` 로 불리던 영역이 제거 되었다

> Perm 영역은 보통 **`Class`의 Meta 정보**나 **`Method`의 Meta 정보**, **`Static` 변수와 `상수` 정보**들이 **저장되는 공간**으로 흔히 `메타데이터 저장 영역`이라고도 한다. 이 영역은 Java 8 부터는 `Native` 영역으로 이동하여 `Metaspace` 영역으로 변경되었다. (다만, 기존 Perm 영역에 존재하던 `Static Object`는 Heap 영역으로 옮겨져서 `GC`의 대상이 최대한 될 수 있도록 하였다)

## 변경사항

| |Java 7|Java 8|
|------|---|---|
|Class 메타 데이터|저장|저장|
|Method 메타 데이터|저장|저장|
|Static Object 변수, 상수|저장|Heap 영역으로 이동|
|메모리 튜닝|Heap, Perm 영역 튜닝|Heap 튜닝, Native 영역은 OS가 동적 조정|
|메모리 옵션|`-XX:PermSize`<br />`-XX:MaxPermSize`|`-XX:MetaspaceSize`<br />`-XX:MaxMetaspaceSize`|

## 왜 Perm이 제거됐고 Metaspace 영역이 추가된 것인가?

> 최근 `Java 8`에서 JVM 메모리 구조적인 개선 사항으로 Perm 영역이 Metaspace 영역으로 전환되고 기존 Perm 영역은 사라지게 되었다. Metaspace 영역은 `Heap`이 아닌 `Native` 메모리 영역으로 취급하게 된다. (Heap 영역은 JVM에 의해 관리된 영역이며, Native 메모리는 OS 레벨에서 관리하는 영역으로 구분된다) Metaspace가 Native 메모리를 이용함으로서 개발자는 **영역 확보의 상한을 크게 의식할 필요가 없어지게 되었다.**

## Java 7 까지의 HotSpot[^1] 구조

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

## Java 8 HotSpot JVM 구조

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

## 1.7 버전대 Java의 PermGen size 확인 방법

OS `Windows 11 Pro(21H2)`을 기준으로 작성하였다.

```shell
# 7352는 현재 JVM이 동작하고 있는 프로그램의 PID
"C:\Program Files\Java\jre1.7\bin\jinfo.exe" -flag PermSize 7352
XX:PermSize=21757952
```

## 1.8 버전대 Java의 Metaspace size 확인 방법

```shell
"C:\Program Files\Java\jre1.8\bin\jinfo.exe" -flag MetaspaceSize 7352
-XX:MetaspaceSize=21807104
"C:\Program Files\Java\jre1.8\bin\jinfo.exe" -flag MaxMetaspaceSize 7352
-XX:MaxMetaspaceSize=18446744073709486080
```

- 별다른 설정없이 `MaxMetaspaceSize`는 18,446,744,073,709,486,080 byte
	- 17,592,186,044,415.9375 MB
	- 이 크기는 64비트 프로세서가 취급할 수 있는 메모리 상한에 가깝다고 함
- `XX:MaxMetaspaceSize` 옵션을 사용하여 이 크기를 줄이는 것도 가능

## Permanent Generation이 제거된 이유

[JEP 122: Remove the Permanent Generation](https://openjdk.java.net/jeps/122) 참고

---

[^1]: 오라클이 소유한 두 종류의 JVM 중 하나로, 오라클은 썬마이크로시스템즈에서 개발된 `HotSpot`과 또 BAE 시스템에서 개발한 `JRockit`을 소유하고 있다.