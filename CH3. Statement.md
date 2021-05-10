# CH3. 문장

## 3.1 배정문

- **배정문(assignment statement)**: 값을 변수에 저장하는데 사용한다.

- 배정문의 형태

  ```c#
  변수 = 식;
  ```

  - 할당(배정) 연산자 `=`는 우측 결합 연산자이므로, 오른쪽의 `식`부터 계산한다. 

  ```c#
  i = j = k = 0
  ```

  - 즉, 먼저 `k`에 `0`을 할당하고, `k`의 값이 `j`에 할당된 후 `j`의 값이 `i`에 할당된다.

- 할당 연산자의 좌변과 우변의 데이터 타입이 다르면 묵시적 형 변환이 일어난다. 우변의 `식`을 캐스트 연산자(형 변환 연산자)를 통해 명시적으로 형을 변환할 수 있다.



## 3.2 혼합문

- **혼합문(compound statement)**: 여러 문장을 한데 묶어 하나의 문장으로 나타낸다.

- 혼합문의 형태

  ```c#
  {
      // 선언 또는 문장
  }
  ```

  - 혼합문은 문장의 블록을 나타낼 때 주로 사용한다.
  - 블록은 문장의 범위를 표시한다. 즉, 여러 개의 문장을 하나의 문장, 하나의 단위로 취급한다.



### 지역 변수(local variable)

- 블록의 내부에서 선언된 변수

- 선언된 블록 내에서만 참조가 가능하다.

- **지역 변수의 이름**

  - 지역 변수의 이름은 블록에 대해 비지역적인(블록 외부의) 변수의 이름과 달라야 한다.
  - C 언어와 달리, <u>지역 변수의 이름이 블록 외부의 변수를 숨기지(hiding) 못하기</u> 때문이다.

  ```c#
  int x;
  {
      int x;		// 에러!
  }
  ```

- 메서드 내에 선언된 변수의 이름

  - <u>메서드 외부에 선언된 변수의 이름과 같아도 된다</u>.

  - 메서드 내부에서 메서드 외부에 선언된 변수에 접근할 때는, 그 외부 변수가 속한 클래스의 이름을 앞에 붙여 접근한다.

    ```c#
    class NonLocalVariable {
        int x;		// nonlocal to method Print
        public void Print() {
            Console.WriteLine(NonLocalVariable.x);
        }
    }
    ```



## 3.3 제어문

- 프로그램은 기본적으로 작성한 순서대로 차례로 실행된다.
- **제어문**: 프로그램의 실행 순서를 바꾸는데 사용한다.
- 제어문의 종류: <u>순서를 제어하는 방법에 따라 (1) 조건문 (2) 반복문 (3) 분기문</u>으로 나눈다.

| 제어문                        | 설명                                                     | 종류                                              |
| ----------------------------- | -------------------------------------------------------- | ------------------------------------------------- |
| 조건문(conditional statement) | 주어진 조건에 따라 실행되는 부분이 다를 때 사용하는 문장 | `if`<br />`switch`                                |
| 반복문                        | 일정한 부분을 주어진 조건을 만족할 때까지 반복하는 문장  | `for`<br />`while`<br />`do-while`<br />`foreach` |
| 분기문(jump statement)        | 제어를 지정된 곳으로 옮기는 문장                         | `break`<br />`continue`<br />`goto`<br />`return` |



### 3.3.1 조건문

- **조건문(conditional statement)**: 조건에 따라 실행되는 부분이 다를 때 사용한다.
- 조건문의 종류: (1) `if`문,  (2) `switch`문



#### 3.3.1.1 if문

- `if`문의 형태

  ```c#
  if (조건식) 문장		// (1)
  if (조건식) 문장1 else 문장2	// (2)
  ```

  - (1)에서 `문장`을 참 부분(true part)라고 한다.
  - (2)에서 `문장1`을 참 부분, `문장2`를 거짓 부분(false part)라고 한다.

- `조건식`의 연산 결과: **반드시 부울형**이다.

- 내포된 `if`문(nested if statement): `if`의 문장 부분에 `if`문이 나타나는 문장

  - 내포된 `if`문의 형태

    ```c#
    // 참 부분에 if 문이 나타나는 경우
    if (조건식)
        if (조건식)
        	문장
    ```

    ```c#
    // else 부분에서 if 문이 나타나는 경우
    if (조건식1) 문장1
    else if (조건식2) 문장2
    else 문장3
    ```



#### 3.3.1.2 switch 문

- `switch`문의 형태

  ```c#
  switch(식) {
      case 상수식1: 문장1 break;
      // ...
      case 상수식n: 문장n break;
      default: 문장; break;
  }
  ```

  - `case` 뒤의 `상수식`: **컴파일 타임에 정해져 있는 상수값만이 가능하며, 스트링 역시 가능**하다.

  - `default`의 의미를 otherwise이다.

  - `break`로 블럭을 탈출한다.

  - `break` 대신 `goto`를 사용할 수 있다.

    ```c#
    case 1: goto case 3;
    ```

- jump table을 만들어서 해당 상수의 `case`의 문장으로 가므로 매우 효율적이다.



### 3.3.2 반복문

- **반복문**: 주어진 조건이 만족될 때까지 일련의 문장을 반복한다.
- 반복문의 종류: (1) `for` (2) `while` (3) `do-while` (4) `foreach`



#### 3.3.2.1 for문

- `for`문: 정해진 횟수만큼 일련의 문장을 반복한다.

- `for`문의 형태

  ```c#
  for (식1; 식2; 식3)
      문장
  
  for (초기식; 조건식; 증감식)
      문장
  ```

  - 제어 변수(control variable): 반복 실행을 제어하는 데 사용한다.
  - `초기화식`: 제어 변수를 초기화한다.
  - `증감식`: 제어 변수의 값을 수정한다.
  - `조건식`: 제어 변수의 값을 검사한다.
  - `문장`: 루프 몸체(loop body), `조건식`이 참인 동안 반복 실행된다.

- `for`문의 실행 순서

  - (1) `초기화식`: 최초 한 번 실행
  - (2) `조건식`: 연산 결과가 참이면 `문장`을 실행하고 (3)으로. 연산 결과가 거짓이면 (4)로.
  - (3) `증감식`: 연산하고 (2)로
  - (4) `for`문 밖으로

- 무한 루프 만들기: `초기식`, `조건식`, `증감식` 모두를 생략하면 무한 루프가 된다.

  - 루프를 종료시키기 위해 `break`나 `return` 문을 사용한다.

- 내포된 `for`문(nested for statement): 다차원 배열(matrix)을 다룰 때 자주 사용한다.

  ```c#
  int[,] matrix = [3, 3];
  
  for (int i = 0; i < matrix.Length; i++) {
      for (int j = 0; j < matrix[i].Length; j++) {
          matrix[i][j] = i * j + j;
      }
  }
  ```

  

#### 3.3.2.2 while문

- `while`문: 주어진 조건식이 참인 경우에만 일련의 문장을 반복한다.

- `while`문의 형태

  ```c#
  while (조건식) 
      문장;
  ```

- `while`문의 실행 순서

  - (1) `조건식`: 연산 결과가 참이면 `문장`을 실행하고 (1)로. 연산 결과가 거짓이면 (2)으로.
  - (2) `while`문 밖으로

- `while`문의 조건식의 결과가 거짓이 되면 이 조건을 `while`문의 결과 조건(result condition)이라고 한다. 즉, 결과 조건은 `!조건식`이다.



#### 3.3.2.3 do-while문

- `do-while`문: 반복되는 문장을 먼저 실행한 후 조건문을 검사한다.

- `do-while`문의 형태

  ```c#
  do
      문장
  while (조건식);
  ```

- `do-while`문의 실행 순서

  - (1) `문장`: 실행 후 (2)로 간다.
  - (2) `조건식`: 연산 결과가 참이면 (1)로, 연산 결과가 거짓이면 (3)으로
  - (3) `do-while`문 밖으로

- `for`문, `while`문은 precondition check이고, `do-while`문은 postcondition check이다.



#### 3.3.2.4 foreach문

- `foreach`문: 데이터의 집합에 대해 반복을 실행한다.

- `foreach`문의 형태

  ```c#
  foreach(자료형 변수 in 집합)
      문장
  ```

  - `변수`는 **읽기 전용이 되므로**, `문장` 내에서 값을 할당하거나 바꿀 수 없다.

- `foreach`의 실행: 집합의 원소의 개수와 반복 횟수가 일치한다.

```c#
string[] albums = ["ultraviolence", "paradise", "lust for life"];
foreach (String album in albums) {
    Console.WriteLine(album);
}
```



### 3.3.3 분기문

- **분기문(jump statement)**: 지정된 곳으로 제어를 옮기는 문장



#### 3.3.3.1 break문

- `break`문: 블록 밖으로 제어를 옮긴다.

- `break`문의 형태

  ```c#
  break;
  ```

- `break`로 반복문 빠져나가기

```c#
int i = 0;
while (true) {
    if (i == 100)
        break;
    i = 0;
}
```



#### 3.3.3.2 continue문

- `continue`문: 다음 반복이 시작되는 곳으로 제어를 옮긴다.

- `continue`의 형태

  ```c#
  continue;
  ```

```c#
// 1부터 100까지 짝수만 더하기
int result = 0;
for (int i = 1; i < 101; i++){
    if (i % 2 != 0)
        continue;
    result += i;
}
```



#### 3.3.3.3 goto문

- `goto`문: 지정된 위치로 제어 흐름을 이동한다.

  - 지정된 위치를 나타내기 위해 명칭 형태를 사용하며, 이 명칭을 레이블(label)이라고 한다.

- `goto`문의 형태

  ```c#
  goto 레이블;
  goto case 상수식;
  goto default;
  ```

  - 두번째, 세번째 형태는 `switch`문에서 사용한다. (3.3.1.2 `switch`문 참고)

- `goto`문이 **분기할 수 없는 경우**

  - (1) 외부에서 복합문 내부로 분기할 수 없다.
  - (2) 메서드 내부에서 외부로 분기할 수 없다.
  - (3) `finally` 블록 내부에서 블록 밖으로 분기할 수 없다.



#### goto로 중첩 반복문 빠져나가기

```c#
for (int i = 0; i < 100; i++){
    for (int j = 0; j < 100; j++) {
        if (i * j == 37) {
            goto Here;
        }
    }
}

Here:
	Console.WriteLine("중첩 반복문을 빠져나왔다!");
```

[.NET DOCS](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/goto#example) 참고



#### 3.3.3.4 return문

- `return`문: 메서드의 실행을 종료하고, **호출한 메서드(caller)에게 제어를 넘긴다**.

- `return`문의 형태

  ```c#
  return;
  return 식;
  ```

```c#
int add(int a, int b) {
    return a + b;
}

int result = 0;
for (int i = 0; i < 30; i++) {
    result = add(result, i);
}
```



## 3.4 오버플로 검사문

- **오버플로(overflow)**: 정수식 연산의 결과가 정수형이 표현할 수 있는 범위를 넘어가는 것. 잘못된 계산 결과를 초래한다.

- `checked`: 오버플로를 명시적으로 검사한다.

- `checked`의 형태

  ```c#
  checked {
      // 오버플로를 검사할 문장
  }
  checked(오버플로를 검사할 정수식)
  ```

  - 여기서 오버플로가 발생하면, `System.OverflowException` 예외가 발생한다.

- `unchecked`: C#에서 오버플로는 기본적으로 검사하지 않지만, 이를 명시하기 위해 사용한다. 또는 오버플로를 의도적으로 검사하지 않기 위해 사용한다.

- `unchecked`의 형태

  ```c#
  unchecked{
      // 오버플로를 검사하지 않을 문장
  }
  ```

  

```c#
public static void Main(String[] args){
    int i, max = int.MaxValue;
    try {
        Console.WriteLine("Start of try statement...");
        i = max + 1;			// 기본적으로 오버플로를 검사하지 않는다.
        unchecked {
            i = max + 1;		// 명시적으로 오버플로를 검사하지 않는다.
        }
        Console.WriteLine("After unchecked statement");
        checked {
            i = max + 1;		// 오버플로를 검사한다.
        }
        Console.WriteLine("After checked statement");
    } catch (OverflowException e) {
        Console.WriteLine("오버플로가 발생했다!");
        Console.WriteLine(e);
    }
}
```

```
Start of try statement...
After unchecked statement
오버플로가 발생했다!
(에러 생략)
```



## 3.5 표준 입출력

### 3.5.1 입출력문

- **입출력(I/O)**: 입력과 출력
- 입출력을 C#의 기본 네임스페이스 `System`의 메서드로 제공한다.
- 표준 입출력: 입출력 장치가 미리 정해진 입출력
- 프로그래밍 언어에서는 일반적으로 파일과 주변장치를 동일하게 취급한다.
  - 즉, 파일 입출력과 주변장치 입출력을 동일한 메서드를 사용하여 처리한다.



#### 3.5.1.1 표준 입력 메서드

| 메서드               | 설명                                                         |
| -------------------- | ------------------------------------------------------------ |
| `Console.Read()`     | 키보드로부터 한 개의 문자를 읽고 유니코드값을 `int`형으로 변환하여 반환한다. |
| `Console.ReadLine()` | 한 라인을 읽어 `string`을 반환한다.                          |



```c#
// 문자를 입력받아 정수와 문자로 표현하기
int i;
char c;

Console.WriteLine("Enter a digit and chacracter : ");
i = Console.Read() - '0';		// 가능한지 보기
c = (char) Console.Read();
Console.WriteLine("i = " + i);
Console.WriteLine("c = " + c);
```



#### 한자릿수 정수 입력받기

```c#
int i = Console.Read() - '0';
```

- `Console.Read()`는 키보드의 한 문자를 읽고, 해당 문자의 유니코드값을 `int`형으로 반환한다.
- 따라서 `'0'`을 빼거나, `'0'`이 `int`형으로 변환된 `48`을 빼준다.



```c#
// char.IsDigit로 검사하여 받기
while(!char.IsDigit(ch = (char)Console.Read()));

do {
    ch = (char)Console.Read();
} while (char.IsDigit(ch));
```

해보기??



#### 3.5.1.2 표준 출력 메서드

| 메서드                        | 설명                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| `Console.Write(매개변수)`     | 화면에 `매개변수`의 값을 출력한다.                           |
| `Console.WriteLine(매개변수)` | 화면에 `매개변수`의 값을 출력한 후 다음 라인으로 출력 위치를 이동한다. |



#### 하나의 정수 입력받기

```c#
// 입력받은 문자열을 정수로 변환하기
int i;

Console.WriteLine("Enter an integer: ");
i = int.Parse(Console.ReadLine());
```

해보기??



### 3.5.2 형식화된 출력

- **형식화된 출력(formatted output)**: 출력하려는 값에 포맷을 명시하여 원하는 형태로 출력하는 것

- 포맷 형태

  ```c#
  {N[,W][:formatCharacter]}
  ```

  - `N`: 매개변수를 위치적으로 지정하는 정수. 0부터 시작한다.
  - `W`: 출력될 자릿수의 폭을 명시한다. 선택적으로 나타낼 수 있다. `-`를 붙이면 좌측 정렬로 출력된다.
  - `formatCharacter`: 형식 지정 문자를 의미한다. 선택적으로 나타낼 수 있다.

- 예) `{0,9:P}`: 첫번째 매개변수의 폭은 9로 한 후, 백분율로 포맷을 출력한다. 매개변수가 `0.3`이면, `[][]30.00[]%`로 출력된다. `[]`은 공백이다.

- 형식 지정 스트링(format string): 매개 변수의 개수와 일치하는 출력 포맷으로 이루어진 스트링 상수

```c#
Console.WriteLine("3 출력하기: {0,4:d}", 3);		// [][][]3
```



형식 지정문자표 이미지??

