---
layout : post
title : webapp을 만들기 위한 플랫폼
date : 2016-06-01
tag : [java, spring, mvc, test]
---

## 서문
Spring Web MVC 테스트를 작성을 좀 더 쉽고 직관적으로 작성하고 싶습니다. JUnit, Spring Test, Mockito를 조합해서 테스트를 작성하다보니 각 요소를 모두 잘 알고 원리를 이해하지 않으면 코드를 작성하는데 불편함이 있습니다. 이 문제를 완화시켜보고자 작은 모듈을 만들어봤습니다.

## Before
1. MockMVC를 매번 생성하고 초기화해야합니다.
2. Mockito를 잘 모르면 코드를 작성하기 힘듭니다.
3. 코드구조가 직관적이지 않습니다.

```java
public class ArticleControllerTest {

	MockMvc mockMvc;
	
	@InjectMocks
	ArticleController controller;

	@Mock
	AuthService authService;

	AuthRequest<Article> simpleArticleRegisterRequest;

	@Before
	public void setup() {
		MockitoAnnotations.initMocks(this);
		mockMvc = MockMvcBuilders.standaloneSetup(controller).build();
        ...
    }

    @Test
    public void testRegisterArticle() throws Exception {
        when(authService.validateAuthKey(isA(AuthKey.class))).thenReturn(true);
        mockMvc.perform(post("/api/articles/register")
                .content(gson.toJson(simpleArticleRegisterRequest))
                .contentType(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk())
            .andDo(print());
    }
}
```

## After
given, when, then 3단계 구조로 되어있고, 자동 완성으로 기존 코드에서 매번 handler와 matcher를 찾아서 써야했던 불편했던 점을 해소했습니다. when 메서드가 mockito의 when 메서드와 이름이 겹쳐서 call로 대신했습니다. 이제는 테스트를 설명적으로 표현하고 그에 대한 내용을 코드로 작성할 수 있습니다. 참고로 테스트를 설명하는 것은 필수입니다. 그리고 테스트에 대한 설명은 System.out에 출력됩니다.
```java
public class ArticleControllerTestWithControllerTest extends ControllerTest{

	@InjectMocks
	ArticleController controller;

	@Mock
	AuthService authService;

	Gson gson = new Gson();

	AuthRequest<Article> simpleArticleRegisterRequest;

	@Before
	public void setup() {
		MockitoAnnotations.initMocks(this);
        ...
    }
    
    @Test
    public void testRegisterArticle() throws Exception {
        given("auth key validation이 성공하는 auth key를 가지고", this.controller, ()->{
            call(authService.validateAuthKey(isA(AuthKey.class))).thenReturn(true);
        });

        when("/api/articles/register로 요청을 보내면")
            .post("/api/articles/register")
            .content(gson.toJson(simpleArticleRegisterRequest))
            .contentType(MediaType.APPLICATION_JSON);


        then("성공으로 응답이 돌아온다.")
            .expect(status().isOk())
            .andDo(print());
    }    
}
```

## 사용법
1. pom.xml에 다음을 추가합니다.
```xml
<repositories>
    <repository>
        <id>easySpringMvcTest</id>
        <url>https://raw.github.com/dyong0/easySpringMvcTest/mvn-repo/</url>
        <snapshots>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
        </snapshots>
    </repository>
</repositories>
```
```xml
<dependency>
    <groupId>easy-springmvc-test</groupId>
    <artifactId>easy-springmvc-test</artifactId>
</dependency>
```

2. ControllerTest를 상속한 테스트를 작성합니다.


## 결론
현재는 Controller에 제한적으로 사용되고 있지만 Service와 같이 범용적인 테스트에도 적용해보고 싶습니다. 의견을 github issue나 이 글에 댓글로 남겨주세요.
