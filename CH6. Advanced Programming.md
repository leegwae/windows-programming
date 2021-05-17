# CH6. Advanced Programming

## CH6.1 제네릭

- 알고리즘은 자료형과 상관없이 동일하나, C#과 같은 strongly typed-language는 자료형 불일치에 따른 컴파일 에러를 방지하기 위해 동일한 의미의 코드를 서로 다른 자료형에 따라 중복해서 작성해야 함. 이를 위하여 제네릭을 도입하였음.
- **제네릭(generic)**: 변수의 형을 매개변수로 하여 클래스나 메서드의 알고리즘을 자료형과 무관하게 기술하는 기법



### 6.1.1 제네릭 클래스

- **제네릭 클래스(generic class)**: **형 매개변수(type parameter)**를 가지는 클래스

- 형 매개변수: <u>객체 생성 시에 전달 받아</u>, 클래스 내의 필드나 메서드를 선언할 때 자료형으로 사용

- 제네릭의 장점

  - 알고리즘의 재사용성을 높임
  - 자료형에 따른 프로그램의 중복을 방지
  - 프로그램의 구조를 단순하게 만듦

- 제네릭 클래스의 정의

  ```c#
  [modifiers] class 클래스이름<타입파라미터들>{
      // 멤버 선언
  }
  ```

  - `타입파라미터들`: 형 매개변수. 대개 `T`라고 쓴다.

- **형식 매개변수 형(formal parameter type)**: 제네릭 클래스를 선언할 때 사용한 형 매개변수
- **실제 형 인자(actual type argument)**: 제네릭 클래스에 대한 객체를 생성할 때 넘긴 자료형
- **매개변수화 된 형(parameterized type)**: 형 매개변수를 갖는 제네릭 클래스

```c#
using System;

namespace GenericClassApp
{
    class SimpleGeneric<T>
    {
        private T[] values;
        private int index;

        public SimpleGeneric(int len)
        {
            values = new T[len];
            index = 0;
        }

        public void add(params T[] args)
        {
            foreach(T e in args)
            {
                values[index++] = e;
            }
        }

        public void print()
        {
            foreach(T e in values)
            {
                Console.Write(e + " ");
            }
            Console.WriteLine();
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            SimpleGeneric<Int32> gInteger = new SimpleGeneric<Int32>(10);
            SimpleGeneric<string> gString = new SimpleGeneric<string>(10);

            gInteger.add(1, 2);
            gInteger.add(1, 2, 3, 4, 5, 6, 7);
            gInteger.print();

            gString.add("purple");
            gString.add("lana");
            gString.print();
        }
    }
}

```

```
1 2 1 2 3 4 5 6 7 0
purple lana
```



### 6.1.2 제네릭 인터페이스

- **제네릭 인터페이스(generic interface)**: 형 매개변수를 가지는 인터페이스

- 제네릭 인터페이스의 정의: 일반 인터페이스를 구현하는 과정과 동일하다.

  ```c#
  [modifiers] interface 인터페이스이름<타입파라미터들>{
      // 인터페이스 몸체
  }
  ```

```c#
using System;

namespace GenericInterfaceApp
{
    interface IGenericInteface <T>
    {
        void SetValue(T x);
        string GetValueType();
    }
    class GenericClass<T> : IGenericInteface<T>
    {
        private T value;
        public void SetValue(T x)
        {
            value = x;
        }
        public string GetValueType()
        {
            return value.GetType().ToString();
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            GenericClass<Int32> gIntger = new GenericClass<Int32>();
            GenericClass<String> gString = new GenericClass<String>();

            gIntger.SetValue(10);
            gString.SetValue("Lana");

            Console.WriteLine("gInteger.value의 타입은 " + gIntger.GetValueType());
            Console.WriteLine("gString.value의 타입은 " + gString.GetValueType());
        }
    }
}

```

```
gInteger.value의 타입은 System.Int32
gString.value의 타입은 System.String
```



### 6.1.3 제네릭 메서드

- **제네릭 메서드(generic method)**: 형 매개변수를 갖는 메서드

- 제네릭 메서드의 정의 예시

  ```c#
  void Swap<데이터타입>(ref 데이터타입 x, ref 데이터타입 Y){
      데이터타입 temp = x;
      x = y;
      y = temp;
  }
  ```

- 제네릭 메서드의 호출 예시

  ```c#
  Swap<Int>(a, b);	// a,b : 정수형
  ```


```c#
// 두 파라미터의 값을 참조하여 바꾸는 Swap 메서드의 제네릭 버전
using System;

namespace GenericMethodApp
{
    class Program
    {
        static void Swap<T>(ref T a, ref T b)
        {
            T temp = a;
            a = b;
            b = temp;
        }
        static void Main(string[] args)
        {
            int x = 10;
            int y = 20;

            Console.WriteLine("x={0},y={1}", x, y);
            Swap<int>(ref x, ref y);
            Console.WriteLine("x={0},y={1}", x, y);
        }
    }
}
```

```
x=10,y=20
x=20,y=10
```



```c#
// 인덱서와 제네릭
using System;

namespace Generic
{
    class MyList<T>
    {
        private T[] array;

        public MyList(int length)
        {
            array = new T[length];
        }

        public T this[int index]
        {
            get
            {
                return array[index];
            }
            set
            {
                if (index >= array.Length)
                {
                    Array.Resize<T>(ref array, index + 1);
                    Console.WriteLine("Array Resized: {0}", array.Length);
                }
                array[index] = value;
                Console.WriteLine("array[{0}]={1}", index, array[index]);
            }
        }

        public int Length
        {
            get
            {
                return array.Length;
            }
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            MyList<int> int_list = new MyList<int>(2);

            Console.WriteLine("===============int_list===============");
            for (int i = 0; i < 5; i++)
            {
                int_list[i] = i;
            }

            MyList<string> string_list = new MyList<string>(2);

            Console.WriteLine("===============string_list===============");
            for (int i = 0; i < 5; i++)
            {
                string_list[i] = i + "번째요소";
            }
        }
    }
}

```

````
===============int_list===============
array[0]=0
array[1]=1
Array Resized: 3
array[2]=2
Array Resized: 4
array[3]=3
Array Resized: 5
array[4]=4
===============string_list===============
array[0]=0번째요소
array[1]=1번째요소
Array Resized: 3
array[2]=2번째요소
Array Resized: 4
array[3]=3번째요소
Array Resized: 5
array[4]=4번째요소
````



```c#
// 제네릭 메서드 예시
using System;

namespace CopyingArrayApp
{

    class Program
    {
        static void CopyArray<T>(T[] source, T[] target)
        {
            for (int i = 0; i < source.Length; i++)
            {
                target[i] = source[i];
            }
        }

        static void Main(string[] args)
        {
            int[] src = { 1, 2, 3, 4, 5 };
            int[] target = new int[src.Length];


            Console.WriteLine("Copy 전: ");
            foreach (int i in target)
            {
                Console.Write(i + " ");
            }
            Console.WriteLine();
            CopyArray<int>(src, target);
            Console.WriteLine("Copy 후: ");
            foreach (int i in target)
            {
                Console.Write(i + " ");
            }
        }
    }
}

```

```
Copy 전:
0 0 0 0 0
Copy 후:
1 2 3 4 5
```



- 제네릭 메서드의 형 매개변수의 이름과 제네릭 클래스의 형 매개변수 이름이 같은 경우: 서로 독립된 형 매개변수의 개념을 가진다.
  - 제네릭 클래스는 객체 생성 시 형 매개변수를 전달받는다.
  - 제네릭 메서드는 호출 시에 유추하여 형 매개변수가 결정된다.



### 6.1.4 형 매개변수의 제한과 키워드 where

- 제네릭을 사용하여 프로그램의 유연성이 높아지긴 했지만, 신뢰성을 위해 동작하는 타입의 범위를 제한할 필요성이 있음.
- `where` 키워드: 제네릭의 범위를 제한한다.
  - `T`를 `S`의 서브클래스로 제한한다.

```c#
<T> where T : S
```



| 제한조건                   | 설명                                             |
| -------------------------- | ------------------------------------------------ |
| `where T : struct`         | `T`는 값형이어야 한다.                           |
| `where T : class`          | `T`는 참조형이어야 한다.                         |
| `where T : new()`          | `T`는 매개변수가 없는 생성자가 있어야 한다.      |
| `where T : 클래스이름`     | `T`는 `클래스이름`의 파생 클래스여야 한다.       |
| `where T : 인터페이스이름` | `T`는 `인터페이스이름`를 구현한 클래스여야 한다. |
| `where T : U`              | `T`는 `U`로부터 파생된 클래스여야 한다.          |



```c#
using System;

namespace BoundedGenericApp
{
    class GenericExceptionType<T> where T : SystemException
    {
        private T value;
        public GenericExceptionType(T v)
        {
            value = v;
        }
        public override string ToString()
        {
            return "Type is " + value.GetType();
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            GenericExceptionType<NullReferenceException> gNullExp 
                = new GenericExceptionType<NullReferenceException>(new NullReferenceException());
            GenericExceptionType<IndexOutOfRangeException> gIdxExp 
                = new GenericExceptionType<IndexOutOfRangeException>(new IndexOutOfRangeException());

            Console.WriteLine(gNullExp);
            Console.WriteLine(gIdxExp);
        }
    }
}

```



## 6.2 에트리뷰트

- **에트리뷰트**: 어셈블리, 클래스, 필드, 메서드, 프로퍼티 등에 다양한 종류의 속성 정보를 추가하기 위해 사용
- 어셈블리에 메타데이터(metadata) 형식으로 저장. 
- 저장된 정보는 .NET 프레임워크나 C# 또는 다른 언어의 컴파일러에 의해 다양한 용도로 사용
- 에트리뷰트를 사용하기 위해서는 .NET 프레임워크의 **리플렉션(reflection)** 기능을 사용
- 에트리뷰트의 정의
  - `positional_parameter`: 생정자 매개변수. 필수적인 정보이다.
  - `named_parameter`: 속성에 해당. 선택적인 정보이다.

```c#
[어트리뷰트이름("positional_parameter", named_parameter = value, ...)]
```

- 에트리뷰트의 종류
  - 표준 에트리뷰트(.NET 프레임워크에서 제공)
  - 사용자 정의 에트리뷰트



### 6.2.1 표준 에트리뷰트

- 표준 에트리뷰트
  - `Conditional` 에트리뷰트
  - `Obsolte` 에트리뷰트



#### 6.2.1.1 Conditional 에트리뷰트

- `Conditional` 에트리뷰트: 조건부 메서드를 작성할 때 사용
  - 전처리기 지시어를 이용하여 명칭을 정의: `#define`으로 정의된 문자를 사용하였을 경우 메서드를 실행하지만, 정의되지 않은 문자를 사용하여 정의하면 메서드가 호출되지 않는다.
  - `System.Diagnostics` 네임스페이스를 사용해야 한다.



```c#
#define JAVA
#define PYTHON
using System;
using System.Diagnostics;

namespace ConditionalAttrApp
{
    class Program
    {
        [Conditional("C")]
        public static void CMethod()
        {
            Console.WriteLine("C 메서드!");
        }
        public static void PythonMethod()
        {
            Console.WriteLine("Python 메서드!");
        }
        static void Main(string[] args)
        {
            CMethod();
            PythonMethod();
        }
    }
}

```

```
Python 메서드!
```



#### 6.2.1.2 Obsolete 어트리뷰트

- `Obsolete` 에트리뷰트: 앞으로 사용되지 않을 메서드를 표시한다.
  - 해당 에트리뷰트를 가진 메서드를 호출할 경우, 컴파일러는 컴파일 과정에서 에트리뷰트에 설정한 내용이 출력한 결과를 발생시킴.

```c#
using System;

namespace ObsoleteAttrApp
{
    class Program
    {
        [Obsolete("경고, Obsolete Method입니다!")]
        public static void OldMethod()
        {
            Console.WriteLine("In the Old Method...");
        }
        static void Main(string[] args)
        {
            OldMethod();
        }
    }
```



### 6.2.2 사용자 정의 에트리뷰트

- **사용자 정의 에트리뷰트**: 프로그래머가 정의한 에트리뷰트
  - `System.Attribute` 클래스의 파생 클래스로 정의
- 사용자 정의 에트리뷰트 정의 예시
  - `에트리뷰트이름Attribute`: 사용할 때는 `Attribute`를 제외하고 `에트리뷰트이름`만을 사용한다.

```c#
public class 에트리뷰트이름Attribute:Attribute{
    
}
```

- 사용자 정의 에트리뷰트 사용 예시

```c#
[에트리뷰트이름()]
```



```c#
using System;

namespace MyAttributeApp
{
    public class MyAttrAttribute : Attribute
    {
        private string message;
        public String Message
        {
            get { return message; }
        }
        public MyAttrAttribute(string message)
        {
            this.message = message;
        }
    }
    [MyAttr("This is Attribute Test")]
    class Program
    {
        static void Main(string[] args)
        {
            Type type = typeof(Program);
            object[] arr = type.GetCustomAttributes(typeof(MyAttrAttribute), true);
            if (arr.Length == 0)
            {
                Console.WriteLine("this class has no custom attributes");
            }
            else
            {
                MyAttrAttribute ma = (MyAttrAttribute)arr[0];
                Console.Write(ma.Message);
            }
        }
    }
}

```

```
This is Attribute Test
```

