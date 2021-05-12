# CH4. 클래스 II

## 4.8 연산자 중복

- **연산자 중복(opreator overriding)**: 시스템 제공 연산자를 <u>특정 클래스만을 위한 연산 의미를 갖도록 재정의</u>하는 것

  - 클래스만을 위한 연산자로, 자료 추상화가 가능
  - 연산 순위, 결합 법칙  등 문법적인 규칙은 변경 불가

- 연산자 중복이 가능한 연산자

  | 종류   | 연산자                                                       |
  | ------ | ------------------------------------------------------------ |
  | 단항   | `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`              |
  | 이항   | `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `<`, `>`, `<=`, `>=` |
  | 형변환 | 변환하려는 자료형 이름                                       |

  - 단항 `true`, `false`는 연산자는 아니지만, 클래스의 객체를 조건식에 사용하는 경우 `bool` 형의 값을 얻기 위해 중복한다. (??)

- 연산자 중복 정의 형태

  ```c#
  public static [extern] 반환형 opeartor 연산기호 (파라미터리스트) {
       // 메서드 몸체
  }
  ```

  - 수정자는 반드시 `static`
  - 지정어 `operator` 사용
  - `연산기호`는 특수 문자 사용



### 4.8.1 연산자 중복 정의 규칙

- 매개변수 형과 반환형 규칙

  | 연산자                  | 매개변수형                                              | 반환형                         |
  | ----------------------- | ------------------------------------------------------- | ------------------------------ |
  | 단항 `+`, `-`, `!`, `~` | 자신의 클래스                                           | 모든 자료형                    |
  | `++`/ `--`              | 자신의 클래스                                           | 자신의 클래스<br />파생 클래스 |
  | `true`/ `false`         | 자신의 클래스                                           | `bool`                         |
  | 쉬프트 연산             | 첫번째 매개변수: 클래스형<br />두번째 매개변수: `int`형 | 모든 자료형                    |
  | 이항                    | 두 개의 매개변수 중 하나는 자신의 클래스                | 모든 자료형                    |

- 대칭적으로 정의해야 하는 연산자: 둘 중 하나만 정의하면 안 된다.

  - `++`/ `--`, `true`/ `false`, `==`/`!=`, `<`/ `>`, `<=`/`>=`



```c#
using System;

namespace OperatorOverloadingApp
{
    class Complex
    {
        private double real;
        private double img;

        public Complex(double real, double img)
        {
            this.real = real;
            this.img = img;
        }

        public static Complex operator+(Complex x1, Complex x2)
        {
            Complex c = new Complex(0, 0);
            c.real = x1.real + x2.real;
            c.img = x1.img + x2.img;

            return c;
        }

        public override string ToString()
        {
            return "(" + real + ", " + img + "i)";
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Complex x, y, c;
            x = new Complex(1, 2);
            y = new Complex(3, 4);
            c = x + y;
            Console.WriteLine(x + "+" + y + "=" + c);
        }
    }
}

```

```
(1, 2i)+(3, 4i)=(4, 6i)
```



### 4.8.2 형 변환 연산자 중복 정의

- 형 변환 연산자의 중복 정의

  - 사용자 정의 형 변환(user-defined type conversion)을 정의
  - 클래스 객체/구조체를 다른 클래스 객체/구조체/C# 기본 자료형으로 변환

- 형 변환 연산자 중복 정의 형태

  ```c#
  public static [extern] explicit operator 데이터타입이름(파라미터){}
  public static [extern] implicit operator 데이터타입이름(파라미터){}
  ```

  - `데이터타입이름`: 기본 자료형 혹은 클래스 이름
  - `파라미터`: 형 변환 연산자는 단항이므로 매개변수는 반드시 한 개
  - `implicit`: 형 변환이 묵시적으로 이루어질 경우 해당 메서드 연산자 정의 사용
  - `explicit`:  형 변환이 명시적으로 이루어질 경우 해당 메서드 연산자 정의 사용



#### 4.8.2.1 중복 정의된 형 변환 연산자의 적용 순서

```c#
using System;

namespace XBoolApp
{
    class XBool
    {
        public bool b;
		
        // Xbool to bool
        public static explicit operator bool(XBool x)
        {
            Console.WriteLine("In the explicit operator bool...");
            return x.b;
        }

        // Xbool to bool
        public static bool operator true(XBool x)
        {
            Console.WriteLine("In the operator true...");
            return x.b ? true : false;
        }

        // Xbool to bool
        public static bool operator false(XBool x)
        { 
            Console.WriteLine("In the operator false...");
            return x.b? false : true;
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            XBool xBool = new XBool();
            xBool.b = false;

            if (xBool) Console.WriteLine("True");
            else Console.WriteLine("False");
            Console.WriteLine((bool)xBool);
        }
    }
}

```



```c#
xBool.b = false;

if (xBool) Console.WriteLine("True");
else Console.WriteLine("False");
```

- `if (xBool)`: 어떤 형변환 연산자도 정의되어있지 않고 논리 연산될 때
  - (1) 해당 객체의 클래스 내에 implicit 연산자가 정의되어 있는지 확인
  - (2) 해당 객체의 클래스 내에 true operator가 정의되어 있는지 확인
- 전체 코드의 개선

```c#
using System;

namespace XBoolApp
{
    class XBool
    {
        public bool b;

        public static implicit operator bool(XBool x)
        {
            Console.WriteLine("In the explicit operator bool...");
            return x.b;
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            XBool xBool = new XBool();
            xBool.b = false;

            if (xBool) Console.WriteLine("True");
            else Console.WriteLine("False");
            Console.WriteLine((bool)xBool);
        }
    }
}

```



#### 4.8.2.2 사용자 정의 형 변환

```c#
using System;

namespace UDTConversionApp
{
    class Mile
    {
        public double distance;
        public Mile(double distance)
        {
            this.distance = distance;
        }

        // Mile operator(implicit): double to Mile
        public static implicit operator Mile(double distance)
        {
            return new Mile(distance);
        }

        // Kilometer operator(explicit): Mile to Kilmeter
        public static explicit operator Kilometer(Mile mile)
        {
            return mile.distance * 1.609;
        }
    }
    class Kilometer
    {
        public double distance;
        public Kilometer(double distance)
        {
            this.distance = distance;
        }

        // Kilometer operator(implicit): double to Kilometer
        public static implicit operator Kilometer(double distance)
        {
            return new Kilometer(distance);
        }

        // Mile operator(explicit): Kilometer to Mile
        public static explicit operator Mile(Kilometer k)
        {
            return k.distance / 1.609;
        }

    }
    class Program
    {
        static void Main(string[] args)
        {
            Kilometer k = new Kilometer(100.0);
            Mile m = (Mile)k;
            Console.WriteLine("{0}km = {1} mile", k.distance, m.distance);

            m = 65.0;
            k = (Kilometer)m;
            Console.WriteLine("{0}km = {1} mile", m.distance, k.distance);
        }
    }
}

```

??? 이해하기



## 4.9 구조체

- **구조체(struct)**: 객체의 구조와 행위를 정의하는 방법. 멤버 변수(필드)와 멤버 함수(메서드)를 가진다.

- 구조체의 형태

  ```c#
  [struct-modifiers] struct 구조체이름 {
      // 멤버 선언
  }
  ```

  - `struct-modifiers`: (1) 접근 수정자 `public`, `protected`, `internal`, `private`와 (2) `new`
  - `구조체이름`: 일반적으로 대문자로 시작



```c#
using System;

namespace StructApp
{
    struct Point
    {
        public int x, y;
        public Point(int x, int y)
        {
            this.x = x;
            this.y = y;
        }

        public override string ToString()
        {
            return "(" + x + "." + y + ")";
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Point[] pts = new Point[3];
            pts[0] = new Point(100, 100);
            pts[1] = new Point(100, 200);
            pts[2] = new Point(200, 100);
            foreach(Point pt in pts)
            {
                Console.WriteLine(pt);
            }
        }
    }
}

```



### 4.9.1 구조체와 클래스의 차이점

| 항목\구분                      | 구조체           | 클래스           |
| ------------------------------ | ---------------- | ---------------- |
| 데이터타입                     | 값형             | 참조형           |
| 객체가 저장되는 곳             | 스택에 저장      | 힙에 저장        |
| 배정 연산에서 복사되는 것      | 내용이 복사된다. | 참조가 복사된다. |
| 상속을 하거나 받을 수 있는가   | 불가능           | 가능             |
| 소멸자를 가질 수 있는가        | 불가능           | 가능             |
| 멤버가 초기값을 가질 수 있는가 | 불가능           | 가능             |



## 4.6 델리게이트

- **델리게이트(delegate)**: 메서드를 참조하기 위한 기법

  - 객체지향적 특징이 반영된 메서드 포인터
  - 이벤트와 스레드를 처리하는데 사용

- 델리게이트의 특징

  - (1) 객체지향적: 정적 메서드 및 인스턴스 메서드 참조 가능
  - (2) 타입안정적: 델리게이트의 형태와 참조하고자하는 메서드의 형태는 항상 일치
  - (3) 델리게이트 객체가 메서드를 참조: 델리게이트 객체를 통하여 메서드를 호출

- 델리게이트는 클래스형이다

  - `System.Delegate` 클래스를 상속한 클래스형으로 간주됨
  - `Delegate` 클래스의 멤버들을 통해 메서드 호출 이외에 다른 사용 방법을 갖게 됨.

- 델리게이트와 함수포인터

  - 메서드를 참조한다는 점에서 유사하다.
  - 그러나 델리게이트는 객체 지향적이며 타입 안정적이다.

  



### 4.6.1 델리게이트의 정의

```c#
[modifiers] delegate 반환형 델리게이트이름(파라미터리스트);
```

- `modifiers`: (1) 접근 수정자(`public`, `protected`, `internal`, `private`) (2) 그 외 수정자:`new`
  - 클래스 밖에서 정의할 경우: `public`, `internal`만 가능하다.
  - 클래스 멤버일 경우: 필드 수정자의 의미와 같다.
- `델리게이트이름`: 일반적으로 대문자로 시작한다.

- <u>델리게이트는 메서드 포인터로서, 참조하려는 메서드의 형태와 델리게이트의 형태를 정확히 일치시켜야한다</u>.
  - **메서드 반환형, 매개변수의 개수와 매개변수의 형**을 일치시켜야한다.



### 4.6.2 델리게이트 객체 생성

```c#
델리게이트이름 변수 = new 델리게이트이름(참조할메서드이름);
```

- (1) 델리게이트 객체 참조 변수 선언 (2) 메서드 이름 전달하여 델리게이트 객체 생성 (3) 델리게이트 객체를 델리게이트 객체 참조 변수에 할당
- `참조할메서드이름`: 델리게이트 객체에 연결할 **메서드의 이름**을 인수로 전달한다.
- `참조할메서드`가 인스턴스 메서드인 경우, 해당 클래스의 객체를 먼저 생성한 후 델리게이트 객체를 생성할 수 있다. 



### 4.6.3 델리게이트 객체 호출

```c#
델리게이트이름(파라미터리스트);
```

- 델리게이트 객체의 호출은 일반 메서드의 호출과 동일하다.



```c#
using System;

namespace DelegateCallApp
{
    // 델리게이트 객체 정의
    delegate void DelegateWithoutParameter();
    delegate void DelegateWithParameter(int i);

    // 참조할 메서드의 클래스 정의
    class DelegateClass
    {
        public void MethodA()
        {
            Console.WriteLine("DelegateClass.MethodA: 매개변수가 없는 메서드");
        }

        public void MethodB(int i)
        {
            Console.WriteLine("DelegateClass.MethodB: 매개변수 i={0}가 있는 메서드", i);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            // 인스턴스 메서드의 객체 생성
            DelegateClass obj = new DelegateClass();

            DelegateWithoutParameter noParameter = new DelegateWithoutParameter(obj.MethodA);
            DelegateWithParameter hasParameter = new DelegateWithParameter(obj.MethodB);

            noParameter();
            hasParameter(100);
        }
    }
}

```



### 4.6.4 멀티캐스트

- **멀티캐스트 델리게이션(multicast delegation)**: 하나의 델리게이트 객체에 여러 개의 메서드를 연결하는 것. 델리게이트를 호출하면 해당 델리게이트 객체에 연결했던 메서드들이 모두 호출된다.
- 등록된 메서드의 호출 순서: 메서드가 등록된 순서와 동일하다.
- 델리게이트를 위한 연산자
  - `+`: 델리게이트 객체에 메서드 등록
  - `-`: 델리게이트 객체에 메서드 제거

```c#
using System;

namespace MultiCaseApp
{
    // 델리게이트 정의
    delegate void MyDelegate();
    
    class CommitManager
    {
        public static void PrintCommitTime()
        {
            Console.WriteLine("COMMIT TIME: " + DateTime.Now.ToString());
        }

        public void PrintCommitMessage()
        {
            Console.WriteLine("COMMIT MESSAGE: Update README.md");
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            CommitManager commitManager = new CommitManager();
            MyDelegate commitHandler = new MyDelegate(commitManager.PrintCommitMessage);
            commitHandler += CommitManager.PrintCommitTime;

            commitHandler();

        }
    }
}

```

```
COMMIT MESSAGE: Update README.md
COMMIT TIME: 2021-05-13 오전 12:12:20
```



#### 델리게이트 객체끼리 연산하기

```c#
d1 = new MyDelegate(obj.MethodA);
d2 = new MyDelegate(obj.MethdoB);
d3 = new MyDelegate(obj.MethodC);

d1 += d2;
d1 += d3;

d2 += d3;

d1 -= d2;

d1();	// MethodA, MethodC 호출
d2();	// MethodB, MethodC 호출
d3();	// MethodC 호출
```



## 4.7 이벤트

- **이벤트(event)**: 사용자 행동에 의해 발생하는 사건
  - 어떤 사건이 발생한 것을 알리기 위해 보내는 메시지
  - C#에서 이벤트 개념을 프로그래밍 언어 수준에서 지원
- **이벤트 처리기(event handler)**: 발생한 이벤트를 처리하기 위한 메서드
- 이벤트-주도 프로그래밍(event-driven programming)
  - 객체에 발생한 사건을 이벤트와 이벤트 처리기를 통해 다른 객체에 통지하고 그에 대한 행위를 처리하는 방식
  - 각 이벤트에 따른 작업을 독립적으로 기술
  - 프로그램의 구조가 체계적이고 구조적이어서, 프로그램의 복잡도를 줄일 수 있다.



### 4.7.1 이벤트 정의

```c#
[event-modifier] event 델리게이트타입 이벤트이름;
```

- `event-modifier`: (1) 접근 수정자 와 (2) 그 외 수정자: `new`, `static`, `virtual`, `sealed`, `override`, `abstract`, `extern`

  - 메서드 수정자와 종류, 의미가 같다: 이벤트 처리기로 메서드를 배정하기 때문

- `이벤트이름`: 일반적으로 대문자로 시작

- 이벤트 정의 순서

  - (1) <u>이벤트 처리기의 형태와 일치하는</u> 델리게이트를 정의

    혹은 `System.EventHandler` 델리게이트를 사용

  - (2) 델리게이트로 이벤트를 선언 (미리 정의된 이벤트인 경우 생략)

  - (3) 이벤트 처리기를 작성

  - (4) 이벤트에 이벤트 처리기를 등록

  - (5) 이벤트 발생

    - 사용자 정의 이벤트인 경우, 명시적으로 클래스 내부에서 이벤트 처리기 호출
    - 미리 정의된 이벤트인 경우 사용자가 이벤트 발생시킴
    - 등록된 메서드(이벤트 처리기)가 호출되어 이벤트를 처리



```c#
using System;

namespace EventHandlingApp
{
    // (1) 델리게이트 정의
    public delegate void MyEventHandler();
    
    class Button
    {
        // (2) 이벤트 정의
        public event MyEventHandler Push;
        public void OnPush()
        {
            if (Push != null)
            {
                Push();
            }
        }
    }

    // (3) 이벤트 처리기 작성
    class EventHandlerClass
    {
        public void MyMethod()
        {
            Console.WriteLine("In the EventHandlerClass.MyMethod...");
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Button button = new Button();
            EventHandlerClass obj = new EventHandlerClass();
            // (4) 이벤트 처리기 등록
            button.Push += new MyEventHandler(obj.MyMethod);
            button.OnPush();		// (5) 이벤트 발생
        }
    }
}

```



### 이벤트에 이벤트 처리기 등록하기

- 델리게이트에 메서드를 등록하던 것과 같은 방법을 사용한다.
- 이벤트 처리기 등록 연산자
  - `=`: 이벤트에 이벤트 처리기 등록
  - `+`: 이벤트에 이벤트 처리기 추가
  - `-`: 이벤트에 이벤트 처리기 제거



### 4.7.2 이벤트의 활용

- 이벤트의 선언: .NET 프레임워크는 이미 정의된 `System.EventHandler` 델리게이트로 이벤트를 선언하기를 권고

  ```c#
  delegate void EventHandler(object sender, EventArgs e);
  ```

  - `sender`: 이벤트를 발생시키는 객체를 나타냄
  - `e`: 해당 이벤트에 대한 추가적인 정보를 주는 객체

- 이벤트와 윈도우 환경

  - 이벤트는 사용자와 상호작용을 위해 주로 사용
  - 윈도우 프로그래밍 환경에서 사용하는 폼, 컴포넌트, 컨트롤에는 다양한 종류의 이벤트가 존재