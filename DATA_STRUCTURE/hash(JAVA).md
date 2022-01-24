# **hash**

+ ### **만들어지게 된 계기**

  
> ArrayList는 내부 인덱스를 이용하여 검색이 한번에 이루어지기 때문에 빠른 검색 속도를 보장하는 반면 데이터의 추가/삭제시 많은 데이터가 밀리거나 당겨지기 때문에 많은 시간이 소요된다.
  
> LinkedList는 추가/삭제시 인근 노드들의 참조값만 수정해 줌으로써 빠른 처리가 가능하지만 데이터를 검색할 경우 해당 노드를 찾기 위해 처음부터 순회 검색을 해야하기 때문에 데이터의 수가 많아질수록 효율이 떨어지는 구조이다.

+ ###  **HASH?**

> Hash는 내부적으로 배열을 사용하여 데이터를 저장하기 때문에 빠른 검색속도를 갖는다.
그리고 데이터 추가/삭제시 기존 데이터를 밀어내거나 당기는 작업이 필요없도록 특별한 알고리즘을 이용하여 데이터와 연관된 고유한 숫자를 만들어 낸 뒤 이를 인덱스로 사용한다.


>특정 데이터가 저장되는 인덱스를 그 데이터만의 고유한 위치로 정하여서 데이터 추가/삭제시 데이터의 이동이 없도록 만들어진 구조이다.

>Hash가 내부적으로 사용하는 배열을 Hash Table 이라고 하며 크기에 따라 성능차이가 날 수 있다

+ ### **HASH TABLE**
  
> key-value 에서 key를 테이블에 저장할 때 key값을 Hash Method를 이용해 계산을 수행한 후, 그 결과값을 배열의 인덱스로 사용하여 저장하는 방식이다. 여기서 key값을 계산하는 것이 Hash Method 이다.

+ ### **HASH METHOD**

> Hash는 특별한 알고리즘을 이용하여 데이터의 고유한 숫자값을 만들어 인덱스로 사용한다고 했다. 이 알고리즘을 구현한 메소드를 Hash Method라고 하며 Hash Method에 의해 반환된 데이터의 고유 숫자값을 Hash Code 라고한다.

>*자바에서는 Object 클래스의 hashCode() 메소드를 이용하여 모든 객체의 Hash Code를 쉽게 구할 수 있다.

>*Hash Method를 이용해서 데이터를 Hash Table에 저장하고 검색하는 기법을 Hashing이라 한다.

>Hash Method는 데이터가 저장되어 있는 곳을 알려주기 때문에 다량의 데이터 중에서도 원하는 데이터를 빠르게 찾을 수 있다.

##  HashTable 클래스
+ HashTable
```public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {

    public synchronized int size() { }

    @SuppressWarnings("unchecked")
    public synchronized V get(Object key) { }

    public synchronized V put(K key, V value) { }
}
```
Hashtable 클래스의 대부분의 API를 보면 위와 같이 메소드 전체에 synchronized 키워드가 존재하는 것을 볼 수 있습니다.(메소드 전체가 임계구역으로 설정 됩니다.) 그렇기 때문에 Multi-Thread 환경에서는 나쁘지 않을 수도? 있습니다

하지만 동시에 작업을 하려해도 객체마다 Lock을 하나씩 가지고 있기 때문에 동시에 여러 작업을 해야할 때 병목현상이 발생할 수 밖에 없습니다.(메소드에 접근하게 되면 다른 쓰레드는 Lock을 얻을 때까지 기다려야 하기 때문입니다.)

Hashtable 클래스는 Thread-safe 하다는 특징이 있긴 하지만, 위와 같은 특징 때문에 멀티쓰레드 환경에서 사용하기에도 살짝 느리다는 단점이 있습니다. 또한 Collection Framework가 나오기 이전부터 존재하는 클래스이기 때문에 최근에는 잘 사용하지 않는 클래스입니다. 


## HashMap 클래스
+ HashMap 

```public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {

    public V get(Object key) {}
    public V put(K key, V value) {}
}
```
HashMap 클래스를 보면 synchronized 키워드가 존재하지 않습니다. 그렇기 때문에 Map 인터페이스를 구현한 클래스 중에서 성능이 제일 좋다고 할 수 있습니다. 하지만 synchronized 키워드가 존재하지 않기 때문에 당연히 Multi-Thread 환경에서 사용할 수 없다는 특징을 가지고 있습니다.


멀티 쓰레드 환경이 아니라면 HashMap을 사용하기에 대체적으로 적합하겠지만, 멀티쓰레드의 환경이라면 HashMap 클래스도 Hashtable 클래스의 대안이 될 수는 없습니다.


## ConcurrentHashMap 클래스
+ Hashtable 클래스의 단점을 보완하면서 Multi-Thread 환경에서 사용할 수 있도록 나온 클래스가 바로 ConcurrentHashMap 입니다.(JDK 1.5에 검색과 업데이트시 동시성 성능을 높이기 위해서 나온 클래스 입니다.)

HashMap, Hashtable, ConcurrentHashMap 클래스 모두 Map의 기능적으로만 보면 큰 차이는 없습니다.

 

그러면 어떤 동기화 방식을 사용하고 어떤 특징이 있길래 Hashtable의 대안 클래스가 될 수 있을까요?

```public class ConcurrentHashMap<K,V> extends AbstractMap<K,V>
    implements ConcurrentMap<K,V>, Serializable {

    public V get(Object key) {}

    public boolean containsKey(Object key) { }

    public V put(K key, V value) {
        return putVal(key, value, false);
    }

    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;                   // no lock when adding to empty bin
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else {
                V oldVal = null;
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key,
                                                              value, null);
                                    break;
                                }
                            }
                        }
                        else if (f instanceof TreeBin) {
                            Node<K,V> p;
                            binCount = 2;
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                    }
                }
                if (binCount != 0) {
                    if (binCount >= TREEIFY_THRESHOLD)
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        addCount(1L, binCount);
        return null;
    }
}
```
위의 코드는 ConcurrentHashMap 클래스의 일부 API 입니다. ConcuurentHashMap에는 Hashtable 과는 다르게 synchronized 키워드가 메소드 전체에 붙어 있지 않습니다. get() 메소드에는 아예 synchronized가 존재하지 않고, put() 메소드에는 중간에 synchronized 키워드가 존재하는 것을 볼 수 있습니다.

 

이것을 좀 더 정리해보면 ConcurrentHashMap은 읽기 작업에는 여러 쓰레드가 동시에 읽을 수 있지만, 쓰기 작업에는 특정 세그먼트 or 버킷에 대한 Lock을 사용한다는 것입니다.

```
public class ConcurrentHashMap<K,V> extends AbstractMap<K,V>
    implements ConcurrentMap<K,V>, Serializable {

    private static final int DEFAULT_CAPACITY = 16;

    // 동시에 업데이트를 수행하는 쓰레드 수
    private static final int DEFAULT_CONCURRENCY_LEVEL = 16;
}
```
ConcurrentHashMap 클래스를 보면 위와 같이 DEFAULT_CAPACITY, DEFAULT_CONCURRENCY_LEVEL가 16으로 설정되어 있습니다.
DEFAULT_CAPACITY는 HashMap에서 보았듯이 버킷의 수 입니다. 그리고 DEFAULT_CONCURRENCY_LEVEL는 동시에 작업 가능한 쓰레드 수라고 생각합니다.

 

버킷의 수 == 동시작업 가능한 쓰레드 수인 이유는 위에서 말했던 것처럼 ConcurrentHashMap은 버킷 단위로 lock을 사용하기 때문에 같은 버킷만 아니라면 Lock을 기다릴 필요가 없다는 특징이 있습니다.(버킷당 하나의 Lock을 가지고 있다라고 생각하면 될 것 같습니다.)

 

즉, 여러 쓰레드에서 ConcurrentHashMap 객체에 동시에 데이터를 삽입, 참조하더라도 그 데이터가 다른 세그먼트에 위치하면 서로 락을 얻기 위해 경쟁하지 않습니다.
참고로 검색(get())에는 동기화가 적용되지 않으므로 업데이트 작업(put() or remove())과 겹칠 수 있습니다. 그래서 검색은 가장 최근에 완료된 업데이트 작업의 결과가 반영됩니다.

출처1: https://devlog-wjdrbs96.tistory.com/269 \
출처2: https://jroomstudio.tistory.com/10