# 웹 서비스 만들기

## 프로젝트 생성하기
![스크린샷 2023-08-15 오후 9.19.15.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.19.15.png)

Build System - Gradle

JDK - 1.8
> [[MacOS M1] Java 버전별로 설치 관리하기 (Temurin, Azul, jEnv)](https://code-l.tistory.com/4)

## Gradle 버전 맞추기

![스크린샷 2023-08-15 오후 9.04.31.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.04.31.png)

```shell
./gradlew wrapper --gradle-version 4.10.2
```

## build.gradle 세팅

```gradle
buildscript {
    ext {
        springBootVersion = '2.1.9.RELEASE'
    }
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'


group = 'org.example'
version = '1.0-SNAPSHOT'
sourceCompatibility = 1.8

repositories {
    mavenCentral()
}

dependencies {
    compile('org.springframework.boot:spring-boot-starter-web')

    testCompile('org.springframework.boot:spring-boot-starter-test')
}

test {
    useJUnitPlatform()
}
```

## .ignore

![스크린샷 2023-08-15 오후 9.10.12.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.10.12.png)

![스크린샷 2023-08-15 오후 9.24.48.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.24.48.png)

.gitignore
```.gitignore
.gradle
.idea
```

## github에 프로젝트 공유하기

![스크린샷 2023-08-15 오후 9.26.52.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.26.52.png)

![스크린샷 2023-08-15 오후 9.27.05.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.27.05.png)

[https://github.com/LuizyHub/my-webservice](https://github.com/LuizyHub/my-webservice)