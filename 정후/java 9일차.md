## < super - 부모 참조 >

부모와 자식의 필드명이 같거나 메서드가 오버라이딩 되어 있으면, 자식에서 부모의 필드나 메서드를 호출할 수 없다.이때 super 키워드를 사용하면 부모를 참조할 수 있다. super 는 이름 그대로 부모 클래스에 대한 참조를 나타낸다.

- this : 자기 자신 클래스에 대한 참조
- super : 부모 클래스에 대한 참조

```java
public class Parent {
	public String value = "parent";
	
	public void hello() {
		System.out.println("Parent.hello");
	}
}
```

```java
public class Child extends Parent {
	public String value = "child";
	
	@Override
	public void hello() {
		System.out.println("Child.hello");
	}
	
	public void call() {
		System.out.println("this value = " + this.value); //this 생략 가능
		System.out.println("super value = " + super.value);
		this.hello(); //this 생략 가능
		super.hello();
	}
}
```

```java
package extends1.super1;
	public class Super1Main {
	public static void main(String[] args) {
		Child child = new Child();
		child.call();
	}
}
```

```java
this value = child
super value = parent
Child.hello
Parent.hello
```

---

## < super - 생성자 >

상속 관계의 인스턴스를 생성하면 결국 메모리 내부에는 자식과 부모 클래스가 각각 다 만들어진다. Child 를 만들면 부모인 Parent 까지 함께 만들어지는 것이다. 따라서 각각의 생성자도 모두 호출되어야 한다.
상속 관계를 사용하면 자식 클래스의 생성자에서 부모 클래스의 생성자를 반드시 호출해야 한다.(규칙)
상속 관계에서 부모의 생성자를 호출할 때는 super(...) 를 사용하면 된다

```
package extends1.super2;
public class ClassA {
	public ClassA() {
		System.out.println("ClassA 생성자");
	}
}
// Class A 기본 생성자 존재
```

```
package extends1.super2;
public class ClassB extends ClassA {
	public ClassB(int a) {
		super(); // 생략 가능 (기본 생성자)
		System.out.println("ClassB 생성자 a="+a);
	}
	public ClassB(int a, int b) {
		super(); // 생략 가능 (기본 생성자)
		System.out.println("ClassB 생성자 a="+a + " b=" + b);
	}
}
// Class B 기본 생성자 없음
```

```
package extends1.super2;
public class ClassC extends ClassB {
	public ClassC() {
		super(10, 20); // 생략 불가능
		System.out.println("ClassC 생성자");
	}
}
```

```
package extends1.super2;
public class Super2Main {
	public static void main(String[] args) {
		ClassC classC = new ClassC();
	}
}
```

```
ClassA 생성자
ClassB 생성자 a=10 b=20
ClassC 생성자
```