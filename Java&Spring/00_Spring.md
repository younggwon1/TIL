# Spring

> 정확하게는 스프링 프레임워크(Spring Framework)이다.

- 자바(JAVA) 플랫폼을 위한 오픈소스(Open Source) 애플리케이션 프레임워크(Framework)
- 자바 엔터프라이즈 개발을 편하게 해주는 오픈 소스 경량급 애플리케이션 프레임워크
- 자바 개발을 위한 프레임워크로 종속 객체를 생성해주고, 조립해주는 도구
- 자바로 된 프레임워크로 자바SE로 된 자바 객체(POJO)를 자바EE에 의존적이지 않게 연결해주는 역할 (POJO(Plain Old Java Object) : 평범한 자바빈즈(Javabeans) 객체를 의미함)



### Spring 특징

- **경량 컨테이너로서 자바 객체를 직접 관리**. 각각의 객체 생성, 소멸과 같은 라이프 사이클을 관리하며 스프링으로부터 필요한 객체를 얻어올 수 있다.
- **스프링은 POJO(Plain Old Java Object) 방식의 프레임워크**. 일반적인 J2EE 프레임워크에 비해 구현을 위해 특정한 인터페이스를 구현하거나 상속을 받을 필요가 없어 기존에 존재하는 라이브러리 등을 지원하기에 용이하고 객체가 가볍다.
- **스프링은 제어 반전(IoC : Inversion of Control)을 지원**. 컨트롤의 제어권이 사용자가 아니라 프레임워크에 있어서 필요에 따라 스프링에서 사용자의 코드를 호출한다.
- **스프링은 의존성 주입(DI : Dependency Injection)을 지원**. 각각의 계층이나 서비스들 간에 의존성이 존재할 경우 프레임워크가 서로 연결시켜준다.
- **스프링은 관점 지향 프로그래밍(AOP : Aspect-Oriented Programming)을 지원**. 따라서 트랜잭션이나 로깅, 보안과 같이 여러 모듈에서 공통적으로 사용하는 기능의 경우 해당 기능을 분리하여 관리할 수 있다.
- **스프링은 영속성과 관련된 다양한 서비스를 지원**. iBatis나 Hibernate 등 이미 완성도가 높은 데이터베이스 처리 라이브러리와 연결할 수 있는 인터페이스를 제공한다.
- **스프링은 확장성이 높음**. 스프링 프레임워크에 통합하기 위해 간단하게 기존 라이브러리를 감싸는 정도로 스프링에서 사용이 가능하기 때문에 수많은 라이브러리가 이미 스프링에서 지원되고 있고 스프링에서 사용되는 라이브러리를 별도로 분리하기도 용이하다.



# 주요 구성 요소 & DI

1. **Spring의 핵심 개념**

   - DI

   - IoC

   - AOP & AOP Proxy

   - AOP in Spring

     1) 주요 구성 요소

     - IoC / DI
     - AOP
     - PSA



2. **DI(Dependency Injection, 의존성 주입)**

> 각 클래스간의 의존관계를 빈 설정(Bean Definition) 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것을 말함. DI를 잘 지원해주는 것이 Spring이다. 
>
> - 개발자들은 단비 빈 설정(Bean Definition)파일에서 의존관계가 필요하다는 정보를 추가하면 된다.
> - 객체 레퍼런스를 컨테이너로부터 주입 받아서, 실행 시에 동적으로 의존관계가 생성된다.
> - 컨테이너가 흐름의 주체가 되어 애플리케이션 코드에 의존관계를 주입해주는 것이다.

- **일체형**

  - Composition : HAS-A 관계

  - A가 B를 생성자에서 생성하는 관계

    ```java
    SPhone a = new SPhone();
    ```

    

- **분리 / 도킹(부착)형**

  - Association 관계

  - A객체가 다른 녀석이 만든 B 객체를 사용. 

    ```java
    Battery b = new Battery(); //Dependency
    SPhone a = new SPhone();
    
    a.setBattery(b); //Injection
    
    //예제 설명
    //A를 스마트폰, B를 배터리라고 하면, 일체형은 A라는 객체의 내부 프로세스에 대해 신경 쓸 필요가 없으며, 분리형은 A와 B를 개별적으로 세팅해 주어야 함. 단 분리형은 내가 원하는 것으로 바꾸어 부착할 수 있음. 이것을 DI의 개념이라 보면 됨.
    
    //따라서 위의 예제처럼 분리/도킹형으로 개발을 하게되면 각 객체(또는 애플리케이션)간의 결합도를 낮출 수 있으며, DI를 사용하는 목적이 이러한 결합도를 낮추기 위함.
    ```

    

- **DI의 종류**

  - **Setter Injection (세터 주입)**

    > 의존성을 입력받는 setter 메서드를 만들고 이를 통해서 의존성을 주입한다.

    ```JAVA
     B b = new B();
    
     A a = new A();
    
     a.setB(b);
    ```

  - **Construction Injection (생성자 주입)**

    > 필요한 의존성을 포함하는 클래스의 생성자를 만들고 이를통해 의존성을 주입한다.

    ```java
     B b = new B();
    
     A a = new A(b);
    ```

  - **Method Injection (메서드 주입)**

    > 의존성을 입력받는 일반 메서드를 만들고 이를통해 의존성을 주입한다.



- **Spring DI 컨테이너의 개념**

  > Spring DI 컨테이너가 관리하는 객체를 빈(Bean)이라고 하고, 이 빈(Bean)들을 관리하는 의마로 컨테이너를 빈 팩토리(Bean Factory)라고 부른다.
  >
  > 객체의 생성과 객체 사이의 런타임(run-time) 관계를 DI관점에서 볼 때는 컨테이너를 Bean Factory라고 한다.
  >
  > Bean Factory에 여러가지 컨테이너 기능을 추가하여 애플리케이션 컨텍스트(Application Context)라고 부름.

```
컨테이너란?
컨테이너는 보통 인스턴스의 생명주기를 관리, 생성된 인스턴스들에게 추가적인 기능을 제공하도록 하는 것이다.
즉, 컨테이너란 당신이 작성한 코드의 처리과정을 위임받은 독립적인 존재라고 생각하면 된다. 컨테이너는 적절한 설정만 되어 있다면 누구의 도움없이도 프로그래머가 작성한 코드를 스스로 참조한 뒤 알아서 객체의 생성과 소멸을 컨트롤해준다.

Spring 컨테이너(IoC 컨테이너) :  IoC 방식으로 Bean을 관리한다는 의미에서 Application Context나 Bean Factory를 의미. Spring Framework의 핵심부에 위치하며, 종속객체 주입을 이용하여 애플리케이션을 구성하는 컴포넌트들을 관리한다.

Spring 컨테이너의 종류
1) Bean Factory
Bean(오브젝트)의 생성과 관계 설정 제어를 담당하는 IoC오브젝트
단순히 컨테이너에서 객체를 생성하고 DI를 처리해주는 기능만을 제공한다.
Bean을 등록, 생성, 조회, 반환, 관리함
Bean을 생성하고 분배하는 책임을 지는 클래스
Bean을 조회할 수 있는 getBean() 메서드가 정의되어 있음

2) Application Context
DI를 위한 빈 팩토리에 엔터프라이즈 애플리케이션을 개발하는 데 필요한 여러 가지 컨테이너 기능을 추가한 것
Bean을 등록, 생성, 조회, 반환, 관리함(Bean Factory와 동일)
국제화가 지원되는 텍스트 메세지를 관리해줌
이미지와 같은 파일 자원을 로드할 수 있는 포괄적인 방법을 제공
listener로 등록된 Bean에게 이벤트 발생을 알려줌

따라서 대부분의 Application에서는 Bean Factory보다는 Application Context를 사용하는 것이 좋다.

3) 실제로 사용되는 구현 클래스
- ClassPathXmlApplicationContext : 클래스패스에 위치한 xml 파일에서 컨텍스트 정의 내용을 읽어들인다.
- FileSystemXmlApplicationContext : 파일 경로로 지정된 xml 파일에서 컨텍스트 정의 내용을 읽어들인다.
- XmlWebApplicationContext : 웹 애플리케이션에 포함된 xml 파일에서 컨텍스트 정의 내용을 읽어들인다.
- GenericXmlApplicationContext : xml 파일을 설정 정보로 사용하는 스프링 컨테이너 구현 클래스이다. 독립형 애플리케이션을 개발할 때 사용한다.
- AnnotationConfigApplicationContext : 그루비 언어로 작성된 설정 정보를 사용하는 스프링 컨테이너이다. 독립형 애플리케이션을 개발할 때 사용된다.
- AnnotationConfigWebApplicationContext : 웹 애플리케이션을 개발할 때 사용하는 스프링 컨테이너로써 자바 코드를 설정 정보로 사용한다.

4) Bean Factory와 Application Context 차이점
- Bean Factory : 처음으로 getBean()이 호출된 시점에서야 해당 Bean을 생성(Lazy Loading)
- Application Context : 컨텍스트 초기화 시점에 모든 single tone Bean을 미리 load한 후 애플리케이션 가동 후에는 Bean을 지연없이 얻을 수 있다.(즉시 사용할 수 있도록 보장.)
```



- **Spring에서의 DI**

  > 작은 부품부터 큰 부품으로, 제품을 만드는 순서가 역순 (Inversion of Control).
  > 그래서 IoC라는 이름이 붙었으며, 이러한 일련의 작업을 스프링은 컨테이너라는 곳에 담아서 처리하여 스프링을 IoC 컨테이너라 함.
  >
  > 스프링은 완성품을 만드는 것보다 부품을 모아서 조립하는 것을 도와줌. 즉, 이러한 DI 구조를 개발자가 좀 더 쉽고, 간편하게 사용할 수 있도록 지원해 주는 프레임워크 중의 하나가 스프링
  >
  > - **Spring에서의 DI**  : 부품들을 생성하고, 제품을 조립해주는 공정과정을 대신해 주는 라이브러리(역할자)
  >   - 개발 핵심 처리 루틴의 수정 없이 제품(객체)를 다른 제품(객체)로 쉽게 대체하여 생성 가능하도록 하는 역할을 함
  >   - 명세서(XML)에 따라서 자동적으로 부품을 활용하여 제품을 조립
  >   - 생성하기 원하는 객체를 명세서(XML)에 기술하고, 그 부품과 의존성(Dependency)들을 보관하는 일을 처리. 그러한 데이터를 보관하는 공간을 컨테이너라 함. (IoC 컨테이너)



- **DI 구현**

> 객체의 생성과 도킹에 대한 내용이 소스 코드 상에 있는 것이 아닌 별도의 텍스트 파일(XML 설정 파일)에 분리하여 존재 (JAVA 소스 컴파일 없이 XML 변경만으로 내용 변경 가능)

```java
//JAVA (DI)
//JAVA(DI) : Property 항목은 실제 값을 record에 바로 주입하는 것이 아닌 setRecord() 함수를 호출하여 주입함.

Record record = new SprRecord();
RecordView view = new SprRecordView();
view.setRecord(record); // Injection 
view.input();
view.print();
```



```java
//XML(스프링 DI) : 객체 생성 시, 패키지명을 포함한 풀 클래스 네임 작성.
//XML에 작성된 명세서를 보고, IoC컨테이너가 각 객체를 생성하고, 값을 주입해줌.
//여기서 ApplicationContext 가 IoC컨테이너 역할을 함.


//XML (스프링 DI) config.xml 

<bean id=“record” class=“di.SprRecord”></bean> // 빈 객체 생성 

<bean id=“view” class=“di.SprRecordView”> // 빈 객체 생성 

  <property name=“record” ref=“record”></property> // setRecord() 호출  

</bean>
    


//JAVA 
// XML을 파싱하여 컨테이너에 담는 작업 
ApplicationContext ctx = new ClassPathXmlApplicationContext(“config.xml”);

RecordView = (RecordView) ctx.getBean(“view”);

//- XML을 활용(스프링 DI)하는 경우는 VIEW에 대한 객체만을 요청했을 뿐, 실제 내부적인 사항은 JAVA코드 상에 드러나지 않음.
//- 새로운 클래스의 bean객체를 만들어 XML에 주입만 시켜줘도, 기존 소스 변경 없이 새로운 형태의 객체 적용이 가능함.
```



- **XML (Bean) Sample**

```xml
XML (스프링 DI) config.xml 

* 빈(Bean) 객체는 반드시 클래스를 사용. 인터페이스나 추상클래스는 객체 생성이 불가능함.
* 빈 객체 생성, 객체 초기화, 객체 값(또는 레퍼런스) 주입.


1)
<bean id=“record” name=“r1,r2 r3;r4” class=“di.SprRecord”>
    <property name=“kor” value=“20”></property>
</bean>

2)

<bean id=“record” name=“r1,r2 r3;r4” class=“di.SprRecord”>
    <constructor-arg value=“20”></constructor-arg>
</bean>

3)

<bean id=“record” name=“r1,r2 r3;r4” class=“di.SprRecord”>
    <constructor-arg name=“kor” value=“20”></constructor-arg>
</bean>

4)

<bean id=“record” ” name=“r1,r2 r3;r4” class=“di.SprRecord”
    p:kor=“50” p:eng=“60” p:math=“70”>

5)

<bean id=“view” class=”di.SprRecordView”>
    <property name=“record” ref=“record”></property>
</bean>
 

1) 이름이 record인 빈 객체 생성 / 별명 4개 : r1,r2,r3,r4 / SprReocrd 클래스 객체 생성. 초기값으로 kor 라는 프로퍼티에 20값 대입 (set함수가 존재해야 위와 같은 프로퍼티 설정이 가능).
2) 이름이 record인 빈 객체 생성 / 생성자(인자가 하나인)를 통해서 값 대입 & 생성.
3) 생성자 중에서 kor 값을 입력받는 생성자를 통해서 20값 대입하고, 생성.
4) 3개의 인자를 받는 생성자를 통해 kor = 50, eng = 60, math = 70 대입 & 생성.
5) 생성된 record 객체를 set함수를 통해 프로퍼티에 저장하고 SprRecordView를 생성.
```



