# Overview

## 목차

* 자바 자료형
* 자료구조와 알고리즘
* 자료구조 분류
* 재귀함수



## 자료구조 란?

자료구조는 컴퓨터에 저장된 자료를 효율적으로 이용할 수 있도록 하는 방법이다.



## 자바 자료형

자바의 자료형은 크게 기본 자료형(Primitive Type)과 참조 자료형(Reference Type)으로 구분된다.

<figure><img src="../../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

자바 정수형 자료형의 최대값과 최소값을 출력하는 예제이다.

```java
public class MyIntTypeTest {
  public static void main(String[] args){
  // byte형
  System.out.printf("%d, %d, %d\n", (byte)0x7F, (byte)0xFF, (byte)0x80);
  // short형
  System.out.printf("%d, %d, %d\n", (short)0x7FFF, (short)0xFFFF, (short)0x8000);
  // int형
  System.out.printf("%d, %d\n", 0x7FFF_FFFF, 0x8000_0000);
  // long형
  System.out.printf("%d, %d\n", 0x7FFFFFFF_FFFFFFFFL, 0x80000000_00000000L);
 }
}

// [실행결과]
// 127, -1, -128
// 32767, -1, -32768
// 2147483647, -2147483648
// 9223372036854775807, -9223372036854775808
```

## 자료구조와 알고리즘

알고리즘(Algorithm)이란 어떠한 문제를 해결하기 위한 단계적 절차이다. 유한성을 가지며, 언젠가는 끝나야 하는 속성을 가지고 있다.

알고리즘은 다음의 조건을 만족해야 한다.

* 입력: 외부에서 제공되는 자료가 0개 이상 존재
* 출력: 적어도 2개 이상의 결과(즉 모든 입력에 하나의 출력이 나오면 안됨)
* 명확성: 수행 과정은 명확하고 모호하지 않은 명령어로 구성
* 유한성(종결성): 유한 번의 명령어를 수행 후  유한 시간 내에 종료
* 효율성: 모든 과정은 명백하게 실행 가능(검증 가능)

알고리즘을 기술하는 방법에는 다음과 같은 방법이 있다

* 자연어(Natural Language)
* 흐름도(Flow Chart)
* 유사코드(Pseudo Code)
* 프로그래밍언어(Programming Language)

알고리즘의 성능 비교는 알고리즘의 시간복잡도 분석 방법이 가장 널리 사용된다. 시간 복잡도(Time Complexity)란 알고리즘이 수행하는 연산의 횟수를 빅오(Big-O) 표기법을 사용하여 점근적(대략적)으로 표기한 것이다. 함수의 상한을 표시하는 빅오 표기법은 다음과 같이 정의 된다.

> 두개의 함수 f(n)과 g(n)이 주어졌을 때, 모든 n≥n0에 대하여 |f(n)| ≤ c|g(n)|을 만족하는 2개의 상수 c와 n0가 존재하면 f(n)=O(g(n))이다.

(예) n≥2 이면, 2n+1 < 3n 이므로 2n+1 = O(n), 이때 상수 c=3, n0=2&#x20;

(예) n0 ≥1일 때 2n^3+5n^2+10 ≤100•n 3이므로 2n3+5n2+10은 O(n^3)

점근적 표기법으로 표현한 알고리즘 시간복잡도의 종류와 의미는 다음과 같다.

<figure><img src="../../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>

## 자료구조 분류

<figure><img src="../../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

## 재귀함수

자료구조와 알고리즘과 관련된 많은 문제에서 반복문을 대신해 재귀함수를 사용하여 구현한다.&#x20;

재귀함수란 함수 내부에서 자기 자신을 호출하는 함수를 의미한다. 재귀함수의 순환적 특징을 활용해서 반복문을 대체할 수 있다.&#x20;

재귀함수(재귀 메소드)는 아래와 같이 2가지의 부분으로 가진다.

&#x20;\- base case : 재귀호출을 멈추는 부분(탈출구라고도 한다)

&#x20;\- recursive case : 재귀호출 부분&#x20;

재귀호출을 할 때는 전달 값의 변화를 통해 문제의 크기를 줄여서 언젠가는 재귀호출을 멈출 수 있도록 해야 한다. 재귀호출에서 사용되는 수식의 의미는 재귀함수 정의가 가지는 의미와 동일해야 한다.&#x20;

다음은 양의 정수 n의 팩토리얼 값(n!)을 반환하는 재귀함수의 구조이다.

```java
public static long Factorial(int n) { // 재귀함수 정의의 의미 : n! 반환
 if ( n<1 )
 return 1; // base case, 탈출구
 return n * Factorial(n-1); // recursive case, 함수정의와 의미 동일( n * (n-1)! == n!)
}
```

아래 그림은 n=3일 때 팩토리얼 값을 계산하는 과정을 보여준다.

<figure><img src="../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

다음은 재귀함수를 사용하여 1! \~ 20! 까지의 값을 출력하는 예제이다.&#x20;

예제) FactTest.java

```java
class FactTest {
public static void main(String[] args) {
for(int i=1; i<=20; i++)
System.out.printf("%2d! = %20d\n", i, Factorial(i));
}
public static long Factorial(int n) {
  if ( n<1 )
  return 1; // base case, 탈출구
  return n * Factorial(n-1); // recursive case
 }
}
// [실행결과]
// 1! = 1
// 2! = 2
// 3! = 6
// 4! = 24
// 5! = 120
// 6! = 720
// 7! = 5040
// 8! = 40320
// 9! = 362880
// 10! = 3628800
// 11! = 39916800
// 12! = 479001600
// 13! = 6227020800
// 14! = 87178291200
// 15! = 1307674368000
// 16! = 20922789888000
// 17! = 355687428096000
// 18! = 6402373705728000
// 19! = 121645100408832000
// 20! = 2432902008176640000
```

다음은 n번째 피보나치 수를 반환하는 재귀함수의 구조이다.

```java
public static long Fib(int n) { // 재귀함수 정의의 의미 : n번째 피보나치 수 반환
 if ( n<=1 )
 return n; // base case, 탈출구
 return Fib(n-1) + Fib(n-2); // recursive case, 함수 정의와 의미 동일
}
```

아래 그림은 n=4일 때 피보나치 수를 계산하는 과정을 보여준다.

<figure><img src="../../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

위 그림과 같이 피보나치 수를 반환하는 재귀함수는 recursive case에서 재귀호출을 두 번씩 수행한다. 즉, Fib(n)은 약 2 n번의 재귀호출을 수행하게 된다. 따라서, n의 큰 경우에는 위 재귀함수를 사용하면 매우 비효율적이다. 재귀함수는 사용하기 전에 반드시 재귀호출 횟수를 고려해야 한다.

다음은 재귀함수를 사용하여 피보나치 수열 20개를 출력한 예제이다.

예제) FibTest.java

<pre class="language-java"><code class="lang-java">class Test {
public static void main(String[] args) {
for(int i=1; i&#x3C;=20; i++)
 System.out.printf("%2d번째 피보나치 수 = %4d\n", i, Fib(i));
}
public static long Fib(int n) { // 재귀함수 정의의 의미 : n번째 피보나치 수 반환
  if ( n&#x3C;=1 )
  return n; // base case, 탈출구
  return Fib(n-1) + Fib(n-2); // recursive case, 함수 정의와 의미 동일
 }
}
// [실행결과]
// 1번째 피보나치 수 = 1
// 2번째 피보나치 수 = 1
// 3번째 피보나치 수 = 2
// 4번째 피보나치 수 = 3
// 5번째 피보나치 수 = 5
// 6번째 피보나치 수 = 8
// 7번째 피보나치 수 = 13
<strong>// 8번째 피보나치 수 = 21
</strong>// 9번째 피보나치 수 = 34
// 10번째 피보나치 수 = 55
// 11번째 피보나치 수 = 89
// 12번째 피보나치 수 = 144
// 13번째 피보나치 수 = 233
// 14번째 피보나치 수 = 377
// 15번째 피보나치 수 = 610
// 16번째 피보나치 수 = 987
// 17번째 피보나치 수 = 1597
// 18번째 피보나치 수 = 2584
// 19번째 피보나치 수 = 4181
// 20번째 피보나치 수 = 6765
</code></pre>

다음은 양의 정수 n을 입력받아, 1에서 n까지 역수들의 합 계산을 재귀함수로 구현한 예제이다.

예제) RevSumTest.java

```java
import java.util.Scanner;
class Test {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.print("정수 입력 : ");
    int n=sc.nextInt();
    System.out.printf("1에서 %d까지 역수들의 합 = %f", n, revSum(n));
}
public static double revSum(int n) { // 함수 정의 의미 : 1/1 + 1/2 + ... + 1/n 반환
  if ( n==1 )
  return 1.0; // base case, 탈출구
  return revSum(n-1) + 1.0/n; // recursive case, 함수 정의와 의미 동일
 }
}

// [실행결과]
// 정수 입력 : 10
// 1에서 10까지 역수들의 합 = 2.928968
```
