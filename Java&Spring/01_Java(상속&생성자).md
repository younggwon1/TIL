# 상속 / 생성자

생성자가 상속을 만나면서 발생한 복잡성을 보여줄 생각이다. 그 맥락에서 super이라는 키워드의 의미도 중요하게 다뤄질 내용이다. 

먼저 기본 생성자의 성질에 대한 이해해보자.



```java
package org.opentutorials.javatutorials.Inheritance.example4;
public class ConstructorDemo {
	public static void main(String[] args) {
		ConstructorDemo  c = new ConstructorDemo();
	}
}
```



위의 예제는 에러를 발생시키지 않는다. ConstructorDemo 객체를 생성할 때 자동으로 생성자를 만들어주기 때문이다. 하지만 아래의 예제는 에러가 발생한다.



```java
package org.opentutorials.javatutorials.Inheritance.example4;
public class ConstructorDemo {
    public ConstructorDemo(int param1) {}
	public static void main(String[] args) {
		ConstructorDemo  c = new ConstructorDemo();
	}
}
```



**매개변수가 있는 생성자**가 있을 때는 자동으로 기본 생성자를 만들어주지 않는다. 따라서 위의 예제는 존재하지 않는 생성자를 호출하고 있다. 이 문제를 해결하기 위해서는 아래와 같이 기본 생성자를 추가해줘야 한다.



```java
package org.opentutorials.javatutorials.Inheritance.example4;
public class ConstructorDemo {
    public ConstructorDemo(){}
	public ConstructorDemo(int param1) {}
	public static void main(String[] args) {
		ConstructorDemo  c = new ConstructorDemo();
	}
}
```



**조금의 수정**

```java
package org.opentutorials.javatutorials.Inheritance.example2;

class Calculator {
    int left, right;

	public void setOprands(int left, int right) {
		this.left = left;
		this.right = right;
	}

	public void sum() {
		System.out.println(this.left + this.right);
	}

	public void avg() {
		System.out.println((this.left + this.right) / 2);
	}
}

class SubstractionableCalculator extends Calculator {
	public SubstractionableCalculator(int left, int right) {
		this.left = left;
		this.right = right;
	}

	public void substract() {
		System.out.println(this.left - this.right);
	}
}

public class CalculatorConstructorDemo4 {
	public static void main(String[] args) {
		SubstractionableCalculator c1 = new SubstractionableCalculator(10, 20);
		c1.sum();
		c1.avg();
		c1.substract();
	}
}
```



SubstractionableCalculator의 생성자로 left와 right의 값을 받아서 초기화시키고 있다. 만약 클래스 Calculator가 메소드 setOprands가 아니라 생성자를 통해서 left, right 값을 설정하도록 하고 싶다면 아래와 같이 코드를 변경해야 할 것이다.



```java
package org.opentutorials.javatutorials.Inheritance.example3;

class Calculator {
    int left, right;
	
	public Calculator(int left, int right){
		this.left = left;
		this.right = right;
	}
	
	public void setOprands(int left, int right) {
		this.left = left;
		this.right = right;
	}

	public void sum() {
		System.out.println(this.left + this.right);
	}

	public void avg() {
		System.out.println((this.left + this.right) / 2);
	}
}

class SubstractionableCalculator extends Calculator {
	public SubstractionableCalculator(int left, int right) {
		this.left = left;
		this.right = right;
	}

	public void substract() {
		System.out.println(this.left - this.right);
	}
}

public class CalculatorConstructorDemo5 {
	public static void main(String[] args) {
		SubstractionableCalculator c1 = new SubstractionableCalculator(10, 20);
		c1.sum();
		c1.avg();
		c1.substract();
	}
}
```



위의 코드를 실행하면 오류가 발생한다. 오류의 내용은 아래와 같다.



```java
Exception in thread "main" java.lang.Error: Unresolved compilation problem: 
    Implicit super constructor Calculator() is undefined. Must explicitly invoke another constructor

	at org.opentutorials.javatutorials.Inheritance.example3.SubstractionableCalculator.<init>(CalculatorConstructorDemo5.java:26)
	at org.opentutorials.javatutorials.Inheritance.example3.CalculatorConstructorDemo5.main(CalculatorConstructorDemo5.java:38)
```



**즉 상위 클래스인 Calculator의 생성자가 존재하지 않는다는 의미다.** 하위 클래스가 호출될 때 자동으로 상위 클래스의 기본 생성자를 호출하게 된다. 그런데 **상위 클래스에 매개변수가 있는 생성자가 있다면 자바는 자동으로 상위 클래스의 기본 생성자를 만들어주지 않는다.** 따라서 존재하지 않는 생성자를 호출하게 되기 때문에 에러가 발생했다. 이 문제를 해결하기 위해서는 아래와 같이 상위 클래스에 기본 생성자를 추가하면 된다.



```java
package org.opentutorials.javatutorials.Inheritance.example3;

class Calculator {
    int left, right;
	
	public Calculator(){
		
	}
	
	public Calculator(int left, int right){
		this.left = left;
		this.right = right;
	}
	
	public void setOprands(int left, int right) {
		this.left = left;
		this.right = right;
	}

	public void sum() {
		System.out.println(this.left + this.right);
	}

	public void avg() {
		System.out.println((this.left + this.right) / 2);
	}
}
class SubstractionableCalculator extends Calculator {
	public SubstractionableCalculator(int left, int right) {
		this.left = left;
		this.right = right;
	}

	public void substract() {
		System.out.println(this.left - this.right);
	}
}

public class CalculatorConstructorDemo5 {
	public static void main(String[] args) {
		SubstractionableCalculator c1 = new SubstractionableCalculator(10, 20);
		c1.sum();
		c1.avg();
		c1.substract();
	}
}
```



그런데 상위 클래스인 Calculator에는 left와 right 값을 초기화할 수 있는 좋은 생성자가 이미 존재한다. 이것을 사용한다면 하위 클래스에서 left와 right의 값을 직접 설정하는 불필요한 절차를 생략할 수 있을 것이다. 어떻게 하면 상위 클래스의 생성자를 호출할 수 있을지 보자.