# static

## static 키워드가 필요한 이유


static 키워드는 주로 멤버 변수와 메서드에 사용되는데, static 키워드가 필요한 이유에 대해 알아보자.

특정 클래스를 통해서 생성된 객체의 수를 세는 단순한 프로그램을 예시로 들자.

1. static 변수가 아닌 멤버 변수를 활용한다면?
    
    - Data
        
        ```java
        package static1;
        public class Data {
        	public String name;
        	public int count;
        	public Data(String name) {
        	 this.name = name;
        	 count++;
        	}
        }
        //객체가 생성될 때 마다 생성자를 통해 인스턴스의 멤버 변수인 count 값을 증가시킨다.
        ```
        
    - DataCountMain
        
        ```java
        package static1;
        public class DataCountMain {
        	public static void main(String[] args) {
        	Data1 data1 = new Data("A");
        	System.out.println("A count=" + data1.count);
        	Data1 data2 = new Data("B");
        	System.out.println("B count=" + data2.count);
        	Data1 data3 = new Data("C");
        	System.out.println("C count=" + data3.count);
        	}
        }
        //객체를 생성하고 카운트 값을 출력한다.
        ```
        
    - 과정
	    ![](https://github.com/JavaGrowthStudy/java-study/assets/151157127/a5e6c03c-082f-4c47-9c4e-b65b29b18027)
    - 실행결과
        
        ```java
        A count=1
        B count=1
        C count=1
        ```
        
    
    인스턴스에 사용되는 멤버 변수 count 값은 인스턴스끼리 서로 공유되지 않는다.
    따라서 원하는 답을 구할 수 없다. 이 문제를 해결하려면 변수를 서로 공유해야 한다.
    
    
2. 이번에는 카운트 값을 저장하는 별도의 객체를 만들어보자.
    
    - Counter
        
        ```java
        package static1;
        public class Counter {
        	public int count;
        }
        ```
        
    - Data
        
        ```java
        package static1;
        public class Data {
        	public String name;
        	public Data(String name, Counter counter) {
        		this.name = name;
        		counter.count++;
        	}
        }
        ```
        
    - DataCountMain
        
        ```java
        package static1;
        public class DataCountMain {
        	public static void main(String[] args) {
        		Counter counter = new Counter();
        		Data2 data1 = new Data("A", counter);
        		System.out.println("A count=" + counter.count);
        		Data2 data2 = new Data("B", counter);
        		System.out.println("B count=" + counter.count);
        		Data2 data3 = new Data("C", counter);
        		System.out.println("C count=" + counter.count);
        	}
        }
        ```
        
    - 과정
        ![](https://github.com/JavaGrowthStudy/java-study/assets/151157127/632a5aa3-876e-4c26-b9ca-d0f737c50c68)
        
    - 실행결과
        
        ```java
        A count=1
        B count=2
        C count=3
        ```
        
    
    그런데 여기에는 약간 불편한 점들이 있다.
    Data 클래스와 관련된 일인데, Counter 라는 별도의 클래스를 추가로 사용해야 한다.
    생성자의 매개변수도 추가되고, 생성자가 복잡해진다. 생성자를 호출하는 부분도 복잡해진다.
    

3. 위 예제를 static 변수를 사용해서 바꿔보자.
    
    - Data
        
        ```java
        package static1;
        public class Data {
        	public String name;
        	public static int count; //static
        	public Data(String name) {
        		this.name = name;
        		count++;
         }
        }
        ```
        
    - DataCountMain
        
        ```java
        package static1;
        public class DataCountMain {
        	public static void main(String[] args) {
        		Data3 data1 = new Data("A");
        		System.out.println("A count=" + Data.count);
        		Data3 data2 = new Data("B");
        		System.out.println("B count=" + Data.count);
        		Data3 data3 = new Data("C");
        		System.out.println("C count=" + Data.count);
        	}
        }
        //count 정적 변수에 접근할 때, 인스턴스 이름이 아닌 클래스 명에 . 을 사용한다. => Data.count
        ```
        
    - 과정
        ![](https://github.com/JavaGrowthStudy/java-study/assets/151157127/71b58674-3b44-4d09-b650-4cec2e0c2716)
       
    - 실행 결과
        
        ```java
        A count=1
        B count=2
        C count=3
        ```
        

정리

static 변수는 쉽게 이야기해서 클래스인 붕어빵 틀이 특별히 관리하는 변수이다. 붕어빵 틀은 1개이므로 클래스 변수도 하나만 존재한다. 반면에 인스턴스 변수는 붕어빵인 인스턴스의 수 만큼 존재한다.

## static 변수


### 용어 정리

```java
public class Data {
	public String name;
	public static int count; //static
}
```

예제 코드에서 name , count 는 둘다 멤버 변수이다.

멤버 변수(필드)는 static 이 붙은 것과 아닌 것에 따라 다음과 같이 분류할 수 있다.

- 멤버 변수(필드)의 종류
    - `인스턴스 변수`: static 이 붙지 않은 멤버 변수, 예) name
        - static 이 붙지 않은 멤버 변수는 인스턴스를 생성해야 사용할 수 있고, 인스턴스에 소속되어 있다. 따라서 인스턴스 변수라 한다.
        - 인스턴스 변수는 인스턴스를 만들 때 마다 새로 만들어진다.
    - `클래스 변수`: static 이 붙은 멤버 변수, 예) count
        - 클래스 변수, 정적 변수, static 변수 등으로 부른다. 용어를 모두 사용하니 주의하자
        - static 이 붙은 멤버 변수는 인스턴스와 무관하게 클래스에 바로 접근해서 사용할 수 있고, 클래스 자체에 소속되어 있다. 따라서 클래스 변수라 한다.
        - 클래스 변수는 자바 프로그램을 시작할 때 딱 1개가 만들어진다. 인스턴스와는 다르게 보통 여러곳에서 공유하는 목적으로 사용된다.

### 변수와 생명주기

- 지역 변수(매개변수 포함): 지역 변수는 스택 영역에 있는 스택 프레임 안에 보관된다. 메서드가 종료되면 스택 프레임도 제거 되는데 이때 해당 스택 프레임에 포함된 지역 변수도 함께 제거된다. 따라서 지역 변수는 생존 주기가 짧다.
- 인스턴스 변수: 인스턴스에 있는 멤버 변수를 인스턴스 변수라 한다. 인스턴스 변수는 힙 영역을 사용한다. 힙 영역은 GC(가비지 컬렉션)가 발생하기 전까지는 생존하기 때문에 보통 지역 변수보다 생존 주기가 길다.
- 클래스 변수: 클래스 변수는 메서드 영역의 static 영역에 보관되는 변수이다. 메서드 영역은 프로그램 전체에서 사용하는 공용 공간이다. 클래스 변수는 해당 클래스가 JVM에 로딩 되는 순간 생성된다. 그리고 JVM이 종료될 때 까지 생명주기가 어어진다. 따라서 가장 긴 생명주기를 가진다.
    - 클래스가 JVM(Java Virtual Machine)에 로딩된다는 말은, 해당 클래스가 사용될 준비가 되었다는 것을 의미한다. 이 과정에서 클래스의 메타데이터(클래스 이름, 부모 클래스 정보, 메서드, 변수 등의 정보)가 메서드 영역에 배치된다.
    - 그러나, 프로그램을 시작할 때 모든 클래스와 클래스 변수가 JVM에 한 번에 로딩되는 것은 아니다. 클래스는 필요할 때 동적으로 로딩된다. 즉, 클래스가 JVM에 로딩되는 시점은 다음과 같은 경우이다. - 해당 클래스의 인스턴스가 처음으로 생성될 때 / 해당 클래스의 static 멤버(변수나 메서드)에 처음 접근할 때 / Class.forName() 메서드를 통해 명시적으로 해당 클래스를 로딩할 때
    - 클래스가 로딩되는 순간, 클래스 변수도 초기화되어 메서드 영역의 static 영역에 할당되고, JVM이 종료될 때까지 그 생명주기가 유지된다. 따라서 클래스 변수는 해당 클래스의 모든 인스턴스에 의해 공유되며, 클래스가 로드된 후부터 JVM이 종료될 때까지 계속 존재하게 된다.

static 이 정적이라는 이유는 바로 여기에 있다. 힙 영역에 생성되는 인스턴스 변수는 동적으로 생성되고, 제거된다.반면에 static 인 정적 변수는 거의 프로그램 실행 시점에 딱 만들어지고, 프로그램 종료 시점에 제거된다. 정적 변수는 이름 그대로 정적이다.

### 정적 변수 접근법

static 변수는 클래스를 통해 바로 접근할 수도 있고, 인스턴스를 통해서도 접근할 수 있다.

DataCountMain

```java
package static1;
public class DataCountMain {
	public static void main(String[] args) {
		Data data1 = new Data("A");
		Data data2 = new Data("B");
		
		//인스턴스를 통한 접근
		System.out.println("A count=" + data2.count); //2
		
		//클래스를 통합 접근
		System.out.println(Data.count); //2
	}
}

//둘의 차이는 없다. 둘다 결과적으로 정적 변수에 접근한다.
```

- 인스턴스를 통한 접근 `data2.count`
    
    정적 변수의 경우 인스턴스를 통한 접근은 추천하지 않는다. 왜냐하면 코드를 읽을 때 마치 인스턴스 변수에 접근하는 것 처럼 오해할 수 있기 때문이다.
    
- 클래스를 통한 접근 `Data.count`
    
    정적 변수는 클래스에서 공용으로 관리하기 때문에 클래스를 통해서 접근하는 것이 더 명확하다. 따라서 정적 변수에 접근할 때는 클래스를 통해서 접근하자.
    

## static 메서드

특정 문자열을 꾸며주는 간단한 기능을 만들어보자. 예를 들어서 "hello" 라는 문자열 앞 뒤에 * 을 붙여서 "_hello_**" 와 같이 꾸며주는 기능이다.

### Static 메서드 사용 이유

- 인스턴스 메서드 활용
    
    - DecoUtil
        
        ```java
        package static2;
        public class DecoUtil {
        	public String deco(String str) {
        		String result = "*" + str + "*";
        		return result;
        	}
        }
        ```
        
    - DecoMain
        
        ```java
        package static2;
        public class DecoMain {
        	public static void main(String[] args) {
        		String s = "hello java";
        		DecoUtil utils = new DecoUtil(); //객체생성
        		String deco = utils.deco(s); //객체를 통해 메서드 호출
        		System.out.println("before: " + s);
        		System.out.println("after: " + deco);
        	}
        }
        ```
        
    - 실행결과
        
        ```java
        before: hello java
        after: *hello java*
        ```
        
    
    deco() 메서드를 호출하기 위해서는 DecoUtil 의 인스턴스를 먼저 생성해야 한다. 그런데 deco()라는 기능은 멤버 변수도 없고, 단순히 기능만 제공할 뿐이다. 때문에, 메서드 하나를 사용하기 위해 인스턴스를 생성해주는 것은 불편하다.
    
- static 메서드 활용
    
    - DecoUtil
        
        ```java
        package static2;
        public class DecoUtil {
        	public static String deco(String str) {
        		String result = "*" + str + "*";
        		return result;
        	}
        }
        ```
        
    - DecoMain
        
        ```java
        package static2;
        public class DecoMain {
        	public static void main(String[] args) {
        		String s = "hello java";
        		String deco = DecoUtil.deco(s); // 클래스를 통한 메서드 호출
        		System.out.println("before: " + s);
        		System.out.println("after: " + deco);
         }
        }
        ```
        
    - 실행 결과
        
        ```java
        before: hello java
        after: *hello* java
        ```
        
    
    static 이 붙은 정적 메서드는 객체 생성 없이 `클래스명 + . (dot) + 메서드 명`으로 바로 호출할 수 있다. 정적 메서드 덕분에 불필요한 객체 생성 없이 편리하게 메서드를 사용했다.
    

+) 추가

인스턴스 메서드와 static 메서드 모두 메서드 영역에 저장된다. 그런데 왜 static 메서드는 클래스를 통해 접근이 가능하고, 인스턴스 메서드는 클래스를 통한 접근이 불가능하고 객체를 통한 접근만 되는 걸까? ⇒ static메서드와 인스턴스 메서드의 저장구조에 차이가 있다기보다는, static이 붙지 않은 인스턴스 메서드는 객체가 생성돼야만 해당 메서드에 접근할 수 있도록 설계되었기 때문이다. 예를 들어, 아래와 같은 코드가 있다고 하자. String 객체가 생성되지 않았을 경우에는 length 메서드는 올바르게 실행될 수가 없다.

```java
String example = "Hello, World!";
int length = example.length();
```

### Static 메서드 사용법

정적 메서드는 객체 생성없이 클래스에 있는 메서드를 바로 호출할 수 있다는 장점이 있다.

하지만 정적 메서드는 언제나 사용할 수 있는 것이 아니다.

- static 메서드는 static 만 사용할 수 있다.
    - 클래스 내부의 기능을 사용할 때, 정적 메서드는 static 이 붙은 정적 메서드나 정적 변수만 사용할 수 있다.
    - 클래스 내부의 기능을 사용할 때, 정적 메서드는 인스턴스 변수나, 인스턴스 메서드를 사용할 수 없다.
- 반대로 모든 곳에서 static 을 호출할 수 있다.
    - 정적 메서드는 공용 기능이다.
    - 따라서 접근 제어자만 허락한다면 클래스를 통해 모든 곳에서 static 을 호출할 수 있다.

DecoData

```java
package static2;
public class DecoData {
	private int instanceValue;
	private static int staticValue;
	
	public static void staticCall() {
		//instanceValue++; //인스턴스 변수 접근, compile error
		//instanceMethod(); //인스턴스 메서드 접근, compile error
		staticValue++; //정적 변수 접근
		staticMethod(); //정적 메서드 접근
	}
	
	public void instanceCall() {
		instanceValue++; //인스턴스 변수 접근
		instanceMethod(); //인스턴스 메서드 접근
		staticValue++; //정적 변수 접근
		staticMethod(); //정적 메서드 접근
	}
	
	private void instanceMethod() {
		System.out.println("instanceValue=" + instanceValue);
	}
	
	private static void staticMethod() {
		System.out.println("staticValue=" + staticValue);
	}
}
```

- staticCall() 메서드를 보자.
    - 이 메서드는 정적 메서드이다. 따라서 static 만 사용할 수 있다. 정적 변수, 정적 메서드에는 접근할 수 있지만, static 이 없는 인스턴스 변수나 인스턴스 메서드에 접근하면 컴파일 오류가 발생한다.
    - 코드를 보면 staticCall() staticMethod() 로 static 에서 static 을 호출하는 것을 확인할 수 있다.
- instanceCall() 메서드를 보자.
    - 이 메서드는 인스턴스 메서드이다. 모든 곳에서 공용인 static 을 호출할 수 있다. 따라서 정적 변수, 정적 메서드에 접근할 수 있다. 물론 인스턴스 변수, 인스턴스 메서드에도 접근할 수 있다.

물론 당연한 이야기지만 다음과 같이 객체의 참조값을 직접 매개변수로 전달하면 정적 메서드도 인스턴스의 변수나 메서드를 호출할 수 있다

```java
public static void staticCall(DecoData data) {
	data.instanceValue++;
	data.instanceMethod();
}
```

### Static 메서드 용어

- static 이 붙은 멤버 메서드는 인스턴스와 무관하게 클래스에 바로 접근해서 사용할 수 있고, 클래스 자체에 소속되어 있다. 따라서 클래스 메서드라 한다.
- static 이 붙지 않은 멤버 메서드는 인스턴스를 생성해야 사용할 수 있고, 인스턴스에 소속되어 있다. 따라서 인스턴스 메서드라 한다.

참고로 방금 설명한 내용은 멤버 변수에도 똑같이 적용된다

### static 메서드 접근법

- 인스턴스를 통한 접근 `data3.staticCall()`
    
    정적 메서드의 경우 인스턴스를 통한 접근은 추천하지 않는다. 왜냐하면 코드를 읽을 때 마치 인스턴스 메서드에 접근하는 것 처럼 오해할 수 있기 때문이다.
    
- 클래스를 통한 접근 `DecoData.staticCall()` 정적 메서드는 클래스에서 공용으로 관리하기 때문에 클래스를 통해서 접근하는 것이 더 명확하다. 따라서 정적 메서드에 접근할 때는 클래스를 통해서 접근하자.
    

### static import

정적 메서드를 사용할 때 해당 메서드를 다음과 같이 자주 호출해야 한다면 static import 기능을 고려하자

```java
DecoData.staticCall();
DecoData.staticCall();
DecoData.staticCall();
```

static import 기능을 사용하면 다음과 같이 클래스 명을 생략하고 메서드를 호출할 수 있다

```java
staticCall();
staticCall();
staticCall();
```

static import 사용 예시

```java
package static2;

//import static static2.DecoData.staticCall;
import static static2.DecoData.*;

public class DecoDataMain {
	public static void main(String[] args) {
		System.out.println("정적 호출");
		staticCall(); //클래스 명 생략 가능
		...
}
```

### main메서드

인스턴스 생성 없이 실행하는 가장 대표적인 메서드가 바로 main() 메서드이다. main() 메서드는 프로그램을 시작하는 시작점이 되는데, 생각해보면 객체를 생성하지 않아도 main() 메서드가 작동했다. 이것은 main() 메서드가 static 이기 때문이다.

정적 메서드는 정적 메서드만 호출할 수 있다. 따라서 정적 메서드인 main() 에서는 정적 메서드만을 호출, 사용해야 한다. 물론 마찬가지로, 객체를 생성한 뒤, (`객체.메서드`와 같이) 객체를 통해 메서드에 접근하면 static이 아닌 메서드를 호출할 수 있다.

```java
public class ValueDataMain {
	public static void main(String[] args) {
		ValueData valueData = new ValueData();
		add(valueData); //메서드 호출
	}
	static void add(ValueData valueData) { //static이어야 한다.
		valueData.value++;
		System.out.println("숫자 증가 value=" + valueData.value);
	}
}
```

