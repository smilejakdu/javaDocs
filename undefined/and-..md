# ✅ 함수형 인터페이스 & 람다.



추상 메소드가 하나만 존재한다면 함수형 인터페이스 입니다.



```java
public interface RunSomething {
    void doIt();
    
    void doItAgain();
}
```

만약 위와 같이 메서드가 두개 존재한다면 함수형 인터페이스가 아닙니다.

인터페이스에서는 abstract 를 원래 생략할 수 있습니다.&#x20;

```java
public interface RunSomething {
    abstract void doIt();
}
```

abstract 를 제가 키워드에 생략했을 뿐이지 추상 메소드라서 이 인터페이스의 구현체를 만들 때 구현해줘야합니다.



## 인터페이스 static

자바 8부터 생긴 기능인데

인터페이스에 static 메소드나 public 키워드도 생략할 수 있습니다.



```java
public interface RunSomething {
    void doIt();
    
    static void printName() {
        System.out.println("ash");
    }
    default void printAge() {
        System.out.println("40");
    }    
}
```

중요한 건 오로지 추상 메소드가 몇개 있냐가 중요하다. 위에선 추상메소드가 `doIt()` 하나만 존재합니다. 한개면 무조건 함수형 인터페이스입니다.



함수형 인터페이스를 정의할 일이 있다면 함수형 인터페이스 위에는 자바에서 제공하는 어노테이션을 붙여줍니다.

```java
@FunctionalInteface
public interface RunSomething {
    void doIt();   
}
```

만약에 위반을 하게 되면 컴파일할때 에러가 발생한다.

<figure><img src="../.gitbook/assets/스크린샷 2024-01-11 오후 6.46.14.png" alt=""><figcaption></figcaption></figure>

그렇기 때문에 `void doIt()` 하나만 두게 된다.&#x20;

위의 인터페이스르 활용해보자 .

람다를 사용할 수 있습니다.

```java
class Main {
    public static void main(String[] args) {
        RunSomething runSomething = () -> System.out.pringln("Hello World");
        runSomething.doIt();
    }
}
```

위와 같이했을때 Hello World 가 출력이 된다.

그리고 인터페이스를 변경해보도록 하자.&#x20;

```java
@FunctionalInterface
public interface RunSomething {
    int doIt(int number);
}
```

위와 같이 인터페이스를 변경했으면 구현하는쪽에서 어떠한 값을 하나 받고 리턴하게 된다.



## 람다

<figure><img src="../.gitbook/assets/스크린샷 2024-01-11 오후 11.19.28.png" alt=""><figcaption></figcaption></figure>

만약 매개변수가 하나뿐인 경우에는 괄호() 를 생략할 수 있습니다.

단, 매개변수의 타입이 있으면 괄호()를 생략할 수 없습니다.

<figure><img src="../.gitbook/assets/스크린샷 2024-01-11 오후 11.20.28.png" alt=""><figcaption></figcaption></figure>

다양하게 변환이 가능하다.



<figure><img src="../.gitbook/assets/스크린샷 2024-01-11 오후 11.22.38.png" alt=""><figcaption></figcaption></figure>

