## < 접근제한자의 종류 >
- **차단 -- ( private << default << protected << public ) -- 허용**
    - `private` : 모든 외부 호출을 막는다.
        - 나의 클래스 안으로 속성과 기능을 숨길 때 사용
        - 외부 클래스에서 해당 기능을 호출할 수 없다.
    - `default` ( = `package-private`) : 같은 패키지안에서 호출은 허용한다.
        - 나의 패키지 안으로 속성과 기능을 숨길 때 사용
        - 외부 패키지에서 해당 기능을 호출할 수 없다.
    - `protected` : 같은 패키지안에서 호출은 허용한다. 패키지가 달라도 상속 관계의 호출은 허용한다.
        - 상속 관계로 속성과 기능을 숨길 때 사용
        - 상속 관계가 아닌 곳에서 해당 기능을 호출할 수 없다.
    - `public` : 모든 외부 호출을 허용한다.
        - 기능을 숨기지 않고 어디서든 호출할 수 있게 공개한다.

* 참고 : 클래스의 접근 제한자는 public, default 만 존재

## < 필드, 메서드에 접근 제한자 사용 예시 >

- access.a 패키지 , AccessData 클래스

```
package access.a;
public class AccessData {

	public int publicField;
	int defaultField;
	private int privateField;

	public void publicMethod() {
		System.out.println("publicMethod 호출 "+ publicField);
	}

	void defaultMethod() {
		System.out.println("defaultMethod 호출 " + defaultField);
	}

	private void privateMethod() {
		System.out.println("privateMethod 호출 " + privateField);
	}

	public void innerAccess() {
		//내부호출 - private를 포함하여 모든 필드 및 메서드에 접근 가능
		System.out.println("내부 호출");
		publicField = 100;
		defaultField = 200;
		privateField = 300;
		publicMethod();
		defaultMethod();
		privateMethod();
	}
}
```
- access.a 패키지, AccessInnerMain 클래스

```java
package access.a;
public class AccessInnerMain {
	public static void main(String[] args) {
		AccessData data = new AccessData();

		//public 호출 가능
		data.publicField = 1;
		data.publicMethod();

		//같은 패키지 default 호출 가능
		data.defaultField = 2;
		data.defaultMethod();

		//private 호출 불가
		//data.privateField = 3;
		//data.privateMethod();
		data.innerAccess();
	}
}
```

실행결과

```java
publicMethod 호출 1
defaultMethod 호출 2
내부 호출
publicMethod 호출 100
defaultMethod 호출 200
privateMethod 호출 300
```
- access.b 패키지 / AccessOuterMain 클래스
  (import access.a.AccessData)

```java
package access.b;

import access.a.AccessData;

public class AccessOuterMain {
	public static void main(String[] args) {
		AccessData data = new AccessData();
		//public 호출 가능
		data.publicField = 1;
		data.publicMethod();

		//다른 패키지 default 호출 불가
		//data.defaultField = 2;
		//data.defaultMethod();

		//private 호출 불가
		//data.privateField = 3;
		//data.privateMethod();
		data.innerAccess();
	}
}
```

실행결과

```
publicMethod 호출 1
내부 호출
publicMethod 호출 100
defaultMethod 호출 200
privateMethod 호출 300
```
+) 참고로 생성자도, 접근 제한자 관점에서 메서드와 같다.

## < 클래스에 접근 제한자 사용 예시 >

- 클래스 레벨의 접근 제어자는 `public` , `default` 만 사용할 수 있다.
    - private , protected 는 사용할 수 없다.
- public 클래스는 반드시 파일명과 이름이 같아야 한다.
    - 하나의 자바 파일에 public 클래스는 하나만 등장할 수 있다.
    - 하나의 자바 파일에 default 접근 제어자를 사용하는 클래스는 무한정 만들 수 있다.

## < 캡슐화 >
-> 데이터와 해당 데이터를 처리하는 메서드를 하나로 묶어서(캡슐로 만들어) (접근 제한자를 통해) 외부에서의 접근을 제한하는 것.

- 캡슐화의 목적
    - 외부로부터 클래스에 정의된 속성과 기능들을 보호하고, 필요한부분만 외부로 노출될 수 있도록 하여

      각 객체 고유의 독립성과 책임 영역을 안전하게 지키고자 하는 목적이 있다.


- 좋은 캡슐화
    - 객체에는 속성(데이터)과 기능(메서드)이 있다.

      캡슐화에서 가장 필수로 숨겨야 하는 것은 속성(데이터)이다.

    - 속성(데이터) : 객체의 데이터는 객체가 제공하는 기능인 메서드를 통해서 접근해야 한다.
    - 기능(메서드) : 사용자 입장에서 꼭 필요한 기능만 외부에 노출하고, 나머지 기능은 모두 내부로 숨겨야 한다.
    - ⇒ 정리하자면 데이터는 모두 숨기고, 기능은 꼭 필요한 기능만 노출하는 것이 좋은 캡슐화이다.

- 캡슐화 vs. 정보 은닉
    - 정보은닉 : 객체지향 언어적 요소를 활용하여, 객체에 대한 구체적인 정보를 노출시키지 않도록 하는 기법
    - 정보은닉의 종류
        1. 객체의 구체적인 타입 은닉 ( = 업캐스팅)
        2. 객체의 필드 및 메소드 은닉 ( = 캡슐화)
        3. 구현 은닉 ( = 인터페이스 & 추상 클래스)
    - 캡슐화 == 정보 은닉이 아니라, 정보 은닉 기법중 하나가 캡슐화이다.