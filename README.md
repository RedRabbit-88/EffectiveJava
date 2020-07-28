# 이펙티브 자바 정리

## 2장 객체 생성과 파괴

### 아이템 1. 생성자 대신 정적 팩터리 메서드를 고려하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/2%EC%9E%A5-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1%EA%B3%BC-%ED%8C%8C%EA%B4%B4#%EC%95%84%EC%9D%B4%ED%85%9C-1

### 아이템 2. 생성자에 매개변수가 많다면 빌더(Builder)를 고려하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/2%EC%9E%A5-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1%EA%B3%BC-%ED%8C%8C%EA%B4%B4#%EC%95%84%EC%9D%B4%ED%85%9C-2

### 아이템 3. private 생성자나 열거 타입으로 싱글턴임을 보증하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/2%EC%9E%A5-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1%EA%B3%BC-%ED%8C%8C%EA%B4%B4#%EC%95%84%EC%9D%B4%ED%85%9C-3

### 아이템 4. 인스턴스화를 막으려거든 private 생성자를 사용하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/2%EC%9E%A5-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1%EA%B3%BC-%ED%8C%8C%EA%B4%B4#%EC%95%84%EC%9D%B4%ED%85%9C-4

### 아이템 5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/2%EC%9E%A5-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1%EA%B3%BC-%ED%8C%8C%EA%B4%B4#%EC%95%84%EC%9D%B4%ED%85%9C-5

### 아이템 6. 불필요한 객체 생성을 피하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/2%EC%9E%A5-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1%EA%B3%BC-%ED%8C%8C%EA%B4%B4#%EC%95%84%EC%9D%B4%ED%85%9C-6

### 아이템 7. 다 쓴 객체 참조를 해제하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/2%EC%9E%A5-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1%EA%B3%BC-%ED%8C%8C%EA%B4%B4#%EC%95%84%EC%9D%B4%ED%85%9C-7

### 아이템 8. finalizer와 cleaner 사용을 피하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/2%EC%9E%A5-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1%EA%B3%BC-%ED%8C%8C%EA%B4%B4#%EC%95%84%EC%9D%B4%ED%85%9C-8

### 아이템 9. try-finally보다는 try-with-resources를 사용하라 (자바 7 이후)
https://github.com/RedRabbit-88/EffectiveJava/wiki/2%EC%9E%A5-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1%EA%B3%BC-%ED%8C%8C%EA%B4%B4#%EC%95%84%EC%9D%B4%ED%85%9C-9

## 3장 모든 객체의 공통 메서드

### 아이템 10. equals는 일반 규약을 지켜 재정의하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C#%EC%95%84%EC%9D%B4%ED%85%9C-10

### 아이템 11. equals를 재정의하려거든 hashCode도 재정의하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C#%EC%95%84%EC%9D%B4%ED%85%9C-11

### 아이템 12. toString을 항상 재정의하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C#%EC%95%84%EC%9D%B4%ED%85%9C-12

### 아이템 13. clone 재정의는 주의해서 진행하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C#%EC%95%84%EC%9D%B4%ED%85%9C-13

### 아이템 14. Comparable을 구현할지 고려하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C#%EC%95%84%EC%9D%B4%ED%85%9C-14

### 아이템 15. 클래스와 멤버의 접근 권한을 최소화하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C#%EC%95%84%EC%9D%B4%ED%85%9C-15

### 아이템 16. public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C#%EC%95%84%EC%9D%B4%ED%85%9C-16

### 아이템 17. 변경 가능성을 최소화하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C#%EC%95%84%EC%9D%B4%ED%85%9C-17

### 아이템 18. 상속보다는 컴포지션을 사용하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C#%EC%95%84%EC%9D%B4%ED%85%9C-18

### 아이템 19. 상속을 고려해 설계하고 문서화하라. 그러지 않았다면 상속을 금지하라
https://github.com/RedRabbit-88/EffectiveJava/wiki/3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C#%EC%95%84%EC%9D%B4%ED%85%9C-19

## 9장 일반적인 프로그래밍 원칙

### 아이템 57. 지역변수의 범위를 최소화하라

>**지역변수의 범위를 줄이는 기법**<br>
>가장 처음 쓰일 때 선언하기 (가장 좋음)
><br>메서드를 작게 유지하고 한 가지 기능에 집중하는 것

* C언어의 경우 지역변수를 코드 블록의 첫 머리에 선언함. 자바에서는 문장을 선언할 수 있는 곳이면 어디든 선언 가능.
* **거의 모든 지역변수는 선언과 동시에 초기화해야 한다**
  * try-catch문은 이 규칙에서 제외
  * 변수를 초기화하는 표현식에서 검사 예외를 던질 가능성이 있다면 try 블록 안에서 초기화
  <br>그렇지 않으면 예외가 블록을 넘어 메서드에까지 전파됨

* 가급적이면 while문 대신 for문을 사용 (반복변수의 값을 반복문이 종료된 후에도 사용해야하지 않다면)
  * 이유 : for문의 경우 반복 변수의 범위가 반복문의 몸체, 그리고 for 키워드와 몸체 사이의 괄호로 제한됨.
```java
for (int i = 0, n = expensiveComputatio(); i< n; i++) {
  // TO-DO
}
```


### 아이템 58. 전통적인 for문보다는 for-each문을 사용하라

```java
// 컬렉션 순회하기
for (Iterator<Element> i = c.iterator(); i.hasNext(); ) {
  Element e = i.next();
  // TO-DO
}

// 배열 순회하기
for (int i = 0; i < a.length; i++) {
  // TO-DO
}
```
* 위 관용구들은 오류가 생길 가능성이 높아진다.
  * 1회 반복에서 반복자는 3번, 인덱스는 4번 등장하여 변수를 잘못 사용할 가능성이 높아짐.
  * 컬렉션이냐 배열이냐에 따라 코드 형태도 달라짐
  * **for-each(Enhanced for statement)문을 사용하면 해결됨**

```java
for (Element e : elements) {
  // TO-DO
}
```
* "elements 안의 각 원소 e에 대해"로 읽음
* 반복대상이 컬렉션이든 배열이든 속도는 동일

```java
// 코드 58-4 버그 소스
// i.next(), j.next()를 같이 호출하므로 정상적으로 2중 반복문이 실행되지 않음.
enum Suit { CLUB, DIAMOND, HEART, SPADE }
enum Rank { ACE, DEUCE, THREE, FOUR, FIVE, SIX, SEVEN, EIGHT, NINE, TEN, JACK, QUEEN, KING }

static Collection<Suit> suits = Arrays.asList(Suit.values());
static Collection<Rank> ranks = Arrays.asList(Rank.values());

List<Card> deck = new ArrayList<>();
for (Iterator<Suit> i = suits.iterator(); i.hasNext(); )
  for (Iterator<Rank> j = ranks.iterator(); j.hasNext(); )
    deck.add(new Card(i.next(), j.next()));


// 코드 58-6 버그는 고쳤지만 보기 안 좋음
for (Iterator<Suit> i = suits.iterator(); i.hasNext(); )
  Suit suit = i.next(); // 처음 반복문의 객체를 저장
  for (Iterator<Rank> j = ranks.iterator(); j.hasNext(); )
    deck.add(new Card(suit, j.next()));
    
    
// 코드 58-7 컬렉션이나 배열의 중첩 반복을 위한 권장 관용구
for (Suit suit : suits)
  for (Rank rank : ranks)
    deck.add(new Card(suit, rank));
```

* **for-each문을 사용할 수 없는 3가지 경우**

  * **파과적인 필터핑 (Destructive filtering)**
  <br>컬렉션을 순회하면서 선택된 원소를 제거해야 하는 경우 반복자의 remove 메서드를 호출해야 함.
  <br>자바8 부터는 Collection의 removeIf 메서드를 사용해서 컬렉션을 명시적으로 순회하는 일을 회피할 수 있음.
  
  * **변형 (transforming)**
  <br>리스트나 배열을 순회하면서 그 원소의 값 일부 혹은 전체를 교체해야 한다면 리스트의 반복자나 배열의 인덱스를 사용해야 함.
  
  * **병렬 반복 (parallel iteration)**
  <br>여러 컬렉션을 병렬로 순회해야 한다면 각각의 반복자와 인덱스 변수를 사용해 엄격하고 명시적으로 제어해야 함.
<br><br>
### 아이템 59. 라이브러리를 익히고 사용하라

>**표준라이브러리를 사용할 때의 장점**
><br>핵심적인 일과 크게 관련 없는 문제를 해결하느라 시간을 허비하지 않아도 됨.
><br>따로 노력하지 않아도 성능이 지속해서 개선됨.
><br>기능이 점점 많아짐
><br>작성한 코드가 많은 사람에게 낯익은 코드가 됨.

```java
// 코드 59-1 흔하지만 문제가 심각한 코드
static Random rnd = new Random();

static int random(int n) {
  return Math.abs(rnd.nextInt()) % n;
}
```
* 3가지 문제점
  * n이 그리 크지 않은 2의 제곱수라면 얼마 지나지 않아 같은 수열이 반복됨.
  * n이 2의 제곱수가 아니라면 몇몇 숫자가 평균적으로 더 자주 반환됨. (n값이 크면 이 현상은 더 두드러짐.)
  * **지정한 범위 "바깥"의 수가 종종 튀어나올 수 있음**
  <br>`rnd.nextInt()`가 반환한 값을 `Math.abs`를 이용해 음수가 아닌 정수로 매핑하기 때문.

* 해결방안
  * 의사난수 생성기, 정수론, 의 보수 계산 등을 고려해서 해결
  * **`Random.nextInt(int)`를 사용!**
  
* 자바7부터는 `Random`은 가급적 사용하지 않는게 좋음
* `ThreadLocalRandom`으로 대체하면 대부분 잘 작동함.
* Fork-Join 풀이나 병렬 스트림에서는 `SplittableRandom`을 사용할 것

```java
// 코드 59-2 transferTo 메서드를 이용해 URL의 내용 가져오기 (자바9부터 가능)
public static void main(String[] args) throws IOException {
  try (InputStream in = new URL(args[0].openStream()) {
    in.transferTo(System.out);
  }
}
```

* **자바 프로그래머가 꼭 알아둬야 할 라이브러리**
  * `java.lang`, `java.util`, `java.io`와 그 하위 패키지들
  * 컬렉션 프레임워크, 스트림 라이브러리 (아이템 45 ~ 48)
  * `java.util.concurrent`의 동시성 기능
<br><br>
### 아이템 60. 정확한 답이 필요하다면 float와 double은 피하라

* float와 double 타입은 과학/공학 계산용으로 넓은 범위의 수를 빠르게 정밀한 **"근사치"**로 계산하도록 설계됨.
* 특히 금융 관련 계산과는 맞지 않음.
```java
// 코드 60-1 오류 발생! 금융 계산에 부동소수 타입을 사용했다.
// 결과값이 0.39999999999 형식으로 출력됨
public static void main(String[] args) {
  double funds = 1.00;
  int itemsBought = 0;
  for (double price = 0.10; funds >= price; price += 0.10) {
    funds -= price;
    itemsBought++;
  }
  System.out.println(itemsBought + "개 구입");
  System.out.println("잔돈(달러):" + funds);
}
```
* **금융 계산에는 `BigDecimal`, `int` 혹은 `long`을 사용해야 한다.**
  * `BigDecimal`의 경우 기본 타입보다 쓰기가 훨씬 불편하고 느림.
  * 기본형의 경우 다룰 수 있는 값의 크기가 제한되고 소수점을 관리해야 함.
  * 9자리 10진수는 `int`, 18자리 10진수는 `long`, 그 이상은 `BigDecimal`을 사용
```java
// 코드 60-2 BigDecimal을 사용한 해법. 속도가 느리고 쓰기 불편
public static void main(String[] args) {
  final BigDecimal TEN_CENTS = new BigDecimal(".10");
  
  int itemsBought = 0;
  BigDecimal funds = new BigDecimal("1.00");
  for (BigDecimal price = TEN_CENTS; funds.compareTo(price) >= 0; price = price.add(TEN_CENTS)) {
    funds = funds.substract(price);
    itemsBought++;
  }
  System.out.println(itemsBought + "개 구입");
  System.out.println("잔돈(달러):" + funds);
}


// 코드 60-3 정수 타입을 사용한 해법
public static void main(String[] args) {
  int funds = 100;
  int itemsBought = 0;
  for (double price = 10; funds >= price; price += 10) {
    funds -= price;
    itemsBought++;
  }
  System.out.println(itemsBought + "개 구입");
  System.out.println("잔돈(센트):" + funds);
}
```
<br><br>
### 아이템 61. 박싱(Boxing)된 기본 타입보다는 기본 타입을 사용하라

* 기본 타입과 박싱된 기본 타입의 차이점

|구분|기본 타입|박싱된 기본타입|
|--|--|--|
|구성|값|값과 식별성(identity) 속성|
|값의 속성|언제나 유효|null을 포함할 수 있음|
|시간/메모리<br>효율성|더 효율적|-|

```java
// 코드 61-1 잘못 구현된 비교자
Comparator<Integer> naturalOrder = (i, j) -> (i < j) ? -1 : (i == j ? 0 : 1);

// naturalOrder.compare(new Integer(42), new Integer(42)) 의 결과는 1을 출력 (실제로는 0을 출력해야 함)
// 이유 : ==로 비교 시 두 "객체 참조"의 식별성을 검사함.
// 2개의 Integer가 다른 인스턴스이기 때문에 결과값이 false임.

// 코드 61-2 문제를 수정한 비교자
Comparator<Integer> naturalOrder = (iBoxed, jBoxed) -> {
  int i = iBoxed, j = jBoxed; // 오토박싱
  return (i < j) ? -1 : (i == j ? 0 : 1);
}
```
* **박싱된 기본 타입에 == 연산자를 사용하면 오류가 발생**

```java
// 코드 61-3 기이하게 동작하는 프로그램
public class Unbelievable {
  static Integer i;
  
  public static void main(String[] args) {
    if (i == 42)
      System.out.println("Unbelievable!");
  }
}

// NullPointerException이 발생함
// 이유 : Integer i를 생성하지 않음. 기본값은 null
```
* 기본 타입과 박싱된 기본 타입을 혼용한 연산에서는 박싱된 기본 타입의 박싱이 자동으로 풀린다.
  * `(i == 42)`에서 `Integer i`가 `int i`로 자동으로 언박싱되며 null을 언박싱하게 됨.

* 박싱된 기본 타입을 쓰는 경우
  * 컬렉션의 원소, 키, 값으로 쓰임.
  <br>컬렉션은 기본 타입을 지원하지 않으므로 박싱된 기본 타입을 사용하게 됨
  * 리플렉션(아이템 65)을 통해 메서드를 호출할 때도 박싱된 기본 타입을 사용해야 함
<br><br>
### 아이템 62. 다른 타입이 적절하다면 문자열 사용을 피하라

* 문자열은 다른 값 타입을 대신하기에 적합하지 않다.
* 문자열은 열거 타입을 대신하기에 적합하지 않다.
* 문자열은 혼합 타입을 대신하기에 적합하지 않다.
```java
// 코드 62-1 혼합 타입을 문자열로 처리한 부적절한 예
// 혹시라도 문자열에 구분자 "#"가 사용되었다면 부적절한 결과 초래
String compoundkey = className + "#" + i.next();

// 이런 경우는 보통 private 정적 멤버 클래스로 선언(아이템 24)
```
* 문자열은 권한(capacity)을 표현하기에 적합하지 않다.
```java
// 코드 62-2 잘못된 예 - 문자열을 사용해 권한을 구분
// 문제점 스레드 구분용 문자열 키가 전역 이름공간에서 공유되어 유일성 보장 못함.
public class ThreadLocal {
  private ThreadLocal() { } // 객체 생성 불가
  
  // 현 스레드의 값을 키로 구분해 저장
  public static void set(String key, Object value);
  
  // (키가 가리키는) 현 스레드의 값을 반환
  public static Object get(String key);
}


// 코드 62-3 Key 클래스로 권한을 구분
public class ThreadLocal {
  private ThreadLocal() { } // 객체 생성 불가
  
  public static class Key { // 권한
    Key() { }
  }
  
  // 위조 불가능한 고유 키를 생성한다.
  public static Key getKey() {
    return new Key();
  }
  
  public static void set(Key key, Object value);
  public static Object get(Key key);
}


// 코드 62-4 리팩터링하여 Key를 ThreadLocal로 변경
public final class ThreadLocal {
  public ThreadLocal();
  public void set(Object value);
  public Object get();
}


// 코드 62-5 매개변수화하여 타입안전성 확보
public final class ThreadLocal<T> {
  public ThreadLocal();
  public void set(T value);
  public T get();
}
```



### 아이템 63. 문자열 연결은 느리니 주의하라

* 문자열 연결 연산자로 문자열 n개를 잇는 시간은 n2에 비례한다
<br>문자열은 불변이라서 두 문자열을 연결할 경우 양쪽의 내용을 모두 복사해야 함.
<br>-> 성능저하 초래
* **성능을 포기하고 싶지 않다면 `String`대신 `StringBuilder`를 사용하자**



### 아이템 64. 객체는 인터페이스를 사용해 참조하라

* 적당한 인터페이스만 있다면 매개변수뿐 아니라 반환값, 변수, 필드를 전부 인퍼체이스 타입으로 선언하라.
* 객체의 실제 클래스를 사용해야 할 상황은 "오직" 생성자로 생성할 때 뿐
* 인터페이스를 타입으로 사용하는 습관을 길러두면 프로그램이 훨씬 유연해짐
* 적합한 인터페이스가 없다면 당연히 클래스로 참조
  * String, BigInteger 같은 값 클래스
  * 클래스 기반으로 작성된 프레임워크가 제공하는 객체들
  <br>ex) OutputStream 등 java.io 패키지의 여러 클래스
  * 인터페이스에는 없는 특별한 메서드를 제공하는 클래스들
  <br>ex) PriorityQueue는 Queue 인터페이스에는 없는 comparator 메서드를 제공
* 적합한 인터페이스가 없다면 클래스의 계층구조 중 필요한 기능을 만족하는 가장 덜 구체적인 클래스를 타입으로 사용


### 아이템 65. 리플렉션보다는 인터페이스를 사용하라

* 리플렉션 기능(java.lang.reflect)을 이용하면 프로그램에서 임의의 클래스에 접근 가능
* **리플렉션의 단점**
  * 컴파일타임 타입 검사가 주는 이점을 하나도 누릴 수 없다.
  * 코드가 지저분하고 장황해진다.
  * 성능이 떨어진다.
* 리플렉션은 아주 제한된 형태로만 사용해야 그 단점을 피하고 이점만 취할 수 있다.
```java
public static void main(String[] args) {
  // 클래스 이름을 Class 객체로 변환
  Class<? extends Set<String>> cl = null;
  try {
    cl = (Class<? extends Set<String>>)
          Class.forName(args[0]);
  } catch (ClassNotFoundException e) {
    fatalError("클래스를 찾을 수 없습니다.");
  }
  
  // 생성자를 얻는다.
  Constructor<? extends Set<String>> cons = null;
  try {
    cons = cl.getDeclaredConstractor();
  } catch (NoSuchMethodException e) {
    fatalError("매개변수 없는 생성자를 찾을 수 없습니다.");
  }
  
  // 집합의 인스턴스를 만든다.
  Set<String> s = null;
  try {
    s = cons.newInstance();
  } catch(IllegalAccessException e) {
    fatalError("생성자에 접근할 수 없습니다.");
  } catch(InstantiationException e) {
    fatalError("클래스를 인스턴스화할 수 없습니다.");
  } catch(InvocationTargetException e) {
    fatalError("생성자가 예외를 던졌습니다: " + e.getCause());
  } catch(ClassCastException e) {
    fatalError("Set을 구현하지 않은 클래스입니다.");
  }
  
  // 생성한 집합을 사용
  s.addAll(Arrays.asList(args).subList(1, args.length));
  System.out.println(s);
}

private static void fatalError(String msg) {
  System.err.println(msg);
  System.exit(1);
}
```
