---
title: 51-2. JDBC
updated: 2019-01-03 09:48:00
category: Oracle
---

**요약:** SQL의 기본

<div class="divider"></div>

### DBMS(DataBase Management System)
- data : 수, 문자, 이미지로 된 자료(연구, 조사의 기본 재료)
- - Data를 체계적으로 정리한 것이 DataBase
- Driver loading 방식[^1]
- **java.sql 패키지**에서 관련 클래스, interface를 제공한다.

### Driver
- Driver는 (외부 jar) : ext 폴더[^2], class path, build path
    - ext : path를 설정하지 않아도 된다.
    - class path : UI 환경
    - build path : eclipse 사용

[^1]: Driver가 제공된다면 모든 RDBMS와 연동할 수 있는데, 그 Driver를 DB 제조사(Vendor)에서 직접 제작하여 제공한다.
[^2]: Java\jdk-.----\jre\lib 의 경로에 위치한 폴더
