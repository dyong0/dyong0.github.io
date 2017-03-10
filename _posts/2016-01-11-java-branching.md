---
layout : post
title : Java 분기 기법
---

특정환 상황을 나타내는 값에 따라 코드를 작성하고 싶을때 사용할 수 있는 분기 기법에 대해 소개합니다.

## 1. C 스타일
{% highlight java %}
//type : 1 if type is first
//       2 if type is second
//       3 if type is third
void branchWithLegacyValue(int type){
    switch(type){
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

branchWithLegacyValue(1);
branchWithLegacyValue(2);
branchWithLegacyValue(3);
{% endhighlight %}

이 기법은 가장 단순하지만 가장 안좋은 기법입니다. 이 기법은 type의 값의 의미를 알 수 있는 방법이 없기 때문입니다.
주석이라도 있으면 다행인데 주석도 없으면 작성자 외에는 전혀 이해할 수 없는 코드입니다.

##2. 상수 정의로 타입에 이름 붙이기
{% highlight java %}
public static int MY_TYPE1 = 1;
public static int MY_TYPE2 = 2;
public static int MY_TYPE3 = 3; 

void branchWithConstant(int type){
    switch(type){
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

branchWithConstant(MY_TYPE1);
branchWithConstant(MY_TYPE2);
branchWithConstant(MY_TYPE3);
{% endhighlight %}

이 기법은 1번 기법의 문제인 "값의 의미를 알 수 없다"를 해결한 기법입니다. 이 기법은 상수를 정의함으로써 값에 설명적인 이름을 붙입니다.
그렇기 때문에 주석이 없어도 상수의 의미를 알 수 있습니다. 하지만 이 방법에도 문제가 있습니다. 
그것은 바로 type에 숫자 0과 같은 값을 넣어도 코드가 실행이 된다는 것입니다.

## 3. 열거형으로 상수 사용하기
{% highlight java %}
public enum MyType{
    TYPE1,
    TYPE2,    
    TYPE3
}

void branchWithEnum(MyType type){
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

branchWithEnum(MyType.TYPE1);
branchWithEnum(MyType.TYPE2);
branchWithEnum(MyType.TYPE3);
{% endhighlight %}

이 기법은 2번 기법의 문제인 "상수의 컴파일 타입과 호환되는 타입의 값과 미리 정의한 상수들을 구분하기 어렵다"를 해결한 방법입니다.
열거형으로 상수 타입을 선언하고 그 안에 상수들을 정의하는 것입니다. 이 방식은 아예 새로운 타입을 선언하기 때문에 호환되는 컴파일 타입이 없습니다. 
그리고 이 기법은 MyType.TYPE1처럼 열거형을 먼저 적고 상수를 적기 때문에 상수들을 한 곳에 모아놓고 사용할 수 있다는 장점도 있습니다.

## 4. 클래스의 static 상수 사용하기
{% highlight java %}
public abstract class MyAbstractClass{
    public static int TYPE1 = 1;
    public static int TYPE2 = 2;
    public static int TYPE3 = 3;
    
    private int type;
    
    public void getType(){
        return this.type;
    } 
}
public class Type1 extends MyAbstractClass{
    public Type1(){
        this.type = MyAbstractClass.TYPE1;
    }
}
public class Type2 extends MyAbstractClass{
    public Type2(){
        this.type = MyAbstractClass.TYPE2;
    }
}
public class Type3 extends MyAbstractClass{
    public Type3(){
        this.type = MyAbstractClass.TYPE3;
    }
}

void branchWithClass(MyAbstractClass branchFactor){
    switch(branchFactor.getType()){
        case MyAbstractClass.TYPE1:
            doSomething1();
        break;
        case MyAbstractClass.TYPE2:
            doSomething2();
        break;
        case MyAbstractClass.TYPE3:
            doSomething3();
        break;
        default:
        break;
    }
}

branchWithClass(new Type1());
branchWithClass(new Type2());
branchWithClass(new Type3());
{% endhighlight %}

이 기법은 3번의 기법과 거의 같은 방법입니다.
열거형 대신 추상 클래스나 클래스를 정의해서 static 변수를 만들어놓고 사용하는 방법입니다.
하지만 별로 추천하는 방법은 아닙니다.

여기까지 나온 방법들은 모두 switch-case 분기 구문을 사용합니다.
이렇게 분기 구문을 사용하면 타입의 개수가 변경될 때마다 그 타입을 사용하는 모든 코드들을 변경해야한다는 아주 큰 단점이 있습니다.
코드의 재사용성 및 유지보수 측면에서 굉장히 좋지 않습니다.

위 문제를 해결하는 방법으로 객체지향 프로그래밍의 다형성을 사용할 수 있습니다.

## 5. 클래스로 다형성 사용하기
{% highlight java %}
public interface MyInterface{
    void doSomething();
}
public class MyType1{
    public void doSomething();
}
public class MyType2{
    public void doSomething();
}
public class MyType3{
    public void doSomething();
}

void branchWithClass(MyInterface branch){
    branch.doSomething();
}

branchWithClass(new MyType1());
branchWithClass(new MyType2());
branchWithClass(new MyType3());
{% endhighlight %}

이 기법은 클래스 다형성을 사용하는 기법입니다. 
이 기법을 사용하면 앞의 1~4번 기법의 문제를 한 번에 해결할 수 있습니다.
하지만 이 기법에도 약간의 단점은 있습니다. 하나의 타입을 생성할 때마다 새로운 클래스를 만들어야 한다는 것입니다.
그래서 작은 계산과 같이 짧은 코드에는 적합하지 않을 수 있습니다.    

## 6. 열거형으로 다형성 사용하기
{% highlight java %}
public enum MyEnum{
    MY_TYPE1{
        public abstract void doSomething(){
            ...    
        } 
    },
    MY_TYPE2{
        public abstract void doSomething(){
            ...    
        } 
    },
    MY_TYPE3{
        public abstract void doSomething(){
            ...    
        } 
    }
    
    public abstract void doSomething();
}

void branchWithClass(MyEnum branch){
    branch.doSomething();
}

branchWithClass(MyEnum.MY_TYPE1);
branchWithClass(MyEnum.MY_TYPE2);
branchWithClass(MyEnum.MY_TYPE3);
{% endhighlight %}
 
이 기법은 열거형을 이용한 다형성을 사용하는 기법입니다.
이 기법은 5번 기법과 다르게 열거형을 정의할 때 모든 타입을 하나의 파일에서 정의할 수 있습니다.
그래서 아주 많은 종류의 짧은 계산에 대한 분기를 해야할 때는 5번보다 이 기법이 훨씬 효율적입니다.

## 결론
앞에서 보신 것처럼 분기 기법은 크게 상수를 사용하는 기법과 다형성을 이용하는 기법이 있습니다.
상수를 사용하는 기법은 가능하면 유지보수가 상대적으로 편리한 열거형을 사용하시는 것이 좋습니다.
다형성을 사용하는 기법은 각 분기에서 처리해야할 것이 많거나 분기마다 변수들을 사용하고 싶을때는 클래스 다형성을 이용하시는 것이 좋습니다.
그렇지 않고 간단하지만 많은 분기에 대해서는 열거형을 사용한 다형성 기법이 효율적입니다.
