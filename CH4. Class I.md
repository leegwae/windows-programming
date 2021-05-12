# CH4. 클래스 I

## 4.1 클래스와 객체

- **클래스(class)**
  - 객체를 정의하는 템플릿(template): **객체**의 구조(structure)와 행위(behavior)를 정의하는 방법이다.
  - OOP의 특성이다: 프로그램의 유연성(flexibility), 이식성(protability), 재사용성(reusability)를 높인다.
  - 자료 추상화(data abstraction)의 방법이다.
- C# 프로그래밍의 기본 단위는 클래스이다.



- **객체(object)**
  - 각 객체가 어떤 유형에 속하는지: 객체 자료형(object type) 또는 객체 클래스(object class)
  - 클래스의 **인스턴스(instance)**이다.



### 4.1.1 클래스의 선언

- 클래스는 **사용자 정의 자료형(user-defined data type)**으로 볼 수 있다.

- 객체를 선언하기 위해서는 해당하는 클래스를 먼저 정의해야 한다.

- 클래스의 정의

  ```c#
  [class-modifier] class 클래스이름 {
      // 클래스 멤버 선언
  }
  ```

  - `클래스이름`: 일반적으로 대문자로 시작



### 클래스 수정자의 종류

- **수정자**: 부가적인 속성을 명시하는 방법
- 클래스 수정자의 종류

| 수정자      | 설명                                                         |
| ----------- | ------------------------------------------------------------ |
| `public`    | 다른 프로그램에서도 사용할 수 있다.                          |
| `internal`  | 같은 assembly에서만 사용할 수 있다. 디폴트.                  |
| `static`    | 정적 클래스(static class). 모든 멤버가 정적 멤버가 된다. 정적 멤버는 객체 단위가 아니라 클래스 단위로 존재한다(클래스 선언만으로도 멤버가 존재한다). |
| `abstract`  | 5장                                                          |
| `sealed`    | 5장                                                          |
| `protected` |                                                              |
| `private`   |                                                              |
| `new`       | 중첩 클래스에서 사용되며, 베이스 클래스의 멤버를 숨긴다.     |
| `partial`   | 부분 클래스. 여러 파일에 나누어져 같은 이름의 클래스를 정의할 수 있다. |



### 4.1.2 객체의 생성

- 클래스 이름은 클래스 형(class type)이라고도 한다.

- 객체의 생성

  - (1) 클래스형의 변수 선언: 객체를 참조하는 객체 참조 변수 선언

  ```c#
  MyClass c1, c2;
  ```

  - (2) 객체 생성: `new` 연산자와 생성자(constructor)를 명시한다.
    - `new 생성자`

  ```c#
  c1 = new MyClass();
  ```

- 클래스의 **생성자**

  - 객체를 생성할 때, 객체의 초기화를 위해 자동으로 호출되는 루틴
  - 클래스 이름과 동일한 이름을 갖는 메서드

- **객체의 멤버 참조하기**: 점 연산자(dot operator) `.` 사용

  ```c#
  객체이름.멤버
  ```

  ```c#
  c1.name
  ```



## 4.2 필드

- **필드(field)**: 객체의 구조를 기술하는 자료 부분(클래스의 데이터)



### 4.2.1 필드의 선언

- 필드의 선언은 변수의 선언과 동일한 형태이다.

- 필드의 선언

  ```c#
  [field-modifier] 데이터타입 필드이름;
  ```

  - `field-modifier`: 필드 수정자. 디폴트는 `private`(클래스 안에서만 접근 가능)

- 필드 수정자: (1) 접근 수정자 와 (2) `new`, `static`, `readonly`, `volatile` 등이 있다.



### 4.2.2 접근 수정자

- **접근 수정자(access modifier)**: 다른 클래스에서 필드의 접근 허용 정도를 나타내는 속성
  - `private`: 정의된 클래스 내에서만 필드 접근이 허용된다. 디폴트.
  - `public`: 모든 클래스 및 네임 스페이스에서 자유롭게 접근이 허용된다.
  - `internal`: 동일한 어셈블리 내에서 자유롭게 접근할 수 있다.
    - 어셈블리: .NET의 실행 가능한 프로그램 또는 실행 프로그램의 일부. 실행 및 배포 단위(.EXE 또는 .DLL 파일)
  - `protected`: 파생 클래스와 정의된 클래스 내에서만 참조가 가능하다.
  - `protected internal`: 파생 클래스와 동일 네임스페이스(= 동일 프로그램)에서 자유롭게 접근할 수 있다.

| 접근 수정자                                    | 같은 클래스 | 파생 클래스 | 네임 스페이스 | 모든 클래스 |
| ---------------------------------------------- | ----------- | ----------- | ------------- | ----------- |
| `private`                                      |             | X           | X             | X           |
| `protected`                                    |             |             | X             | X           |
| `internal`                                     |             | X           |               | X           |
| `protected internal`<br />`internal protected` |             |             |               | X           |
| `public`                                       |             |             |               |             |



### 4.2.3 static 수정자와 정적 필드

- **정적 필드(static field)**: `static` 수정자를 사용하여 선언한 필드

  - 클래스 단위로 존재한다: 객체를 생성하지 않아도 존재하는 멤버이다.

- **정적 필드의 참조하기**: 객체를 통해서 참조할 수 없다.

  ```c#
  클래스이름.정적필드이름
  ```

  ```c#
  Animal.count
  ```

- **인스턴스 필드(instance field)**: `static` 없이 선언한 필드

  - 생성된 객체를 통하여 참조한다.



### 4.2.4 그 외 수정자

#### 4.2.4.1 new 수정자와 volatile 수정자

- `new`: 상속 계층에서, 상위 클래스의 멤버를 하위 클래스에서 새롭게 재정의하기 위해 사용하는 수정자
- `volatile`: 필드 최적화로 발생하는 side effect를 막는다. `readonly`와 함께 사용할 수 없다.



#### 4.2.4.2 readonly 수정자와 const 수정자

- 차이점: **값이 결정되는 시점이 다르다**.

| 수정자     | 값이 결정되는 시점      | 선언된 변수                    |
| ---------- | ----------------------- | ------------------------------ |
| `readonly` | 실행 중에 값이 결정     | **읽기전용 필드**              |
| `const`    | 컴파일 시간에 값이 결정 | **상수 멤버(constant member)** |

- 공통점: 모두 **값이 변할 수 없는** 속성을 가진다. 즉, 선언과 동시에 초기화가 이루어져야 하며, 선언 후 값을 변경하지 못한다.

- `readonly`의 사용: 상수 멤버로 선언할 수 없는 형이거나, 컴파일 시간에 값을 결정할 수 없을 때 사용한다.

- 상수 멤버의 선언

  ```c#
  [const-modifiers] const 데이터타입 상수멤버이름;
  ```

```c#
public const int PI = 3.141592;
public static readonly Color Black = new Color(0, 0, 0);
```

해보기??

```c#
private readonly int Sum = 3 + 5;
```



## 4.3 메서드

- **메서드(method)**: 객체의 행위를 기술하는 방법
  - 객체의 상태를 검색하고 변경하는 작업
  - 특정한 행동을 처리하는 프로그램 코드를 포함하는 함수의 형태



### 4.3.1 메서드의 선언

- 메서드의 정의

  ```c#
  [method-modifier] 반환형 메서드이름(파라미터1, ...) {
      // 메서드 몸체
  }
  ```

  - `반환형`: return type. 메서드가 아무것도 반환하지 않는다면 `void`형이 된다.
  - `메서드이름`: 일반적으로 대문자로 시작
  - `method-modifier`: 메서드 수정자.



### 메서드 수정자

- 메서드 수정자: (1) 접근 수정자 와 (2) `new`, `virtual`, `override`, `selaed`(5.1 파생 클래스 참고), `static`, `abstract`, `extern` 등이 있다.



### 메서드 접근 수정자

- 접근 수정자: 다른 클래스에서 메서드의 접근 허용 정도를 나타내는 속성
  - `public`, `protected`, `internal`, `private`
  - 필드의 접근 수정자와 의미가 같다.



### static 수정자와 정적 메서드

- **정적 메서드(static method)**: `static`으로 선언된 메서드

  - 전역 함수(global function)과 같은 역할을 한다.
  - 해당 클래스의 정적 필드 혹은 다른 정적 메서드만 참조 가능

- **정적 메서드 참조하기**

  ```c#
  클래스이름.메서드이름();
  ```

- **인스턴스 메서드**: 각 객체마다 가지는 메서드



### abstract 수정자와 extern 수정자

- 공통점: 메서드를 선언할 때, 메서드 몸체 대신 `;`으로 끝난다.
- 차이점

| 수정자     | 메서드가 정의되는 곳    |
| ---------- | ----------------------- |
| `abstract` | 하위 클래스에 정의된다. |
| `extern`   | 외부에 정의된다.        |



### 4.3.2 매개변수

- **매개변수**: 메서드 내에서만 참조될 수 있는 <u>지역 변수</u>
- 매개변수의 종류
  - (1) **형식 매개변수(formal parameter)**: 메서드를 정의할 때 사용하는 매개변수
  - (2) **실 매개변수(actual parameter)**: 메서드를 호출할 때 사용하는 매개변수
- 매개변수의 자료형: 기본형, 참조형 모두 사용할 수 있다.



#### 매개변수의 이름과 this 지정어

- 매개변수는 참조 범위가 메서드 내로 제한된다. 따라서 클래스 내의 필드와 이름이 일치해도 된다.
- `this`: 자기 자신의 객체를 가리킨다.
  - 필드의 이름과 매개변수의 이름을 구별해야 할 때 사용한다.

```c#
class Animal {
    string name;
    public Animal(string name) {
        this.name = name;
    }
} 
```



### 4.3.3 매개변수 전달

- **매개변수 전달(parameter trasmission)**: 메서드 호출 시 실 매개변수가 형식 매개변수로 전달되는 것
- 매개변수 전달 방식의 종류: (1) **값 호출(call by value)** (2) **참조 호출(call by reference)**



#### 4.3.3.1 값 호출

- 실 매개변수의 값이 형식 매개변수로 전달된다.

```c#
class CallByValueApp{
    static void Increment(int x) {
        x += 1;
        Console.WriteLine("x의 값은 {0}", x);
    }
    public static void Main(string[] args) {
        int x = 1;
        Increment(x);
        Console.WriteLine("x의 값은 {0}", x);
    }
}
```

```
x의 값은 2
x의 값은 1
```



#### 4.3.3.2 참조 호출

- 주소 호출(call by address)라고도 함: 실 매개변수의 주소가 형식 매개변수로 전달된다.
- 참조 호출 방법
  - (1) 매개변수 수정자 이용하기
  - (2) 객체 참조를 매개변수로 사용하기



#### 매개변수 수정자

| 수정자 | 설명                                                         |
| ------ | ------------------------------------------------------------ |
| `ref`  | 메서드로 매개변수가 전달될 때 반드시 초기화되어 있어야한다.<br />(그렇지 않으면 컴파일러 오류 발생) |
| `out`  | 메서드로 매개변수가 전달될 때 초기화되어 있지 않아도 된다.   |



#### 매개변수 수정자 ref

```c#
// ref example
namespace CallByReferenceApp
{
    class Program
    {
        static int Increment(ref int x)
        {
            x += 1;
            return x;
        }

        static void Main(string[] args)
        {
            int a = 3;
            Console.WriteLine(Increment(ref a));
        }
    }
}
```

- 메서드 정의

  ```c#
  [method-modifiers] 반환형 메서드이름(ref 데이터타입 변수명) {
      // 메서드 몸체
  }
  ```

  - `ref`를 사용해도 메서드는 어떠한 값을 반환할 수 있다.

- 메서드 호출

  ```c#
  데이터타입 변수이름 = 식;			// 변수 선언하고 초기화
  메서드이름(ref 변수이름);		// 메서드 호출
  ```

  - 인자를 전달할 때, 전달하는 `변수이름` 앞에  반드시 `ref`을 명시한다.

  - 이때, 전달하는 변수는 반드시 초기화되어 있어야 한다. 변수에 아무런 값이 할당되어있지 않다면 다음과 같은 오류가 발생한다.

    ```
    CS0165	Use of unassigned local variable 'a'
    ```



#### 매개변수 수정자 out

```c#
using System;

namespace CallByReferenceApp
{
    class Program
    {
        static void Sum(int x, int y, out int result)
        {
            result = x + y;
        }

        static void Main(string[] args)
        {
            int result;
            Sum(1, 2, out result);
            Console.WriteLine("1+2={0}", result);
        }
    }
}

```

```
1+2=3
```

- 메서드 정의

  ```c#
  [method-modifiers] 반환형 메서드이름(out 데이터타입 변수이름) {
      // 변수이름에 할당하는 문장을 포함한 메서드 몸체
  }
  ```

  - 인자로 받은 변수가 초기화되었는지의 여부와 상관없이, 반드시 메서드 몸체에서 값을 할당해주어야 한다.

    ```c#
    static void Sum(int x, int y, out int result){
        // empty
    }
    ```

    ```
    The out parameter 'result' must be assigned to before control leaves the current method
    ```

  - `out`를 사용해도 메서드는 어떠한 값을 반환할 수 있다.

- 메서드 호출

  ```c#
  데이터타입 변수이름;			// 변수 선언, [초기화]
  메서드이름(out 변수이름);	// 메서드 호출
  ```

  - 인자를 전달할 때, 반드시 전달하는 `변수이름` 앞에 `out`을 명시한다.



### 4.3.4 지정어 params와 매개변수 배열

- **매개변수 배열(parameter array)**: 실 매개변수의 개수가 가변적인 경우, 메서드를 정의할 때 형식 매개변수를 결정할 수 없을 때 사용한다.

- 메서드 정의: 지정어 `params`와 배열형을 명시한다.

  ```c#
  반환형 메서드이름(params 배열형[] 배열이름) {
      // 메서드 몸체
  }
  ```

  - `배열형`: 매개변수의 자료형이 모두 다른 경우, `object` 자료형을 사용한다.

- 메서드 호출

  ```c#
  메서드이름(파라미터1, 파라미터2,...);		// 파라미터 명시하기
  메서드이름(new 배열형[] {파라미터1, 파라미터2, ...})	// 배열 넘기기
  ```

  - 가변적인 파라미터를 넘기거나, 직접 `new` 연산자로 생성한 배열 객체를 넘길 수 있다.

```c#
using System;

namespace ParameterArrayApp
{
    class Program
    {
        static int Sum(params int[] nums)
        {
            int result = 0;

            foreach (int num in nums)
            {
                result += num;
            }

            return result;
        }

        static void PrintObjects(params object[] objects)
        {
            Console.WriteLine("The Start of PrintObjects Method");
            for (int i = 0; i < objects.Length; i++)
            {
                Console.WriteLine(i + "번째 파라미터: " + objects[i]);
            }
            Console.WriteLine("The End of PrintObjects Method");
        }

        static void Main(string[] args)
        {
            object[] objects = { "purple", 9, 'F', 3.14 };
            Console.WriteLine("Sum(10)={0}", Sum(10));
            Console.WriteLine("================================");
            PrintObjects(objects);
            Console.WriteLine("================================");
            PrintObjects();
            Console.WriteLine("================================");
            PrintObjects("purple", 9, 'F', 3.14);
        }
    }
}

```

```
Sum(10)=10
================================
The Start of PrintObjects Method
0번째 파라미터: purple
1번째 파라미터: 9
2번째 파라미터: F
3번째 파라미터: 3.14
The End of PrintObjects Method
================================
The Start of PrintObjects Method
The End of PrintObjects Method
================================
The Start of PrintObjects Method
0번째 파라미터: purple
1번째 파라미터: 9
2번째 파라미터: F
3번째 파라미터: 3.14
The End of PrintObjects Method
```



### 4.3.5 Main() 메서드

- `Main` 메서드는 C# 응용 프로그램의 시작점이다.

- `Main` 메서드의 형태

  ```c#
  public static void Main(string[] args){
      
  }
  ```

  - `args`: 명령어 라인으로부터 스트링을 전달받을 수 있다.

- 명령어 라인으로 `Main` 메서드에 매개변수 전달하기

  ```
  c:>실행파일명 인수1, 인수2, ..., 인수n
  ```

해보기??



### 명명된 매개변수(named parameter)

- 인수를 매개변수에 할당하는 기준은 인수가 전달된 "순서"이다.
- **명명된 매개변수(named parameter)**: 메서드를 호출할 때 매개변수의 이름을 명시하여 데이터를 바인드하는 기능

```c#
using System;

namespace NamedParameterApp
{
    class Program
    {
        static void PrintInformation(string name, int age)
        {
            Console.WriteLine("name: {0} age: {1}", name, age);
        }
        static void Main(string[] args)
        {
            PrintInformation(age: 25, name: "Lana");
        }
    }
}

```

- 메서드 호출

  ```c#
  메서드이름(매개변수: 인수);
  ```

  - 할당하고자 하는 `인수`와 `매개변수` 사이에 `:`를 명시한다.

  - 메서드 선언에 정의된 순서대로 매개변수들을 명시하지 않아도 된다.

  - 명명된 매개변수를 사용한다면 메서드의 모든 매개변수에 대하여 명명된 매개변수를 사용해야한다.

    ```c#
    PrintInformation(age: 25, "Lana");
    ```

    ```
    Named argument 'age' is used out-of-position but is followed by an unnamed argument
    ```



### 선택적 매개변수(optional parameter)

- **선택적 매개변수(optional arguments)**: 메서드 선언시 매개변수에 디폴트 값을 할당한다. 해당 매개변수에 값을 전달하지 않는다면 디폴트 값이 할당된다.

```c#
using System;

namespace OptionalArgumentsApp
{
    class Program
    {
        static void PrintInformation(string name, int age=0)
        {
            Console.WriteLine("name: {0} age: {1}", name, age);
        }
        static void Main(string[] args)
        {
            PrintInformation("Lana");
            PrintInformation("Cony", 3);
        }
    }
}

```

```
name: Lana age: 0
name: Cony age: 3
```

- 메서드 정의

  ```c#
  [method-modifiers] 반환형 메서드이름(requiredParameter1, ..., requiredparemeterN, optionalArguments) {
      
  }
  ```

  - `requiredParameter`: 선택적 매개변수가 아닌 변수

  - required parameter와 optional parameter의 순서: 반드시 required parameter - optional parameter 순이다.

    - 전달된 인수가 순서대로 매개변수에 할당되기 때문이다.
    - optional parameter가 required parameter보다 앞에 오면 다음과 같은 에러가 발생한다.

    ```c#
    static void PrintInformation(int age=0, string name)
    ```

    ```
    CS1737	Optional parameters must appear after all required parameters
    ```

- 메서드 호출

  - (1) 선택적 매개변수에 넘기는 값 생략하기: 메서드를 정의할 때 지정한 디폴트 값이 선택적 매개변수에 할당된다.

    ```c#
    PrintInformation("Lana");
    // age <- 0
    ```

  - (2) 선택적 매개변수에 넘기는 값 생략하지 않기 : 전달한 인수의 값이 선택적 매개변수에 할당된다.

    ```c#
    PrintInformation("Cony", 3);
    // age <- 3
    ```



### 4.3.6 메서드 오버로딩

- **시그네처(signature)**: 메서드를 구별하는데 사용하는 정보
  - (1) 메서드의 이름
  - (2) 매개변수의 개수
  - (3) 매개변수의 자료형
  
- 시그네처의 예시

  ```c#
  반환형 메서드이름(데이터타입1 파라미터1, ..., 데이터타입N 파라미터N){ /* 메서드 몸체 */}	// 메서드 정의
  ```

  ```
  시그네처: 반환형_메서드이름_데이터타입1_데이터타입2..._데이터타입N
  ```

  ```c#
  void Foo() {}				// void_Foo
  void Foo(string s) {}		// void_Foo_string
  void Foo(int x, int y) {}	// void_Foo_int_int
  ```

- **메서드 중복(method overloading)**: 메서드의 이름만 같고 매개변수의 개수와 자료형이 다른 경우이다.
  - 메서드가 중복되는 경우, <u>메서드를 호출할 때 컴파일러에 의하여 메서드가 구별</u>된다.



#### 선택적 매개변수와 메서드 오버로딩

```c#
void MyMethod(string s1 = "", string s2 = "") {
    Console.WriteLine("선택적 매개변수");
}

void MyMethod() {
    Console.WriteLine("메서드 오버로딩");
}
```

`MyMethod()`를 호출한 결과는 무엇인가?

- `메서드 오버로딩`이 출력된다: 메서드 오버로딩은 선택적 매개변수에 앞선다.



### 4.3.7 생성자

- **생성자**: 객체가 생성될 때 자동으로 호출되는 메서드
  - 생성자의 이름: 클래스의 이름과 동일하다.
  - 반환형을 갖지 않는다.
  - `new` 연산자로 함께 객체를 초기화할 때 주로 사용된다. (C# 에서 모든 객체는 동적 기억장소인 Heap에 저장된다.)
  - 메서드 오버로딩이 가능하다.
- 디폴트 생성자: 매개변수를 가지지 않는 생성자

```c#
class Album {
    string name;
    string writer;
    
    public Album() {			// 디폴트 생성자
        name = "ultraviolence";
        writer = "Lana";
    }
    
    public Album(string name){	// 생성자1
        this.name = name;
        writer = "Lana";
    }
    
    public Album(string name, string writer){	// 생성자2
        this.name = name;
        this.writer = writer;
    }
    
    override public string ToString(){
        return "name: "+name+"writer: " + writer;
    }
}

class Program {
    public static void Main(string[] args){
        Album a1, a2;
        
        a1 = new Album();				// 디폴트 생성자 호출
        a2 = new Album("paradise");		// 생성자1 호출
        
        Console.WriteLine(a1.ToString());
        Console.WriteLine(a2.ToString());
    }
}
```

```
name: ultraviolence  writer: Lana
name: paradise  writer: Lana
```



#### 정적 생성자(static constructor)

- **정적 생성자(static constructor)**: `static`으로 선언된 생성자
  - 클래스 로딩 시간에 실행되어 클래스의 정적 필드를 초기화한다.
    - 매개변수와 접근 수정자를 가질 수 없다.
    - 명시적으로 호출할 수 없다.
    - `Main` 메서드보다 먼저 실행된다: 클래스가 정의되는 순간 호출된다.



#### 정적 필드를 초기화하는 방법

- (1) 정적 필드를 선언과 동시에 초기화하기
- (2) 정적 생성자 이용하기



**(1) 정적 필드를 선언과 동시에 초기화하기**

```c#
static int staticWithInitializer = 100;
```

**(2) 정적 생성자 이용하기**

```c#
class StaticConstructor()
{
    static int staticWithNoInit;
    
    static StaticConstructor()
    {
        staticWithNoInit = 100;
    }
}
```



```c#
using System;

namespace StaticConstructorApp
{
    class StaticConstructor
    {
        static int staticWithInitializer = 100;
        static int staticWithNoInitializer;

        static StaticConstructor()
        {
            staticWithNoInitializer = staticWithInitializer + 100;
        }

        public static void PrintStaticVariable()
        {
            Console.WriteLine("(1) 정적 필드 선언과 동시에 초기화하기 = {0}\n(2) 정적 생성자 이용하기 = {1}",
                staticWithInitializer, staticWithNoInitializer);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            StaticConstructor.PrintStaticVariable(); 
        }
    }
}

```



### 4.3.8 소멸자

- **소멸자(destructor)**: 클래스의 객체가 소멸될 때 필요한 행위를 기술한 메서드

  - 컴파일러에 의해 자동으로 호출된다. 따라서 매개변수를 가질 수 없다.

- 소멸자의 이름: 생성자의 이름 앞에 `~` (tilde)를 붙인다.

  ```c#
  ~생성자이름(){
      \\ 메서드 몸체
  }
  ```



#### 4.3.8.1 Finalize() 메서드

- 컴파일러는 내부적으로 소멸자를 `Finalize()` 메서드로 변환하여 컴파일한다.
- `Finalize()` 메서드의 정의: 프로그래머는 반드시 소멸자를 통해서만 `Finalize()` 메서드를 재정의할 수 있다.
- `Finalize()` 메서드의 호출 시점: 객체가 더 이상 참조되지 않을때 가비지 콜렉터가 호출한다.
  - `Main()` 메서드 내부에서 호출되었을 경우, `Main()` 메서드의 실행이 종료되고 호출된다.
  - GC(Garbage Collector): 객체는 힙에 동적으로 생성된다. 어떤 것도 이 객체를 참조하지 않을때, GC는 가비지 객체를 회수한다.

```c#
// 프로그래머: destructor의 정의
class Foo {
    public Foo(){
        Console.WriteLine("In the Constructor...");
    }
    
    ~Foo(){
        Console.WriteLine("In the Destructor..."");
        
    }
}
```

```c#
// 컴파일러: finalize 메서드의 정의
override protected void Finalize() {
    try {
        Console.WriteLine("In the desctructor...");
    } finally {
        base.Finalize();
    }
}
```



#### 4.3.8.2 Dispose() 메서드

- `Dispose()` 메서드: CLR(Common Language Reuntime)에서 관리되지 않은 리소스(resource; 자원)를 직접 해제할 때 사용한다.
- `Dispose()` 메서드의 호출 시점: 자원이 스코프를 벗어나면 즉시 시스템이 호출한다.

```c#
using System;

namespace DisposeApp
{
    class DisposeClass : IDisposable
    {
        public void Dispose()
        {
            Console.WriteLine("In the Dispose method...");
            GC.SuppressFinalize(this);
        }
    }
    class Program
    {

        static void Main(string[] args)
        {
            Console.WriteLine("Start of Main...");
            
            using (DisposeClass obj = new DisposeClass())
            {
                // obj(자원)의 사용
            }
            Console.WriteLine("End of Main...");
        }
    }
}

```

```
Start of Main...
In the Dispose method...
End of Main...
```



#### using문과 자원의 해제

```c#
using (DisposeClass obj = new DisposeClass())
{
	// obj(자원)의 사용
}
```

- `using`문: 자원을 획득하여 사용한 후, 즉시 해제하고자 할 때 사용한다.
  - 자원: 클래스나 구조체
  - 자원은 `System.IDisposable` 인터페이스를 구현한 `Dispose()` 메서드를 반드시 가지고 있어야 한다.



## 4.4 프로퍼티

- **프로퍼티(property)**: 정보 은닉을 위한, 클래스의 `private` 필드를 형식적으로 다루는 일종의 메서드

  - **셋-접근자(set-accessor)**: 값을 지정
  - **겟 접근자(get-accessor)**: 값을 참조

- 프로퍼티의 형태

  ```c#
  [property-modifiers] 반환형 프로퍼티이름 {
      get {
          // get-accessor
      }
      set {
          // set-accessor
      }
  }
  ```

  - `반환형`: 프로퍼티나 인덱서는 `void` 타입으로 선언할 수 없다.
  - `프로퍼티이름`: 일반적으로 대문자로 시작한다.
  - 프로퍼티에 대응하는 `private` 필드: 일반적으로 소문자로 시작한다.
  - `property-modifiers`: (1) 접근 수정자 와 (2) `new`, `static`, `virtual`, `sealed`, `override`, `abstract`, `extern`
    - 메서드 수정자와 의미가 같다.
  - `get` 블럭: 반드시 `return`문을 포함한다.

- 프로퍼티의 호출: 필드처럼 사용되지만 메서드처럼 동작한다.

  - 배정문의 왼쪽: 셋-접근자 호출
  - 배정문의 오른쪽: 겟-접근자 호출

  ```c#
  using System;
  
  namespace PropertyApp
  {
      class Album
      {
          private string title;
          public string Title
          {
              get { return title; }
              set { title = value; }
          }
      }
      class Program
      {
          static void Main(string[] args)
          {
              Album album = new Album();
              string title;
  
              album.Title = "Video Game";		// set-accessor 호출
              title = album.Title;			// get-accessor 호출
              Console.WriteLine("Title: {0}", title);
          }
      }
  }
  
  ```

  ```
  Title: Video Game
  ```

  - 셋 접근자 내부의 `value` 지정어: 매개변수와 같은 역할을 하여, 배정문의 우변이 연산된 결과를 전달받는다.



### 프로퍼티와 readonly 필드

- 셋-접근자와 겟-접근자는 각각 홀로 정의될 수 있다.

- 겟-접근자만을 정의한 프로퍼티: `readonly` 수정자와 선언된 필드와 같은 의미를 가질 수 있다.

  ```c#
  class RWOnlyPropety {
      private int readonlyField = 100;
      public int ReadonlyField{
          get { return readonlyField; }
      }
      
      private int writeonlyField = 300;
      public int WriteOnlyField {
          set { writeonlyField = value; }
      }
  }
  ```

  

### 자동 구현 프로퍼티: 관련된 private 필드가 없는 프로퍼티

- `private` 필드 없이 `public` 프로퍼티를 선언하면, 컴파일러가 `private` 필드를 만든다.
  - 컴파일러가 만든 필드에는 접근할 수 없다.

```c#
using System;

namespace PropertyWithoutFieldApp
{
    class Foo
    {
        public string PropertyWithoutField
        {
            get { return "This is get-accessor of property without field"; }
            set { Console.WriteLine(value); }
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Foo foo = new Foo();

            Console.WriteLine(foo.PropertyWithoutField);
            foo.PropertyWithoutField = "This is set-accessor of property without field";
        }
    }
}

```

```
This is get-accessor of property without field
This is set-accessor of property without field
```



### 프로퍼티를 메서드로 바꿔보기

- 프로퍼티의 형태

  ```c#
  반환형 프로퍼티이름 {
      get {/* get-accessor */}
      set {/* set-accessor */}
  }
  ```

- 메서드의 형태

  ```c#
  반환형 Get프로퍼티이름() {/* get-accessor */}
  void Set프로퍼티이름(데이터타입 파라미터) {/* set-accessor */}
  ```



### 배열 프로퍼티

```c#
using System;

namespace ArrayPropertyApp
{
    class ArrayProperty
    {
        private int[] nums = new int[10];
        public int[] Nums
        {
            get { return nums; }
            set { Nums = value; }
        }
    }
    class Program
    {

        static void Main(string[] args)
        {
            ArrayProperty arrayProperty = new ArrayProperty();
            
            for (int i = 0; i < arrayProperty.Nums.Length; i++)
            {
                arrayProperty.Nums[i] = i;
                Console.Write(i + " ");
            }
        }
    }
}

```



## 4.5 인덱서

- **인덱서(Indexer)**: 배열 연산자 `[]`로 객체를 다루는 프로퍼티

- 인덱서의 형태

  ```c#
  [indexer-modifiers] 반환형 this[파라미터리스트]{
      set { /* indexer body */}
      get { /* indexer body */}
  }
  ```

  - `this`: 프로퍼티이름 대신 사용
  - `[]`: 안에 파라미터를 명시한다.
  - `indexer-modifiers`: (1) 접근 수정자 (2) 그 외 수정자: `new`, `virtual`, `override`, `abstract`, `sealed`, `extern`
    - 메서드 수정자와 의미가 같다.

```c#
using System;

namespace IndexerApp
{
    class Album
    {
        private string[] title = new string[4];
        public string this[int index]
        {
            get { return title[index]; }
            set { title[index] = value; }
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Album album = new Album();

            for (int i = 0; i < 4; i++)
            {
                album[i] = "Video Game " + i;
                Console.WriteLine("{0}번째: {1}", i, album[i]);
            }
        }
    }
}

```

```
0번째: Video Game 0
1번째: Video Game 1
2번째: Video Game 2
3번째: Video Game 3
```



### 인덱서로 연관 배열(associative array) 구현하기

- 자료 구조의 연관 배열: 키와 값이 일대일 대응한다. 딕셔너리로 불리기도 하다.

- **인덱서를 다차원 배열 형식으로 정의하기**

  ```c#
  using System;
  
  namespace StringIndexerApp
  {
      class StringIndexer
      {
          public char this[string str, int index]
          {
              get { return str[index]; }
          }
          
          public string this[string str, int index, int len]
          {
              get { return str.Substring(index, len);  }
          }
      }
      class Program
      {
          static void Main(string[] args)
          {
              string str = "world!";
              StringIndexer stringIndexer = new StringIndexer();
  
              for (int i = 0; i < str.Length; i++)
              {
                  Console.Write(str[i]);
              }
              Console.WriteLine();
  
              Console.WriteLine("substring: " + stringIndexer[str, 1, 3]);
          }
      }
  }
  
  ```

  ```
  world!
  substring: orl
  ```

