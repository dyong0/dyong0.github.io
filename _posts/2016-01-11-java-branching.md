---
layout : post
title : Java 분기 기법
date : 2016-01-11
---

Java에서 상태에 따라 다른 동작을 구현하고 싶을때 사용할 수 있는 분기 기법에 대해 소개합니다.

## 1. 오늘까지만 일 할래요 기법

```java
//type : 1 if type is first
//       2 if type is second
//       3 if type is third
void branchByLiteral(int type) {
    switch(type) {
        case 1:
            doSomething1();
        break;
        case 2:
            doSomething2();
        break;
        case 3:
            doSomething3();
        break;
        default:
        break;
    }
}

branchByLiteral(1);
branchByLiteral(2);
branchByLiteral(3);
```

레거시 코드에서 자주 볼 수 있는 형태입니다. 몹시 바쁜 일정에 쫓기다 보면 종종 작성하게 됩니다. 이 글에서 소개하는 기법 중 가장 좋지 않습니다. 왜냐하면 상태에 대한 정보를 알려면 이 함수를 읽어봐야 하기 때문입니다. 더욱이나 주석이 없다면 상태의 의미를 유추하기도 정말 어려운 코드입니다. 무조건적으로 재작성이 요구됩니다.

## 2. 프로그래밍 언어를 무시하는 기법

```java
public static int MY_TYPE1 = 1;
public static int MY_TYPE2 = 2;
public static int MY_TYPE3 = 3;

void branchByConstant(int type) {
    switch(type) {
        case MY_TYPE1:
            doSomething1();
        break;
        case MY_TYPE2:
            doSomething2();
        break;
        case MY_TYPE3:
            doSomething3();
        break;
        default:
        break;
    }
}

branchByConstant(MY_TYPE1);
branchByConstant(MY_TYPE2);
branchByConstant(MY_TYPE3);
```

이 기법은 앞의 기법의 문제인 **값의 의미를 알 수 없다**를 해결한 기법입니다. 이 기법은 상수를 정의해서 값에 설명적인 이름을 붙입니다. 그렇기 때문에 주석이 없어도 상수의 의미를 알 수 있고, 이 함수와 상수들을 외부에서 사용하기에도 나쁘지 않습니다. 하지만 이 기법 역시 문제가 있습니다. 상수의 타입이 primitive이기 때문에, `branchByConstant(1)`과 같은 코드도 동작하기 때문입니다. 이 기법도 가능하면 사용하지 않는게 좋습니다.

## 3. 프로그래밍 언어를 존중하는 기법
```java
public enum MyType {
    TYPE1,
    TYPE2,
    TYPE3
}

void branchByEnum(MyType type) {
    switch(type){
        case MyType.TYPE1:
            doSomething1();
        break;
        case MyType.TYPE2:
            doSomething2();
        break;
        case MyType.TYPE3:
            doSomething3();
        break;
        default:
        break;
    }
}

branchByEnum(MyType.TYPE1);
branchByEnum(MyType.TYPE2);
branchByEnum(MyType.TYPE3);
```

이 기법은 앞의 기법의 문제인 **상수의 컴파일 타입과 호환되는 타입의 값을 넣어도 동작한다**를 해결한 방법입니다. 열거형으로 상수의 타입을 명시적으로 정의하기 때문에, 이 열거형 타입을 상속하지 않는 이상 호환되는 타입이 존재하지 않습니다.

## 5. 프로그래밍 언어를 더욱 존중하는 기법: 다형성
```java
public interface MyInterface {
    void doSomething();
}
public class MyType1 {
    public void doSomething();
}
public class MyType2 {
    public void doSomething();
}
public class MyType3 {
    public void doSomething();
}

void branchByPolymorphismAndEnum(MyInterface branch) {
    branch.doSomething();
}

branchByPolymorphismAndEnum(new MyType1());
branchByPolymorphismAndEnum(new MyType2());
branchByPolymorphismAndEnum(new MyType3());
```

이 기법은 OOP의 다형성을 이용합니다. 앞에 나열한 기법들의 문제들을 모두 해결합니다. 하지만 이 기법에도 약간의 단점은 있습니다. 하나의 분기를 추가할 때마다 새로운 클래스를 만들어야 한다는 것입니다. 그래서 작은 계산과 같이 짧은 코드에는 적합하지 않을 수 있습니다.

### 6. 열거형으로 다형성
```java
public enum MyEnum {
    MY_TYPE1 {
        public abstract void doSomething() {
            // do something
        }
    },
    MY_TYPE2 {
        public abstract void doSomething() {
            // do something
        }
    },
    MY_TYPE3 {
        public abstract void doSomething() {
            // do something
        }
    }

    public abstract void doSomething();
}

void branchByPolymorphismAndEnum(MyEnum branch){
    branch.doSomething();
}

branchByPolymorphismAndEnum(MyEnum.MY_TYPE1);
branchByPolymorphismAndEnum(MyEnum.MY_TYPE2);
branchByPolymorphismAndEnum(MyEnum.MY_TYPE3);
```

이 기법은 앞에 기법에서 나온 **다형성에서 짧은 코드가 반복될 때, 클래스를 매번 추가해야 하는 문제**를 해결합니다.

## 결론
앞에서 보신 것처럼 분기에 상수를 사용하거나 다형성을 이용할 수 있습니다. 상수를 사용해야 한다면 열거형을 사용해야 유지보수가 쉬워집니다. 다형성을 이용할 수 있는 경우에도 짧은 코드 조각들이 많으면 열거형을 사용하는게 좋습니다.

## 참고
- 도서: 클린코드