---
title: Iterators
tags: java
---

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

        // 왜 콜렉션이 iterator통해서 next()를 하는가?
        
        // 멀티 스레드에서 동일한 콜렉션을 공유하게 되면 불안전한 조회가 될 수 있다.
        // 한 스레드에서 값을 읽으면 다른 스레드는 하나 증가된 인덱스를 읽게 된다.
        // 데이터는 공유하는 것이 맞지만 인덱스는 공유해선 안 된다

        // 반복할 인덱스를 각자 가져가게 하는 것이 iterator 

        System.out.println(set.iterator().next()); // 1
        System.out.println(set.iterator().next()); // 1
        System.out.println(set.iterator().next()); // 1
        System.out.println(set.iterator().next()); // 1

        for(Integer n : set)
        System.out.println(n);
        // 1 3 5 7 8

        // Iterator를 활용해서 나온 것이 foreach

        set.forEach((v) ->{
            System.out.println(v);
        });
        // 1 3 5 7 8
        // 펑셔널 프로그래밍

        set.forEach(new Consumer<Integer>() {
            public void accept(Integer value){
                System.out.println(value);
            }
        });
        // 1 3 5 7 8
    }
}
```