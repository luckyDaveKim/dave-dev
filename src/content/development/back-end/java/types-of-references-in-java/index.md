---
template: post
draft: false
image: img/cover.jpg
date: 2020-11-27 12:50:10 +09:00
title: Java References 종류와 GC (Garbage Collection) 관계 알아보기
excerpt: Java의 References 종류인 Strong, Soft, Weak, Phantom와 GC(Garbage Collection)의 관계에 대해 알아봅니다.
tags:
  - java
  - reference
  - strong_reference
  - soft_reference
  - weak_reference
  - phantom_reference
  - 기술_면접_질문
---

# 개요
이 포스팅에서는 Java의 References 종류인 `Strong Reference`, `Soft Reference`, `Weak Reference`, `Phantom Reference`에 대해 알아보고, GC(Garbage Collection)과의 관계에 대해 알아보겠습니다.   

# Reference 종류는 왜 나누어져 있는가?
Java의 Reference를 나누어 놓은 이유는 효율적인 GC 처리를 위함입니다.  
개발자는 적절한 Reference 사용하여, GC에 의해 제거될 데이터에 우선순위를 적용하여 좀더 효율적인 메모리 관리를 하기 위해 Reference의 종류를 나누어 제공하는 것 입니다.

# Reference의 종류와 사용방법은?
이제 Reference의 종류가 왜 다양하게 존재하는지 이유를 알았으니, 각각의 Reference 종류와 사용방법에 대해 알아보겠습니다.  
Reference는 4가지 종류 `Strong Reference`, `Soft Reference`, `Weak Reference`, `Phantom Reference` 로 구분되어 있으며, 뒤로 갈수록 GC에 의해 제거될 우선순위가 높습니다.  

## Strong Reference 란?
`Strong Reference`는 GC가 실행될 때 unreachable 상태일 경우 GC에 의해 제거될 수 있으며,
사용 방법은 우리가 기본적으로 Java에서 변수를 선언하는 방식으로 다음과 같습니다.  

```java
public class StrongReferenceExample {
  public static void main(String[] args) {
    /* 1) Strong Reference로 생성 */
    Printer printer = new Printer();

    /* 2) print() 메서드 호출 */
    printer.print();

    /*
      3) printer에 null 할당
      이제부터 printer의 heap 데이터는 GC에 의해 제거될 가능성이 있음
    */
    printer = null;
  }

  public static class Printer {
    public void print() {
      System.out.println("printing...");
    }
  }
}
```

1. `Strong Reference`로 `Printer`의 인스턴스를 생성하여 `printer`변수에 할당합니다.
2. `printer`의 메서드 `print()`를 호출합니다.
3. `printer`에 null을 할당하여, Heap 메모리의 `Printer` 인스턴스는 unreachable 상태가 되고, 향후 GC에 의해 제거될 수 있습니다.

위 코드를 실행시 출력은 다음과 같습니다.  
출력 : 
```text
printing...
```

## Soft Reference 란?
[`Soft Reference`](https://docs.oracle.com/javase/8/docs/api/java/lang/ref/SoftReference.html)는 GC가 실행될 때 GC 판단에 의해 메모리가 부족하다고 판단되면 GC에 의해 제거될 수 있으며 (GC 로직에 따라 바로 제거되지 않을 수 있음),
`SoftReference` 인스턴스 생성을 통해 사용 가능하고 사용 방법은 다음과 같습니다.  

```java
public class SoftReferenceExample {
  public static void main(String[] args) {
    /* 1) Strong Reference로 생성 */
    Printer printer = new Printer();

    /* 2) print() 메서드 호출 */
    printer.print();

    /* 3) Soft Reference로 생성 */
    SoftReference<Printer> softReference = new SoftReference<>(printer);

    /* 4) Soft Reference값의 printer() 메서드 호출 */
    softReference.get()
      .print();

    /*
      5) printer에 null 할당
    */
    printer = null;

    /*
      6) GC를 실행합니다.
      `System.gc()`을 호출 하더라도 바로 GC가 동작한다고 보장할수는 없지만, 예제상 GC가 동작하였다고 가정함
    */
    System.gc();

    /*
      7) Soft Reference값의 printer() 메서드 호출
      `NullPointerException`이 날수도 있고, 아닐수도 있음
    */
    softReference.get()
      .print();
  }

  public static class Printer {
    public void print() {
      System.out.println("printing...");
    }
  }
}
```

1. `Strong Reference`로 `Printer`의 인스턴스를 생성하여 `printer`변수에 할당합니다.
2. `printer`의 메서드 `print()`를 호출합니다.
3. `Soft Reference`로 `printer`를 생성합니다.
4. `Soft Reference`인 `printer`를 가져와서 `print()`를 호출합니다.
5. `printer`에 null을 할당합니다.
6. GC를 실행합니다. `System.gc()`을 호출 하더라도 바로 GC가 동작한다고 보장할수는 없지만, 예제상 GC가 동작하였다고 가정합니다.  
   GC의 판단에 의해 메모리가 부족하다고 판단되면, `softPrinter`가 제거됩니다.
7. `Soft Reference`인 `printer`를 가져와서 `print()`를 호출합니다.  
   GC의 판단에 의해 `softReference`가 제거되었다면 `NullPointerException`이 발생하고, 그렇지 않다면 정상적으로 `print()` 메서드가 실행됩니다.

## Weak Reference 란?
[`Weak Reference`](https://docs.oracle.com/javase/8/docs/api/java/lang/ref/WeakReference.html)는 GC가 실행될 때 무조건! GC에 의해 제거되며,
`WeakReference` 인스턴스 생성을 통해 사용 가능하고 사용 방법은 다음과 같습니다.  

> Soft Reference는 GC가 수행될 때 제거될 수도 있고 아닐 수도 있지만,  
> Weak Reference는 GC가 수행될 때 항상 제거됩니다.

```java
public class WeakReferenceExample {
  public static void main(String[] args) {
    /* 1) Strong Reference로 생성 */
    Printer printer = new Printer();

    /* 2) print() 메서드 호출 */
    printer.print();

    /* 3) Weak Reference로 생성 */
    WeakReference<Printer> weakReference = new WeakReference<>(printer);

    /* 4) Weak Reference값의 printer() 메서드 호출 */
    weakReference.get()
      .print();

    /*
      5) printer에 null 할당
    */
    printer = null;

    /*
      6) GC를 실행합니다.
      `System.gc()`을 호출 하더라도 바로 GC가 동작한다고 보장할수는 없지만, 예제상 GC가 동작하였다고 가정함
    */
    System.gc();

    /*
      7) Weak Reference값의 printer() 메서드 호출
      `NullPointerException`이 발생합니다!!!
    */
    softReference.get()
      .print();
  }

  public static class Printer {
    public void print() {
      System.out.println("printing...");
    }
  }
}
```

1. `Strong Reference`로 `Printer`의 인스턴스를 생성하여 `printer`변수에 할당합니다.
2. `printer`의 메서드 `print()`를 호출합니다.
3. `Weak Reference`로 `printer`를 생성합니다.
4. `Weak Reference`인 `printer`를 가져와서 `print()`를 호출합니다.
5. `printer`에 null을 할당합니다.
6. GC를 실행합니다. `System.gc()`을 호출 하더라도 바로 GC가 동작한다고 보장할수는 없지만, 예제상 GC가 동작하였다고 가정합니다.  
   GC에 의해 무조건 `weakPrinter`가 제거됩니다.
7. `Weak Reference`인 `printer`를 가져와서 `print()`를 호출합니다.  
   GC에 의해 `weakReference`가 제거되었기 때문에 `NullPointerException`이 발생합니다.

## Phantom Reference 란?
[`Phantom Reference`](https://docs.oracle.com/javase/8/docs/api/java/lang/ref/PhantomReference.html)는 `Soft Reference`, `Weak Reference`와 다르게 사용하기 위함 보다는, **올바르게 삭제**하고 **삭제 이후 작업을 조작**하기 위함이며,
`PhantomReference.get()` 메서드는 항상 `null`을 반환하며, `PhantomReference`의 referent 객체를 수동으로 `clear()` 메서드를 실행해야하는 특징이 있습니다.  

> `PhantomReference.get()` 메서드 확인해 보면, 다음과 같이 항상 `null`을 반환하도록 되어 있습니다.  
> ```java
> public class PhantomReference<T> extends Reference<T> {
>   // ...
> 
>   public T get() {
>     return null;
>   }
> }
> ```

여기서 다음 두가지에 대해 짚고 넘어가도록 하겠습니다. "올바르게 삭제"와 "삭제 이후 작업을 조작" 한다는 것이 무슨 뜻일까요?  

* 올바르게 삭제
Java의 최상위 객체인 `Object`에는 여러 메서드들이 있는데 그중 `finalize()` 메서드가 있습니다. 이 메서드는 GC에 의해 호출 되지만, 몇가지 문제점이 있습니다.  
첫째, 언제 실행이 될지 모릅니다.  
둘째, GC에 따라 실행이 되지 않을 수 있습니다.  
셋째, 예외가 발생되면 무시됩니다.  
넷째, 성능 저가하 발생할 수 있습니다.  
다섯째, 잘못된 구현시 문제가 발생할 수 있습니다.  
잘못된 구현의 예로는 GC에 의해 제거된 객체가 부활(resurrect)하는 것인데, 이는 `finalize()` 메서드에 `Strong Reference`를 갖도록 코딩되어 있을 경우 발생합니다.  
하지만 `Phantom Reference`는 메모리에서 해지된 후 `enqueue`되기 때문에 `finalize()`에서 객체가 부활하지 않습니다.  
(`Phantom Reference`의 `enqueue`에 대해서는 아래에서 좀더 자세히 알아보겠습니다.)

* 삭제 이후 작업을 조작
대용량의 데이터를 연속적으로 조작하는 경우, 데이터의 크기가 너무 커서 선행된 데이터가 삭제된 후 이후 데이터를 조작해야 하는 경우가 있을 수 있습니다.  
이럴 경우 `Phantom Referece`를 사용하여 메모리에서 데이터가 삭제된 후(삭제 될 때까지 대기 후) 이후 작업을 진행하도록 조작 할 수 있습니다.  














GC가 실행될 때 GC 판단에 의해 메모리가 부족하다고 판단되면 GC에 의해 제거될 수 있으며 (GC 로직에 따라 바로 제거되지 않을 수 있음),
`SoftReference` 인스턴스 생성을 통해 사용 가능하고 사용 방법은 다음과 같습니다.  










`Strong Reference`는 GC가 실행될 때, unreachable 상태일 경우 GC에 의해 제거될 수 있으며,
사용 방법은 우리가 기본적으로 Java에서 변수를 선언하는 방식으로 다음과 같습니다.  



Java의 접근 제어자의 종류에 대해 알아보고, 각 접근제어자별 차이점에 대해 알아보도록 하겠습니다.

# 접근 제어자(Access Modifier) 란?
Java의 인터페이스, 클래스, 메서드 그리고 변수 앞에 선언하며, 접근할 수 있는 범위를 한정해 주는 역할을 합니다.  
접근 제어자의 종류는 4가지로 `private` < `default` < `protected` < `public` 가 존재하며, 오른쪽으로 갈수록 접근 허용 범위가 커집니다.  
이러한 접근제어자를 통해 클래스 외부에 불필요한 노출을 줄일 수 있으며, 보다 유지보수가 용이한 프로그래밍이 가능하도록 도와줍니다.  
또한, 접근 제어를 통해 객체지향 프로그래밍(OOP, Object Oriented Programing)의 4대 특성 중 하나인, 캡슐화(Encapsulation)를 가능하도록 합니다.

# 접근 제어자의 종류 및 예시
접근 제어자의 종류와 특성을 예시를 통해 알아보도록 하겠습니다.  
예시에 사용될 프로젝트 구조는 다음과 같습니다.
```text
.
└─ com.example
│ └─ home
│ │ └─ IMyMeMine.java
│ │ └─ Main.java
│ └─ Child.java
│ └─ Dog.java
│ └─ Cow.java
```

## Private 제어자
`private` 제어자가 붙은 인터페이스, 클래스, 메서드 그리고 변수는 해당 클래스 내에서만 접근이 가능합니다.  
**즉, `private` 제어자의 접근 허용 범위는 동일 클래스입니다.**

![private](img/private.png)

`IMyMeMine` 클래스를 살펴보겠습니다.
```java
public class IMyMeMine {
  private String name;

  IMyMeMine() {
    /* private 변수 사용 */
    String myName = this.name;

    /* private 메서드 사용 */
    this.say();

    /* private 클래스 사용 */
    Wallet myWallet = new Wallet();
  }

  private void say() {}

  private interface Storable {
    void store();
  }

  /* private 인터페이스 사용 */
  private static class Wallet implements Storable {
    public Wallet() {}

    @Override
    public void store() {}
  }
}
```
`IMyMeMine` 클래스 내부에서, 동일 클래스 내의 `private` 제어자를 자유롭게 접근할 수 있습니다.

이번에는 `Main` 클래스를 살펴보겠습니다.
```java
public class Main {
  public static void main(String[] args) {
    IMyMeMine iMyMeMine = new IMyMeMine();

    /* private 변수 사용불가 */
    // String myName = iMyMeMine.name;

    /* private 메서드 사용불가 */
    // iMyMeMine.say();

    /* private 클래스 사용불가 */
    // IMyMeMine.Wallet wallet = new IMyMeMine.Wallet();
  }
  
  /* private 인터페이스 사용불가 */
  // public static class NestedClass implements IMyMeMine.Storable {
  //   @Override
  //   public void store() {}
  // }
}
```
`Main` 클래스는 `IMyMeMine` 클래스와 동일 클래스가 아니므로, `Main` 클래스 에서는 `IMyMeMine` 클래스 내부의 `private` 제어자로 선언된 클래스, 인터페이스, 메서드 그리고 변수를 사용할 수 없습니다.

## Default 제어자
`default` 제어자는 아무 접근 제어자도 설정하지 않을 경우에 `default` 제어자가 설정 되며, `default` 제어자가 붙은 인터페이스, 클래스, 메서드 그리고 변수는 `private` 접근 권한 + 동일 패키지 내에서만 접근이 가능합니다.  
**즉, `default` 제어자의 접근 허용 범위는 동일 클래스 + 동일 패키지입니다.**

![default](img/default.png)

`IMyMeMine` 클래스를 살펴보겠습니다.
```java
class IMyMeMine {
  String name;

  IMyMeMine() {
    /* default 변수 사용 */
    String myName = this.name;

    /* default 메서드 사용 */
    this.say();

    /* default 클래스 사용 */
    Wallet myWallet = new Wallet();
  }

  void say() {}

  interface Storable {
    void store();
  }

  /* default 인터페이스 사용 */
  static class Wallet implements Storable {
    public Wallet() {}

    @Override
    public void store() {}
  }
}
```
`default` 제어자는 `private` 제어자의 접근 권한을 포함하므로, 클래스 내부에서는 `default` 제어자를 자유롭게 접근할 수 있습니다.

이번에는 `Main` 클래스를 살펴보겠습니다.
```java
public class Main {
  public static void main(String[] args) {
    IMyMeMine iMyMeMine = new IMyMeMine();

    /* private 변수 사용 */
    String myName = iMyMeMine.name;

    /* private 메서드 사용 */
    iMyMeMine.say();

    /* private 클래스 사용 */
    IMyMeMine.Wallet wallet = new IMyMeMine.Wallet();
  }
  
  /* private 인터페이스 사용 */
  public static class NestedClass implements IMyMeMine.Storable {
    @Override
    public void store() {}
  }
}
```
`Main` 클래스와 `IMyMeMine` 클래스는 동일 패키지에 있으므로, `private` 제어자 때와는 다르게 `Main` 클래스에서 `IMyMeMine` 클래스의 `default` 제어자로 선언된 클래스, 인터페이스, 메서드 그리고 변수 사용이 가능합니다.

이번에는 `Child` 클래스를 살펴보겠습니다.
```java
class Child {
  Child() {
    /* default 클래스 사용불가 */
    // IMyMeMine iMyMeMine = new IMyMeMine();

    /* default 변수 사용불가 */
    // String myName = iMyMeMine.name;

    /* default 메서드 사용불가 */
    // iMyMeMine.say();

    /* default 클래스 사용불가 */
    // IMyMeMine.Wallet wallet = new IMyMeMine.Wallet();
  }

  /* default 인터페이스 사용불가 */
  // public static class NestedClass implements IMyMeMine.Storable {
  //   @Override
  //   public void store() {}
  // }
}
```
`Child` 클래스는 `IMyMeMine` 클래스와 다른 패키지에 있으므로, `Child` 클래스에서 `IMyMeMine` 클래스의 `default` 제어자로 선언된 클래스, 인터페이스, 메서드 그리고 변수를 사용할 수 없습니다.

## Protected 제어자
`protected` 제어자가 붙은 인터페이스, 클래스, 메서드 그리고 변수는 `default` 접근 권한 + 자식 클래스에서 접근이 가능합니다.  
**즉, `protected` 제어자의 접근 허용 범위는 동일 클래스 + 동일 패키지 + 자식 클래스입니다.**

![protected](img/protected.png)

`IMyMeMine` 클래스를 살펴보겠습니다.
```java
public class IMyMeMine {
  protected String name;

  protected IMyMeMine() {
    String myName = this.name;
    this.say();
    Wallet myWallet = new Wallet();
  }

  protected void say() {}

  protected interface Storable {
    void store();
  }

  protected static class Wallet implements Storable {
    public Wallet() {}

    @Override
    public void store() {}
  }
}
```
`protected` 제어자는 `default` 제어자의 접근 권한을 포함하므로, 클래스 내부에서는 `protected` 제어자를 자유롭게 접근할 수 있습니다.

이번에는 `Child` 클래스를 살펴보겠습니다.
```java
class Child extends IMyMeMine {
  Child() {
    /* protected 변수 사용 */
    String myName = this.name;

    /* protected 메서드 사용 */
    this.say();

    /* protected 클래스 사용 */
    Wallet myWallet = new Wallet();
  }

  /* protected 인터페이스 사용 */
  public static class NestedClass implements IMyMeMine.Storable {
    @Override
    public void store() {
    }
  }
}
```
`Child` 클래스는 `IMyMeMine` 클래스를 상속하였으므로, `Child` 클래스에서 `IMyMeMine` 클래스의 `protected` 제어자로 선언된 클래스, 인터페이스, 메서드 그리고 변수 사용이 가능합니다.

이번에는 `Dog` 클래스를 살펴보겠습니다.
```java
class Dog {
  Dog() {
    /* protected 변수 사용불가 */
    // String myName = this.name;

    /* protected 메서드 사용불가 */
    // this.say();

    /* protected 클래스 사용불가 */
    // Wallet myWallet = new Wallet();
  }

  /* protected 인터페이스 사용불가 */
  // public static class NestedClass implements Storable {
  //   @Override
  //   public void store() {}
  // }
}
```
`Dog` 클래스는 `IMyMeMine` 클래스를 상속하지 않았으므로, `Dog` 클래스에서 `IMyMeMine` 클래스의 `protected` 제어자로 선언된 클래스, 인터페이스, 메서드 그리고 변수를 사용할 수 없습니다.

## Public 제어자
`public` 제어자가 붙은 인터페이스, 클래스, 메서드 그리고 변수는 동일 프로젝트 내에서는 어디서든 자유롭게 접근이 가능합니다.

![public](img/public.png)

`IMyMeMine` 클래스를 살펴보겠습니다.
```java
public class IMyMeMine {
  public String name;

  public IMyMeMine() {
    String myName = this.name;
    this.say();
    Wallet myWallet = new Wallet();
  }

  public void say() {}

  public interface Storable {
    void store();
  }

  public static class Wallet implements Storable {
    public Wallet() {}

    @Override
    public void store() {}
  }
}
```

이번에는 `Cow` 클래스를 살펴보겠습니다.
```java
class Cow {
  public Cow() {
    IMyMeMine iMyMeMine = new IMyMeMine();

    /* public 변수 사용 */
    String myName = iMyMeMine.name;

    /* public 메서드 사용 */
    iMyMeMine.say();

    /* public 클래스 사용 */
    IMyMeMine.Wallet wallet = new IMyMeMine.Wallet();
  }

  /* public 인터페이스 사용 */
  public static class NestedClass implements IMyMeMine.Storable {
    @Override
    public void store() {}
  }
}
```
`IMyMeMine` 클래스와 `Cow` 클래스에서 모두 `IMyMeMine` 클래스의 `public` 제어자로 선언된 클래스, 인터페이스, 메서드 그리고 변수 사용이 가능합니다.

# 요약
지금까지 예시를 통한 각각의 접근 제어자에 대해 알아보았습니다.  
접근 제어자별 접근 가능한 권한을 표로 한눈에 나타내자면 다음과 같습니다.  

| 구분 \ 접근 제어자 | private | default | protected | public |
|:---:|:---:|:---:|:---:|:---:|
| 동일 클래스 | O | O | O | O |
| 동일 패키지 | X | O | O | O |
| 자식 클래스 | X | X | O | O |
| 모든 클래스 | X | X | X | O |
