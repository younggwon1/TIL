# Overriding

상속은 상위 클래스의 기능을 하위 클래스에게 물려주는 기능이다. 그렇다면 하위 클래스는 상위 클래스의 메소드를 주어진 그대로 사용한다면 제약이 상당할 것이다. 이런 제약을 벗어나려면 하위 클래스가 부모 클래스의 기본적인 동작방법을 변경할 수 있어야 한다. 이런 맥락에서 도입된 기능이 메소드 오버라이딩(overriding)이다.

이 예제는 클래스 Calculator의 기본적인 동작 방법을 상속 받은 SubstractionableCalculator에 빼기 기능을 추가하고 있다. 이것은 상위 클래스의 기능에 새로운 기능을 추가한 것이다. 만약 상위 클래스에서 물려 받은 메소드 sum을 호출했을 때 아래와 같이 그 결과를 좀 더 친절하게 알려줘야 한다면 어떻게 해야할까?



```
실행 결과는 30입니다.
```



상속 주제의 예제를 조금 변경해보자.

```java
package org.opentutorials.javatutorials.overriding.example1;

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
	
	public void sum() {
		System.out.println("실행 결과는 " +(this.left + this.right)+"입니다.");
	}
	
	public void substract() {
		System.out.println(this.left - this.right);
	}
}

public class CalculatorDemo {
	public static void main(String[] args) {
		SubstractionableCalculator c1 = new SubstractionableCalculator();
		c1.setOprands(10, 20);
		c1.sum();
		c1.avg();
		c1.substract();
	}
}
```



실행결과는 아래와 같다.



```
실행 결과는 30입니다.
15
-10
```



메소드 sum이 SubstractionableCalculator에 추가 되었다. 실행결과는 c1.sum이 상위 클래스의 메소드가 아니라 하위 클래스의 메소드 sum을 실행하고 있다는 것을 보여준다. 하위 클래스 입장에서 부모 클래스란 말하자면 기본적인 동작 방법을 정의한 것이라고 생각할 수 있다. **하위 클래스에서 상의 클래스와 동일한 메소드를 정의하면 부모 클래스로부터 물려 받은 기본 동작 방법을 변경하는 효과를 갖게 된다.** 기본동작은 폭넓게 적용되고, 예외적인 동작은 더 높은 우선순위를 갖게하고 있다. 이것을 메소드 오버라이딩(overriding)이라고 한다.



### 오버라이딩의 조건

상위 클래스에서 정의하고 있는 메소드 avg는 계산 결과를 화면에 출력하고 있다. 그런데 계산 결과를 좀 더 다양하게 사용하기 위해서 메소드 avg가 화면에 결과를 출력하는 대신 계산 결과를 리턴.

```java
package org.opentutorials.javatutorials.overriding.example1;

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
	
	public void sum() {
		System.out.println("실행 결과는 " +(this.left + this.right)+"입니다.");
	}
	
	public int avg() {
		return (this.left + this.right)/2;
	}
	
	public void substract() {
		System.out.println(this.left - this.right);
	}
}

public class CalculatorDemo {
	public static void main(String[] args) {
		SubstractionableCalculator c1 = new SubstractionableCalculator();
		c1.setOprands(10, 20);
		c1.sum();
		c1.avg();
		c1.substract();
	}
}
```

이것은 아래와 같은 에러를 발생한다.

```java
Exception in thread "main" java.lang.Error: Unresolved compilation problem: 
    The return type is incompatible with Calculator.avg()

	at org.opentutorials.javatutorials.overriding.example1.SubstractionableCalculator.avg(CalculatorDemo.java:26)
	at org.opentutorials.javatutorials.overriding.example1.CalculatorDemo.main(CalculatorDemo.java:40)
```

overriding을 하기 위해서는 메소드의 리턴 형식이 같아야 한다. 즉 클래스 Calculator의 메소드 avg는 리턴 타입이 void이다. 그런데 이것을 상속한 클래스 SubstractionableCalculator의 리턴 타입은 int이다. 오버라이딩을 하기 위해서는 아래의 조건을 충족시켜야 한다.

- 메소드의 이름
- 메소드 매개변수의 숫자와 데이터 타입 그리고 순서
- 메소드의 리턴 타입

위와 같이 메소드의 형태를 정의하는 사항들을 통털어서 메소드의 서명(signature)라고 한다. 즉 위의 에러는 메소드들 간의 서명이 달라서 발생한 문제다. 아래와 같이 상위 클래스의 코드를 변경해보자.

```java
package org.opentutorials.javatutorials.overriding.example1;

class Calculator {
    int left, right;

	public void setOprands(int left, int right) {
		this.left = left;
		this.right = right;
	}

	public void sum() {
		System.out.println(this.left + this.right);
	}

	public int avg() {
		return ((this.left + this.right) / 2);
	}
}

class SubstractionableCalculator extends Calculator {
	
	public void sum() {
		System.out.println("실행 결과는 " +(this.left + this.right)+"입니다.");
	}
	
	public int avg() {
		return ((this.left + this.right) / 2);
	}
	
	public void substract() {
		System.out.println(this.left - this.right);
	}
}

public class CalculatorDemo {
	public static void main(String[] args) {
		SubstractionableCalculator c1 = new SubstractionableCalculator();
		c1.setOprands(10, 20);
		c1.sum();
		c1.avg();
		c1.substract();
	}
}
```



상위 클래스와 하위 클래스의 서명이 같기 때문에 메소드 오버라이딩을 할 수 있었다. 그런데 위의 코드를 보면 중복이 발생했다. 메소드 avg의 부모와 자식 클래스가 같은 로직을 가지고 있다. 중복은 제거 되어야 한다. 생성자와 마찬가지로 super를 사용하면 이 문제를 해결 할 수 있다.



```java
package org.opentutorials.javatutorials.overriding.example1;

class Calculator {
    int left, right;

	public void setOprands(int left, int right) {
		this.left = left;
		this.right = right;
	}

	public void sum() {
		System.out.println(this.left + this.right);
	}

	public int avg() {
		return ((this.left + this.right) / 2);
	}
}

class SubstractionableCalculator extends Calculator {
	
	public void sum() {
		System.out.println("실행 결과는 " +(this.left + this.right)+"입니다.");
	}
	
	public int avg() {
		return super.avg();
	}
	
	public void substract() {
		System.out.println(this.left - this.right);
	}
}

public class CalculatorDemo {
	public static void main(String[] args) {
		SubstractionableCalculator c1 = new SubstractionableCalculator();
		c1.setOprands(10, 20);
		c1.sum();
		System.out.println("실행 결과는" + c1.avg());
		c1.substract();
	}
}
```

하위 클래스의 메소드 avg에서 상위 클래스의 메소드를 호출하기 위해서 super를 사용하고 있다. 덕분에 코드의 중복을 제거 할 수 있었다.