# 6.3 Exception

- **예외(exception)**: 실행 시간에 발생하는 에러(run-time error)
  - 메서드 호출과 실행, 부정확한 데이터, 시스템 에러 등 다양한 상황에 의해 야기
  - 프로그램의 비정상적인 종료 혹은 잘못된 실행 결과로 이어질 수 있음
- **예외 처리(exception handler)**: 기대되지 않은 상황에 대해 예외를 발생시키고, 야기된 예외를 적절히 처리하는 것.
- 언어 시스템에서 예외 처리를 위한 방법을 제공하면,
  - 응용 프로그램의 신뢰성(reliability)가 향상
  - 예외 검사와 처리를 위한 프로그램 코드를 소스에 깔끔하게 삽입 가능



## 6.3.1 예외 정의

- 예외 클래스: C#은 예외를 하나의 객체로 취급한다.
  - `Exception` 클래스 또는 그것의 파생 클래스들 중 하나로부터 확장
- 새로운 예외 크래스 정의하기: 일반적으로 `Exception` 클래스의 파생 클래스 `ApplicationException` 클래스를 확장하여 정의함.
  - 예외에 관련된 메시지를 스트링 형태로 예외 객체에 담아 전달 가능

```c#
using System;

namespace UserExceptionApp
{
    class UserException : ApplicationException {
        public UserException(string msg) : base(msg) { }
    }
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                throw new UserException("throw a user exception!");
            } catch (UserException e)
            {
                Console.WriteLine(e.Message);
            }
        }
    }
}

```



- 예외의 종류
  - **시스템 정의 예외(system-defined exception** 혹은 **predefined exception)**
  - **프로그래머 정의 예외(programmer-defined exception)**

| 구분                                                         | 시스템 정의 예외                                             | 프로그래머 정의 예외                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 어떻게 발생하는가                                            | 묵시적 예외 발생: 프로그램의 실행을 지속할 수 없을 때, CLR에 의해 자동적으로 생성, 발생 | 명시적 예외 발생: 프로그래머에 의해 의도적으로 생성, 발생    |
| 예외 클래스                                                  | `SystemException`<br />`IOException`                         | `ApplicationException`                                       |
| 발생한 예외에 대한 예외 처리기가 존재하는지 컴파일러가 검사하는가 | 검사되지 않은 예외(unchecked exception): 검사되지 않는다.    | 검사된 예외(checked exception)                               |
| 기타                                                         | 프로그래머가 처리하지 않으면 CLR이 디폴트 예외 처리기(default exceptio handler)로 처리한다. | 프로그래머가 예외를 정의하였을 때, 컴파일러는 예외 처리기의 존재를 검사하고 없다면 컴파일 에러를 발생시킨다. |

- `Exception` 클래스의 계층도
  - `Object`
  - `Exception`
    - `ApplicationException`
    - `SystemException`

- 시스템 정의 예외의 종류
  - `ArithmeticException`: 산술 연산시 발생하는 예외
  - `IOException`
  - `IndexOutOfRangeException`: 배열, 스트링, 벡터 등과 같이 인덱스를 사용하는 객체에서 인덱스의 범위가 벗어날 때 발생하는 에외
  - `ArrayTypeMismatchException`: 배열의 원소에 잘못된 타입의 객체를 할당하였을 때 발생하는 예외
  - `InvliadCaseException`: 명시적 형 변환이 실패할 때 발생하는 에외
  - `NullReferenceException`: `null`을 사용하여 객체를 참조할 때 발생하는 예외
  - `OutOfMemoryException`: 메모리 할당(`new`)가 실패했을 때 발생하는 예외



## 6.3.2 예외 발생

- **예외 발생**: 예외가 발생하는 것

  - 묵시적 예외 발생: 시스템에 의해 일어나는 경우

  - 명시적 예외 발생: 프로그래머에 의해 일어나는 경우

    ```c#
    throw ApplicationExeptionObject;
    ```

    - 프로그래머는 생성된 메서드 내부에 예외 처리기(예외를 처리하는 코드)를 두어 직접 처리해야 한다.

```c#
// 묵시적 예외 발생: 시스템에 의해 예외가 발생한 경우
using System;

namespace SystemExThrowApp
{
    class Program
    {
        static void Main(string[] args)
        {
            int[] arr = { 1, 2, 3 };
            try
            {
                arr[4] = 3;
            } catch(IndexOutOfRangeException e)
            {
                Console.WriteLine(e.Message);
            }
        }
    }
}

```

```c#
Index was outside the bounds of the array.
```



## 6.3.3 예외 처리

- **예외 처리 구문(`try-catch-finally` 블록)**: 예외를 검사하고 처리해주는 문장
- 예외 처리 구문의 형태
  - `try`  블록: 예외를 검사한다. 여기서 예외가 발생하면, 블록 안의 다른 문장은 실행하지 않고 곧장 `catch` 블록으로 간다.
  - `catch` 블록: 예외를 처리하는 **예외 처리기(exception handler)**
    - `예외타입`: 처리할 예외의 타입을 명시
    - `예외참조변수`: 참조 변수가 필요하지 않다면 생략 가능
      - `catch (IndexOutOfRangeException) {}`
    - 컴파일러는 `catch` 블록에 나열된 순서대로 예외 처리기를 찾는다.
      - 따라서, 상속 관계에 있는 형이라면 서브클래스에 대한 예외 처리기를 먼저 기술해준다.
  - `finally` 블록: 선택적. 예외의 발생과 관련없이 무조건 실행된다.

```c#
try {
    
} catch (예외타입1 예외참조변수1){
    
} catch(예외타입2 예외참조변수2){
    
} finally {
    
}
```

- 예외 처리기의 실행 순서
  - (1) `try` 블록 내에서 예외를 검사, 또는 명시적으로 예외가 발생
  - (2) 해당 `catch` 블록(예외 처리기)를 찾아 예외를 처리
  - (3) `finally` 블록 실행
- 디폴트 예외 처리기: 단순히 에러에 대한 메시지를 출력하고 프로그램을 종료
  - 시스템 정의 예외가 발생했을 때, 해당 예외를 프로그래머가 처리하지 않았다면 발생



## 6.3.4 예외 전파

- 예외 전파(exception propagation): 호출한 메서드로 예외를 전파하여, 특정 메서드에서 예외를 모아 처리한다.
- 예외 전파의 과정
  - (1) 예외를 처리하는 `catch` 블록이 없으면, 호출한 메서드로 예외를 전파함.
  - (2) 예외 처리기를 찾을 때까지의 모든 실행은 무시
  - (3) 예외 처리기를 찾지 못한다면 원래의 메서드를 호출한 프로그램으로 제어를 되돌아가고 디폴트 예외 처리기 실행
  - (4) 예외 처리기를 찾았다면 실행

```c#
// exception propagation 예시
using System;

namespace ExPropagationApp
{
    class Program
    {
        static void OccurError()
        {
            int a = 5;
            int b = 0;
            a = a / b;
            Console.WriteLine("End of OccurError Method");
        }
        static void CallOccurError()
        {
            Console.WriteLine("ERROR OCCURED!");
            OccurError();
            Console.WriteLine("End of CallOccurError");
        }
        static void Main(string[] args)
        {
            try
            {
                CallOccurError();
            } catch (DivideByZeroException e){
                Console.WriteLine("DivideByZeroException is procced");
                Console.WriteLine(e.Message);
            }
            Console.WriteLine("End of Main");
        }
    }
}

```

```
ERROR OCCURED!
DivideByZeroException is procced
Attempted to divide by zero.
End of Main
```

