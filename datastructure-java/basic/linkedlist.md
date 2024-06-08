# LinkedList

## 목차

* 연결리스트 개념
* 연결리스트 구현
* 자바 LinkedList 활용
* 연결리스트 응용



## 연결리스트 개념

연결리스트(Linked List)는 데이터의 집합을 저장하기 위해 사용되는 자료구조로, 순차리스트 방식에서의 삽입, 삭제 연산 시간 및 저장 공간의 문제를 개선한 자료구조이다.

<figure><img src="../../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

원형 연결리스트는 단순 연결리스트에서 마지막 노드가 첫 번째 노드를 가리켜 원형 구조로 연결되어 있는 리스트이다.

<figure><img src="../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

원형 연결리스트에서는 현재 노드의 이전 노드를 탐색하려면 전체 리스트를 한 바퀴 순회해야 하는 불편함이 있다. 이 문제를 해결하기 위해 연결리스트를 양쪽 방향으로 순회할 수 있도록 만든 리스트가 이중 연결리스트이다.

<figure><img src="../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

### 순차리스트와 연결리스트 비교

아래는 순차리스트와 연결리스트에서 기본적인 연산들의 시간복잡도를 비교한 표이다.

<table data-full-width="true"><thead><tr><th width="350.3333333333333">작업(연산)</th><th>순차리스트</th><th>연결리스트</th></tr></thead><tbody><tr><td>이전 데이터/다음 데이터 찾기</td><td>O(1)</td><td>O(1)</td></tr><tr><td>맨 뒤에 데이터 추가/삭제하기</td><td>O(1)</td><td>O(1)</td></tr><tr><td>맨 뒤 이외의 위치에 데이터 추가/삭제하기</td><td>O(n)</td><td>O(1)</td></tr><tr><td>임의의 위치의 데이터 찾기</td><td>O(1)</td><td>O(n)</td></tr><tr><td>리스트 크기 구하기</td><td>O(n)</td><td>O(n)</td></tr></tbody></table>



## 연결리스트 구현

### 단순 연결리스트(Single LinkedList)

다음은 단순 연결리스트에서 사용하는 노드의 기본적인 구조이다.

```java
class Node {
   Object data;
   Node next;
   public Node(Object data){
         this.data = data;
         this.next = null;
   }
}
```

<figure><img src="../../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

### 단순 연결리스트에 데이터 삽입

단순 연결리스트에 데이터를 삽입하는 경우는 세 가지로 나눌 수 있다. 리스트 가장 처음에 삽입, 리스트 중간에 삽입, 리스트 가장 마지막에 삽입하는 경우이다.

1\) 리스트 처음에 삽입하기

<figure><img src="../../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

```javascript
// 단순 연결리스트 처음에 데이터 삽입
void insertFirstNode(String str) {
    Node node = new Node(str);
    if (head == null) {
        head = node;
    } else {
        Node current = head;
        node.next = current;
        head = node;
    }
}
```

2. 리스트 중간에 삽입하기

<figure><img src="../../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

```java
// 단순 연결리스트 중간에 데이터 삽입
void insertMiddleNode(Node pre, String str) {
    Node node = new Node(str);
    node.next = pre.next;
    pre.next = node;
}
```

3. 리스트 마지막에 삽입하기&#x20;

<figure><img src="../../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

```java
// 단순 연결리스트에 마지막에 데이터 삽입
void insertLastNode(String str) {
    Node node = new Node(str);
        if (head == null) {
        head = node;
    } else {
        Node current = head;
        while (current.next != null) {
            current = current.next;
        }
        current.next = node;
    }
}
```

4. 단순 연결리스트에서 데이터 삭제

삭제하는 경우 역시 세 가지가 있다. 첫 번째 노드 삭제, 중간 노드 삭제, 마지막 노드 삭제로 나눌 수 있다.

1\) 첫 번째 노드 삭제 Head 노드에 첫 번째 노드의 링크 필드값을 저장한다. 그러면 Head 노드는 두 번째 노드를 가리킨다. 그 다음 첫 번째 노드였던 '가' 노드를 삭제한다.

<figure><img src="../../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

2\) 중간 노드 삭제

'나' 노드를 삭제하고 싶다면, 삭제할 노드의 앞 노드('가' 노드) 링크 필드에 삭제하고자 하는 중간 노드의 링크 필드값을 저장한다. 중간 노드였던 '나' 노드를 삭제한다.

<figure><img src="../../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>

<pre class="language-java"><code class="lang-java">// 단순 연결리스트 중간 데이터 삭제
void deleteMiddleNode(String str) {
    if (head == null) {
        System.out.println("지울 노드가 존재하지 않습니다.");
        } else {
        Node prev = head;
        Node current = head.next;
        while ( !current.data.equals(str) ) {
            prev = current;
            current = current.next;
        }
<strong>        prev.next = current.next;
</strong>    }
}
</code></pre>

3\) 마지막 노드 삭제

마지막 노드의 앞 노드 링크 필드에 null 값을 저장한다. 마지막 노드였던 '라' 노드를 삭제한다.

<figure><img src="../../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

```java
// 단순 연결리스트 마지막 데이터 삭제
void deleteLastNode() {
    if (head == null) {
        System.out.println("지울 노드가 존재하지 않습니다.");
    } else {
        Node prev = head;
        Node current = head.next;
    while (current.next != null) {
        prev = current;
        current = current.next;
    }
        prev.next = null;
    }
}
```

다음은 단순 연결리스트를 자바로 구현하여 활용한 예제이다.

예제) SingleLinkedListTest.java

```java
class Node { // 노드 클래스 정의
    String data; // 데이터 필드
    Node next; // 링크 필드
    public Node() {
    this.data = null;
    this.next = null;
    }
    public Node(String data) {
        this.data = data;
        this.next = null;
    }
    public Node(String data, Node next) {
        this.data = data;
        this.next = next;
    }
}
class LinkedList { // 단순 연결리스트 클래스 정의
    private Node head; // 헤드 포인터(참조변수)
    
    public LinkedList() {
        this.head = null;
    }
     // 단순 연결리스트에 마지막에 데이터 삽입
    void insertLastNode(String str) {
        Node node = new Node(str);
        if (head == null) {
        head = node;
        } else {
            Node current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = node;
        }
    }
 // 단순 연결리스트 중간에 데이터 삽입
void insertMiddleNode(Node pre, String str) {
    Node node = new Node(str);
    node.next = pre.next;
    pre.next = node;
}
// 단순 연결리스트 처음에 데이터 삽입
void insertFirstNode(String str) {
    Node node = new Node(str);
    if (head == null) {
        head = node;
    } else {
        Node current = head;
        node.next = current;
        head = node;
    }
}
// 단순 연결리스트 마지막 데이터 삭제
void deleteLastNode() {
    if (head == null) {
        System.out.println("지울 노드가 존재하지 않습니다.");
    } else {
        Node prev = head;
        Node current = head.next;
        while (current.next != null) {
            prev = current;
            current = current.next;
        }
        prev.next = null;
    }
}
// 단순 연결리스트 중간 데이터 삭제
void deleteMiddleNode(String str) {
    Node node = new Node(str);
    if (head == null) {
        System.out.println("지울 노드가 존재하지 않습니다.");
    } else {
        Node prev = head;
        Node current = head.next;
        while (current.data != node.data) {
            prev = current;
            current = current.next;
        }
        prev.next = current.next;
    }
}
// 단순 연결리스트 처음 데이터 삭제
void deleteFirstNode() {
    if (head == null) {
        System.out.println("지울 노드가 존재하지 않습니다.");
    } else {
        head = head.next;
    }
}
// 단순 연결리스트에서 데이터 검색
Node searchNode(String str) {
    Node node = new Node(str);
    if (head == null) {
        System.out.println("노드가 비어있습니다.");
        return null;
    } else {
        Node current = head;
        while (current.data != node.data) {
            current = current.next;
        }
        return current;
    }
}
// 단순 연결리스트 전체 데이터 출력
void printList() {
    if (head == null) {
        System.out.println("출력할 리스트가 존재하지 않습니다.");
    } else {
        Node current = head;
        System.out.print("[");
        while (current.next != null) {
            System.out.print(current.data + " ");
            current = current.next;
        }
        System.out.print(current.data);
        System.out.print("]");
        System.out.println();
        }
    }
}
public class SingleLinkedListTest { // 단순 연결리스트 테스트 코드
    public static void main(String[] args) {
        LinkedList list = new LinkedList(); // 연결리스트 객체 생성
        System.out.println("(1) 공백 리스트에 노드 5개 삽입하기");
        list.insertLastNode("월");
        list.insertLastNode("화");
        list.insertLastNode("수");
        list.insertLastNode("목");
        list.insertLastNode("토");
        list.printList();
        System.out.println("(2) \"목\"노드 뒤에 \"금\" 노드 삽입하기");
        Node pre = list.searchNode("목");
        if (pre == null) {
        System.out.println("검색 실패 >> 찾는 데이터가 없습니다.");
        } else {
        list.insertMiddleNode(pre, "금");
        }
        list.printList();
        System.out.println("(3) 리스트의 첫번째에 노드 추가하기");
        list.insertFirstNode("일");
        list.printList();
        System.out.println("(4) 리스트의 마지막 노드 삭제하기");
        list.deleteLastNode();
        list.printList();
        System.out.println("(5) 리스트의 중간 노드 \"수\" 삭제하기");
        list.deleteMiddleNode("수");
        list.printList();
        System.out.println("(6) 리스트의 첫번째 노드 삭제하기");
        list.deleteFirstNode();
        list.printList();
    }
}
// [실행결과]
// (1) 공백 리스트에 노드 5개 삽입하기
// [월 화 수 목 토]
// (2) "목"노드 뒤에 "금" 노드 삽입하기
// [월 화 수 목 금 토]
// (3) 리스트의 첫번째에 노드 추가하기
// [일 월 화 수 목 금 토]
// (4) 리스트의 마지막 노드 삭제하기
// [일 월 화 수 목 금]
// (5) 리스트의 중간 노드 "수" 삭제하기
// [일 월 화 목 금]
// (6) 리스트의 첫번째 노드 삭제하기
// [월 화 목 금]
```

### 이중 연결리스트(Double LinkedList)

```java
class Node {
   Node prev;
 Object data;
   Node next;
   public Node(Object data){     
         this.prev = null;
         this.data = data;
         this.next = null;        
   }
}
```

<figure><img src="../../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

## 자바 LinkedList 활용

LinkedList list = new LinkedList<>(); // 연결리스트에 정수 객체를 저장할 경우

LinkedList list = new LinkedList<>(); // 연결리스트에 String 객체를 저장할 경우

ArrayList와 LinkedList는 둘 다 List 인터페이스를 구현하고 있어 사용법이 유사하나, 내부적으로 사용하는 자료구조에 차이가 있습니다. ArrayList는 배열(순차리스트)을 내부적으로 사용하여 임의의 위치에 있는 원소에 빠르게 접근할 수 있지만, 데이터 삽입 또는 삭제 시에는 해당 위치 이후의 모든 원소를 이동시켜야 하는 오버헤드가 발생한다. 반면에 LinkedList는 각 노드가 다음 노드를 가리키는 구조로 데이터 삽입 또는 삭제가 빠르지만, 임의의 위치에 있는 원소에 접근할 때는 상대적으로 느리다. 따라서, 데이터 삽입 또는 삭제가 빈번한 경우에는 LinkedList를 사용하는 것이 효율적이며, 데이터 추가 또는 임의 접근이 빈번한 경우에는 ArrayList를 사용하는 것이 성능상 유리하다.

다음은 자바 LinkedList 클래스를 활용한 예제이다.

예제) LinkedListTest.java

```java
import java.util.LinkedList;

public class LinkedListTest {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<String>(); // 자바 LinkedList 객체 생성list.add("포도"); // 리스트에 데이터 추가
        
        list.add("딸기");
        list.add("복숭아");
        list.add(2, "키위"); // 리스트에 데이터 삽입
        list.set(0, "오렌지"); // 리스트 데이터 변경
        
        System.out.println("삭제 전 : " + list);
        
        list.remove(1); // 리스트에서 데이터 삭제
        list.remove("키위");
        
        System.out.println("삭제 후 : " + list); // 리스트 전체 데이터 출력
        int num = list.size(); // 리스트 전체 데이터 개수
        
        System.out.print("역순 출력 : ");
        for (int cnt =num-1; cnt >= 0; cnt--) {
            String str = list.get(cnt); // 특정 위치 데이터 검색
            System.out.print(str + " ");
        }
    }
}

// [실행결과]
// 삭제 전 : [오렌지, 딸기, 키위, 복숭아]
// 삭제 후 : [오렌지, 복숭아]
// 역순 출력 : 복숭아 오렌지 
```

## 연결리스트 응용

이번 절에서는 연결리스트 응용으로 자바 LinkedList를 이용하여 다항식을 표현하는 방법에 대해 알아보자.

다항식은 항을 표현하기 위해 계수(coef), 지수(expo), 다음 노드를 가리키는 참조 변수로 이루어져 있다. 자바에서 LinkedList를 활용하면 다음 노드를 가리키는 참조 변수는 자동으로 생성되기 때문에, 다항식의 노드(Node) 클래스를 정의할 때에는 계수와 지수만 필드로 포함시키면 된다.

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

다음으로 자바 LinkedList로 표현된 두 다항식 A = 4x^3 + 3x^2 + 5x, B = 3x^4 + x^3 + 2x + 1 의 합을 계산하는 방법을 알아보자. 두 다항식 A, B를 합하여 다항식 C를 만드는 방법은 두 다항식을 항을 따라 순회하면서 지수가 같은 동차항은 계수를 합하여 다항식 C에 포함시키고, 동차항이 없는 항은 그대로 다항식 C에 포함시키면 된다.

<figure><img src="../../.gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>

다음은 자바 LinkedList 클래스로 표현된 두 다항식의 합을 구현한 예제이다.

예제) PolynomialTest3.java

<pre class="language-java"><code class="lang-java">import java.util.LinkedList;
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
public class PolynomialTest3 {
    public static void main(String[] args) {
        // 다항식 A 만들기
        LinkedList&#x3C;Node> polyA = new LinkedList&#x3C;>();
        polyA.add(new Node(4, 3));
        polyA.add(new Node(3, 2));
        polyA.add(new Node(5, 1));
        System.out.println("다항식 A : " + polyA);
        
        // 다항식 B 만들기
        LinkedList&#x3C;Node> polyB = new LinkedList&#x3C;>();
        polyB.add(new Node(3, 4));
        polyB.add(new Node(1, 3));
        polyB.add(new Node(2, 1));
        polyB.add(new Node(1, 0));
        System.out.println("다항식 B : " + polyB);
        LinkedList&#x3C;Node> polyC = AddPolynomial(polyA, polyB);
        System.out.println("다항식 A+B : " + polyC);
}
static LinkedList&#x3C;Node> AddPolynomial(LinkedList&#x3C;Node> A, LinkedList&#x3C;Node> B) {
    int i = 0, j = 0;
    
<strong>    LinkedList&#x3C;Node> C = new LinkedList&#x3C;>();
</strong><strong>    
</strong>    // A나 B 둘 중 하나가 모든 항에 대해 덧셈이 끝나는 경우 while 종료
    while (i &#x3C; A.size() &#x26;&#x26; j &#x3C; B.size()) {
        // B의 지수가 A의 지수보다 큰 경우
        if (A.get(i).expo > B.get(j).expo) {
            C.add(new Node(A.get(i).coef, A.get(i).expo));
            ++i;
        }
        
        // A의 지수가 B의 지수보다 큰 경우
        else if (A.get(i).expo &#x3C; B.get(j).expo) {
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
        while (i &#x3C; A.size()) {
            C.add(new Node(A.get(i).coef, A.get(i).expo));
            ++i;
        }
        
        // B에 항이 남아 잇는 경우 C에 추가
        while (j &#x3C; B.size()) {
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

</code></pre>





