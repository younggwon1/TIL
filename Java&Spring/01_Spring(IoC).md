# IoC(Inversion of Control)

> 객체의 생성, 생명주기의 관리까지 모든 객체에 대한 제어권이 바뀌었다는 것을 의미한다.
>
> 다시한번 말해서 제어의 역전이란, 인스턴스 생성부터 소멸까지의 인스턴스 생명주기 관리를 개발자가 아닌 컨테이너가 대신 해준다는 의미이다. 즉, 컨테이너 역할을 해주는 프레임워크에게 제어하는 권한을 넘겨서 개발자의 코드가 신경써야할 것을 줄이는 전략이다.
>
> 즉, 외부(컨테이너)에서 제어를 한다.
>
> 다시 생각해보면, 어떠한 Library를 사용하고 싶다고 가정한다면 개발자는 이를 사용하기 위해서는 Library를 호출한다. 즉 주도권 자체가 개발자가 가지고 있다. 하지만 SpringFrameWork에서 IoC는 프레임워크 내에 라이브러리가 구성되어 있고 프레임워크에서 개발자가 작성한 코드를 호출하기 때문에 주도권이 프레임워크에 있다.



```
IoC 컨테이너는 다른 용어로 Bean Factory, Application Context라고도 불림.
스프링의 IoC 컨테이너는 일반적으로 Application Context를 말함.
 
* Bean Factory를 Application Context 또는 IoC컨테이너라 말하기도 하지만, 사실 Application Context는 빈을 좀 더 확장한 개념.
* Application Context는 그 자체로 IoC와 DI를 위한 Bean Factory이면서 그 이상의 기능을 가짐.
* ApplicationContext 인터페이스는 BeanFactory 인터페이스를 상속한 서브인터페이스.
* 실제로 스프링 컨테이너 또는 IoC 컨테이너라고 말하는 것은 바로 이 ApplicationContext 인터페이스를 구현한 클래스의 오브젝트.
* 컨테이너가 본격적인 IoC 컨테이너로서 동작하려면 POJO클래스와 설정 메타정보가 필요.
(스프링 애플리케이션 : POJO 클래스와 설정 메타정보를 이용해 IoC 컨테이너가 만들어주는 오브젝트의 조합.)
```



![캡처](https://user-images.githubusercontent.com/42603919/78347818-fd871f00-75db-11ea-973a-db73a50b75e3.PNG)



```
* DL(Dependecy Lookup) - JNDI 같은 저장소에 의하여 관리되고 있는 bean을 개발자들이 직접 컨테이너(Container)에서 제공하는 API를 이용하여 lookup하는 것을 말함. 따라서 container와의 종속성이 생김. (JNDI 컨테이너에 의존성이 강하다.)
오브젝트간에 디커플링(decoupling)을 해주는 면에서 장점이 있지만 이렇게 만들어진 오브젝트는 컨테이너 밖에서 실행 할 수 없고 JNDI외의 방법을 사용할 경우 JNDI관련 코드를 오브젝트내에 일일히 변경해 줘야 하며 테스트하기 매우 어렵고 코드 양이 매우 증가하고 매번 Casting해야 하고 NamingException같은 checked exception을 처리하기 위해서 exception처리구조가 매우 복잡해지는 단점이 있음.
EJB container, Spring container 에서 지원.

 
* DI (Dependecy Injection) – 각 class 사이의 의존관계를 빈 설정 정보를 바탕으로 container가 자동적으로 연결해 주는 것을 말함. 따라서 lookup과 관련된 코드들이 오브젝트 내에서 완전히 사라지고 컨테이너에 의존적이지 않은 코드를 작성할 수 있음.

 단지 빈 설정 파일에서 의존관계가 필요하다는 정보를 추가하면 됨.
1) Setter Injection – 클래스 사이의 의존관계를 연결시키기 위하여 setter 메소드를 이용하는 방법. property tag 사용.
2) Constructor Injection – 클래스 사이의 의존관계를 연결시키기 위하여 constructor를 이용하는 방법. constructor-arg tag 사용.
3) Method Injection – 싱글톤(Singleton) 인스턴스와 non singleton 인스턴스의 의존관계를 연결시킬 필요가 있을 때 사용하는 방법.
```

