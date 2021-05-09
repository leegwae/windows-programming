# CH2. 언어 구조

## 2.1 어휘구조

- **어휘**: 프로그램을 구성하는, 문법적으로 의미있는 최소 단위. **토큰(token)**이라고 함.
- 토큰의 종류
  - 특수 형태: 지정어(keyword), 연산자(operator). 구분자(delimiter) - `,` `:` `()` 등
  - 일반형태: 명칭(identifier), 리터럴(literal) - 상수, `2`, `"string"`



### 2.1.1 지정어

- **지정어(keyword)**: 프로그래밍 언어 설계 시 그 기능과 용도가 이미 정의되어 있는 단어
- 식별자로 사용할 수 없다.
- 문맥 지정어(contextual keyword): 지정어는 아니지만 문맥에 따라 지정어처럼 사용됨.
  - 예) `await`, `value`, `get`, `add` 등



### 2.1.2 명칭

- **명칭(identifier)**: 변수, 상수, 배열, 클래스, 메서드, 레이블을 식별하기 위하여 붙이는 이름

- 명칭의 형태: 문자로 시작하며 대소문자를 구분한다.

- 지정어를 명칭으로 사용하기: 지정어 앞에 `@`를 붙인다.

  ```c#
  int @int = 1;
  ```

  - 이때, `@`를 지정어가 아닌 일반 명칭에 붙여 사용할 경우 그렇지 않은 경우와 구분하지 않는다. 즉, `i`와 `@i`는 같은 명칭이므로 중복하여 선언될 수 없다.

- 변수: 소문자, 메서드/클래스: 대문자, 인터페이스: `I`로 시작함

- 자바 문자 집합(charcter set): 유니코드를 사용하고 16비트로 한 문자 표현. 한글을 포함하여 $\pi$같은 것도 식별자로 가능하다.



### 2.1.3 리터럴

- **리터럴(literal)**: 자신의 표기법이 곧 자신의 값이 되는 상수



(1) 정수형 상수

- 정수형 상수의 종류: 10진수, 16진수(접두어 `0x` 혹은 `0X`), (8진수는 지원하지 않음)
- 정수형 상수의 비트 수: 기본적으로는 32비트. `long`형은 64비트이며, 접미어로 `l`  또는 `L`(권장)을 붙인다.



(2) 실수형 상수: 기본적으로 `double`형

- 실수형 상수의 분류
  - 지수 부분의 유무에 따라
    - 고정소수점(fixed-point): `1.44`
    - 부동소수점(floating-point): `0.1414E1`, `0.1414e01`
  - 용도에 따라: 과학 연산(`float`: 32비트, `double`: 64비트), 회계 연산(`decimal`: 128비트)
  - 정밀도에 따라: `float`(`-f`, `-F`), `double`(`-d`, `-D`), `decimal`(`-m`, `-M`)



(3) 부울형 상수: `false`와 `true` 값만 가지며, 다른 자료형(예: 0, 1)으로 상호 변환되지 않는다.

(4) 문자 상수: `''`로 감싸 표현한다. 

- escape sequence: 특수한 문자를 표현한다. 이미지??

(5) 스트링 상수: `""`로 감싸 표현된 스트링이다. `System.String` 클래스의 객체로 취급한다.

- 축어적 스트링 상수(verbatim string literal): 스트링 상수 내에 이스케이프 문자열을 포함한 것

  - 리터럴 앞에 `@`을 붙이면 이스케이프 문자열이 그대로 스트링 값이 된다.

  ```c#
  string a = "hello\tworld";		// hello	world
  string b = "hello\\tworld";		// hello\tworld
  string c = @"hello\\tworld";	// hello\tworld
  ```

(6) 객체 참조 리터럴(object referene): `null`이 여기에 속한다. 아무 객체도 가리키지 않거나, 부적절하거나 객체를 생성할 수 없는 경우, 혹은 초기화에 사용한다.



### 2.1.4 주석

- 주석의 종류
  - `// comment`: `//`부터 새로운 줄 전까지 주석으로 간주
  - `/* comment */`: `/*`와 `/*` 사이의 모든 문자를 주석으로 간주. 주석 안에 다른 주석이 포함될 수 없다.
  - `/// comment`: ``///` 다음의 문자들을 주석으로 간주
    - C# 프로그램에 대한 웹 보고서를 작성할 때 사용
    - XML 요소를 이용하여 기술
    - 컴파일 시 /doc 옵션을 사용하여 XML 문서 생성



## 2.2 자료형

- 값형: 숫자형, 문자형, 부울형, 열거형, 구조체형
- 참조형: 클래스형, 인터페이스, 배열형, 델리게이트형

Stack - Heap - Data- Text

```c#
using System;
int Glob;		// 전역 변수: Data 영역에 만들어진다.
class Cat {}
class MyClass {
    public static void Main() {
        int I;	// 지역 변수: Stack에 만들어진다.
        int[] Arr;	// 포인터: 객체를 가리킨다. Stack에 만들어진다.
        Cat c;
        
        Arr = new int[3];	/// 객체: heap에 동적으로 만들어진다.
        c = new Cat();
        // Time T1
        A(); 
        // <= return Address from A()
    }
    public static void A() {
        int J;
        // Time T2
    }
}
```

T1: I, Arr, c - Object Cat, Object Array - Glob - (empty)

T2: I, Arr, c, return Addr from A(), J - Object Cat, Object Array - Glob - (empty)

Arr, c는 각각 Heap 영역의 Object Array와 Object Cat을 가리킨다.

이미지??



- 포인터가 사라지면 객체 역시 사라져야한다. 가비지 콜렉터가 아무도 가리키지 않는 객체를 회수한다.

- C#의 자료형은 CTS에서 정의한 형식으로 표현할 수 있다.

  ```c#
  System.Int32.x;		// CTS 형으로 정수형 변수 x의 선언
  int x;				// C# 형으로 정수형 변수 x의 선언
  ```



### 2.2.1 값형

(1) 정수형

- 정수형의 종류
  - 부호 있는(signed) 정수형: `sbyte`(8비트), `short`(16비트), `int`(32비트), `long`(64비트)
  - 부호 없는(unsigned) 정수형: `byte`(8비트), `ushort`(16비트), `uint`(32비트), `ulong`(64비트)

이미지??



(2) 실수형: 실수의 표현 방법과 실수 연산은 IEEE 754 표준을 따른다.

- 실수형의 종류
  - 부동소수점(floating-point)
    - 정밀도에 따라 `float`(32비트), `double`(64비트)로 나눈다.
  - 10진 자료형(decimal): 고도의 정밀도를 요하는 계산에 이용
    - 28의 유효자릿수
    - 구조체로 처리하여 효율성이 떨어짐.



(3) 문자형: 16비트 유니코드를 사용

(4) 부울형: `true` 혹은 `false` 값을 가진다. 다른 자료형으로 변환할 수 없다.

(5) 초기값: 선언된 변수가 컴파일러에 의해 묵시적으로 갖게 되는 초기값(intial value). 

- 메서드 안에서 선언된 지역 변수는 묵시적인 초기값을 갖지 못한다.

이미지??



### 2.2.2 열거형

- **열거형**: 서로 관련 있는 상수들의 모음을 심볼릭한 명칭의 집합으로 정의한 것.

- 기호 상수(symbolic constant): 이 집합들의 원소로 기술한 명칭

  - 집합에 열거된 순서에 따라 0부터 순서값을 가진다.

  - 정수형으로 교환하여 사용할 수 있다.

  - 순서값을 임의로 지정할 수도 있다.

    ```c#
    enum Color { Purple, Blue = 10, Yellow }
    // Purple = 1, Blue = 10, Yellow = 11
    ```

    

```c#
namespace EnumTypeApp {
    enum Color { Purple, Blue, Yellow }
    class Program {
        static void Main(string[] args){
            color color = Color.Blue;
            color++;
            
            int num = (int) color;
            Console.WriteLine("Cardinality of" + color + "=" + num);
        }
    }
}
```



### 2.2.3 참조형

- **참조형(reference type)**: 객체를 가리키는 형



### 2.2.4 배열형

- 배열형: 같은 자료형의 값을 여러 개 저장할 때 사용하는 자료형. 순서가 있는 원소들의 모임이다.

- 배열의 사용

  - (1) 배열 선언: 배열 객체 참조 변수(포인터) 선언

    ```c#
    int[,] matrix;		// 2차원 배열 변수 선언
    ```

  - (2) 배열 객체 생성: `new` 연산자로 객체를 동적으로 생성하고, 참조 변수가 생성된 객체를 가리키도록 할당

    ```c#
    matrix = new int[10,100];
    ```

  - (3) 배열에 값 저장: `Length` 프로퍼티로 배열의 길이 접근. 인덱스 범위를 초과하면 `IndexOutOfRangeException` 발생

- 배열의 배열: 배열 객체를 가리키는 포인터 배열

  - 배열의 원소가 배열이다. 
  - 다차원 배열과 구분된다.
  - <u>각 원소에 해당하는 배열이 서로 다른 크기를 가질 수 있다.</u>

- 배열의 선언 동시에 초기화하기

  ```c#
  int[] arr1 = new int[] { 1, 2, 3 };
  int[] arr2 = { 1, 2, 3 };
  // 두 배정문은 같은 의미를 가진다.
  ```

- 배열의 배열 초기화하기

  ```c#
  int[][] arr = new int[3][];
  int i, j;
  
  for (i = 0; i < arr.Length; i++){
      arr[i] = new int[i+1];
  }
  
  for (i = 0; i < arr.Length; i++) {
      for (j = 0; j < arr[i].Length; j++) {
          arr[i][j] = i;
      }
  }
  ```

  

### 2.2.5 스트링형

- **스트링형**: 문자열을 표현하기 위해 사용하는 자료형
- 내부적으로 CTS 자료형인 `System.String`과 동일하다.
- 스트링 객체는 그 내용을 변경할 수 없어, 스트링 연산 결과로 항상 새로운 객체를 만드므로 연산이 잦을 경우 부하가 많이 걸리는 문제점이 있다.
- `StringBuilder` 클래스: 객체에 저장된 내용을 임의로 변경할 수 있다. 스트링을 수정하는 다양한 메서드를 제공한다.

```c#
using System;
using System.Text;
class StringApp{
    public static void Main() {
        StringApp obj = new StringApp();
        StringBuilder sb = new StringBuilder();
        string str = "Class name is ";
        
        Console.WriteLine(str + obj.ToString());
        sb.append(str);
        sb.append(obj.ToString());
        Console.WriteLine(sb);
    }
}
```

- `System.Object.ToString()`: 객체를 스트링으로 변환하여 반환한다.
- `Append(스트링)`: `스트링`을 후미에 연결한다. 



## 2.3 연산자

- **식(expression)**: 문장에서 값을 계산하는데 사용된다.
  - 연산자(operator)와 피연산자(operand)로 구성
  - 연산자의 종류(식의 값)에 따라 산술식, 관계식, 논리식으로 구분
- **연산자(operator)**: 피연산자가 어떻게 계산될지를 나타내는 기호이다.
  - 식의 의미를 결정
  - C#에는 48개의 연산자 정의됨

이미지??



### 2.3.1 산술 연산자

- **산술 연산자(arithmetic operator)**: 수치 연산을 나타내는 연산자.
  - 단항 산술 연산자: `+`, `-`
  - 이항 산술 연산자: `+`, `-`, `*`, `/`, `%` (맨 뒤의 두 개는 효율성이 굉장히 떨어진다)
- 산술식: 산술 연산자를 포함하는 식. 식의 계산 결과는 수치값이다.
- 나머지 연산자(remainder operator): `x % y = x - (x/y) * y`
- **묵시적 형변환(implicit conversation)**: 연산의 결과는 피연산자의 타입에 따라 달라질 수 있다. 산술 연산자에서는 피연산자의 타입이 더 큰 쪽으로 다른 피연산자의 타입이 형변환한다.
  - `정수형/정수형` => `정수형`
  - `정수형/double형` => `double형`



### 2.3.2 관계 연산자

- **관계 연산자(relational operator)**: 두 개의 값을 비교하는 이항 연산자
- 관계식: 관계 연산자를 포함하는 식. 식의 결과는 `true` 혹은 `false`이다.
- 연산자 우선순위: (1) 관계 연산자는 산술 연산자보다 우선순위가 낮다. (2) 비교 연산자(`>`, `<`, `<=`, `>=`)는 항등 연산자(`==`, `!=`)보다 우선순위가 높다.



### 2.3.3 논리 연산자

- **논리 연산자(logical operator)**: 두 피연산자의 논리 관계를 나타내는 연산자
- `&&`(논리곱), `||`(논리합), `!`(논리부정)
- 연산자 우선순위: (1) 논리 연산자는 산술 연산자와 관계 연산자보다 우선순위가 낮다. (2) 논리부정 - 논리곱- 논리합 순으로 우선순위가 높다.



### 2.3.4 증가 연산자와 감소 연산자

- **증가 연산자(increment operator)**와 **감소 연산자(decrement operator)**: 정수형 변수의 값을 증가시키거나 감소시키는 연산자
  - 전위 연산자(prefix operator): 연산하기 전에 변수의 값을 증감시킨다.
  - 후위 연산자(postfix operator): 연산한 후에 변수의 값을 증감시킨다.
  - 문장에서 변수와 증감 연산자만 사용한다면 전위와 후위 연산자의 의미가 같다. 즉, `i++;`과 `++i`의 의미는 같다.
- <u>변수가 아닌 식이나 정수형 변수가 아니라면 사용할 수 없다</u>.



### 2.3.5 비트 연산자

- **비트 연산자(bitwise operator)**:  비트 단위로 연산을 처리한다. <u>피연산자는 반드시 정수형이다</u>.
- `&`(비트논리곱), `|`(비트논리합), `^`(XOR), `<<`(왼쪽 시프트 이동), `>>`(오른족 시프트 이동), `~`(1의 보수)
- 연산자 우선순위: 1의 보수 - 시프트 연산 - 논리곱- XOR - 논리합 순으로 우선순위가 높다.
- 비트 이동 연산자
  - 왼쪽 이동: 빈곳은 0으로 채운다. `x << n`은 `x * 2^n`과 같은 의미이다.
  - 오른쪽 이동: 빈곳은 부호 비트(양수는 0, 음수는 1)로 채운다. `x >> n`은 `x / 2^n`과 같은 의미이다.



### 2.3.6 조건 연산자

- **조건 연산자(conditional operator)**: `if`문과 같은 의미를 가지는 삼항 연산자
- `식1 ? 식2 : 식3`: `식1`이 참이면, `식2`를 반환하고 아니라면 `식3`를 반환한다.



### 2.3.7 복합 배정 연산자

- **복합 배정 연산자(compound assignment operator)**: 이항 연산자와 배정 연산자가 결합된 연산자
- 산술 연산자, 비트 연산자와 결합한다.
- 다음과 같이, 복합 배정문에서는 왼쪽의 식(값)을 하나의 피연산자로 간주한다. 즉,

```c#
a = a * b + 1;		// (1)
a = a * (b + 1);	// (2)
a *= b + 1;			// (2)의 의미를 가진다.
```



### 2.3.8 캐스트 연산자

- **캐스트 연산자(cast operator)**: 자료형을 변환하는 연산자
- `(자료형) 식`

```c#
(float) 9 === 9.0;
(float) (1 / 2) === 0.0;
(float) 1 / 2 === 0.5; // ((float) 1) / 2
```



### 2.3.9 형 검사 연산자

- **형 검사 연산자(type testing operator)**
  - `is` 연산자: 데이터 타입이 지정한 타입과 호환 가능한지 검사. 부울형을 반환한다.
  - `as` 연산자: 데이터 타입을 지정한 타입으로 변환. <u>변환 가능하면 변환된 타입의 포인터를, 그렇지 않다면 `null`을 반환한다.</u>

``` c#
static void AsOpertor(object obj) {
    Console.WriteLine(obj + " as string == null: " + (obj as string == null));
}
public static void Main(string[] args) {
    AsOperator(10);		// 10 as string == null : true
    AsOperator("ABC");	// ABC as string == null : false
}
```

- 정수형 상수인 10인 `string`으로 변환할 수 없으므로, `10 as string`의 값으로 `null`이 반환된다. 반대로, `"ABC"`는 스트링 상수이므로 `ABC as string`의 값으로 `null`이 반환되지 않는다.

?? 확인하기



### 2.3.10 지정어 연산자

- **지정어 연산자(keyword operator)**: 연산의 의미를 C# 지정어로 나타낸 연산자
- 지정어 연산자의 종류
  - `new`: 객체를 생성하고 반환하는 연산자
  - `typeof`: 객체의 타입을 반환하는 연산자
  - `checked`: 정수식의 오버플로를 검사하는 연산자
  - `unchecked`: 정수식의 오버플로를 무시하는 연산자

```c#
class ALBUM {
	private string name;
}

class Program {
    static void Main(string[] args) {
        Type t = typeof(ALBUM);
        string className = t.ToString();
    }
}
```



### 2.3.11 연산자 우선순위

- 결합 법칙: 같은 순위를 갖는 연산자가 여러 개 나타낼 때, 왼쪽에서 오른쪽으로 계산해간다면 좌측 결합(left associativity)이고 오른쪽에서 왼쪽으로 계산해간다면 우측 결합(right associativity)이다.
- 우측 결합 연산자: 단항 연산자, 전위 증감 연산자, `(자료형)`, 복합 배정 연산자

이미지??



## 2.4 형 변환

- **형 변환(type conversion)**: 피연산자의 자료형이 서로 다를 때 일정한 규칙에 의해 피연산자의 자료형을 일치시키는 것. 묵시적 형 변환과 명시적 형 변환으로 구분된다.



### 2.4.1 묵시적 형 변환

- **묵시적 형 변환(implicit type conversion)**: <u>컴파일러에 의해 자동적으로 수행</u>되는 형 변환
  - 작은 크기의 자료형이 큰 크기의 자료형으로 변환된다. 예) `a = 5 / 10.0`



### 2.4.2 명시적 형 변환

- **명시적 형 변환(explicit type conversion)**: 캐스트 연산자를 사용하여 수행되는 형 변환
  - `(자료형) 식`
  - 큰 크기의 자료형에서 작은 크기의 자료형으로 변환할 때 정밀도 상실이 일어난다. <u>실행 시간 예외는 발생하지 않는다.</u>

```c#
int big = 1234567890;
float approx;
approx = (float)big;
Consoel.WriteLine("difference =" + (big - (int)approx));
```

?? 확인하기



- 정밀도 상실이 일어나는 이유: `int`의 정밀도는 4바이트, `float`의 정밀도는 4바이트이지만 1비트가 `signed`나 지수를 저장하기 때문이다.



### 2.4.3 형 변환 금지

- 부울형은 다른 자료형으로의 형 변환이 금지되어 있다.



### 2.4.4 박싱과 언박싱

- **박싱(boxing)**: 값형의 데이터를 참조형으로 변환하는 것. 컴파일러에 의해 묵시적으로 이루어진다.

  - 스택에 저장된 값형의 데이터를 힙 영역의 참조형으로 변환하여 사용할 수 있게 한다.

  - 박싱이 일어나면, 스택에 저장된 값형의 데이터를 힙에 저장하기 위해 힙에 작은 상자(box)를 만든 후 힙의 상자에 값형의 데이터를 복사한다.

    ```c#
    int i = 123;
    object o = i;	// 박싱
    ```

이미지 ??

- **언박싱(unboxing)**: 참조형의 데이터를 값형으로 변환하는 것. 캐스팅을 통하여 명시적으로 행해진다.
  - <u>반드시 박싱될 때의 자료형으로 언박싱을 해주어야한다</u>. 그렇지 않으면 `System.InvalidCaseException` 예외가 발생한다.

```c#
int i = 123;
object o = i;		// boxing

Console.WriteLine(o);
try {
    double d = (short)bar;	// unboxing: 에러 발생, 반드시 int로 언박싱해야한다.
    Consoel.WriteLine(d);
} catch (InValidCaseException e) {
    Console.WriteLine(e + "Error");
}
```

- **널이 가능한 형(nullable type)**

  - 값형은 0으로, 참조형은 `null`로 초기값이 설정된다.

  - 참조형처럼, 값형 역시 값이 없는 경우를 나타낼 필요가 있을 때 다음과 같이 널이 가능한 형을 정의한다.

    ```c#
    타입? a = null;
    ```

```c#
double? num1 = null;
double? num2 = 10.0;

if (num1.Hasvalue) Consoel.WriteLine("num1 =" + num1.Value);
else Console.WriteLine("num1 does not have value.");

if (num1.HasValue) Console.WriteLine("num2 =" + num2.Value);
else Console.WriteLine("num2 does not have value.");
```

- 널이 가능한 형은 다음의 프로퍼티를 가지고 있다.
  - `HasValue`: 변수에 값이 있는지 알려준다.
  - `value`: 값을 나타낸다.

