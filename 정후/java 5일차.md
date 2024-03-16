## < final+지역변수 / final+매개변수 >

#### final 지역변수
- final 을 지역 변수에 설정할 경우 최초 한번만 할당할 수 있다.
- 이후에 변수의 값을 변경하려면 컴파일 오류가 발생한다.
- final 지역 변수 선언시 바로 초기화 한 경우 이미 값이 할당되었기 때문에 이후에 값을 재할당할 수 없다.

#### final 매개변수
- 매개변수에 final 이 붙으면 메서드 내부에서 매개변수의 값을 변경할 수 없다.
- 따라서 메서드 호출 시점에 사용된 값이 끝까지 사용된다.

```
public class FinalLocalMain {
	public static void main(String[] args) {
		
		//final 지역 변수1
		final int data1;
		data1 = 10; //최초 한번만 할당 가능
		//data1 = 20; //컴파일 오류
		
		//final 지역 변수2
		final int data2 = 10;
		//data2 = 20; //컴파일 오류
		
		method(10);
	}
	
	//final 매개변수
	static void method(final int parameter) {
		//parameter = 20; 컴파일 오류
	}
}
```

## < final + 멤버변수(필드) >

- final 을 필드에 사용할 경우 해당 필드는 한번만 초기화 될 수 있다.
- final 필드를 초기화 하는 방법 (둘 중 한 가지만 사용 가능하다.- 이미 값이 설정되어버리기 때문.)
    1. 생성자 초기화
    2. 필드 초기화

### 생성자 초기화

```java
package final1;

// final 필드 - 생성자 초기화
public class ConstructInit {
	final int value;
	public ConstructInit(int value) {
		this.value = value;
	}
}
```

### 필드 초기화

```java
package final1;

// final 필드 - 필드 초기화
public class FieldInit {
	static final int CONST_VALUE = 10;
	final int value = 10;
}
```

### main

```java
package final1;

public class FinalFieldMain {
	public static void main(String[] args) {
	
		// final 필드 - 생성자 초기화
		System.out.println("생성자 초기화");
		ConstructInit constructInit1 = new ConstructInit(10); //10
		ConstructInit constructInit2 = new ConstructInit(20); //20
		System.out.println(constructInit1.value); //10
		System.out.println(constructInit2.value); //20
		
		// final 필드 - 필드 초기화
		System.out.println("필드 초기화");
		FieldInit fieldInit1 = new FieldInit();
		FieldInit fieldInit2 = new FieldInit();
		FieldInit fieldInit3 = new FieldInit();
		System.out.println(fieldInit1.value); //10
		System.out.println(fieldInit2.value); //10
		System.out.println(fieldInit3.value); //10
		
		// 상수
		System.out.println("상수");
		System.out.println(FieldInit.CONST_VALUE); //10
		
	}
}
```

코드설명

- 생성자 초기화 - `ConstructInit`
    - 각 인스턴스마다 final 필드에 다른 값을 할당할 수 있다.
    - (물론 final 을 사용했기 때문에 생성 이후에 이 값을 변경하는 것은 불가능하다.)
- 필드 초기화 - `FieldInit`
    - 모든 인스턴스가 다음 그림과 같이 같은 값을 가진다.

![](https://velog.velcdn.com/images/austinan/post/678202e6-94e9-4369-858f-69335cb27028/image.png)


    - 여기서는 FieldInit 인스턴스의 모든 value 값은 10 이 된다.
    - 생성자 초기화와 다르게 필드 초기화는 필드의 코드에 해당 값이 미리 정해져있기 때문이다.
    - 모든 인스턴스가 같은 값은 값을 사용하기 때문에 결과적으로 메모리를 낭비하게 된다.(물론 JVM에 따라서 내부 최적화를 시도할 수 있다) 또 메모리 낭비를 떠나서 같은 값이 계속 생성되는 것은 개발자가 보기에 명확한 중복이다. 이럴 때 사용하면 좋은 것이 바로 static 영역이다.
- static final - `FieldInit.MY_VALUE`
    - FieldInit.MY_VALUE 는 static 영역에 존재한다. 그리고 final 키워드를 사용해서 초기화 값이 변하지 않는다.
    - static 영역은 단 하나만 존재하는 영역이다. MY_VALUE 변수는 JVM 상에서 하나만 존재하므로 앞서 설명한 중복과 메모리 비효율 문제를 모두 해결할 수 있다.
    - 이런 이유로 **필드에 final + 필드 초기화를 사용하는 경우 static 을 붙여서 사용하는 것이 효과적이다.**