---
template: post
draft: false
image: img/cover.jpg
date: 2021-07-10 15:57:59 +09:00
title: Spring Bean 등록하는 여러 가지 방법
excerpt: Spring Bean을 XML 및 Java로 등록하는 방법과 컴포넌트 스캔을 활용하는 법에 대해 알아봅니다.
tags:
  - spring_framework
  - spring_bean
  - bean
---

# 개요
 이 포스팅에서는 [Spring Bean](/development/back-end/spring-framework/what-is-the-spring-ioc-container-and-bean)을 등록하는 여러 가지 방법에 대해 알아봅니다.

# Spring Bean 등록 방법
 Spring Bean을 설정하는 방법에는 크게 XML 설정을 통한 방법과 Java 설정을 통한 방법이 있습니다.
그리고 각 설정 방법마다 Bean을 직접 등록하는 방법과 컴포넌트 스캔을 활용하여 등록하는 방법이 있습니다.
 각 방법에 따라 다음 정보를 Bean으로 등록하는 예시를 통해 설정 방법을 알아보겠습니다.  

```
MyService 객체가 MyRepository 의존성을 갖고 있다.
```

 다음과 같이 `MyService`와 `MyRepository` 클래스를 정의합니다.

```java
public class MyService {

  private final MyRepository myRepository;

  public MyService(MyRepository myRepository) {
    this.myRepository = myRepository;
  }

}

public class MyRepository {

}
```

## XML 설정을 통한 방법
 Spring Bean을 XML 설정을 통해 등록할 때는 기본적으로 다음 XML 파일이 필요합니다.  
 resource 폴더 아래에 **application.xml** 파일을 생성합니다. 그리고 `<beans>` 엘리먼트 아래에 등록을 원하는 Bean 정보를 입력합니다.  

 예시 샘플은 다음과 같습니다.
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans ...>
  ...
</beans>
```

 이제 XML 설정을 통해 Bean을 직접 등록하는 방법과 컴포넌트 스캔을 통한 등록 방법에 대해 알아보겠습니다.

### XML에 Bean을 직접 등록
 XML에 등록하고 싶은 Bean 정보를 직접 등록하는 방법입니다.  
 Spring Bean으로 등록을 원하는 정보를 **applicaiton.xml** 파일의 `<beans>` 엘리먼트 아래에 `<bean>` 엘리먼트 추가를 통해 Bean으로 등록할 수 있습니다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans ...>

  <bean id="MyService"
        class="com.example.my.MyService">
    <property name="myRepository"
              ref="myRepository"/>
  </bean>

  <bean id="MyRepository"
        class="com.example.my.MyRepository">
  </bean>

</beans>
```

각 엘리먼트에 등록할 수 있는 정보는 다음과 같습니다.
- `<bean>` 엘리먼트 : 등록할 Bean 정보를 명시합니다.
- `<bean>`의 `id` 속성 : Bean의 이름을 대문자로 시작하는 카멜케이스로 명시합니다.
- `<bean>`의 `class` 속성 : Bean 등록을 원하는 클래스의 위치를 명시합니다.
- `<property>` 엘리먼트 : Bean에 의존성 주입에 대한 정보를 명시합니다.
- `<property>`의 `name` 속성 : 클래스에 정의한 의존성 주입 받을 변수명을 명시합니다.
- `<property>`의 `ref` 속성 : 의존성 주입 받을 Bean의 이름을 소문자로 시작하는 카멜케이스로 명시합니다.

### XML에 컴포넌트 스캔을 통한 Bean 등록
 이번에는 컴포넌트 스캔을 통한 Bean 정보를 등록하는 방법입니다.  
 기존 **application.xml** 파일에 다음과 같이 컴포넌트 스캔에 대한 정보를 입력합니다.

 `<context:component-scan>` 엘리먼트를 등록하여 컴포넌트 스캔을 통한 Bean 등록을 할 것이라고 알려줍니다.
그리고 `base-package` 속성에 컴포넌트 스캔을 진행할 대상 루트 패키지를 작성합니다.
그러면 해당 패키지를 포함한 모든 하위 패키지의 Bean 설정을 스캔하여 등록하게 됩니다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans ...>

  <context:component-scan base-package="com.example.my" />

</beans>
```

## Java 설정을 통한 방법
 이번에는 Spring Bean을 Java 설정을 통해 등록하는 방법에 대해 알아보겠습니다.
Java로 설정하게 되면 여러 어노테이션(Annotation)이 사용되게 됩니다.
Java 설정을 통해 Bean을 직접 등록하는 방법과 컴포넌트 스캔을 통한 등록 방법에 대해 알아보며, 사용되는 어노테이션에 대해 설명하겠습니다.

### Java에 Bean을 직접 등록
 Java에 등록하고 싶은 Bean 정보를 직접 등록하는 방법입니다.
Class를 생성한 후 `@Configuration` 어노테이션을 설정하고 메소드에 `@Bean` 어노테이션을 달아서 Bean을 등록할 수 있습니다.

```java
@Configuration
public class ApplicationConfig {

  @Bean
  public MyService myService(MyRepository myRepository) {
    return new MyService(myRepository);
  }
  
  @Bean
  public MyRepository myRepository() {
    return new MyRepository
  }
  
}
```

각 어노테이션의 정보는 다음과 같습니다.
- `@Configuration` 어노테이션 : 해당 클래스가 Java 설정에 사용되는 클래스라는 것을 의미하며, 클래스명은 자유롭게 지정 가능합니다.
- `@Bean` 어노테이션 : 해당 메소드가 Bean을 등록하는 메소드라는 것을 의미하며, 메소드명을 바탕으로 Bean의 이름이 지정됩니다. 또한, 메소드의 파라미터는
Bean에 등록된 객체가 있다면 자동으로 전달됩니다.

### Java에 컴포넌트 스캔을 통한 Bean 등록
 이번에는 컴포넌트 스캔을 통한 Bean 정보를 등록하는 방법입니다.
이전과 같이 `@Configuration` 어노테이션을 설정하고 `@ComponentScan` 어노테이션을 추가하면 됩니다.

```java
@Configuration
@ComponentScan(basePackages = "com.example.my")
public class ApplicationConfig {
  
}
```

`@ComponentScan`의 속성으로 `basePackages`와 `basePackageClasses`가 있습니다.
- `basePackages`는 XML 설정에서 **base-package**와 동일하게 컴포넌트 스캔을 진행할 대상 루트 패키지를 설정하면 됩니다.
- `basePackageClasses`는 **basePackage**의 타입 세이프(Type Safe) 하지 않은 점을 개선한 것으로, 설정된 클래스의 패키지부터 모든
하위 패키지에 대해 컴포넌트 스캔을 진행하게 됩니다.

그래서 다음과 같이 `basePackageClasses` 속성으로 타입 세이프 하게 등록할 수 있습니다.

```java
@Configuration
@ComponentScan(basePackageClasses = ApplicationConfig.class)
public class ApplicationConfig {
  
}
```

# 줄이며...
 지금까지 Spring Bean을 등록하는 방법에 대해 알아보았습니다. 다음에는 Spring IoC 컨테이너에 대해 자세히 알아보겠습니다.
