---
layout : post
title : JUnit 철학
date : 2016-02-10
tag : [java, junit]
---

# 1. JUnit의 테스트들은 순서를 보장하지 않는다.
```java
public class MyTest{
  
  private int sequence = 0;
  
  @Test
  public void testGuaranteeSequential_first(){
    assertEquals(0, sequence); //success always
    sequnce = 3;
  }
  
  @Test
  public void testGuaranteeSequential_second(){
    assertEquals(3, sequence); //fails always
  }

}
```

# 2. Test를 하기 위해 매 테스트마다 Test 클래스 인스턴스를 생성한다.

```java
public class MyTest{
  
  private int sequence = 0;
  
  @Test
  public void testAlwaysNewInstance_1(){
    sequence++;
    assertEquals(1, sequence); //success always
  }
  
  @Test
  public void testAlwaysNewInstance_2(){
    sequence++;
    assertEquals(1, sequence); //success always
  }
  
  @Test
  public void testAlwaysNewInstance_3(){
    sequence++;
    assertEquals(1, sequence); //success always
  }
}
```
