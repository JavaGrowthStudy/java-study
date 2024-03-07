# < 기본형과 참조형 >
-> 기본형
* char, byte, int ,long, float, double
* 직접 사용할 수 있는 값
* null 값 불가능

-> 참조형
* String, Integer, Class, Interface, Array, Map, Set
* 실제 객체의 참조값, 즉 위치
* null 값 가능

### < 기본형과 참조형의 저장 공간 >
![](https://velog.velcdn.com/images/austinan/post/d1676c90-6d62-4ca7-bf9b-e05d32e5f4f7/image.png)

- 기본형
    - 메모리의 스택(stack)에 기본형 변수가 저장되고, 그 변수 안에 실제값이 할당됨.
- 참조형
    - 메모리의 스택(stack)에 참조형 변수가 저장되고, 그 변수 안에 실제 객체의 주소값이 할당됨.
    - 메모리의 힙(heap)에 실제 객체가 저장됨.

### < Wrapper Class >
: 기본형을 객체로 다루기 위해 사용하는 클래스
![](https://velog.velcdn.com/images/austinan/post/5838eb44-797f-4f08-9900-3451076e8604/image.png)

- 래퍼 클래스가 필요한 이유
    - null 값을 표현해야 할때
    - Collection Frame Work를 사용해야 할 때 (컬렉션프레임워크는 객체만 저장 가능)
    - 매개변수로 객체를 필요로 한 상황
    - 그 외 기본형 값이 아닌 객체로 저장,사용해야하는 상황
    - 등

### < 값의 비교 >

```
Integer wrapper_value1 = 123; //자동 박싱
Integer wrapper_value2 = new Integer(123); //새로운 객체 생성
Integer wrapper_value3 = Integer.valueOf(123); //존재하는 인스턴스면 재활용


// ==
System.out.println(wrapper_value1 == wrapper_value2); //flase (주소값비교)
System.out.println(wrapper_value1 == wrapper_value3); //true (주소값비교)
System.out.println(wrapper_value2 == 123); //true (내부값비교) - 자동언박싱


// .equals()
System.out.println(wrapper_value2.equals(wrapper_value3)); //true (내부값비교)
System.out.println(wrapper_value2.equals(123)); //true (내부값비교)


// .compareTo()
// 같으면 0, 오른쪽이 더 크면 -1, 왼쪽이 더 크면 1
System.out.println(wrapper_value2.compareTo(wrapper_value3));  //0 (내부값비교)
```

* new Integer() : 호출할 때마다 매번 새로운 객체 생성
* Integer.valueOf() : 이미 생성한 객체 있을 시에는 재활용
* Integer.parseInt() : 기본형인 int 로 반환
* 래퍼 클래스끼리 비교 시에는 == 는 주소값, equals() 는 내부값을 비교하지만, 래퍼 클래스와 기본형 타입을 비교 시에는 둘 다 내부값을 비교한다.
* 래퍼클래스는 연산자를 사용하지 못하므로 .compareTo() 를 사용한다.

# < 생성자 >

### < 기본 생성자 >
-> 매개변수가 없는 생성자
-> 클래스에 생성자가 하나도 없으면, 자바 컴파일러는 기본 생성자를 자동으로 만들어준다.

### < this.value vs this() >
- `this` : ‘인스턴스’ 자신을 가리키는 참조변수를 의미.
    - 생성자의 매개변수로 선언된 변수의 이름이 인스턴스 변수와 같을 때 두 변수를 구분하기 위해 사용
- `this()` : ‘생성자’를 의미.
    - 자기 자신 생성자를 호출할 때 사용
    - this()생성자의 규칙
        - 생성자 내부에서만 사용 가능
        - 생성자 내부 중에서도 첫 줄에서만 사용 가능
