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

## 테스트 코드 작성하기

[Application.java](src/main/java/springboot/Application.java)
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

[HelloController.java](src/main/java/springboot/web/HelloController.java)
```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```
1. @RestController	컨트롤러를 JSON을 반환하는 컨트롤러로 만들어 줍니다.	
예전에는 @ResponseBody를 각 메소드마다 선언했던 것을 한번에 사용할 수 있게 해준다고 생각하면 됩니다.

2. @GetMapping	HTTP Method인 Get의 요청을 받을 수 있는 API를 만들어 줍니다.
예전에는 @RequestMapping(method = RequestMethod.GET)으로 사용되었습니다. 이제 이 프로젝트는 /hello로 요청이 오면 문자열 hello를 반환하는 기능을 가지게 되었습니다.

⌘ + N
![스크린샷 2023-08-15 오후 9.48.59.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.48.59.png)

![스크린샷 2023-08-15 오후 9.50.11.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.50.11.png)

[HelloControllerTest.java](src/test/java/springboot/web/HelloControllerTest.java)

```java
@RunWith(SpringRunner.class)
@WebMvcTest(controllers = HelloController.class)
public class HelloControllerTest {

    @Autowired
    private MockMvc mvc;

    @Test
    public void hello가_리턴된다() throws Exception {
        String hello = "hello";

        mvc.perform(get("/hello"))
                .andExpect(status().isOk())
                .andExpect(content().string(hello));
    }
}
```

1. @RunWith(SpringRunner.class)	
   - 테스트를 진행할 때 JUnit에 내장된 실행자 외에 다른 실행자를 실행시킵니다.	
   - 여기서는 SpringRunner라는 스프링 실행자를 사용합니다. 
   - 즉, 스프링 부트 테스트와 JUnit 사이에 연결자 역할을 합니다. 

2. @WebMvcTest	
   - 여러 스프링 테스트 어노테이션 중, Web(Spring MVC)에 집중할 수 있는 어노테이션 입니다.	
   - 선언할 경우 @Controller, @ControllerAdvice 등을 사용할 수 있습니다.	
   - 단, @Service, @Component, @Repository 등은 사용할 수 없습니다.	
   - 여기서는 컨트롤러만 사용하기 때문에 선언합니다.

3. @Autowired
   - 스프링이 관리하는 빈(Bean)을 주입 받습니다. 

4. private MockMvc mvc	
   - 웹 API를 테스트할 때 사용합니다.	
   - 스프링 MVC 테스트의 시작점입니다.	
   - 이 클래스를 통해 HTTP GET, POST 등에 대한 API 테스트를 할 수 있습니다. 

5. mvc.perform(get("/hello"))
   - MockMvc를 통해 /hello 주소로 HTTP GET 요청을 합니다.	
   - 체이닝이 지원되어 아래와 같이 여러 검증 기능을 이어서 선언할 수 있습니다. 

6. .andExpect(status( ).isOk( ))	
   - mvc.perform의 결과를 검증합니다.	
   - HTTP Header의 Status를 검증합니다.	
   - 우리가 흔히 알고 있는 200, 404, 500 등의 상태를 검증합니다.	
   - 여기선 OK 즉, 200인지 아닌지를 검증합니다.

5. .andExpect(content( ).string(hello))	
   - mvc.perform의 결과를 검증합니다.	
   - 응답 본문의 내용을 검증합니다.	
   - Controller에서 hello를 리턴하기 때문에 이 값이 맞는지 검증합니다.