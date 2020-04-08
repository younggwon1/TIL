# Overloading

우리의 계산기는 2개의 값(left, right)에 대한 연산(sum, avg) 만을 수행 할 수 있다. 그런데 만약 3개의 값을 대상으로 연산을 해야 한다면 어떻게 해야할까? 우선 아래와 같이 입력값을 3개 받아야 할 것이다. 

```java
c1.setOprands(10, 20, 30);
```



이를 위해서 기존의 setOprands 메소드를 아래와 같은 모습을 수정한다면 2개의 입력값을 받을 수 없게 될 것이다.



```java
public void setOprands(int left, int right, int third){
    this.left = left;
    this.right = right;
    this.third = third;
}
```



이런 경우 아래와 같이 메소드의 이름을 변경하면 될 것이다.



```java
c1.setOprands2(10, 20);
c1.setOprands3(10, 20, 30);
```



이것도 좋은 방법이지만 매개변수의 수에 따라서 메소드의 이름이 달라지는 것은 왠지 깔끔한 방법이 아닌 것 같다. 그럼 어떻게 해야 좋을까? 코드를 보자.



```java
package org.opentutorials.javatutorials.overloading.example1;

class Calculator{
    int left, right;
    int third = 0;
     
    public void setOprands(int left, int right){
        System.out.println("setOprands(int left, int right)");
        this.left = left;
        this.right = right;
    }
    
    public void setOprands(int left, int right, int third){
        System.out.println("setOprands(int left, int right, int third)");
    	this.left = left;
        this.right = right;
        this.third = third;
    }
    
    public void sum(){
        System.out.println(this.left+this.right+this.third);
    }
     
    public void avg(){
        System.out.println((this.left+this.right+this.third)/3);
    }
}
 
public class CalculatorDemo {
     
    public static void main(String[] args) {
         
        Calculator c1 = new Calculator();
        c1.setOprands(10, 20);
        c1.sum();       
        c1.avg();
        c1.setOprands(10, 20, 30);
        c1.sum();       
        c1.avg();
        
    }
 
}
```



실행 결과는 아래와 같다.



```java
setOprands(int left, int right)
30
15
setOprands(int left, int right, int third)
60
30
```

아래 코드를 보자.

```java
c1.setOprands(10,20); 
```

이 코드의 실행 결과는 화면에 아래와 같은 메시지를 출력한다.

```java
setOprands(int left, int right)
```

다음 코드를 보자.

```java
c1.setOprands(10, 20, 30);
```

실행 결과는 아래와 같다.

```java
setOprands(int left, int right, int third)
```

이를 통해서 알 수 있는 것은 매개변수의 숫자에 따라서 같은 이름의, 서로 다른 메소드를 호출하고 있다는 것을 알 수 있다.

이름은 같지만 시그니처는 다른 메소드를 중복으로 선언 할 수 있는 방법을 메소드 오버로딩(overloading)이라고 한다. 



### 오버로딩의 규칙

결론적으로 말하면 메소드 오버로딩은 매개변수를 사용한다. 즉 매개변수가 다르면 이름이 같아도 서로 다른 메소드가 되는 것이다. 반면에 매개변수는 같지만 리턴타입이 다르면 오류가 발생한다. 아래의 코드를 보자.

```java
package org.opentutorials.javatutorials.overloading.example1;
public class OverloadingDemo {
    void A (){System.out.println("void A()");}
	void A (int arg1){System.out.println("void A (int arg1)");}
	void A (String arg1){System.out.println("void A (String arg1)");}
	//int A (){System.out.println("void A()");}
	public static void main(String[] args) {
		OverloadingDemo od = new OverloadingDemo();
		od.A();
		od.A(1);
		od.A("coding everybody");
	}
}
```

3행과 4행의 메소드 A는 매개변수의 숫자가 다르다. 4행과 5행의 메소드 A는 인자의 숫자는 같지만 매개변수의 데이터 타입이 다르다. 이런 경우는 오버로딩이 가능하다. 메소드를 호출 할 때 전달되는 인자의 데이터 타입에 따라서 어떤 메소드를 호출할지를 자바가 판단 할 수 있기 때문이다. 하지만 메소드의 반환값은 메소드를 호출하는 시점에서 전달되지 않는 정보이기 때문에 오버로딩의 대상이 될 수 없다.

## 상속과 오버로딩

상속의 관계에서도 오버로딩을 사용할 수 있을까? 물론이다. 

```java
package org.opentutorials.javatutorials.overloading.example1;
public class OverloadingDemo2 extends OverloadingDemo{
    void A (String arg1, String arg2){System.out.println("sub class : void A (String arg1, String arg2)");}
	void A (){System.out.println("sub class : void A ()");}
	public static void main(String[] args) {
		OverloadingDemo2 od = new OverloadingDemo2();
		od.A();
		od.A(1);
		od.A("coding everybody");
		od.A("coding everybody", "coding everybody");
		
	}
}
```

실행 결과는 아래와 같다.

```java
sub class : void A ()
void A (int arg1)
void A (String arg1)
sub class : void A (String arg1, String arg2)
```

클래스 OverloadingDemo2는 OverloadingDemo을 상속 받고 있다. OverloadingDemo2의 3행에서 정의된 메소드 A는 문자열을 데이터타입으로 하는 두개의 매개변수를 가지고 있다. 이러한 형태의 변수는 부모 클래스에서는 정의되어 있지 않기 때문에 메소드 오버로딩이 되는 것이다. 반면에 4행에서 정의된 메소드 A는 매개변수가 없다. 그리고 부모 클래스에는 이미 매개변수가 없는 메소드 A가 존재한다. 이 둘은 매개변수의 형태가 같기 때문에 오버로딩이 아니라 오버라이딩에 해당한다.

## overriding VS overloading

riding(올라탄다)을 이용해서 부모 클래스의 메소드의 동작방법을 변경하고, loading을 이용해서 같은 이름, 다른 매개변수의 메소드들을 여러개 만들 수 있다는 사실을 아는 것이 중요하다.



