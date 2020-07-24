Generic
===
Generic
---
* ```java
  List<string> strList = new ArrayList<>();
* 위와 같은코드에서 <> 안에 들어가는것은 타입인데 어떤 타입이 들어와도 같은식으로 코드를 처리하고 싶을때 사용하는것이 Generic이다. 여기서 ArrayList는 Generic타입으로 ArrayList<T>로 선언되어있다.
* 즉, 유연한 클래스 및 인터페이스를 설계하기 위함이다.
* Generic Type
  * 종류
    * E - Element
    * K - Key
    * N - Number
    * T - Type
    * V - Value
  * 인스턴스화 불가능
  * static불가능
* interface
  * 인터페이스로 구현시 다음과같이 한다.
  * ```java
    interface InterfaceSample<T1, T2> {
        T1 testMethod1(T2 t);
        T2 testMethod2(T1 t);
    }
  * ```java
    class TestSampleInerfaceImp implements InterfaceSample<String, Integer> {
        @Override
        public String testMethod1(Integer t) {
               return null;
        }
  
        @Override
        public Integer testMethod2(String t) {
               return null;
        }
    }
* Method
  * 메소드의 파라미터에 <T>가 선언되어 있다면, ReturnType 앞에 <T>를 선언함
  * ```java
    class TestMethod {
      public static <T> List<T> method(List<T> list, T item) {
         list.add(item);
         return list;
      }
    }
* Unbounded wildcard type - [@joongwon medium - 자세한 내용](https://medium.com/@joongwon/java-java%EC%9D%98-generics-604b562530b3)
  * 어떠한 타입이 들어와도 상관이 없다는것을 의미
  * List<?> 와 같이 <?>가 들어간것
  * ```java
    private static void printList(List<?> list) {
       for (Object elem: list)
       System.out.print(elem + " ");
    }
* Bounded type parameter - [@joongwon medium - 자세한 내용](https://medium.com/@joongwon/java-java%EC%9D%98-generics-604b562530b3)
  * 특정 타입만 들어올수있는것
  * ```java
    public class Box<T extends Number> {
       public void set(T value) {}
    }
  * Number(숫자)만 한정인데 여기서 만약에 Box<String> 으로 한다면 에러 발생한다.
  * 다음과 같이 클래스도 가능
    ```java
    class A {} 
    interface B {}
    interface C {}

    class D<T extends A & B & C> { // 다중상속은 불가하니 클래스는 하나만가능한지, 아니면 선언은 해놓고 하나만 가져다 쓰면 상관없는지는 잘모름
    }
* Recursive type bound - [@joongwon medium - 자세한 내용](https://medium.com/@joongwon/java-java%EC%9D%98-generics-604b562530b3)
  * 타입 매개변수가 자신을 포함하는 수식에 의해 한정될 수 있다.
    * 예를들어서 <T extends Comparable<T>> 에서 T가 Comparable<T>안에 T에 포함되는것이니 extends로 한정될수 있다는 뜻이다.
  * 가장 대표적인 예로서 비교를 하는 Comparable이 있다.
  * ```java
    // 불가능하다. 왜냐하면 T에 들어가는값중에 비교불가능한값이 들어갈수있기 때문이다.
    public static <T> int countGreaterThan(T[] anArray, T elem) {
      int count = 0;
      for (T e : anArray)
          if (e > elem)  // compiler error
              ++count;
      return count;
    }
    
    // 가능하다. 왜냐하면 T에 들어가는값이 Comparable(비교가능한값만 들어갈수있음)에 들어가는값만 들어가기 때문이다.
    public static <T extends Comparable<T>> int countGreaterThan(T[] anArray, T elem) {
      int count = 0;
      for (T e : anArray)
         if (e.compareTo(elem) > 0)
             ++count;
      return count;
    }
* Subtyping in generics - [@joongwon medium - 자세한 내용](https://medium.com/@joongwon/java-java%EC%9D%98-generics-604b562530b3)
  * ![](https://miro.medium.com/max/338/1*IDjS9R5JyV8whMXfvTMkYg.gif)
  * ```java
    // 불가능하다... 왜냐하면 boxTest에는 Box<Number>가 들어가는것이지 Box<Double>이 들어갈수없다. 딱봐도 안된다. 타입이 그냥 다른것이다.
    public void boxTest(Box<Number> n) { /* ... */ }
    
    boxTest(new Box<Double>());
    boxTest(new Box<Integer>());
* Wildcard and subtyping - [@joongwon medium - 자세한 내용](https://medium.com/@joongwon/java-java%EC%9D%98-generics-604b562530b3)
  * ```java
    // 불가능하다. 왜냐하면 List<Number>만 들어갈수 있기 때문이다.
    List<Integer> list = new ArrayList<>();
    addAll(list); // compile error

    public void addAll(List<Number> list) { /* ... */ }

    // 가능하다. addAll 매개변수로 List<> 안에 들어갈 매개변수가 Number안쪽으로 한정되어있고 Number는 Integer 상위타입이기 때문이다.
    // 지금 계속해서 반복적으로 말하고자 하는것은 List<Integer> 와 List<Number>는 아예 다른것이고 평등한 타입이고
    // 단순히 Integer와 Number는 상위, 하위타입으로 지정된 관계라는 사실이다.
    List<Integer> list1 = new ArrayList<>();
    List<Float> list2 = new ArrayList<>();
    addAll(list1);  // ok
    addAll(list2);  // ok

    public void addAll(List<? extends Number> list) { /* ... */ }
    
* Bounded wildcard type - [@joongwon medium - 자세한 내용](https://medium.com/@joongwon/java-java%EC%9D%98-generics-604b562530b3)
  * 종류
    * Upper bounded wildcard
      * ```java
        <? super T>
    * Lower bounded wildcard
      * ```java
        <? extends T>
* PECS - [@joongwon medium - 자세한 내용](https://medium.com/@joongwon/java-java%EC%9D%98-generics-604b562530b3)
  * ```java
    //  Producer-extends는 읽기만 가능하고 Consumer-super는 쓰기만 가능하다는 것이다.
    // 그럼 언제 어떤 상황에서 extends를 사용하고 super를 사용해야 할까? Oracle 문서에서는 In, Out 개념으로 가이드 하고 있다. 예를 들어 copy(src, dest)라는 메소드가 있다고 하자.
    // 여기서 src는 데이터를 복사할 데이터를 제공하므로(생산) In 인자가 되고 dest는 다른 곳에서 사용할 데이터를 받아들이므로(소비) Out 인자가 되므로 In의 경우 extends 키워드를 사용하고 Out의 경우는 super를 사용하라고 한다.
    public void doSomething(List<? extends MyClass> list) {
        for (MyClass e : list) { // Ok
            System.out.println(e);
        }
    }

    public void doSomething(List<? extends MyClass> list) {
        list.add(new MyClass()); // Compile Error
    }

    public void doSomething(List<? super MyClass> list) {
        for (MyClass e : list) { // Compile Error
            System.out.println(e);
        }
    }

    public void doSomething(List<? super MyClass> list) {
        list.add(new MyClass()); // Ok
    }

* Generic method
  * ```java
    //아래와 같은 형태의 메소드로 사용가능하다.
    public class Pair<K, V> {
      private K key;
      private V value;

      public Pair(K key, V value) {
         this.key = key;
         this.value = value;
      }

      public void setKey(K key) { this.key = key; }
      public void setValue(V value) { this.value = value; }
      public K getKey()   { return key; }
      public V getValue() { return value; }
    }

    public class Util {
        public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
            return p1.getKey().equals(p2.getKey()) &&
                    p1.getValue().equals(p2.getValue());
        }
    }
    
    
    public static void main(String... args) {
        Pair<Integer, String> p1 = new Pair<>(1, "apple");
        Pair<Integer, String> p2 = new Pair<>(2, "pear");
        boolean result = Util.<Integer, String>compare(p1, p2);
        boolean result = Util.compare(p1, p2); // type inference
    }
     