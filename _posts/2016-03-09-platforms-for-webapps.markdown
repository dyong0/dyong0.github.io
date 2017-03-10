---
layout : post
title : webapp을 만들기 위한 플랫폼
date : 2016-03-09
tag : [webapp, platform, java, spring, jascript, nodejs]
---

# 서론
## 스프링이 필수일까?
회사에서 Java+Spring-mvc 환경에서 webapp을 만들면서 느꼈던 것 중 하나는 **"왜 스프링을 써야할까?"** 였습니다. 다른 플랫폼에서도 webapp은 만들 수 있습니다. 심지어 C언어를 기반으로 한 플랫폼에서도 만들 수 있습니다. 
그렇다면 왜 Java를 사용하는 환경에서 개발하는 걸까요? 그 이유는 언어적 특성상 Java가 생산성이 좋고, 이미 우리나라에서는 웹개발 HR인프라가 Java에 초점이 맞추어져있기 때문입니다.
Java를 사용해서 웹개발을 쉽고 지속적으로 유지보수가 가능하게 하려면 프레임워크를 적용해야합니다. 그래야 여러 개발자가 큰 틀 안에서 단방향으로 시스템을 제작할 수 있습니다. 그 중 2010년 쯤부터 인기를 얻기 시작하던 Spring이 자리를 잡게 되었고, 아직 이를 대체할만한 혁신적인 물건이 나타나지 않았습니다.
Spring은 확장성이 매우 뛰어나서 웹 개발에서는 Spring-mvc를 적용하고 있고, 이 자체도 확장해서 사용할 수 있도록 설계되어 있습니다. 그래서 Java+Spring-mvc 환경에서 개발을 하는 경우가 많지만 다른 플랫폼에서도 webapp을 만들 수 있습니다.

## 다른 플랫폼에서는 webapp을 어떻게 만드는가
한 예로, Python을 사용하는 곳에서는 Flask라는 프레임워크가 있습니다. 간단히 예제를 한번 보시죠.

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```

```sh
$ python hello.py
 * Running on http://127.0.0.1:5000/
```

이렇게 짧고 명쾌한 코드로도 webapp을 만들 수 있습니다. Java를 사용하는 환경이었다면 상상도 못했을 분량이죠. Flask에 대해 공부해보고 싶으시다면 다음 링크를 참조해보세요.
http://flask-docs-kr.readthedocs.org/ko/latest/

# js로 webapp 만들기
js는 front-end 개발에 관심이 높아지던 시기에 nodejs의 등장과 함께 실무에서 back-end에도 적용 가능한 언어가 되었습니다. nodejs는 network기능도 제공해서 이를 이용해 http통신이 가능합니다. 그말인 즉, webapp을 개발할 수 있는 환경이 구성되어 있다는 얘기입니다. 그래서 어차피 front-end에서 js로 개발하니 back-end도 js로 개발하면 좋지 않을까? 라는 생각을 실제로 실현할 수 있게 되었죠.
사람들은 nodejs에서 사용가능한 webapp 프레임워크를 만들기 시작했고, express라는 아주 쓸만한 프레임워크가 만들어졌습니다. 
이제 js+nodejs+express를 사용해서 아주 간단한 webapp을 만들어보겠습니다.

## 시스템 설계
- 서버에 있는 자원(상수값)을 읽어 출력하는 시스템
- root(/)경로로 접근하면 "my resource"를 응답으로 보내줍니다.

## 개발

### 코드

```javascript
var express = require('express');
var app = express();

var MY_RESOURCE='my resource';

app.get('/', function (req, res) {
  res.send(MY_RESOURCE);
});

app.listen(3000);
```

### 실행

```shell
$ node app.js
```

# js webapp vs Java webapp

위에서 만든 js webapp을 Java+Spring+mvc를 사용해 만든 것과 비교해보세요.

## js webapp

### 사전 준비

``` shell
$ npm install express --save
```

### 코드

```javascript
var express = require('express');
var app = express();

var MY_RESOURCE='my resource';

app.get('/', function (req, res) {
  res.send(MY_RESOURCE);
});

app.listen(3000);
```

### 실행

```shell
$ node app.js
```

## Java webapp

### 사전준비

```shell
$ yum install tomcat7 tomcat7-admin-webapps tomcat7-webapps
```

```xml
<!-- web.xml -->
<web-app>

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/root-context.xml</param-value>
    </context-param>
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value></param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    
</web-app>
```

```xml
<!-- root-context.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="test"/>
    
</beans>
```

**! 실행 과정을 짧게하기 위해 maven을 사용합니다.**
``` xml
<!-- pom.xml -->
<project>
    <groupId>com.test</groupId>
    <artifactId>test-app</artifactId>
    <version>1.0</version>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.3-SNAPSHOT</version>
            </plugin>
        </plugins>
    </pluginManagement>
</project>
```

### 코드

``` java
package test;

@Controller
public class TestController{
	private final String MY_RESOURCE="my resource";
    
    @RequestMapping("/")
    @ResponseBody
    public String test(){
    	return MY_RESOURCE;
    }
}
```

### 실행

```shell
$ mvn tomcat7:run
```

# 결론
webapp은 Java+Spring-mvc 위에서만 만들 수 있는 것이 아닙니다. 다른 플랫폼도 충분히 가능하고 모두 장단점이 있습니다. webapp은 무조건 Java로 만들어야한다는 고정관념을 버릴 수 있었으면 합니다. 아래에 js webapp과 Java webapp의 장단점을 나열했습니다. 비교한 내용을 보고 js를 찬양하더라도, Java가 필수가 아니라는 것을 알게되면 좋을 것 같습니다.

## js webapp 장단점
- 장점
	- Java webapp에 비해서 상대적으로 기술 습득을 위해 투자하는 비용이 적다.
	- js를 사용하기 때문에 js 특유의 유연한 코드를 작성할 수 있다.
	- front-end와 back-end를 하나의 언어를 사용할 수 있고, 그러므로 공용 비즈니스 로직을 공유할 수 있다.
	- 세계적으로 사용자가 많다.
	- 성능이 절대 Java webapp에 비해 나쁘지 않다.
	- nodejs는 유연하게 clustring을 할 수 있도록 설계되었다.
	- single thread 기반 엔진을 사용하므로 multi threading을 할 수 없다.
	- 계속해서 급진적으로 변하는 흐름위에 있다.
- 단점
	- nodejs 위에서 webapp이 실행되므로 nodejs와 nodejs 주변 기술을 익혀야 한다.
	- single thread 기반 엔진을 사용하므로 multi threading을 할 수 없다.
	- 계속해서 급진적으로 변하는 흐름위에 있다.

## Java webapp 장단점
- 장점
	- Java를 사용하는 HR인프라가 구성되어 있다.
	- Java webapp 관련 기술에 대한 노하우가 쌓여있다.
	- Oracle이 Java를 관리한다.
- 단점
	- 장점을 제외한 모든 것
	- Oracle이 Java를 관리한다.

# 참고
http://flask-docs-kr.readthedocs.org/ko/latest/
http://expressjs.com/ko/starter/hello-world.html
https://tomcat.apache.org/maven-plugin-trunk/index.html
https://maven.apache.org/guides/introduction/introduction-to-the-pom.html
http://zetawiki.com/wiki/Yum%EC%9C%BC%EB%A1%9C_%ED%86%B0%EC%BA%A37_%EC%84%A4%EC%B9%98
