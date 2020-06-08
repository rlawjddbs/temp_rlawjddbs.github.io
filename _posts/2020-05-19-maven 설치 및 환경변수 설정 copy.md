---
title: maven 설치 및 환경변수 설정 (macOS)
updated: 2020-05-18 23:12
category: Java
---
### 1. Maven 다운  
[http://maven.apache.org/download.cgi](http://maven.apache.org/download.cgi)
tar.gz로 끝나는 파일 받기

### 2. terminal 실행 ([ ] 대괄호 없이 입력)  
```terminal
# 다운받은 maven 압축파일을 설치하고싶은 경로로 이동시킨다.
mv [다운받은maven경로] [이동시킬경로]

# 이동 후 해당 파일 압축해제
tar -zxvf [maven 압축파일명.tar.gz]

# 위 두가지 명령어를 한번에 처리하려면
tar -zxvf [maven 압축파일 경로 및 파일명.tar.gz] [압축해제할 경로 및 파일명.tar.gz]
```
> #### zxvf 뜻
> - z means (un)z̲ip.
> - x means ex̲tract files from the archive.
> - v means print the filenames v̲erbosely.
> - f means the following argument is a f̱ilename.
  
### 3. maven 압축해제 후 환경변수로 등록 (.bash)  
mac의 경우 보통 `/Users/username/.bash_profile` 에 환경변수를 등록한다고 하는데 어째서인지 그런 파일은 없음. 대신 `.bash` 라는 파일이 있음.[^1]
  
```terminal
vi ~/.bash
```
Vim Editor로 .bash 파일을 열고 알파벳 `i`(Insert)키를 눌러 아래 내용을 추가한다. 
  
```terminal
# 환경변수 등록
# JAVA_HONME
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_191.jdk/Contents/Home
export PATH=${PATH}:${JAVA_HOME}/bin
# MAVEN_HOME
export MAVEN_HOME=/Users/zeongyun/dev/maven/apache-maven-3.6.3
export PATH=$PATH:$MAVEN_HOME/bin
```


입력 후 esc 키를 누른 후  `:wq`(Write, Quit)를 입력하고 엔터키 입력을 하면 Vim Editor에서 빠져나온다. 
아직 `mvn -version`을 입력해도 `command not founc mvn` 같은 문구만 뜬다. `source` 명령어를 이용해 .bash 에 작성한 명령어를 실행해야 한다.  
  
```terminal
source ~/.bash
mvn -version
```

### 결과
<a href="https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/_posts/imgs/0518/result.png" style="border-bottom:0;" target="_new">![](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/_posts/imgs/0518/result.png)</a>

### MAVEN 프로젝트 만들기
```terminal
$mvn archetype:generate
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] 
[INFO] >>> maven-archetype-plugin:3.1.2:generate (default-cli) > generate-sources @ standalone-pom >>>
[INFO] 
[INFO] <<< maven-archetype-plugin:3.1.2:generate (default-cli) < generate-sources @ standalone-pom <<<
[INFO] 
[INFO] 
[INFO] --- maven-archetype-plugin:3.1.2:generate (default-cli) @ standalone-pom ---
[INFO] Generating project in Interactive mode
[INFO] No archetype defined. Using maven-archetype-quickstart (org.apache.maven.archetypes:maven-archetype-quickstart:1.0)
Choose archetype:
1: remote -> am.ik.archetype:elm-spring-boot-blank-archetype (Blank multi project for Spring Boot + Elm)
2: remote -> am.ik.archetype:graalvm-blank-archetype (Blank project for GraalVM)
3: remote -> am.ik.archetype:graalvm-springmvc-blank-archetype (Blank project for GraalVM + Spring MVC)
4: remote -> am.ik.archetype:graalvm-springwebflux-blank-archetype (Blank project for GraalVM + Spring MVC)
5: remote -> am.ik.archetype:maven-reactjs-blank-archetype (Blank Project for React.js)
6: remote -> am.ik.archetype:msgpack-rpc-jersey-blank-archetype (Blank Project for Spring Boot + Jersey)
7: remote -> am.ik.archetype:mvc-1.0-blank-archetype (MVC 1.0 Blank Project)
8: remote -> am.ik.archetype:spring-boot-blank-archetype (Blank Project for Spring Boot)
9: remote -> am.ik.archetype:spring-boot-docker-blank-archetype (Docker Blank Project for Spring Boot)
10: remote -> am.ik.archetype:spring-boot-gae-blank-archetype (GAE Blank Project for Spring Boot)
11: remote -> am.ik.archetype:spring-boot-jersey-blank-archetype (Blank Project for Spring Boot + Jersey)
12: remote -> am.ik.archetype:spring-fu-jafu-blank-archetype (Blank project for Vanilla Spring WebFlux.fn)
13: remote -> am.ik.archetype:vanilla-spring-webflux-fn-blank-archetype (Blank project for Vanilla Spring WebFlux.fn)
14: remote -> at.chrl.archetypes:chrl-spring-sample (Archetype for Spring Vaadin Webapps)
15: remote -> be.cloudway:gramba-aws-lambda-archetype (-)
16: remote -> biz.turnonline.ecosystem:turnonline-ecosystem-microservice-archetype (TurnOnline.biz Ecosystem: Serverless Microservice Archetype)
17: remote -> br.com.address.archetypes:struts2-archetype (an archetype web 3.0 + struts2 (bootstrap + jquery) + JPA 2.1 with struts2 login system)
18: remote -> br.com.address.archetypes:struts2-base-archetype (An Archetype with JPA 2.1; Struts2 core 2.3.28.1; Jquery struts plugin; Struts BootStrap plugin; json Struts plugin; Login System using Session and Interceptor)
19: remote -> br.com.anteros:Anteros-Archetype (Anteros Archetype for Java Web projects.)
20: remote -> br.com.codecode:vlocadora-json (Modelos com Anotações Gson)
21: remote -> br.com.diogoko:maven-doclet-archetype (A Maven archetype to create Doclets for Javadoc)
22: remote -> br.com.ingenieux:elasticbeanstalk-docker-dropwizard-webapp-archetype (A Maven Archetype for Publishing Dropwizard-based Services on AWS' 
2714: remote -> us.fatehi:schemacrawler-archetype-maven-project (-)
2715: remote -> us.fatehi:schemacrawler-archetype-plugin-command (-)
2716: remote -> us.fatehi:schemacrawler-archetype-plugin-dbconnector (-)
2717: remote -> us.fatehi:schemacrawler-archetype-plugin-lint (-)
2718: remote -> ws.osiris:osiris-archetype (Maven Archetype for Osiris)
2719: remote -> xyz.luan.generator:xyz-gae-generator (-)
2720: remote -> xyz.luan.generator:xyz-generator (-)
2721: remote -> za.co.absa.hyperdrive:component-archetype (-)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 1615: 
Choose org.apache.maven.archetypes:maven-archetype-quickstart version: ######### 엔터 입력
1: 1.0-alpha-1
2: 1.0-alpha-2
3: 1.0-alpha-3
4: 1.0-alpha-4
5: 1.0
6: 1.1
7: 1.3
8: 1.4
Choose a number: 8: ######### Enter 입력
Define value for property 'groupId': com.lab.dbs ######### 직접 입력
Define value for property 'artifactId': sample ######### 직접 입력
Define value for property 'version' 1.0-SNAPSHOT: : ######### Enter 입력
Define value for property 'package' com.lab.dbs: : ######### Enter 입력
Confirm properties configuration:
groupId: com.lab.dbs
artifactId: sample
version: 1.0-SNAPSHOT
package: com.lab.dbs
 Y: : y ######### y 입력 시 설정한 내용대로 MAVEN PROJECT 생성
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.4
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: com.lab.dbs
[INFO] Parameter: artifactId, Value: sample
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.lab.dbs
[INFO] Parameter: packageInPathFormat, Value: com/lab/dbs
[INFO] Parameter: package, Value: com.lab.dbs
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: groupId, Value: com.lab.dbs
[INFO] Parameter: artifactId, Value: sample
[INFO] Project created from Archetype in dir: /Users/zeongyun/dev/workspace/test_mvn/sample
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  04:38 min
[INFO] Finished at: 2020-06-08T20:25:59+09:00
[INFO] ------------------------------------------------------------------------
$    
```



[^1]: mac catalina 업데이트 후 설치한 zsh 설치와 관련있는지 확인 필요.  