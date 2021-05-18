# CH5. 파생 클래스와 인터페이스

- 파생 클래스: 이미 존재하는 클래스에 정보를 추가하여 만든 새로운 클래스
- C#은 단일 상속만을 지원한다: 다중 상속이 필요한 경우 인터페이스를 사용한다.
- 네임스페이스: 클래스와 인터페이스 등 여러 자료형을 묶어준다.



## 5.1 파생 클래스

- **상속(inheritance)**: 베이스 클래스의 모든 멤버들이 파생 클래스로 전달 되는 기능
- 상속의 종류: (1) 단일 상속: 베이스 클래스가 1개인 경우 (2) 다중 상속: 베이스 클래스가 2개인 경우
- C#은 단일 상속만을 지원한다.



### 5.1.1 파생 클래스의 정의

```c#
[class-modifiers] class 파생클래스이름 : 베이스클래스이름 {
	\\ 멤버 선언
}
```



#### 5.1.1.1 파생 클래스의 필드

- 클래스의 필드 선언 방법과 동일하다.
- 파생 클래스는 베이스 클래스의 모든 멤버를 상속받는다.
  - 단, `private` 멤버는 상속은 받지만 접근할 수 없다.
  - `public`, `protected` 멤버는 접근할 수 있다.



#### 베이스 클래스의 필드와 파생 클래스의 필드 이름이 같은 경우

- 베이스 클래스의 필드와 파생 클래스의 필드의 이름이 동일할 때, <u>베이스 클래스의 필드는 숨겨지며 파생 클래스의 필드는 보인다</u>. 즉, 베이스 클래스의 필드는 다만 상속되었다.



#### base 지정어로 숨겨진 필드에 접근하기

- `base.필드`: 파생 클래스 내부에서 베이스 클래스의 필드에 접근할 수 있다.

```c#
using System;

namespace HiddenNameApp
{
    class BaseClass
    {
        protected int a = 1;
        protected int b = 2;
    }
    class DerivedClass : BaseClass 
    {
        new int a = 3;
        new double b = 4.5;
        public void Output()
        {
            Console.WriteLine("BaseClass : a = {0}, DerivedClass:a = {1}", base.a, a);
        }
    }
  
    class Program
    {
        static void Main(string[] args)
        {
            DerivedClass obj = new DerivedClass();
            obj.Output();
        }
    }
}

```

- 베이스 클래스의 필드와 이름이 같은 파생 클래스의 필드의 선언에 `new`를 사용하여 hiding을 명시적으로 나타낸다. 이는 필드뿐만 아니라 메서드에서도 동일하다.
  - Line `12`: `new` 지정어를 사용하지 않으면 다음과 같은 경고 메시지가 출력된다.

> CS0108	'DerivedClass.a' hides inherited member 'BaseClass.a'. Use the new keyword if hiding was intended.



#### 5.1.1.2 파생 클래스의 생성자

- 클래스의 생성자와 형태와 의미가 같다.
- 파생 클래스의 생성자 내부에서 베이스 클래스의 생성자를 호출하지 않는다면, <u>베이스 클래스의 기본 생성자(default constructor)가 컴파일러에 의해 내부적으로 호출된다</u>.
  - 기본 생성자는 파라미터가 없으므로, 만일 베이스 클래스의 필드를 초기화하는 값을 전달하려면 반드시 베이스 클래스의 생성자를 호출해야한다.
- `base()`: 베이스 클래스의 생성자를 명시적으로 호출한다.
- `base()`를 호출할 때의 실행 과정
  - (1) 필드를 초기화하는 부분 실행
  - (2) 베이스 클래스의 생성자 실행
  - (3) 파생 클래스의 생성자 실행



#### 중복 정의된 베이스 클래스의 생성자 호출하기

```c#
파생클래스(파라미터리스트1) : base(파라미터리스트2) {

}
```

- 오버로딩된 베이스 클래스의 생성자 중 하나의 지정하는 방법: 파생 클래스의 생성자를 정의할 때 베이스 클래스의 생성자를 다음과 같이 명시한다.
  - `파라미터리스트2`: 파라미터들은 `파라미터리스트1`에 속할 수도 있으며, 리터럴 혹은 `파생클래스`의 필드일 수 있다.

```c#
class BaseClass
{
    public int a, b;
    public BaseClass()
    {
        a = 1; b = 1;
    }

    public BaseClass(int a, int b)
    {
       this.a = a; this.b = b;
    }
}
class DerivedClass : BaseClass
{
    public int c;
    public DerivedClass() : base()
    {
        c = 1;
    }
    public DerivedClass(int a, int b, int c) : base(a, b)
    {
        this.c = c;
    }
}
```



### 5.1.2 메서드 재정의

- **메서드 재정의**(overriding) 및 하이딩(hiding): 베이스 클래스에서 구현된 메서드와 파생 클래스에서 구현된 메서드의 <u>시그네쳐가 일치할 때</u>, 베이스 클래스에서 구현된 메서들를 파생 클래스에서 구현된 메서드로 대체하는 것.
- 메서드 중복(overloading): 베이스 클래스에서 구현된 메서드와 파생 클래스에서 구현된 메서드의 <u>이름이 동일하고 시그네쳐가 일치하지 않을 때,</u> 메서드가 중복되었다고 하며 둘 다 사용할 수 있다.



#### 5.1.2.1 메서드 재정의

```c#
using System;

namespace OverridingAndOverloadingApp
{
    class BaseClass
    {
        protected void PrintSomething()
        {
            Console.WriteLine("PrintSomething of BaseClass");
        }
    }
    class DerivedClass : BaseClass
    {
        new public void PrintSomething()
        {
            Console.WriteLine("Overriding: PrintSomething of DerivedClass without parameter");
        }

        public void PrintSomething(int i)
        {
            Console.WriteLine("Overloading: PrintSomething of DerivedClass with parameter " + i);
        }
    }
    class Program
    {

        static void Main(string[] args)
        {
            DerivedClass derived = new DerivedClass();

            derived.PrintSomething();
            derived.PrintSomething(100);
        }
    }
}

```

```
Overriding: PrintSomething of DerivedClass without parameter
Overloading: PrintSomething of DerivedClass with parameter 100
```



#### 5.1.2.2 지정어 virtual과 가상 메서드, new와 override

- **가상 메서드(virtual method)**: 지정어 `virtual`로 선언된 인스턴스 메서드
  - 파생 클래스에서 재정의하고 사용될 메서드임을 의미한다.
- 파생 클래스에서 가상 메서드를 재정의하는 방법: `new`나 `override` 지정어를 사용한다.
  - `new`: 객체의 타입에 따라(객체 참조 변수의 타입에 따라) 호출된다. 
  - `override`: 객체 참조가 가리키는 객체에 따라 호출된다. 

```c#
using System;

namespace VirtualMethodApp
{
    class BaseClass
    {
        virtual public void MethodA()
        {
            Console.WriteLine("Base Method A...");
        }
        virtual public void MethodB()
        {
            Console.WriteLine("Base Method B...");
        }
        virtual public void MethodC()
        {
            Console.WriteLine("Base Method C...");
        }
    }
    class DerivedClass : BaseClass
    {
        new public void MethodA()
        {
            Console.WriteLine("Derived Method A...");
        }
        override public void MethodB()
        {
            Console.WriteLine("Derived Method B...");
        }
        public void MethodC()
        {
            Console.WriteLine("Derived Method C...");
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            BaseClass derived = new DerivedClass();

            derived.MethodA();
            derived.MethodB();
            derived.MethodC();
        }
    }
}

```

```
Base Method A...
Derived Method B...
Base Method C...
```

- Line `41`: `derived`객체 참조는 `BaseClass` 타입이므로, 지정어 `new`에 따라 베이스 메서드 호출
- Line `42`: `derived` 객체 참조가 가리키는 객체의 타입은 `Derived`이므로, 지정어 `override`에 따라 파생 메서드 호출
- Line `43`: `derived`의 타입에 따라 `BaseClass`의 메서드가 호출된다. (5.1.5 다형성 참고)



#### 5.1.2.3 수정자 sealed와 봉인 메서드

- **봉인 메서드(sealed method)**: 수정자 `sealed`로 선언된 메서드
  - 해당 메서드는 파생 클래스에서 재정의하는 것을 허용하지 않는다.
- 봉인 클래스: 수정자 `sealed`로 선언된 클래스
  - 해당 클래스의 모든 메서드는 묵시적으로 봉인 메서드이다.



#### 5.1.2.4 수정자 abstract와 추상 클래스

- **추상 클래스(abstract class)**: 추상 메서드를 하나 이상 갖는 클래스

  - 추상 클래스는 개체를 가질 수 없다.
  - 반드시 다른 클래스에 의하여 상속된 후 사용되어야한다.

- **추상 메서드(abstract method)**: 실질적인 구현 없이 메서드 선언만 있는 경우

- 추상 클래스의 선언

  ```c#
  abstract class 추상클래스이름{
      [method-modifiers] abstract 반환형 추상메서드이름();
      // 멤버 선언
  }
  ```

- `abstract` 수정자는 `vritual` 수정자의 의미를 포함한다.

  - 파생 클래스는 <u>반드시 추상 클래스의 추상 메서드를 구현</u>해야한다.

- 추상 메서드의 구현

  ```c#
  override [method-modifiers] 반환형 추상메서드이름(){
      // 메서드 몸체
  }
  ```

```c#
// 추상 클래스와 파생 클래스에서의 추상 메서드 구현
using System;

namespace AbstractClassApp
{
    abstract class AbstractClass
    {
        public abstract void MethodA();
        public void MethodB()
        {
            Console.WriteLine("Implementation of MethodB()");
        }
    }
    class ImpClass : AbstractClass
    {
        override public void MethodA()
        {
            Console.WriteLine("Implementaion of MethodA()");
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            ImpClass obj = new ImpClass();

            obj.MethodA();
            obj.MethodB();
        }
    }
}

```

```
Implementaion of MethodA()
Implementation of MethodB()
```



### 5.1.3 메서드 설계

- 파생 클래스에서 메서드를 재정의할 때, 베이스 클래스의 메서드에 기능을 추가하는 새로운 기능을 갖도록 할 수 있다.

```c#
class BaseClass {
    public void PrintSomething(){
        Console.WriteLine("PrintSomething of BaseClass...");
    }
}

class DerivedClass : BaseClass{
    public void PrintSomething(){
        base.PrintSomething();
        Console.WriteLine("PrintSomething of DerivedClass...");
    }
}
```



### 5.1.4 클래스형 변환

- 상향식 캐스트(casting up): 파생 클래스의 객체가 베이스 클래스형으로 바뀌는 형 변환.
  - 자동으로 변환되는 타당한 변환이다.
- 하향식 캐스트(casting down): 베이스 클래스의 객체가 파생 클래스형으로 바뀌는 형 변환
  - 캐스팅 연산자로 명시적으로 이루어지는 타당하지 않은 변환이다. 실행 시간에 예외(`System.InvalidCastException`)이 발생한다.



### 5.1.5 다형성(Polymorphism)

- **다형성(polymorphsim)**: 적용하는 객체에 따라 메서드의 의미가 달라지는 것
  - 베이스 클래스에서 `virtual`로 선언하고 파생 클래스에서 `override`로 메서드를 재정의해야 한다.



## 5.2 인터페이스

- **인터페이스(interface)**: 사용자 접속을 기술할 수 있는 프로그래밍 단위. 
- 인터페이스의 멤버
  - <u>구현되지 않은 멤버</u>들로 구성되었다.
  - 멤버로 메서드, 프로퍼티, 인덱서, 이벤트가 가능하다.
- 인터페이스와 상속: 인터페이스를 상속받은 클래스는 반드시 인터페이스의 멤버를 구현하도록 강제된다.



### 5.2.1 인터페이스 선언

```c#
[interface-modifiers][partial]interface 인터페이스이름 {
	// interface body
}
```

- `인터페이스이름`: 일반적으로 첫글자는 대문자로 시작한다.
- <u>인터페이스의 멤버: 몸체가 구현되지 않은 메서드, 프로퍼티, 인덱서, 이벤트</u>
  - 인터페이스의 메서드는 묵시적으로 추상 메서드이다.
    - 따라서 <u>인터페이스를 상속받는 클래스가 인터페이스의 모든 메서드를 구현하지 않는다면, 추상 클래스로 선언해야 한다.</u>
  - <u>인터페이스의 멤버들은</u> 클래스 의존적이므로 `static`일 수 없으며, 묵시적으로 <u>`public`이다</u>.
    - 따라서 <u>인터페이스의 멤버들을 구현할 때, `public` 접근 수정자를 지정한다</u>.
- 인터페이스는 생성자를 갖지 않는다.
- `interface-modifiers`: (1) 접근 수정자 (2) `new`
- `partial`: 인터페이스를 서로 다른 소스 파일에서 부분 선언할 수 있다. (클래스, 구조체, 인터페이스 모두 부분 선언할 수 있다.)

```c#
// 인터페이스 선언 예시
public interface IExample {
	void MyMethod();		// 메서드
    void MyProperty{get; set;}	// 프로퍼티
    string this[int index]{get; set;}	// 인덱서
    event MyEventHandler MyHandler;		// 이벤트
}
```



### 5.2.2 인터페이스 확장과 다중 상속

- 인터페이스는 인터페이스를 상속 받을 수 있다. 전자를 베이스 인터페이스(derived class), 후자를 파생 인터페이스(base inteface)라고 한다.
- 베이스 인터페이스는 파생 인터페이스로부터 모든 멤버를 상속받느다.

```c#
[interface-modifiers]interface 인터페이스이름 : 베이스인터페이스1, ... {
	// interface body
}
```

- **다중 상속(multiple inheritance)**: 여러 개의 인터페이스로부터 상속받는 것
  - 베이스 인터페이스의 멤버가 파생된 인터페이스의 멤버와 이름이 같다면 베이스 인터페이스의 멤버는 숨겨진다. 이는 `new`로 지정어로 재정의할 수 있다.
  - 여러 개의 베이스 인터페이스의 멤버의 이름이 같다면, 베이스 인터페이스의 이름을 통해 멤버를 참조할 수 있다(5.2.3 예시 참고).



### 5.2.3 인터페이스 구현

- 인터페이스는 메서드, 프로퍼티, 인덱서, 이벤트의 선언으로 이루어져있으므로 반드시 클래스를 통하여 구현된 후 객체를 가질 수 있다. 즉, 생성자를 가지지 않는다.

```c#
[class-modifier] 클래스이름 : 인터페이스리스트 {
    // 멤버 선언
}
```



```c#
// 베이스 인터페이스들의 메서드 이름이 동일할 때, 클래스에서 메서드를 구현하는 방법
// 캐스팅 업: 파생 클래스 객체를 인터페이스로 상향 형변환
using System;

namespace ImplementingInterfaceApp
{
    interface IX {
        void PrintHello();
    }
    interface IY {
        void PrintHello();
    }
    class MyClass : IX, IY
    {
        void IX.PrintHello()
        {
            Console.WriteLine("Implementation of IX");
        }

        void IY.PrintHello()
        {
            Console.WriteLine("Implementation of IY");
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            MyClass m = new MyClass();

            // m.PrintHello();      에러
            ((IX)m).PrintHello();	// 캐스트 연산자로 상향 형변환
            ((IY)m).PrintHello();

            IX ix = m;				// 자동 형변환
            IY iy = m;
            ix.PrintHello();
            iy.PrintHello();
        }
    }
}

```



### 다이아몬드 상속

- 클래스는 클래스와 인터페이스를 동시에 상속받을 수 있다.

```c#
[class-modifiers] 클래스이름 : 베이스클래스, 인터페이스리스트 {
	// 멤버 정의
}
```

- 다이아몬드 상속(diamond inheirtance): 아래와 같은 상속 관계를 가질 때, 다이아몬드 상속이라고 한다.

```c#
interface IX {}
interface IY : IX {}
class A : IX {}
class B : A, IY {}
```



```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace DiamondApp
{
    interface IX { void XMethod(int i); }
    interface IY : IX { void YMethod(int i); }
    class A : IX
    {
        private int a;
        public int PropertyA
        {
            get { return a; }
            set { a = value; }
        }
        public void XMethod(int i) { a = i; }
    }
    class B : A, IY
    {
        private int b;
        public int PropertyB
        {
            get { return b; }
            set { b = value; }
        }
        public void YMethod(int i) { b = i; }
    }
    class Program
    {
        public static void Main()
        {
            B obj = new B();
            obj.XMethod(5); obj.YMethod(10);
            Console.WriteLine("a = {0}, b = {1}",
                obj.PropertyA, obj.PropertyB);
        }
    }
}

```

```
a = 5, b = 10
```



### 구조체로 인터페이스 구현하기

```c#
[struct-modifiers] struct 구조체이름 : 인터페이스리스트 {
    // 멤버 선언
}
```



```c#
using System;

namespace StructImpApp
{
    interface IList
    {
        void Insert(string s);
        void Print();
    }

    struct List : IList
    {
        private string[] list;
        private int indexOfLastElement;

        public List(int length)
        {
            list = new string[length];
            indexOfLastElement = -1;
        }

        public void Insert(string s)
        {
            if (indexOfLastElement == list.Length)
            {
                Console.WriteLine("리스트가 꽉 차 있습니다.");
                return;
            }

            list[++indexOfLastElement] = s;
        }

        public void Print()
        {
            for (int i = 0; i <= indexOfLastElement; i++)
            {
                Console.Write("({0}) {1} ", i + 1, list[i]);
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            List l = new List(10);
            l.Insert("purple");
            l.Insert("ultraviolence");
            l.Insert("blue");
            l.Print();
        }
    }
}

```

```
(1) purple (2) ultraviolence (3) blue
```



### 인터페이스와 추상 클래스

- (공통점) 객체를 가질 수 없다.
- (차이점)

| 구분                                           | 인터페이스              | 추상 클래스                     |
| ---------------------------------------------- | ----------------------- | ------------------------------- |
| 다중 상속을 지원하는가?                        | 다중 상속 지원          | 단일 상속 지원                  |
| 메서드를 구현할 수 있는가?                     | 오직 메서드 선언만 가능 | 메서드 부분적으로 구현 가능(??) |
| 메서드 구현시, `override` 지정어를 사용하는가? | 사용하지 않음           | 사용해야 함.                    |



## 5.3 네임스페이스

- **네임스페이스(namespace)**: 서로 연관된 클래스, 인터페이스, 구조체, 열거형, 델리게이트, 하위 네임스페이스(subnamespace)을 하나의 단위로 묶어주는 것.



### 5.3.1 네임스페이스 선언

```c#
namespace 네임스페이스이름{
    
}
```



### 5.3.2 네임스페이스 사용

- (1) `using`문 사용하기

```c#
using 네임스페이스;
```

- (2) `네임스페이스`와 `.`연산자 사용하기

```c#
네임스페이스.클래스
```

