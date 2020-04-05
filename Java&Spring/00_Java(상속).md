# 상속

> 상속은 객체지향의 재활용성을 극대화시킨 프로그래밍 기법이라고 할 수 있다. 동시에 객체지향을 복잡하게 하는 주요 원인이라고도 할 수 있다.

- 어떤 객체가 있을 때 그 객체의 변수와 메소드를 다른 객체가 물려 받을 수 있는 기능을 상속이라고 한다.

- 부모 클래스(parent class) 와 자식 클래스(children class)는 자바 지정예약어 extends에 의하여 정해진다.

- 하나의 부모 클래스(parent class)는 여러개의 자식 클래스(children)을 가질 수 있다.

- 반대로 하나의 클래스는 여러개의 클래스로부터 상속을 받을수는 없다.

- 부모 클래스(parent class)로부터 상속받은 자식 클래스는 부모 클래스의 자원(source) 모두를 사용 할 수 있다.

- 반대로 부모클래스는 자식클래스의 자원을 가져다 쓸 수는 없다.

- 자식클래스는 또다른 클래스의 부모 클래스가 될 수 있다. 

- 자식클래스는 부모클래스로부터 물려받은 자원을 override 하여 수정해서 사용 할 수 있다.

- 부모클래스가 상속받은 자원도 자식클래스가 사용 가능하다.



```java
Calculator c1 = new Calculator();
c1.setOprands(10, 20);
c1.sum();
c1.avg(); 
c1.substract();
```





```java
package org.opentutorials.javatutorials.Inheritance.example1;

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
	public void substract() {
		System.out.println(this.left - this.right);
	}
}

public class CalculatorDemo1 {

	public static void main(String[] args) {

		SubstractionableCalculator c1 = new SubstractionableCalculator();
		c1.setOprands(10, 20);
		c1.sum();
		c1.avg();
		c1.substract();
	}

}
```



다음과 같은 코드를 분석해보자.



```java
class SubstractionableCalculator extends Calculator {
    public void substract() {
		System.out.println(this.left - this.right);
	}
}
```



우선 새로운 클래스인 SubstractionableCalculator을 정의했다. 이 클래스의 본체에는 substract라는 메소드만이 존재한다. 하지만 이 클래스를 인스턴스화한 c1은 아래와 같이 정의하지 않은 메소드들을 호출하고 있다.



```java
SubstractionableCalculator c1 = new SubstractionableCalculator();
c1.setOprands(10, 20);
c1.sum();
c1.avg();
c1.substract();
```



이것이 가능한 이유는 extends Calculator 때문이다. 이것은 클래스 Calculator를 상속받는다는 의미다. 따라서 SubstaractableCalculator는 Calculator에서 정의한 메소드 setOprands, sub, avg를 사용할 수 있게 된다.