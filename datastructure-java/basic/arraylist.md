# ArrayList

## 목자

* 순차리스트 개념
* 순차리스트 구현
* 자바 ArrayList 활용
* 순차리스트 응용



## 순차리스트(Linear List) 개념

리스트는 다음과 같이 순서가 있는 원소들의 집합으로 표현할 수 있다.

리스트 = (원소1, 원소2, ... , 원소n)

원소가 하나도 없는 리스트는 공백 리스트(empty list)라고 한다.

<figure><img src="../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

빈자리를 만들기 위한 원소들의 이동 횟수는 (마지막 원소 인덱스 - 삽입할 자리 인덱스 + 1) 이 된다

<figure><img src="../../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

빈자리를 채우기 위한 원소들의 이동 횟수는 (마지막 원소의 인데스 - 삭제한 원소의 인덱스) 가 된다.

## 순차리스트 구현

### 순차리스트에 데이터 추가, 삽입

```javascript
public boolean add(Object element) { // 순차리스트에 데이터 추가
 elementData[size] = element;
 size++;
 return true;
}
 public boolean add(int index, Object element) { // 순차리스트에 데이터 삽입
 for (int i = size - 1; i >= index; i--) {
  elementData[i + 1] = elementData[i];
 }
   elementData[index] = element;
   size++;
  return true;
}
```

### 순차리스트에서 데이터 삭제

```java
public Object remove(int index) { // 순차리스트에서 데이터 삭제
 Object removed = elementData[index];
 for (int i = index + 1; i <= size - 1; i++) {
  elementData[i - 1] = elementData[i];
 }
 
 size--;
 elementData[size] = null;
 return removed;
} 
```

순차리스트에서 데이터 임의접근(Random Access)

```java
public Object get(int index) { // 순차리스트 데이터 임의접근(읽기)
 return elementData[index];
}
```

순차리스트에서 데이터 검색

```java
public int indexOf(Object o) { // 순차리스트에서 데이터 검색
 for (int i = 0; i < size; i++) {
  if (o.equals(elementData[i])) {
   return i;
   }
 }
 return -1;
}
```

예제) MyArrayListTest.java

<pre class="language-java"><code class="lang-java">class MyArrayList {
 private int size = 0; // 순차리스트에 포함된 데이터 수 저장
 private Object[] elementData = new Object[100]; // 내부 자료구조로 배열 사용
 public MyArrayList() { }

 public boolean add(Object element) { // 순차리스트에 데이터 추가
  elementData[size] = element;
  size++;
  return true;
 }
 
 public boolean add(int index, Object element) { // 순차리스트에 데이터 삽입
  for (int i = size - 1; i >= index; i--) {
   elementData[i + 1] = elementData[i];
  }
  elementData[index] = element;
  size++;
  return true;
 }

 public String toString() { // 순차리스트를 문자열로 변환
 String str = "[";
 for (int i = 0; i &#x3C; size; i++) {
   str += elementData[i];
  if (i &#x3C; size - 1)
   str += ",";
  }
  return str + "]";
 }

 public Object remove(int index) { // 순차리스트에서 데이터 삭제
  Object removed = elementData[index];
   for (int i = index + 1; i &#x3C;= size - 1; i++) {
    elementData[i - 1] = elementData[i];
   }
  size--;
  elementData[size] = null;
  return removed;
 }

 public Object get(int index) { // 순차리스트 데이터 임의접근(읽기)
  return elementData[index];
 }
 
 public int size() { // 순차리스트
  return size;
 }
 
 public int indexOf(Object o) { // 순차리스트에서 데이터 검색
  for (int i = 0; i &#x3C; size; i++) {
   if (o.equals(elementData[i])) {
   return i;
   }
  }
  return -1;
 }
}
public class MyArrayListTest {
 public static void main(String[] args){
   MyArrayList numbers = new MyArrayList(); // 순차리스트 객체 생성
    numbers.add(10); // 데이터 추가
    numbers.add(20);
    numbers.add(30);
    numbers.add(40);
    System.out.println("순차리스트(MyArrayList)");
    System.out.println(numbers);
    numbers.add(1, 50); // 데이터 삽입
    System.out.println("\nadd(1, 50)");
    System.out.println(numbers);
    numbers.remove(2); // 데이터 삭제
    System.out.println("\nremove(2)");
    System.out.println(numbers);
    System.out.println("\nget(2)");
    System.out.println(numbers.get(2)); // 데이터 접근
    System.out.println("\nsize()");
    System.out.println(numbers.size()); // 데이터 수
    System.out.println("\nindexOf(30)");
    System.out.println(numbers.indexOf(30)); // 데이터 검색
    System.out.println("\nfor문");
    for (int i = 0; i &#x3C; numbers.size(); i++) { // 순차리스트 순회
    System.out.print(numbers.get(i) + " ");
  }
 }
}
<strong>// [실행결과]
</strong>// 순차리스트(MyArrayList)
// [10,20,30,40]
// add(1, 50)
// [10,50,20,30,40]
// remove(2)
// [10,50,30,40]
// get(2)
// 30
// size()
// 4
// indexOf(30)
// 2
// for문
// 10 50 30 40
</code></pre>

## 자바 ArrayList 활용

ArrayList arr1 = new ArrayList<>(); // 순차리스트에 String 객체를 저장할 경우

ArrayList arr2 = new ArrayList<>(); // 순차리스트에 Integer 객체를 저장할 경우

<figure><img src="../../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

다음은 자바 ArrayList 클래스를 사용한 예제이다.

예제) ArrayListTest.java

<pre class="language-java"><code class="lang-java">import java.util.ArrayList;
public class ArrayListTest {
   public static void main(String[] args) {
    ArrayList&#x3C;Integer> numbers = new ArrayList&#x3C;>(); // 자바 ArrayList 객체 생성
    numbers.add(10); // 데이터 추가
    numbers.add(20);
    numbers.add(30);
    numbers.add(40);
    System.out.println("순차리스트(자바 ArrayList)");
    System.out.println(numbers); // 모든 데이터 출력
    numbers.add(1, 50); // 데이터 삽입
    System.out.println("\nadd(1, 50)");
    System.out.println(numbers);
    numbers.remove(2); // 데이터 삭제
    System.out.println("\nremove(2)");
    System.out.println(numbers);
    System.out.println("\nget(2)");
    System.out.println(numbers.get(2)); // 데이터 접근(읽기)
    System.out.println("\nsize()");
    System.out.println(numbers.size()); // 데이터 수 확인
    System.out.println("\nindexOf(30)");
    System.out.println(numbers.indexOf(30)); // 데이터 검색
    System.out.println("\nfor each문");
    for (int value : numbers) { // for each문으로 순차리스트 순회
    System.out.print(value + " ");
    }
    System.out.println();
    System.out.println("\nfor문"); // for문으로 순차리스트 순회
    for (int i = 0; i &#x3C; numbers.size(); i++) {
    System.out.print(numbers.get(i) + " ");
   }
 }
}
<strong>// [실행결과]
</strong>// 순차리스트(자바 ArrayList)
// [10, 20, 30, 40]
// add(1, 50)
// [10, 50, 20, 30, 40]
// remove(2)
// [10, 50, 30, 40]
// get(2)
// 30
// size()
// 4
// indexOf(30)
// 2
// for each문
// 10 50 30 40
// for문
<strong>// 10 50 30 40
</strong></code></pre>

## 순차리스트 응용

예를 들어, 다항식 A = 3x^3 + 2x^2 + 5 는 3개 항으로 구성된 다항식이며, 차수는 3이 된다.

다항식을 순차리스트 표현하는 방법에는 계수 표현법과 <계수, 지수>쌍 표현법이 있다.

### 계수 표현법

예를 들어, 다항식 A  3x^3 + 2x^2 + 5를 계수 표현법으로 나타내면 다음과 같다.

<figure><img src="../../.gitbook/assets/image (129).png" alt=""><figcaption></figcaption></figure>

계수 표현법으로 표현된 두 다항식의 합을 구현하는 방법을 알아보자.

계수 표현법으로 표현된 두 다항식 A\ = 4x^3 + 3x^2 + 5x, B = 3x^4 + x^3 + 2x + 1 를 합하여 다항식 C를 만드는 방법은 인덱스 0부터 시작해서 같은 인덱스에 저장된 두 계수 값을 더해서 다항식 C에 포함시킨다.

<figure><img src="../../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

다음은 계수 표현법으로 표현된 두 다항식의 합을 자바 ArrayList 클래스를 사용하여 구현한 예제이다.

예제) PolynomialTest1.java

```java
import java.util.ArrayList;
 public class PolynomialTest1 {
   public static void main(String[] args) {
   // 다항식 A 만들기
   ArrayList<Integer> polyA = new ArrayList<>();
   polyA.add(0);
   polyA.add(5);
   polyA.add(3);
   polyA.add(4);
   System.out.println("다항식 A : " + toPoly(polyA));
   
   // 다항식 B 만들기
   ArrayList<Integer> polyB = new ArrayList<>();
      polyB.add(1);
      polyB.add(2);
      polyB.add(0);
      polyB.add(1);
      polyB.add(3);
      System.out.println("다항식 B : " + toPoly(polyB));
      ArrayList<Integer> polyC = AddPolynomial(polyA, polyB); // 다항식 합
      System.out.println("다항식 A+B : " + toPoly(polyC));
   }
   static ArrayList<Integer> AddPolynomial(ArrayList<Integer> A, ArrayList<Integer> B) {
      int i ;
      int min = (A.size()<B.size())?A.size():B.size(); // 더 작은 차수 확인
      ArrayList<Integer> C = new ArrayList<>(); // 다항식 C 생성
      for(i=0; i<min; i++) { // 동일 차수끼리 계수 합
      C.add(A.get(i) + B.get(i));
   }
   // 남은 항을 다항식 C에 복사
   if(A.size()<B.size())
   for(i=min; i<B.size(); i++)
      C.add(B.get(i));
      else
      for(i=min; i<A.size(); i++)
      C.add(A.get(i));
    return C;
   }
   static String toPoly(ArrayList<Integer> list) { // ArrayList를 다항식 형태의 문자열로 변환int i;
   StringBuffer s= new StringBuffer("[");
   for(i=list.size()-1; i>=0; i--){
      s.append(String.format("%dx^%d", list.get(i), i));
      if(i>0)
      s.append(", ");
   }
   s.append("]");
   return s.toString();
 }
}
// [실행결과]
// 다항식 A : [4x^3, 3x^2, 5x^1, 0x^0]
// 다항식 B : [3x^4, 1x^3, 0x^2, 2x^1, 1x^0]
// 다항식 A+B : [3x^4, 5x^3, 3x^2, 7x^1, 1x^0]

```

### <계수, 지수>쌍 표현법

<계수, 지수>쌍 표현법으로 다항식 A = 3x^3000 + 2x^2 + 5 을 표현하면 다음과 같다.

<figure><img src="../../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

```java
class Node {
int coef; // 다항식 항의 계수
int expo; // 다항식 항의 지수
    public Node(int coef, int expo) {
        this.coef = coef;
        this.expo = expo;
    }
    public String toString() { // 다항식 객체 문자열 변환
    return String.format("%dx^%d", coef, expo);
  }
}
```

두 다항식 A = 4x^3 + 3x^2 + 5x, B = 3x^4 + x^3 + 2x + 1 을 합하여 다항식 C를 만드는 방법은 두 다항식을 항을 따라 순회하면서 지수가 같은 동차항은 계수를 합하여 다항식 C에 포함시키고, 동차항이 없는 항은 그대로 다항식 C에 포함시키면 된다.

<figure><img src="../../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

다음은 <계수, 지수>쌍으로 표현된 두 다항식의 합을 자바 ArrayList 클래스를 활용하여 구현한 예제이다.

예제) PolynomialTest2.java

```java
import java.util.ArrayList;
class Node {
    int coef; // 다항식 항의 계수
    int expo; // 다항식 항의 지수
    public Node(int coef, int expo) {
    this.coef = coef;
    this.expo = expo;
    }
        public String toString() { // 다항식 객체 문자열 변환
        return String.format("%dx^%d", coef, expo);
    }
}
public class PolynomialTest2 {
    public static void main(String[] args) {
        // 다항식 A 만들기
        ArrayList<Node> polyA = new ArrayList<>();
        polyA.add(new Node(4, 3));
        polyA.add(new Node(3, 2));
        polyA.add(new Node(5, 1));
        System.out.println("다항식 A : " + polyA);
    
        // 다항식 B 만들기
        ArrayList<Node> polyB = new ArrayList<>();
        polyB.add(new Node(3, 4));
        polyB.add(new Node(1, 3));
        polyB.add(new Node(2, 1));
        polyB.add(new Node(1, 0));
        System.out.println("다항식 B : " + polyB);
        ArrayList<Node> polyC = AddPolynomial(polyA, polyB);
        System.out.println("다항식 A+B : " + polyC);
    }
    
    static ArrayList<Node> AddPolynomial(ArrayList<Node> A, ArrayList<Node> B) {
    int i = 0, j = 0;
    ArrayList<Node> C = new ArrayList<>();
    
    // A나 B 둘 중 하나가 모든 항에 대해 덧셈이 끝나는 경우 while 종료
    while (i < A.size() && j < B.size()) {
        // B의 지수가 A의 지수보다 큰 경우
        if (A.get(i).expo > B.get(j).expo) {
        C.add(new Node(A.get(i).coef, A.get(i).expo));
        ++i;
    }
    // A의 지수가 B의 지수보다 큰 경우
    else if (A.get(i).expo < B.get(j).expo) {
        C.add(new Node(B.get(j).coef, B.get(j).expo));
        ++j;
    }
    // A의 지수와 B의 지수가 같은 경우
    else {
        C.add(new Node(A.get(i).coef + B.get(j).coef, A.get(i).expo));
        ++i;
        ++j;
        }
    }
    // A에 항이 남아 있는 경우 C에 추가
    while (i < A.size()) {
        C.add(new Node(A.get(i).coef, A.get(i).expo));
        ++i;
    }
    // B에 항이 남아 잇는 경우 C에 추가
    while (j < B.size()) {
        C.add(new Node(B.get(j).coef, B.get(j).expo));
        ++j;
    }
    return C;
    }
}
// [실행결과]
// 다항식 A : [4x^3, 3x^2, 5x^1]
// 다항식 B : [3x^4, 1x^3, 2x^1, 1x^0]
// 다항식 A+B : [3x^4, 5x^3, 3x^2, 7x^1, 1x^0]
```



