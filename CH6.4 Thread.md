# 6.4 Thread

- 순차 프로그램(sequential program)
  - 각각 시작, 순차적 실행, 종료를 가진다.
  - 프로그램의 실행 과정에서 오직 하나의 실행 점(execution point)를 갖는다.
- 스레드
  - 순차 프로그램과 유사하게 시작, 실행, 종료를 가진다: C#에서 단일 스레드 개념은 순차 프로그램과 유사하다.
  - 실행되는 동안 한 시점에서 단일 실행 점을 가진다.
  - 프로그램 내에서만 실행이 가능하다.
    - 스레드는 프로그램 내부에 있는 **제어의 단일 순차 흐름(signle sequential flow of control)**이다.
- **멀티스레드(multithread) 시스템**: 하나의 프로그램 내에 스레드가 여러 개 존재하는 시스템
  - 공유 힙(shared heap; 동적 데이터-객체를 저장), 공유 데이터(shared data; 전역 변수를 저장), 코드를 공유함으로써 문맥 전환(context switching) 시 부담이 적다.
    - ​	스택(stack; 함수 내부의 지역 변수, return address 저장)은 스레드 별로 가진다.
  - 한 개의 프로그램 내에서 동일한 시점에 각각 다른 작업을 수행하는 여러 개의 스레드가 존재하므로 복잡한 문제들이 야기될 수 있다.
- C#에서는 언어 수준에서 스레드를 지원한다.



## 6.4.1 스레드 프로그래밍

- C#에서 스레드의 개념: 스레드는 객체이며, 스레드가 실행하는 단위는 메서드이다.
- `System.Threading` 네임스페이스
  - 스레드 객체를 위해 `Thread` 클래스 제공
  - 메서드 연결을 위해 `ThreadStart` 델리게이트 제공

- 스레드 프로그래밍의 순서

  - (1) 스레드 몸체에 해당하는 메서드 작성

    ```c#
    void ThreadBody() {/**/}
    ```

  - (2) 작성된 메서드를 `ThreadStart`에 연결

    ```c#
    ThreadStart ts = new ThreadStart(ThreadBody);
    ```

  - (3) 생성된 델리게이트를 이용하여 스레드 객체를 생성

    ```c#
    Thread t = new Thread(ts);
    ```

  - (4) 스레드의 실행을 시작(`Start()` 메서드 호출)

    ```c#
    t.Start();
    ```

```c#
// 스레드 프로그램
using System;
using System.Threading;

namespace ThreadApp
{
    class Program
    {
        static void PrintNumbers()
        {
            Console.WriteLine("========= Start of PrintNumbers =========");
            for (int i = 0; i < 5; i++)
            {
                Console.WriteLine(DateTime.Now.Second + ":" + i);
                Thread.Sleep(1000);
            }
            Console.WriteLine("========= End of PrintNumbers =========");
        }

        static void Main(string[] args)
        {
            ThreadStart start = new ThreadStart(PrintNumbers);
            Thread thread = new Thread(start);

            Console.WriteLine("========= Start of Main =========");
            thread.Start();
            Console.WriteLine("========= End of Main =========");
        }
    }
}

```



### Thread 클래스 프로퍼티

| 프로퍼티               | 반환     | 설명                                     |
| ---------------------- | -------- | ---------------------------------------- |
| `Thread.CurrentThread` | `Thread` | 현재 실행 중인 스레드 객체를 반환한다.   |
| `Thread.isAlive`       | `bool`   | 스레드의 실행 여부를 반환한다.           |
| `Thread.Name`          | `string` | 스레드의 이름을 지정하거나 반환한다.     |
| `Thread.isBackground`  | `bool`   | 스레드가 백그라운드 스레드인지 반환한다. |
| `Thread.ThreadState`   | `string` | 스레드의 상태를 반환한다.                |
| `Thread.Priority`      | `int`    | 스레드의 우선순위를 지정하거나 반환한다. |

```c#
using System;
using System.Threading;

namespace ThreadPropertyApp
{
    class Program
    {
        static void ThreadBody()
        {
            Thread myself = Thread.CurrentThread;
            for (int i = 0; i < 3; i++)
            {
                Console.WriteLine("{0} is activated => {1}", myself.Name, i);
                Thread.Sleep(1000);
            }
        }
        static void Main(string[] args)
        {
            ThreadStart start = new ThreadStart(ThreadBody);
            Thread t1 = new Thread(start);
            Thread t2 = new Thread(start);
            t1.Name = "PURPLE"; t2.Name = "BLUE";
            t1.Start(); t2.Start();
        }
    }
}

```

```
PURPLE is activated => 0
BLUE is activated => 0
PURPLE is activated => 1
BLUE is activated => 1
PURPLE is activated => 2
BLUE is activated => 2
```



## 6.4.2 스레드의 상태

- 스레드의 상태: (1) Unstarted (2) Running (3) Suspended (4) Stopped
- 시작 메서드: `Start()`
- 종료 조건: (1) 메서드의 종료 (2) `Abort()` 실행

이미지??

| 메서드             | 설명                                             |
| ------------------ | ------------------------------------------------ |
| `Thread.Start()`   | 스레드를 실행한다.                               |
| `Thread.Abort()`   | 스레드를 종료시킨다.                             |
| `Thread.Join()`    | 스레드의 실행이 종료될 때까지 기다린다.          |
| `Thread.Suspend()` | 스레드를 대기 상태로 만든다.                     |
| `Thread.Resume()`  | 대기 상태의 스레드를 실행 상태로 만든다.         |
| `Thread.Sleep()`   | 지정한 시간 동안 실행을 멈추고 대기 상태로 간다. |



```c#
using System;
using System.Threading;

namespace ThreadStateApp
{
    class Program
    {
        static void ThreadBody()
        {
            while (true) { /* infinite loop */}
        }
        static void Main(string[] args)
        {
            ThreadStart start = new ThreadStart(ThreadBody);
            Thread thread = new Thread(start);

            Console.Write("STEP 1: " + thread.ThreadState);
            thread.Start();     Thread.Sleep(100);

            Console.WriteLine("STEP 2: " + thread.ThreadState);
            thread.Suspend();   Thread.Sleep(100);

            Console.WriteLine("STEP 3: " + thread.ThreadState);
            thread.Resume();    Thread.Sleep(100);

            Console.WriteLine("STEP 4: " + thread.ThreadState);
            thread.Abort(); Thread.Sleep(100);

            Console.WriteLine("STEP 5: " + thread.ThreadState);
        }
    }
}
```

```
STEP 1 : Unstarted
STEP 2 : Running
STEP 3 : Suspended
STEP 4 : Running
STEP 5 : Aborted
```

! 나는 안됨



```c#
// 완전수(perfect number: 자신을 제외한 약수의 합과 같은 수)를 구하는 메서드
using System;
using System.Threading;

namespace ThreadJoinApp
{
    class ThreadJoin
    {
        public int start;
        public int perfectNumber = -1;
        public void ThreadBody()
        {
            int sum;

            for (int i = start; ; i++)
            {
                sum = 0;
                for (int j = 1; j <= i / 2; j++)
                {
                    if (i % j == 0) sum += j;
                }
                
                if (i == sum)
                {
                    perfectNumber = i;
                    break;
                }
            }
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            ThreadJoin obj = new ThreadJoin();
            ThreadStart start = new ThreadStart(obj.ThreadBody);
            Thread t = new Thread(start);

            Console.Write("Enter a number: ");
            obj.start = int.Parse(Console.ReadLine());
            t.Start();  t.Join();
            Console.WriteLine("The pefect number over {0} is {1}.", obj.start, obj.perfectNumber);
        }
    }
}

```

```
Enter a number: 100
The pefect number over 100 is 496.
```



## 6.4.3 스레드의 스케쥴링

- **스케줄링(scheduling)**: 여러 스레드의 실행 순서를 제어하는 것

  - 스레드의 스케줄링에 따라 스레드의 작업 순서가 달라진다.

- **스레드의 우선순위(priority)**에 따라 스레드의 실행 순서가 결정된다.

  - 스레드가 생성될 때 그 스레드를 만든 스레드의 우선순위가 상속된다.

  - `Thread.Priority` 프로퍼티로 스레드의 우선순위를 참조하거나 변경 가능

  - `ThreadPriority` 열거형

    | 멤버          | 설명                           |
    | ------------- | ------------------------------ |
    | `Highest`     | 가장 높은 우선순위를 나타낸다. |
    | `AboveNormal` | 높은 우선순위를 나타낸다.      |
    | `Normal`      | 표준 우선순위를 나타낸다.      |
    | `BelowNormal` | 낮은 우선순위를 나타낸다.      |
    | `Lowest`      | 가장 낮은 우선순위를 나타낸다. |



```c#
// Thread.Prioirty 참조하기
using System;
using System.Threading;

namespace TheadingPrioirtyApp
{
    class Program
    {
        static void ThreadBody()
        {
            Thread.Sleep(1000);
        }
        static void Main(string[] args)
        {
            Thread t = new Thread(new ThreadStart(ThreadBody));
            t.Start();
            Console.WriteLine("Current Prioirty: " + t.Priority);
            ++t.Priority;
            Console.WriteLine("Higher Prioirty: " + t.Priority);
        }
    }
}

```

```
Current Prioirty: Normal
Higher Prioirty: AboveNormal
```



```c#
using System;
using System.Threading;

namespace SchedulerApp
{
    class Program
    {
        static int interval;
        static void ThreadBody()
        {
            Thread myself = Thread.CurrentThread;
            Console.WriteLine("Starting Thread : " + myself.Name);
            for (int i = 1; i <= 3 * interval; i++)
            {
                if (i % interval == 0)
                    Console.WriteLine(myself.Name + " : " + i);
            }
            Console.WriteLine("Ending Thread : " + myself.Name);
        }
        static void Main(string[] args)
        {
            Console.Write("Interval value : ");
            interval = int.Parse(Console.ReadLine());
            
            Thread.CurrentThread.Name = "Main Thread";
            // Thread.CurrentThread.Priority = ThreadPriority.Highest;
            Thread worker = new Thread(new ThreadStart(ThreadBody));
            worker.Name = "Worker Thread";
            worker.Start();
            
            ThreadBody();
        }
    }
}

```

```
Interval value : 100
Starting Thread : Main Thread
Main Thread : 100
Main Thread : 200
Main Thread : 300
Ending Thread : Main Thread
Starting Thread : Worker Thread
Worker Thread : 100
Worker Thread : 200
Worker Thread : 300
Ending Thread : Worker Thread
```

```
Interval value : 10000
Starting Thread : Main Thread
Starting Thread : Worker Thread
Main Thread : 10000
Worker Thread : 10000
Main Thread : 20000
Worker Thread : 20000
Main Thread : 30000
Ending Thread : Main Thread
Worker Thread : 30000
Ending Thread : Worker Thread
```



## 6.4.4 동기화

- 비동기 스레드
  - 각각의 스레드는 실행에 필요한 모든 자료와 메서드를 포함
  - 병행으로 실행 중인 다른 스레드의 상태 또는 행위에 관계되지 않는 자신만의 공간에서 실행
- 동기 스레드
  - 동시에 실행되는 스레드들이 자료를 공유
  - 다른 스레드의 상태와 행위를 고려
  - 동기화 문제 발생: 여러 개의 스레드가 동일한 리소스에 동시에 접근할 경우
- 동기화 문제의 해결: (1) `lock`문 (2) `Monitor` 클래스



### 6.4.4.1 lock문

- `lock`문: 동일한 객체에 대하여 여러 스레드의 중첩 실행을 방지. 아토믹 루틴(atomic routine)이 되어 실행 순서를 제어할 수 있음.
- `lock`문의 형태
  - `obj`: 잠그고자 하는 객체
  - `<문장>`: `obj` 를 동기화 방법을 다루는 문장. 보통 블록으로 구성

```c#
lock (object obj)
    <문장>
```



```c#
// ThreadBody에 lock을 추가
using System;
using System.Threading;

namespace LockStatementApp
{
    class Program
    {
        void ThreadBody()
        {
            Thread myself = Thread.CurrentThread;
            lock (this){
                for (int i = 0; i < 3; i++)
                {
                    Console.WriteLine("{0} is activated => {1}", myself.Name, i);
                    Thread.Sleep(1000);
                }
            }

        }
        static void Main(string[] args)
        {
            Program obj = new Program();
            ThreadStart start = new ThreadStart(obj.ThreadBody);
            Thread t1 = new Thread(start);
            Thread t2 = new Thread(start);
            t1.Name = "PURPLE"; t2.Name = "BLUE";
            t1.Start(); t2.Start();
        }
    }
}

```

```
PURPLE is activated => 0
PURPLE is activated => 1
PURPLE is activated => 2
BLUE is activated => 0
BLUE is activated => 1
BLUE is activated => 2
```



### 6.4.4.2 Monitor 클래스

- `Monitor`: 임의의 객체에 대한 특정 작업 동기화하기 위해 사용

```c#
Monitor.Enter();
<동기화가 필요한 작업>
Monitor.Exit();
```

| 메서드              | 설명                                                         |
| ------------------- | ------------------------------------------------------------ |
| `Enter(Object obj)` | 스레드 객체 `obj`가 특정 문장을 선점할 수 있도록 지정한다.   |
| `Exit(Object obb)`  | 스레드 객체 `obj`가 선점한 특정 문장의 실행을 다른 스레드 객체가 접근할 수 있도록 한다. |

```c#
using System;
using System.Threading;

namespace MonitorApp
{
    class Program
    {
        void ThreadBody()
        {
            Thread myself = Thread.CurrentThread;
            Monitor.Enter(this);
            for (int i = 0; i < 3; i++)
            {
                Console.WriteLine("{0} is activated => {1}", myself.Name, i);
                Thread.Sleep(1000);
            }
            Monitor.Exit(this);

        }
        static void Main(string[] args)
        {
            Program obj = new Program();
            ThreadStart start = new ThreadStart(obj.ThreadBody);
            Thread t1 = new Thread(start);
            Thread t2 = new Thread(start);
            t1.Name = "PURPLE"; t2.Name = "BLUE";
            t1.Start(); t2.Start();
        }
    }
}

```

```
PURPLE is activated => 0
PURPLE is activated => 1
PURPLE is activated => 2
BLUE is activated => 0
BLUE is activated => 1
BLUE is activated => 2
```

