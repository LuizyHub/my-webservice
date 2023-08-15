# 웹 서비스 만들기

## 개요
개발자가 되고싶었던 코딩루이지

억대 연봉을 받고 있는 친구에게 찾아간다

"나 너와같은 개발자가 되고싶어! 뭐 부터 해봐야 할까?"

친구는 나의 말을 귀담아 들어주고, 회사 동료들이랑도 회의를 해봤다고 한다.

공통적으로 나온 대답은 "개발부터 배포까지를 한번 다 경험해보는게 좋겠다" 라는 답과 함께

책 한권을 추천해줬다.

[**_스프링 부트와 AWS로 혼자 구현하는 웹 서비스_**](https://freelec.co.kr/book/스프링-부트와-aws로-혼자-구현하는-웹-서비스/#tabs_desc_796_3)

이 책을 시작으로 게시판 만들기의 과정들을 익혀보자!

<!-- TOC -->
* [웹 서비스 만들기](#웹-서비스-만들기)
  * [개요](#개요)
  * [프로젝트 생성하기](#프로젝트-생성하기)
  * [Gradle 버전 맞추기](#gradle-버전-맞추기)
  * [build.gradle 세팅](#buildgradle-세팅)
  * [.ignore](#ignore)
  * [github에 프로젝트 공유하기](#github에-프로젝트-공유하기)
  * [테스트 코드 작성하기](#테스트-코드-작성하기)
  * [lombok](#lombok)
<!-- TOC -->

---

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

---

## build.gradle 세팅
[build.gradle](build.gradle)
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

---

## .ignore

![스크린샷 2023-08-15 오후 9.10.12.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.10.12.png)

![스크린샷 2023-08-15 오후 9.24.48.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.24.48.png)

.gitignore
```.gitignore
.gradle
.idea
```

---

## github에 프로젝트 공유하기

![스크린샷 2023-08-15 오후 9.26.52.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.26.52.png)

![스크린샷 2023-08-15 오후 9.27.05.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%209.27.05.png)

[https://github.com/LuizyHub/my-webservice](https://github.com/LuizyHub/my-webservice)

## 테스트 코드 작성하기

---

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

## lombok

---

[build.gradle](build.gradle)
```gradle
dependencies {
    compile('org.projectlombok:lombok')
}
```
추가

![스크린샷 2023-08-15 오후 11.44.41.png](image%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202023-08-15%20%EC%98%A4%ED%9B%84%2011.44.41.png)

plugin 설치

[src/main/java/springboot/web/dto/HelloResponseDto.java](src/main/java/springboot/web/dto/HelloResponseDto.java) 추가

```java
@Getter
@RequiredArgsConstructor
public class HelloResponseDto {

    private final String name;
    private final int amount;
}
```
1. @Getter	
   - 선언된 모든 필드의 get 메소드를 생성해 줍니다. 

2. @RequiredArgsConstructor	
   - 선언된 모든 final 필드가 포함된 생성자를 생성해 줍니다.	
   - final이 없는 필드는 생성자에 포함되지 않습니다.


[HelloController.java](src/main/java/springboot/web/HelloController.java) 추가
```java
@RestController
public class HelloController {
    
   @GetMapping("/hello/dto")
   public HelloResponseDto helloDto(@RequestParam("name") String name,
                                    @RequestParam("amount") int amount) {
      return new HelloResponseDto(name, amount);
   }
}
```

@RequestParam	
- 외부에서 API로 넘긴 파라미터를 가져오는 어노테이션입니다.	
- 여기서는 외부에서 name (@RequestParam("name")) 이란 이름으로 넘긴 파라미터를 메소드 파라미터 name(String name)에 저장하게 됩니다.

[src/test/java/springboot/web/HelloControllerTest.java](src/test/java/springboot/web/HelloControllerTest.java) 추가

```java
@RunWith(SpringRunner.class)
@WebMvcTest(controllers = HelloController.class)
public class HelloControllerTest {
    
    @Test
    public void helloDto가_리턴된다() throws Exception {
        String name = "hello";
        int amount = 1000;

        mvc.perform(
                        get("/hello/dto")
                                .param("name", name)
                                .param("amount", String.valueOf(amount)))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name", is(name)))
                .andExpect(jsonPath("$.amount", is(amount)));
    }
}
```

1. param	
   - API 테스트할 때 사용될 요청 파라미터를 설정합니다.	
   - 단, 값은 String만 허용됩니다.	
   - 그래서 숫자/날짜 등의 데이터도 등록할 때는 문자열로 변경해야만 가능합니다.

2. jsonPath		
   - JSON 응답값을 필드별로 검증할 수 있는 메소드입니다.		
   - $를 기준으로 필드명을 명시합니다.		
   - 여기서는 name과 amount를 검증하니 $.name, $.amount로 검증합니다.

