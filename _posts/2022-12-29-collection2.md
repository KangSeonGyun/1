---
title: Collection 2
tags: java
---

Link: [Collection][id]

[id]: https://ksg0000.github.io/2022/12/29/collection.html

##### Data Structure - 자료구조, 일련의 일정 타입들의 데이터 모임 또는 관계를 나타낸 구성체

자료구조와 알고리즘은 서로 뗄 수 없는 의존적인 관계이다.   
알고리즘 문제를 풀기 위해 문제를 해석한 다음 보통 자료구조를 선택한다.   
자료구조를 선택하면 해당 자료구조에 맞는 알고리즘을 선택하는데 보다 더 효율적인 알고리즘을 선택할 수 있다는 장점이 있다.   
(물론 선후 관계가 바뀔 수도 있다.)

자료구조는 프로그래밍의 가장 기본이기 때문에 많은 언어들에서도 표준 라이브러리로 다양한 자료구조를 지원하고 있다.   
자바의 대표적인 자료구조인 Collection을 배우고 이와 관련한 자료구조들을 직접 구현해보자

##### 자료구조의 분류 / 형태에 따른 자료구조 분류

자료구조 분류법은 많은 분류법이 있지만, 대표적으로 많이 분류되는 방법으로   
선형 자료구조(Linear Data Structure)와 비선형 자료구조(Nonlinear Data Structure)로 나눌 수도 있다.

선형 자료구조(Linear Data Structure)   
데이터가 일렬로 연결된 형태라고 보면 된다. 우리가 흔히 쓰는 int[] 배열같은 것이라 생각하면 쉽다.   
선형 자료구조는 대표적으로 리스트(List)와 큐(Queue), 덱(Deque)이 있다.

비선형 자료구조(Nonlinear Data Structure)   
일렬로 나열된 것이 아닌, 각 요소가 여러 개의 요소와 연결 된 형태를 생각하면 된다. 쉽게 생각해서 거미줄 같다고 보면된다.   
대표적인 비선형 자료구조는 그래프(Graph)와 트리(Tree)가 있다.   

두 가지 분류에 해당되지 않는 자료구조 집합(Set)   
기타 자료구조 또는 집합 자료구조로 본다 / 집합의 경우는 데이터가 연결 된 형식이 아니다.   
집합(set)은 table에 가까운 자료구조   

그리고 파일 자료구조도 있는데, 이 것은 필자가 설명하려는 방향과는 조금 다르므로 별다른 설명은 하지 않겠다.   
파일구조는 순차파일, 색인파일, 직접파일이 있다는 정도로만 알아두자.   

##### Java Collections FrameWork

Java - Java 언어를 의미   
Collections - 어떤 일정한 부류의 것을 수집하여 한 공간에 모아놓은 것   
Framework - 어떤 문제를 해결하기 위한 구조의 뼈대가 되는 기본 구조를 의미   
일정 타입의 데이터들이 모여 쉽게 가공 할 수 있도록 지원하는 자료구조들의 뼈대(기본 구조)

기본 구조라고 한다면 딱 떠올라야 할 것이 바로 Interface(인터페이스)다. 인터페이스 자체가 기본 뼈대(추상 구조)만 있다.   
자바에서 제공하는 Collection은 크게 3가지 인터페이스로 List(리스트), Queue(큐), Set(집합)으로 나뉘어 있다.   
앞서 설명한 '형태에 따른 자료구조'라고 보면 된다. 그리고 각 분야별로 '구현' 된 것들이 있다.

<img src="/assets/images/Java_Collection.png" width="100%" height="100%" title="참고 이미지" alt="이미지" />

점선은 구현 관계이고, 실선은 확장 관계다. (인터페이스끼리는 다중 상속이 가능하다) 또한 Collection을 구현한 클래스 및 인터페이스들은 모두 java.util 패키지에 있다.

List, Queue, Set 이 3가지의 형태에 따른 자료구조들이 있고 Queue와 Set에는 조금 더 구체화 되어 Deque과 SortedSet이라는 형태에 따른 자료구조가 있는 것이다. 
그리고 이 형태에 따른 자료구조들은 각각 '구현'이 되어 class로 제공된다. 바로 녹색 부분이 '구현된 자료구조'라고 보면 된다.

##### Iterable은 뭘까?

Iteration 이라는 단어가 '반복', '되풀이'를 의미한다. Iterable 이라 한다면 '반복 가능한' 이라는 정도로 이해하면 될 것이다.   
저기서 제공하고 있는 class 들은 모두 객체형태로 내부 구현 또한 대개 Object[] 배열 형태가 아니라 각각의 객체를 갖고 움직인다. 그래서 객체의 데이터들을 모두 순회하면서 출력하려면 사용자들이 각각의 데이터 순회 방법을 알거나 하나씩 get() 같은 메소드를 통해 데이터를 하나씩 꺼내와야 한다.

하지만 Iterable 인터페이스를 보면 알겠지만 for-each메소드를 제공한다. 즉, Iterable 인터페이스를 쓰는 모든 클래스들은 기본적으로 for-each 문법을 쉽게 사용할 수 있다. 한마디로 반복자로 구현되어 나오게 하는 것이다.

##### Map은 Collection인가?

이는 관점 차이이긴 한데, 일단 Java에서는 Collection 이라고 보지 않는다.   
매핑(mapping)이 collections 이라고 보기 힘들고, collections 또한 mapping이라고 보기 힘들다는 것. 그렇기 때문에 의도적으로 Collections Interface를 상속하지 않았다는 뜻이다.

그리고 Collection Interface과 Map Interface의 호환성 문제도 있다.   
첫 번째로 Collection Interface를 보면 이를 상속하여 구현된 클래스들은 모두 단일 데이터를 처리하지만, Map은 키(key)와 데이터(value)가 쌍을 이루며 처리하기 때문에 굳이 Map을 위해 Collection에서 Map과 관련한 메소드를 만들고 Map에서 Collection을 상속할 필요가 없다.

두 번째로는 Iterable Interface와 Map 간의 문제도 있다. 아까 잠깐 언급했듯이 Iterable은 반복가능한 형태를 의미한다고 했다. 그런데 Map은 구조상 키(key)에 대응되는 값(value)이라는 특징을 갖고 있는데, 반복자로 뱉기 위해서는 key와 value 중 어느 것으로 반복할 것인가? 즉, Iterable Interface의 iterator() 을 구현함에도 문제가 발생할 수 밖에 없다. (그래서 내부적으로 key 와 value의 별도 인터페이스도 있다.)

마지막으로 Java에서는 상속이라는 모델링 자체가 한가지 유형의 공통성을 모델링 한다는 것에 의미가 있는데 위 1번, 2번 이유 모두 상속과는 거리가 멀다.

***

##### List Interface(리스트 인터페이스)

대표적인 선형 자료구조로 주로 순서가 있는 데이터를 목록으로 이용할 수 있도록 만들어진 인터페이스다. 좀 더 쉽게 얘기하면 우리가 배열에서 쓸 때 int[] array = new int[10]; 처럼 쓴다. 하지만, 이 처럼 선언한 배열의 경우 10개의 공간 외에는 더이상 사용하지 못한다. 즉, array[13] = 32; 라고 해주더라도 할당된 크기(범위) 밖이기 때문에 IndexOutofBoundsException 라는 에러가 발생한다.

이러한 단점을 보완하여 List를 통해 구현된 클래스들은 '동적 크기'를 갖으며 배열처럼 사용할 수 있게 되어있다.

한마디로 배열의 기능 + 동적 크기 할당이 합쳐져 있다고 보면 된다.

**<List Interface를 구현하는 클래스>**
1. ArrayList   
2. LinkedList   
3. Vector (+ Vector를 상속받은 Stack)

**<List Interface에 선언된 대표적인 메소드>**
<img src="/assets/images/Java_List_Interface.png" width="100%" height="100%" title="참고 이미지" alt="이미지" />

Program.java
-------------

```java


```

출처 Link: [Java - 자바 컬렉션 프레임워크][id2]

[id2]: https://st-lab.tistory.com/142