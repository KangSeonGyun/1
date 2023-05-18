---
title: Iterators
tags: java
---

왜 콜렉션이 iterator통해서 next()를 하는가?
        
멀티 스레드에서 동일한 콜렉션을 공유하게 되면 불안전한 조회가 될 수 있다.

1, 3, 5, 7, 8을 갖고있는 Set Collection과 2 개의 스레드가 있고 iterator를 사용하지 않으면 다음과 같은 상황이 생긴다..

첫 번째 스레드에서 값을 읽으면 1이 읽히고 index가 하나 증가하여 다음에 읽히는 값은 3일 것이다.

두 번째 스레드에서 Set을 읽으면 처음부터 1이 아닌 3이 읽힌다.

즉, 데이터(1, 3, 5, 7, 8)는 공유하는 것이 맞지만 인덱스는 공유해선 안 되며 반복할 인덱스를 각자 가져가게 하는 것이 iterator다.

```java
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;
import java.util.function.Consumer;

public class App {
    public static void main(String[] args) throws Exception {

        Set<Integer> set = new HashSet<>();

        set.add(1);
        set.add(3);
        set.add(5);
        set.add(7);
        set.add(8);

        Iterator<Integer> it = set.iterator();

        while(it.hasNext())
            System.out.println(it.next());
            // 1 3 5 7 8

        for(Integer n : set)
        System.out.println(n);
        // 1 3 5 7 8
    }
}
```

iterator를 사용하면 다음과 같이 인덱스를 공유하지 않는다.

```java
        System.out.println(set.iterator().next()); // 1
        System.out.println(set.iterator().next()); // 1
        System.out.println(set.iterator().next()); // 1
        System.out.println(set.iterator().next()); // 1
```

Iterator를 활용해서 나온 foreach

```java
        set.forEach((v) ->{
            System.out.println(v);
        });
        // 1 3 5 7 8
```

함수적 인터페이스(Functional Interface)입니다. 함수적 인터페이스는 단일 추상 메서드를 가진 인터페이스

set이라는 컬렉션의 각 요소를 순회하면서 Consumer<Integer> 인터페이스를 구현한 익명 클래스를 생성하고, 해당 익명 클래스의 accept 메서드를 호출하여 요소를 처리하는 기능을 수행합니다.

forEach() 메서드는 내부적으로 Set의 요소를 하나씩 가져와서 Consumer 객체의 accept() 메서드를 호출하며, 요소를 accept() 메서드의 매개변수로 전달합니다.

```java
        set.forEach(new Consumer<Integer>() {
            public void accept(Integer value){
                System.out.println(value);
            }
        });
        // 1 3 5 7 8
```
