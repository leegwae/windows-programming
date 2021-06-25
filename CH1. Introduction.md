# CH1. 개요

- 2000년 마이크로소프트가 .NET 환경을 위하여 제작한 언어
- 컴포넌트 지향 프로그래밍 언어
- 자바의 단점을 보완하였음
  - 실행 방법: 자바는 interpretation, c#은 컴파일 방법
  - 자바 언어를 대체할 수 있는 언어
- C++에 기반을 두어 객체 지향적 특성 지원



## 1.1 소개

- 마이크로소프트사의 앤더스 헬스버그가 고안
- .NET 환경에서 용이하게 응용 프로그램 개발을 할 수 있도록 설계됨
- 설계 목표: 간단하며 핸대적이고 객체-지향적이며 타입-안정적인 것



### C# 언어의 특징

- 객체지향적: 자료 추상화
  - 자료추상화: 자료구조와 그 자료구조에 수행할 수 있는 연산을 정의하고, 이 사용자 정의된 자료형을 내장 자료형처럼 사용할 수 있는 기능
- 델리게이트와 이벤트
  - 델리게이트: 메서드를 다른 객체에 전달해야 하는 프로그래밍 기법
- 멀티스레드: 동시처리(concurrent processing)을 위한 멀티스레드(multithread) 지원
- 예외처리
  - 예외: 실행 시간에 일어나는 에러
  - 예외를 언어 수준에서 체계적으로 다룰 수 있는 방법
- 연산자 중복, 제네릭



### C# 언어가 다른 언어로부터 받은 영향

<img src=".\Images\1-1.PNG" alt="1-1" style="zoom:50%;" />

- C 언어 계열에 속하는 범용 프로그래밍 언어

- C: 연산자와 문장 등 기초적인 프로그래밍 언어의 기능
- C++: 주로 객체지향 속성, 연산자 중복, 제네릭
- 자바: 예외 처리, 스레드 개념



### 1.1.1 개발환경

- 크게 IDE를 통한 개발과 SDK를 통한 개발로 나눌 수 있음.



### 1.1.2 실행 과정

- C# 소스 프로그램의 이름: 일반적으로 메인 메서드가 있는 클래스 이름과 소스 파일 이름을 동일하게 작성(강제적이지 않음)
- C# 프로그램의 실행 과정
  - `HelloWorld.cs` 소스 프로그램
  - **C# 컴파일러 CSC**(`csc.exe`)를 통한 컴파일
  - `HelloWorld.exe` 실행 파일 생성
  - **CLR(Common Language Reuntime)**을 통한 실행
- 실행 파일은 intermediate language(가상 기계가 실행할 수 있는 명령어) 형태로 컴파일되어 있다. CLR은 이 명령어를 가져다가 interpreter에서 실행한다.



```c#
// HelloWorld.cs
using System;

class HelloWorld{
    public static void Main(){
        Console.WriteLine("Hello, world!");
    }
}
```

- C#에서 클래스는 프로그램의 기본 단위이다.
- 하나의 응용 프로그램이 되기 위하여 `Main` 메서드가 존재해야 한다.
- `Console.WriteLine()`은 `Console` 클래스에 속해 있으며, .NET 프레임워크에서 제공하는 `System` 네임 스페이스에 속해 있다.

```c#
using System;					// (1)
System.Console.WriteLine();		// (2)
```

- 네임 스페이스는 (1)과 같이 `using`문으로 가져오며, 만약 `using`문을 명시하지 않았다면 (2)처럼 명시해야한다.



## 1.2 기본 특징

### 1.2.1 자료형

- 본디 OOP에서는 모든 자료형은 객체이지만, 성능을 위하여 일부는 value type이다.
- **자료형**은 변수나 상수가 가질 수 있는 값과 수행될 수 있는 연산의 종류를 결정한다.
- 크게 <u>(1) 값형(value type) (2) 참조형(reference type)</u>으로 나눈다.
- 값형: 숫자형, 문자형, 부울형, 열거형, 구조체형
- 참조형: 클래스형, 인터페이스, 배열형, 델리게이트형
- 숫자형(numeric type)
  - 정수형
    - 부호 있는(signed): `sbyte`, `short`, `int`, `long`
    - 부호 없는(unsigned): `byte`, `ushort`, `uint`, `ulong`
  - 실수형 - `float`, `double`
- 십진형(decimal type): `decimal`, 10진 연산을 ㅣ수행
- 문자형: `char`, 단일 문자를 저장, 유니코드를 사용한다.
- 부울형: `true` 혹은 `false`, 다른 자료형으로 변환될 수 없다.



### 1.2.2 연산자

- 표준 C 언어와 유사
- 형 검사 연산자(type testing operator)
  - `is`: 호환 가능한지 검사
  - `as`: 형을 변환함



### 1.2.3 배열

- **배열(array)**: 같은 자료형을 갖는 값들을 여러 개 저장하는 자료구조

- 배열 사용 3단계

  - 배열 변수 선언: 배열을 가리키는 참조 변수 선언

    ```c#
    자료형[] 배열이름;	
    자료형[,] 배열이름;
    자료형[][] 배열이름;
    ```

    ```c#
    int[] vector;
    short[,] matrix;
    long[][] arrayOfArray;
    object[] myArray1, myArray2;
    ```

  - 배열 객체 생성: `new` 연산자로 배열 객체를 생성(하고 참조 변수에 할당)

    ```c#
    new 자료형[배열의길이]
    ```

    ```c#
    vector = new int[3];
    matrix = new short[10,100];
    ```

  - 배열 사용

    ```c#
    for (int i = 0; i < vector.Length; i++) {
        vector[i] = i;
    }
    ```



### 1.2.4 스트링

- **스트링(string)**은 문자의 배열이 아니라 `System.String` 클래스의 객체이다.

- `string`은 `System.String`의 alias이다.

- 스트링 상수: `""`로 묶는다.

- 스트링의 선언과 초기화

  ```c#
  string s1 = "Hello";
  string s2 = new String("Hello");
  // Hello 라는 스트링 상수로 s2를 초기화한다.
  ```

  - 위 두 문장은 내부적으로 동등한 의미를 가진다.

- 스트링 연결(string concatenation): `+`

  ```c#
  string s = "LANA ";
  s += "ALBUM";		// s == "LANA ALBUM"
  ```

  

## 1.3 주요 특징

### 1.3.1 클래스

- 객체는 어떠한 유형을 가지는 것들로 분류될 수 있고, 객체지향에서 이것을 객체 자료형(object type) 혹은 객체 클래스(object class)라고 한다.
- **클래스의 구조**: 크게 <u>(1) **필드**: 객체의 속성을 나타낸다 (2) **메서드**: 객체의 행위를 정의한다</u> 로 나눌 수 있다.
- 이들을 통틀어 **클래스 멤버(class member)**라고 하며, 필드 계통과 메서드 계통으로 나눈다.
- 클래스의 설계: 객체를 위한 클래스를 만드는 일. 재사용성(reusability)를 고려한다. 일반적으로 정보 은닉(information hiding)을 위해 필드는 `private`, 메서드는 `public`으로 선언한다.



### 1.3.2 프로퍼티

- **프로퍼티(property)**: 클래스의 `private` 필드를 형식적으로 다루는 일종의 메서드
- 프로퍼티의 구성: <u>(1) **셋-접근자(set-accessor)**: 값을 지정한다. (2) **겟 접근자(get-accessor)**: 값을 참조한다.</u>
- 프로퍼티의 동작: 필드처럼 사용하지만 메서드처럼 동작한다. 배정문의 왼쪽에서 사용하면 셋-접근자를, 오른쪽에서 사용하면 겟-접근자를 호출한다.

```c#
private int privateValue;
public int PrivateValue {
    get { return privateValue; }
    set { privateValue = value; }
}
public void PrintValue() {
    Console.WriteLine(privateValue);
}
// ...
obj.PrivateValue = 3;					// set-accessor 호출
Console.WriteLine(obj.PrivateValue); 	// get-accessor 호출
```



### 1.3.3 연산자 중복

- **연산자 중복(operator overloading)**: 내장 연산자(시스템에서 제공하는 연산자)를 재정의하는 것. 자료 추상화를 이룰 수 있다.

```c#
class Even {
    int evenNumber;
    public static Even operator++(Even e) {
        e.evenNumber += 2;
        return 2;
    }
}
```

```c#
public static [extern] 반환형 operator연산자 (파라미터1, 파라미터2) {
    
}
```

- 연산 순위나 결합 법칙 등 문법적인 규칙은 변경 불가



### 1.3.4 델리게이트

- **델리게이트(delegate)**: 메서드를 참조하기 위한 기법

  - 객체지향적이고 타입 안정적인 메서드 포인터

- 델리게이트의 선언

  ```c#
  [수정자] delegate 반환형 델리게이트이름(파라미터1, ...);
  ```

- 델리게이트 프로그래밍

  - (1) 델리게이트 정의: 참조할 메서드와 반환형, 매개변수의 개수와 매개변수의 자료형이 일치한다.

    ```c#
    delegate void MyDelegate();
    ```

  - (2) 참조할 메서드 정의

    ```c#
    class DelegateClass {
        public void MyMethod() {
            Console.WriteLine("In the DelegateClass.MyMethod...");
        }
    }
    ```

  - (3) 델리게이트 객체 생성

    ```c#
    class DelegateApp {
        public static void Main() {
            DelegateClass obj = new DelegateClass();
            MyDelegate md = new MyDelegate(obj.MyMethod);	// (4)
            md();	// (5)
        }
    }
    ```

  - (4) 델리게이트 객체에 메서드 연결

  - (5) 델리게이트를 통해 메서드 호출



### 1.3.5 이벤트

- **이벤트(event)**: 사용자 행동에 의해 발생하는 사건. C# 에서는 델리게이트를 이용하여 이벤트를 처리한다.

- 이벤트의 정의

  ```c#
  [이벤트-수정자] event 델리게이트타입 이벤트이름;
  ```

- 이벤트-주도 프로그래밍(event-driven programming)

  - 객체에 발생한 사건을 이벤트와 이벤트 처리기를 통하여 다른 객체에 알리고, 그에 대한 행위를 처리하도록 함.
  - 각 이벤트에 따른 작업을 독립적으로 기술
  - 프로그램의 구조가 체계적/구조적이며 복잡도를 줄일 수 있다.

- 이벤트 프로그래밍

  - (1) 델리게이트 정의: 이벤트 처리기와 형태가 일치. 혹은 `System.EventHandler` 델리게이트 사용
  - (2) 이벤트 선언: 미리 정의된 이벤트(predefined event)인 경우 이 과정을 생략
  - (3) 이벤트 처리기 작성
  - (4) 이벤트 처리기 등록: 이벤트에 이벤트 처리기를 등록한다.
  - (5) 이벤트 발생
    - 미리 정의된 이벤트의 경우, 사용자에 의해 이벤트 발생. 
    - 사용자 정의 이벤트의 경우, 명시적으로 델리게이트 객체를 호출하여 이벤트 처리기를 작동

```c#
using System;
class EventApp{
    public EventHandler MyEvent;		// (2)
    void MyEventHandler(object sender, EventArgs e) {	// (3)
        Console.WriteLine("Hello, world!");
    }
    public EventApp() {
        this.MyEvent += new EventHandler(MyEventHandler);	// (4)
    }
    
    public void InvokeEvent() {			
        if (MyEvent != null) {
            MyEvent(this, null);		// (5)
        }
    }
    public static void Main() {
        new EventApp().InvokeEvent();
    }
}
```



### 1.3.6 제네릭

- **제네릭(generics)**: 자료형을 매개변수(형 매개변수)로 가질 수 있음.
- 제네릭의 단위: 클래스 ,구조체 , 인터페이스, 메서드
- 제네릭 클래스: **범용 클래스** 또는 포괄 클래스

```c#
class Stack<StackType> {
    private StackType[] stack = new StackType[100];
    public void Push(StackType element) {}
    public StackType Pop() {}
}
```

```c#
Stack<int> stk1 = new Stack<int>();
Stack<double> stk2 = new Stack<double>();
```



### 1.3.7 스레드

- **스레드(thread)**: 실행의 흐름. 시작, 실행, 종료의 순서를 가진다.
  - 실행되는 동안에 한 시점에서 단일 실행점을 가진다.
  - 프로그램 내에서만 실행 간으하다.
    - 스레드는 프로그램 내부에 있는 제어의 단일 순차 흐름(single sequential flow of control)이다.
- 멀티스레드(multithread): 스레드가 하나의 프로그램 내에 여러 개 존재할 수 있는 시스템
- 스레드 프로그래밍
  - (1) 메서드 작성: 스레드 몸체에 해당하는 메서드 작성
  - (2) 메서드를 `ThreadStart` 델리게이트에 연결
  - (3) 스레드 객체 생성: 생성된 델리게이트를 이용하여 스레드 객체 생성
  - (4) 스레드 실행 시작: `Start()` 메서드 호출

```c#
using System;
using System.Threading;
class ThreadApp {
    static void ThreadBody() {		// (1)
        Console.WriteLine("In the thread Body...");
    }
    public static void Main() {
        ThreadStart ts = new ThreadStart(ThreadBody);	// (2)
        Thread t = new Thread(ts);		// (3)
        Console.WirteLine("*** Start of Main");
        t.Start();		// (4)
        Console.WriteLine("*** End of Main");
    }
}
```



## 1.4 .NET 프레임워크

- 마이크로소프트가 개발한 프로그램 개발 환경



### 1.4.1 공통 언어 스펙

- **CLS(Common Language Specification)**: 언어의 상호 운용성(interoperability)을 지원하기 위한 스펙(specification)
  - 공통 언어(Common Language): CLS를 만족하는 언어
  - 예) C#, Visual Basic. .NET, J#, Managed C++, J# 등
  - BCL(Base Class Library; 기본 클래스 라이브러리): .Net 프레임워크에서 제공. CLS를 만족하면 BCL를 사용할 수 있다.



### 1.4.2 공통 자료형 시스템

- **CTS(Common Type System; 공통 자료형 시스템)**: 다른 언어와 상호 운용성에 필요한 공통의 자료형

<img src=".\Images\1-2.PNG" alt="1-2" style="zoom:50%;" />



### 1.4.3 실행 모델

<img src=".\Images\1-3.PNG" alt="1-3" style="zoom:50%;" />

- **실행 모델(excution model)**: 소스 프로그램이 작성되어 타겟 머신(target machine)에서 실행되는 방법을 의미
- **모듈**: 실행하는데 필요한 내용을 가지고 있는 *.exe 또는 *.dlll 형식의 파일
- 역어셈블리 과정: 이진 형태의 어셈블리 파일을 텍스트 형태로 된 파일(*.il)로 변환
- 어셈블리 과정: IL 파일을 어셈블라 파일로 변환
- IL 파일: 텍스트 형태로 된 중간 언어 파일, 컴파일된 코드를 확인할 수 있으며 디버깅에 사용할 수 있다.

<img src=".\Images\1-4.PNG" alt="1-4" style="zoom:50%;" />

JIT : just in time



### 1.4.4 공통 언어 런타임

- **CLR(Common Language Runtime; 공통 언어 런타임)**: .NET 프레임워크의 실행 시스템
- 프로그램의 실행을 도와주는 실행 환경을 포함한다.
  - 필수적인 실행 환경 컴포넌트: 메모리 관리기, 예외 처리기, 스레드 지원

<img src=".\Images\1-5.PNG" alt="1-5" style="zoom:60%;" />