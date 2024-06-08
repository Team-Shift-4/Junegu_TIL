# Queue

## 목차

* 큐 개념
* 큐 구현
* 자바 Queue 활용
* 데크(Deque) 활용



## 큐(Queue) 개념

<figure><img src="../../.gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>

큐는 대기 줄과 유사한 개념으로, 데이터의 제거는 항상 가장 앞에서 수행되고, 데이터의 삽입은 가장 뒤에서 수행되는 구조이다. 이러한 특성으로 큐는 선입선출(FIFO, First In First Out) 형태의 자료구조로 알려져 있다.



## 큐 구현

큐는 구현하는 방법에 따라 선형 큐, 원형 큐, 연결 큐로 구분된다.

### 선형 큐(Linear Queue)

선형 큐는 배열을 선형으로 사용하여 큐를 구현한다. 삽입을 반복하면 rear 위치가 계속 늘어나고, 삭제를 반복하면 front 위치가 계속 늘어난다. 선형 큐에서는 rear의 위치가 마지막 인덱스인지를 체크하여 포화상태를 검사한다. 만약 rear가 maxSize-1이면, 선형 큐(배열) 의 앞 공간이 비었음에도 포화상태로 인식하고 삽입 연산을 수행하지 않는다. 이러한 문제점으로 인해 선형 큐는 실무에서 사용되지 않는 경우가 있다.

<figure><img src="../../.gitbook/assets/image (156).png" alt=""><figcaption></figcaption></figure>

다음은 선형 큐를 자바로 구현한 예제이다.

예제) LinearQueueTest.java

```java
class LinearQueue { // 선형 큐 클래스 정의
    Integer data[];
    int front;
    int rear;
    int size;

    LinearQueue(int maxSize) { // 생성자, maxSize 크기의 배열 생성
        data = new Integer[maxSize];
        front = -1;
        rear = -1;
        size = maxSize;
    }

    public void enQueue(Integer elem) { // 큐 삽입 연산
        if (full()) {
            System.out.println("Queue is full");
            return;
        }
        rear = rear + 1;
        data[rear] = elem;
    }

    public Integer deQueue() { // 큐 삭제 연산
        if (empty()) {
            System.out.println("Queue is empty");
            return null;
        }
        front = front + 1;
        return data[front];
    }

    public Integer peek() { // 큐 조회 연산
        if (empty()) {
            System.out.println("Queue is empty");
            return null;
        }
        return data[front + 1];
    }

    public boolean empty() { // 공백상태 확인
        return front == rear;
    }

    public boolean full() { // 포화상태 확인
        return rear == size - 1;
    }
}

public class LinearQueueTest {
    public static void main(String[] args) {
        LinearQueue q = new LinearQueue(10); // 크기 10인 선형 큐 생성
        for (int i = 1; i <= 5; i++) // 큐에 삽입
            q.enQueue(i);
        System.out.print("배열큐 출력 : ");
        for (int i = 1; i <= 5; i++) // 큐에서 삭제
            System.out.print(q.deQueue() + " ");
    }
}
// [실행결과]
// 배열큐 출력 : 1 2 3 4 5 
```

### 원형 큐(Circular Queue) 구현

<figure><img src="../../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

다음은 원형 큐를 자바로 구현한 예제이다.

예제) CircularQueueTest.java

```java
class CircularQueue { // 원형 큐 클래스 정의
    private int front;
    private int rear;
    private int queueSize; // 큐 크기
    private int ItemArray[];

    public CircularQueue(int queueSize) { // 생성자, queueSize 크기의 배열 생성
        this.front = 0;
        this.rear = 0;
        this.queueSize = queueSize;
        ItemArray = new int[queueSize];
    }

    // 큐가 비어있는지 확인
    public boolean isEmpty() {
        return (front == rear);
    }

    // 큐가 가득차 있는지 확인
    public boolean isFull() {
        return ((rear + 1) % this.queueSize == front);
    }

    // 큐의 삽입 연산
    public void enQueue(int item) {
        if (isFull()) {
            System.out.println("큐가 포화 상태");
        } else {
            rear = (rear + 1) % queueSize;
            ItemArray[rear] = item;
        }
    }

    // 큐의 삭제 후 반환 연산
    public int deQueue() {
        if (isEmpty()) {
            System.out.println("큐가 공백 상태");
            return 0;
        } else {
            front = (front + 1) % queueSize;
            return ItemArray[front];
        }
    }

    // 큐의 현재 front값 출력
    public int peek() {
        if (isEmpty()) {
            System.out.println("큐가 비어있음");
            return 0;
        } else {
            return ItemArray[(front + 1) % queueSize];
        }
    }

    // 전체 큐값 출력
    public void print() {
        if (isEmpty()) {
            System.out.println("큐가 비어있음");
        } else {
            int f = front;
            while (f != rear) {
                f = (f + 1) % queueSize;
                System.out.print(ItemArray[f] + " ");
            }
            System.out.println();
        }
    }
}

public class CircularQueueTest {
    public static void main(String[] args) {
        CircularQueue q = new CircularQueue(10); // 원형 큐 객체 생성
        for (int i = 1; i <= 5; i++) // 큐에 삽입
            q.enQueue(i);
        System.out.print("원형큐 출력 : ");
        for (int i = 1; i <= 5; i++) // 큐에서 삭제
            System.out.print(q.deQueue() + " ");
    }
}

// [실행결과]
// 원형큐 출력 : 1 2 3 4 5
```

<figure><img src="../../.gitbook/assets/image (158).png" alt=""><figcaption></figcaption></figure>

### 연결 큐(Linked Queue) 구현

<figure><img src="../../.gitbook/assets/image (159).png" alt=""><figcaption></figcaption></figure>

다음은 연결 큐를 자바로 구현한 예제이다

예제) LinkedQueueTest.java

```java
class LinkedQueue {

    private class Node { // 연결큐를 구성하는 노드 객체의 타입

        // 노드는 data와 다음 노드를 가진다.
        private int data;
        private Node nextNode;

        Node(int data) {
            this.data = data;
            this.nextNode = null;
        }
    }

    // 큐는 front노드와 rear노드를 가진다.
    private Node front;
    private Node rear;

    public LinkedQueue() {
        this.front = null;
        this.rear = null;
    }

    // 큐가 비어있는지 확인
    public boolean empty() {
        return (front == null);
    }

    // item을 큐의 rear에 넣는다.
    public void enQueue(int item) {
        Node node = new Node(item);
        node.nextNode = null;

        if (empty()) {
            rear = node;
            front = node;
        } else {
            rear.nextNode = node;
            rear = node;
        }
    }

    // front의 데이터를 반환한다.
    public int peek() {
        if (empty()) throw new ArrayIndexOutOfBoundsException();
        return front.data;
    }

    // front를 큐에서 제거한다.
    public int deQueue() {
        int item = peek();
        front = front.nextNode;

        if (front == null) {
            rear = null;
        }
        return item;
    }
}

public class LinkedQueueTest {
    public static void main(String[] args) {
        LinkedQueue q = new LinkedQueue(); // 연결큐 객체 생성
        for (int i = 1; i <= 5; i++) // 큐에 삽입
            q.enQueue(i);
        System.out.print("연결큐 출력 : ");
        for (int i = 1; i <= 5; i++) // 큐에서 삭제
            System.out.print(q.deQueue() + " ");
    }
}

// [실행결과]
// 연결큐 출력 : 1 2 3 4 5
```

## 자바 Queue 활용

자바 API에는 큐와 관련 클래스로 Queue 인터페이스와 Queue 인터페이스를 구현하는 LinkedList 클래스가 있다.

먼저, Queue 인터페이스에 선언된 주요 메소드를 살펴보자.

* boolean offer(E e),
* boolean add(E e)&#x20;
* E poll(),
* E remove()&#x20;
* E peek()

Queue queue = new LinkedList<>();

예제) QueueTest1.java

```java
import java.util.*;

public class QueueTest1 {
    public static void main(String args[]) {
        Queue<String> queue = new LinkedList<>(); // 큐로 사용할 LinkedList 객체 생성
        queue.offer("one"); // 큐에 데이터 추가
        queue.offer("two");
        queue.offer("three");

        System.out.print("자바 큐 출력 : ");
        while (!queue.isEmpty()) {
            String str = queue.poll(); // 큐의 데이터를 가져와 출력
            System.out.print(str + " ");
        }
    }
}

// [실행결과]
// 자바 큐 출력 : one two three 
```

## 데크(Deque) 활용

데크(Deque)는 "double-ended queue"의 줄임말로, 양쪽 끝에서 데이터를 삽입하거나 삭제할 수 있는 자료구조이다. 일반적인 큐와는 달리 데크는 양쪽 방향에서의 삽입과 삭제가 가능하며, 리스트와 유사하지만 중간에 데이터를 삽입 또는 삭제할 수 없는 특성이 있다. 이로써 데크는 큐와 스택의 특성을 모두 갖고 있어 다양한 활용이 가능하다.

<figure><img src="../../.gitbook/assets/image (160).png" alt=""><figcaption></figcaption></figure>

Deque dq = new LinkedList<>();

자바 언어에서 큐와 관련된 클래스는 Queue 인터페이스를 구현하는 LinkedList 클래스로 제공되며, 이를 통해 큐 자료구조를 활용할 수 있다.

Deque dq = new ArrayDeque<>();

다음은 자바 LinkedList 클래스를 데크 자료구조로 활용한 예제이다.

예제) DequeTest.java

```java
import java.util.*;

public class DequeTest {
    public static void main(String args[]) {
        Deque<String> deque = new LinkedList<>(); // 데크 객체 생성
        deque.add("Java"); // 뒤에 추가 : add() == addLast() == offerLast() = offer()
        deque.addFirst("jQuery"); // 앞에 추가 : addFirst() == offerFirst()
        deque.addLast("HTML5"); // 뒤에 추가
        deque.offer("AngualarJS");
        deque.offerFirst("NodeJS");
        deque.offerLast("Javascript");

        System.out.println("데크 원소 : " + deque); // 데크 출력, deque.toString() 호출
        System.out.println("삭제전 첫번째 원소 : " + deque.getFirst());
        deque.removeFirst();// 앞에서 삭제 : removeFirst()==pollFirst()==poll()==remove()
        System.out.println("삭제후 첫번째 원소 : " + deque.peekFirst());
        System.out.println("삭제전 마지막 원소 : " + deque.getLast());
        deque.removeLast(); // 뒤에서 삭제 : removeLast()==pollLast()
        System.out.println("삭제후 마지막 원소 : " + deque.peekLast());
    }
}

// [실행결과]
// 데크 원소 : [NodeJS, jQuery, Java, HTML5, AngualarJS, Javascript]
// 삭제전 첫번째 원소 : NodeJS
// 삭제후 첫번째 원소 : jQuery
// 삭제전 마지막 원소 : Javascript
// 삭제후 마지막 원소 : AngualarJS
```





