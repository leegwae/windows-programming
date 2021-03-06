# _Function

## System.Object

| 메서드       | 반환     | 설명                                 | 참고 |
| ------------ | -------- | ------------------------------------ | ---- |
| `ToString()` | `string` | 객체를 스트링으로 변환하여 반환한다. |      |
|              |          |                                      |      |



## StringBuilder

| 메서드           | 반환 | 설명                                  | 참고  |
| ---------------- | ---- | ------------------------------------- | ----- |
| `Append(스트링)` |      | 스트링 객체 뒤에 `스트링`을 연결한다. | 2.2.5 |
|                  |      |                                       |       |



## System.Console

| 메서드                      | 반환     | 설명                                                         | 참고    |
| --------------------------- | -------- | ------------------------------------------------------------ | ------- |
| `Console.Read()`            | `int`    | 키보드로부터 한 개의 문자를 읽고, 유니코드값을 `int`형으로 반환한다. | 3.5.1.1 |
| `Console.ReadLine()`        | `string` | 한 라인을 읽어 `string`으로 반환한다.                        |         |
| `Console.Write(스트링)`     |          | 화면에 `스트링`을 출력한다.                                  | 3.5.1.2 |
| `Console.WriteLine()스트링` |          | 화면에 `스트링`을 출력하고 다음 줄로 출력 위치를 이동한다.   |         |



## System.Threading

| 메서드                      | 반환          | 설명                                                   | 참고 |
| --------------------------- | ------------- | ------------------------------------------------------ | ---- |
| `ThreadStart(메서드)`       | `ThreadStart` | 메서드를 스레드 몸체로 가지는`ThreadStart` 객체를 반환 |      |
| `Thread(ThreadStart start)` | `Thread`      |                                                        |      |
|                             |               |                                                        |      |



## System.Threading.Thread

| 메서드                   | 반환 | 설명                                         | 참고 |
| ------------------------ | ---- | -------------------------------------------- | ---- |
| `Start()`                |      | 스레드의 실행을 시작한다.                    |      |
| `Sleep(int millisecond)` |      | `millisecond` 만큼 스레드의 실행을 중지한다. |      |
|                          |      |                                              |      |



## int

| 메서드              | 반환  | 설명                             | 참고 |
| ------------------- | ----- | -------------------------------- | ---- |
| `int.Parse(스트링)` | `int` | `스트링`을 `int`형으로 반환한다. |      |
|                     |       |                                  |      |



## char

| 메서드               | 반환   | 설명                                           | 참고 |
| -------------------- | ------ | ---------------------------------------------- | ---- |
| `char.IsDigit(문자)` | `bool` | `문자`가 digit인지 판별하여 부울형을 반환한다. |      |
|                      |        |                                                |      |

