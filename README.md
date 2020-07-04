# 이펙티브 자바 정리

## 2장 객체 생성과 파괴

### 아이템 1. 생성자 대신 정적 팩터리 메서드를 고려하라

* 클래스는 생성자와 별도로 정적 팩터리 메서드(static factory method)를 제공할 수 있다.
<br>(해당 클래스의 인스턴스를 반환함)
* 정적 팩터리 메서드가 생성자보다 좋은 **장점 5가지**
  * 이름을 가질 수 있다.
    * 이름을 잘 지으면 반환될 객체의 특성을 쉽게 묘사할 수 있다.
    <br>ex) BigInteger.probablePrime
  * 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.
    * 같은 객체가 자주 요청되는 상황이라면 성능을 상당히 끌어올려 준다.
    * 반복되는 요청에 같은 객체를 반환하는 식으로 언제 어느 인터페이스가 살아 있게 살지를 철저히 통제 가능<br>
    = **인스턴스 통제(instance-controlled) 클래스**
  * 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
    * 반환할 객체의 클래스를 자유롭게 선택할 수 있는 유연성 제공
  * 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
    * 반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없음.
  * 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
    * JDBC(Java Database Connectivity)와 같은 서비스 제공자 프레임워크를 만들 수 있는 유연성 제공
>인스턴스를 통제하는 이유<br>
>>* 클래스를 싱글턴으로 만들 수도, 인스턴스화 불가로 만들 수도 있다.<br>
>>* 불변 값 클래스에서 동치인 인스턴스가 단 하나뿐임을 보장할 수 있다. (a == b일 때만 a.equals(b)가 성립)
---
>서비스 제공자 프레임워크(Service Provider Framework) 구성<br>
>>* 서비스 인터페이스(Service Interface) : 구현체의 동작을 정<br>ex) JDBC Connection<br>
>>* 제공자 등록 API(Provider registration API) : 제공자가 구현체를 등록할 떄 사용<br>ex) JDBC DriverManager.registerDriver<br>
>>* 서비스 접근 API(Service access API) : 클라인트가 서비스의 인스턴스를 얻을 때 사용<br>ex) JDBC DriverManager.getConnection

* 정적 팩터리 메서드가 생성자보다 좋지 않은 **단점 5가지**
  * 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
  <br>상속을 하려면 public이나 protected 생성자가 필요하기 때문
  * 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
  <br>API 문서를 잘 써놓고 메서드 이름도 알려진 규약을 따라 작성 필요


### 아이템 2. 생성자에 매개변수가 많다면 빌더(Builder)를 고려하라

* 생성자나 정적 팩터리 메서드의 경우 선택적 매개변수가 많을 때 대응이 어려움.
* 점층적 생성자 패턴을 적용할 경우 매개변수 개수가 많아지면 코드를 작성하거나 읽기 어렵다.
```java
// 코드 2-1 점층적 생성자 패턴 - 확장하기 어려움
public class NutritionFacts {
  private final int servingSize;  // (ml, 1회 제공량)    필수
  private final int servings;     // (회, 총 n회 제공량) 필수
  private final int calories;     // (1회 제공량당)      선택
  private final int fat;          // (g/1회 제공량)      선택
  
  public NutritionsFacts(int servingSize, int servings) {
    this(servingSize, servings, 0);
  }
  
  public NutritionsFacts(int servingSize, int servings, int calories) {
    this(servingSize, servings, calories, 0);
  }
  
  public NutritionsFacts(int servingSize, int servings, int calories, int fat) {
    this.servingSize = servingSize;
    this.servings    = servings;
    tihs.calories    = calories;
    this.fat         = fat;
  }
}

// 사용예시
NutritionsFacts cocaCola = new NutritionsFacts(240, 8, 100, 0);
```
* 자바 빈즈 패턴에서는 객체 하나를 만들려면 메서드를 여러 개 호출해야 하고,<br>
객체가 완전히 생성되기 전까지는 일관성(consistency)이 무너진 상태에 놓이게 된다.
```java
// 코드 2-2 자바빈즈 패턴 - 일관성이 꺠지고, 불변으로 만들 수 없다.
public class NutritionFacts {
  private final int servingSize = -1;  // 필수, 기본값 없음
  private final int servings    = -1;  // 필수, 기본값 없음
  private final int calories    = 0;
  private final int fat         = 0;
  
  // 생성자
  public NutritionsFacts() { }
  
  public NutritionsFacts(int servingSize, int servings, int calories) {
    this(servingSize, servings, calories, 0);
  }
  
  public void setServingSize(int val) { servingSize = val };
  public void setServings(int val) { servings = val };
  ...
}

// 사용예시
NutritionFacts cocaCola = new NutritionFacts();
cocaCola.setServingSize(240);
cocaCola.setServings(8);
...
```
* 빌더 패턴 = 점층적 생성자 패턴의 안전성 + 자바 빈즈 패턴의 가독성
```java
// 코드 2-3 빌더 패턴 - 점층적 생성자 패턴의 안전성 + 자바 빈즈 패턴의 가독성
public class NutritionFacts {
  private final int servingSize;
  private final int servings;
  private final int calories;
  private final int fat;
  
  public static class Builder {
    // 필수 매개변수
    private final int servingSize;
    private final int servings;
    
    // 선택 매개변수 - 기본값으로 초기화
    private int calories = 0;
    private int fat = 0;
    
    public Builder(int servingSize, int servings) {
      this.servingSize = servingSize;
      this.servings = servings;
    }
    
    public Builder calories(int val) {
      calories = val;
      return this;
    }
    
    public Builder fat(int val) {
      fat = val;
      return this;
    }
    
    public NutritionFacts build() {
      return new NutritionFacts(this);
    }
  }
  
  private NutritionFacts(Builder builder) {
    servingSize = builder.servingSize;
    servings = builder.servings;
    calories = builder.calories;
    fat = builder.fat;
  }
}

// 사용예시
NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8).calories(100).build();
```


### 아이템 3. private 생성자나 열거 타입으로 싱글턴임을 보증하라

* 싱글턴(singleton) : 인스턴스를 오직 하나만 생성할 수 있는 클래스
* 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있음.
* **public 필드 방식**의 장점
  * 해당 클래스가 싱글턴임이 API에 명백히 드러남
  * 간결하다
```java
// 코드 3-1 public static final 필드 방식의 싱글턴
public class Elvis {
  public static final Elvis INSTANCE = new Elvis();
  private Elvis() { }
  
  public void leaveTheBuilding() { }
}
```

* **정적 팩터리 방식**의 장점
  * API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있음. (getInstance()를 변경)
  * 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있음.
  * 정적 팩터리의 메서드 참조를 Supplier로 사용할 수 있음.
  <br>ex) Elvis::getInstance -> Supplier<Elvis>
```java
// 코드 3-2 정적 팩터리 방식의 싱글턴
public class Elvis {
  private static final Elvis INSTANCE = new Elvis();
  private Elvis() { }
  public static Elvis getInstance() { return INSTANCE; }
  
  public void leaveTheBuilding() { }
}
```

* 원소가 하나인 열거 타입을 선언
  * **대부분의 상황에서 가장 좋음**
```java
public enum Elvis {
  INSTANCE;
  
  public void leaveTheBuilding() { }
}
```


### 아이템 4. 인스턴스화를 막으려거든 private 생성자를 사용하라

```java
public class UtilityClass {
  // 기본 생성자가 만들어지는 것을 막는다(인스턴스화 방지용)
  private UtilityClass() {
    throw new AssertionError();
  }
}
```


### 아이템 5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

* 사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않다.
```java
// 코드 5-1 정적 유틸리티를 잘못 사용한 예 - 유연하지 않고 테스트하기 어려움
public class SpellChecker {
  // 사전을 언제나 하나만 사용한다고?? 유연하지 않음
  private static final Lexicon dictionary = ...;
  
  // 객체 생성 방지
  private SpellChecker() { }
  ...
}


// 코드 5-2 싱글턴을 잘못 사용한 예 - 유연하지 않고 테스트하기 어려움
public class SpellChecker {
  // 사전을 언제나 하나만 사용한다고?? 유연하지 않음
  private static final Lexicon dictionary = ...;
  
  // 객체 생성 방지
  private SpellChecker() { }
  public static SpellChecker INSTANCE = new SpellChecker(...);
  ...
}
```
* 인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨주는 방식이 좋다.
```java
// 코드 5-3 의존 객체 주입 방식
public class SpellChecker {
  private static final Lexicon dictionary;
  
  public SpellChecker(Lexicon dictionary) {
    this.dictionary = Objects.requireNonNull(dictionary);
  }
  ...
}
```


### 아이템 6. 불필요한 객체 생성을 피하라

* 생성자 대신 정적 팩터리 메서드를 사용하면 불필요한 객체 생성을 피할 수 있음.
<br>ex) `Boolean(String)` 대신 `Boolean.valueOf(String)` 사용

* 생성 비용이 비싼 객체의 경우 다른 방법을 모색해라.
```java
// 코드 6-1 생성 비용이 비싼 객체
static boolean isRomanNumeral(String s) {
  return s.matches("^(?=.)M*(C[MD]...");
}

// 코드 6-2 값비싼 객체를 재사용해 성능을 개선
public class RomanNumerals {
  private static final Pattern ROMAN = Pattern.complie("^(?=.)M*(C[MD]...");
  
  static boolean isRomanNumeral(String s) {
    return ROMAN.matcher(s).matches();
  }
}
```

* 불필요한 오토박싱은 피하라
```java
// 코드 6-3 잘못된 오토박싱
private static long sum() {
  // long이 아닌 박싱된 기본 타입인 Long을 사용해서 오토박싱에 많은 자원 소모
  Long sum = 0L;
  for (long i = 0; i<= INTEGER.MAX_VLAUE; i++)
    sum += i;
  
  return sum;
}
```


### 아이템 7. 다 쓴 객체 참조를 해제하라

T.B.D


### 아이템 8. finalizer와 cleaner 사용을 피하라

T.B.D


### 아이템 9. try-finally보다는 try-with-resources를 사용하라 (자바 7 이후)

* InputStream, OutputStream, java.sql.Connection 등의 경우 자원 사용 후 close메서드를 호출해야 함.
```java
// 코드 9-1 try-finally - 좋은 방식이 아니다
static String firstLineOfFile(String path) throws IOException {
  BufferedReader br = new BufferedReader(new FileReader(path));
  try {
    return br.readLine();
  } finally {
    br.close();
  }
}

// 코드 9-3 try-with-resources - 자원을 회수하는 최선책!
static String firstLineOfFile(String path) throws IOException {
  // try문이 끝나면 자동으로 close 호출
  try (BufferedReader br = new BufferedReader(new FileReader(path))) {
    return br.readLine();
  }
}

// 코드 9-4 복수의 자원을 처리하는 try-with-resources - 짧고 매혹적
static void copy(String src, String dst) throws IOException {
  try (InputStream in = new FileInputStream(src);
       OutputStream out = new FileOutputStream(dst)) {
    byte[] buf = new byte[BUFFER_SIZE];
    int n;
    while ((n = in.read(buf)) >= 0) {
      out.write(buf, 0, n);
    }
  }
}
```

## 3장 모든 객체의 공통 메서드

### 아이템 10. equals는 일반 규약을 지켜 재정의하라

* 다음과 같은 상황에서는 재정의하지 말 것
  * 각 인스턴스가 본질적으로 고유할 때
  <br>ex) Thread
  * 인스턴스의 "논리적 동치성(logical equality)"을 검사할 일이 없다.
  * 상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는다.
  * 클래스가 private이거나 package-private이고 equals 메서드를 호출할 일이 없다.
* **equals를 재정의해야 할 때는 논리적 동치성을 확인해야할 경우만!**
* equals 메서드 재정의할 때 지켜야할 일반 규약
  * **반사성(reflexivity)**
  <br>null이 아닌 모든 참조값 x에 대해, x.equals(x)는 true다.
  * **대칭성(symmetry)**
  <br>null이 아닌 모든 참조값 x, y에 대해, x.equals(y)가 true면 y.equals(x)도 true다.
  * **추이성(transitivity)**
  <br>null이 아닌 모든 참조값 x, y, z에 대해, x.equals(y)가 true이고 y.equals(z)도 true면 x.equals(z)도 true다.
  * **일관성(consistency)**
  <br>null이 아닌 모든 참조값 x, y에 대해, x.equals(y)를 반복해서 호출하면 항상 true를 반환하거나 항상 false를 반환한다.
  * **null-아님**
  <br>null이 아닌 모든 참조값 x에 대해, x.equals(null)은 false다.


### 아이템 11. equals를 재정의하려거든 hashCode도 재정의하라

* equals를 재정의한 클래스 모두에서 hashCode도 재정의해야 함.
<br>재정의하지 않을 경우 HashMap, HashSet을 사용할 때 문제 발생.
* hashCode 규약
  * equals 비교에 사용되는 정보가 변경되지 않았다면, 애플리케이션이 실행되는 동안 그 객체의 hashCode 메서드는 항상 같은 값을 반환
  * equals(Object)가 두 객체를 같다고 판단했다면, 두 객체의 hashCode는 같은 값을 반환
  * equals(Object)가 두 객체를 다르게 판단했더라도, 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다.
  <br>단, 다른 객체에 대해서는 다른 값을 반환해야 해시테이블의 성능이 좋아진다.
* 좋은 해시 함수라면 서로 다른 인스턴스에 다른 해시코드를 반환
* 이상적인 해시 함수는 주어진 인스턴스들을 32비트 정수 범위에 균일하게 분배
* **성능을 높이려고 해시코드를 계산할 때 핵심 필드를 생략해서는 안 된다.**
```java
// 코드 11-2 전형적인 hashCode 메서드
// PhoneNumber 인스턴스의 핵심 필드 3개만을 사용해 간단한 계산을 수행
@Override public int hashCode() {
  int result = Short.hashCode(areaCode);
  result = 31 * result + Short.hashCode(prefix);
  result = 31 * result + Short.hashCode(lineNum);
  return result;
}

// 코드 11-3 한 줄짜리 hashCode 메서드 - 성능이 아쉬움
@Override public int hashCode() {
  return Objects.hash(lineNum, prefix, areaCode);
}

// 코드 11-4 해시코드를 지연 초기화하는 hashCode 메서드 - 스레드 안정성까지 고려해야 함
private int hashCode; // 자동으로 0으로 초기화

@Override public int hashCode() {
  int result = hashCode;
  if (result == 0) {
    result = Short.hashCode(areaCode);
    result = 31 * result + Short.hashCode(prefix);
    result = 31 * result + Short.hashCode(lineNum);
    hashCode = result;
  }
  return result;
}
```


### 아이템 12. toString을 항상 재정의하라

* Object의 기본 toString 메서드 : **클래스_이름@16진수로_표시한_해시코드** 를 반환
* toString을 잘 구현한 클래스는 사용하기에 훨씬 즐겁고, 그 클래스를 사용한 시스템은 디버깅하기 쉽다.
* toString은 그 객체가 가진 주요 정보 모두를 반환하는 게 좋다.


### 아이템 13. clone 재정의는 주의해서 진행하라

* **Cloneable 인터페이스의 가장 큰 문제**
<br>clone 메서드가 Object 클래스에 protected로 선언됨. 따라서 Cloneable을 구현하는 것만으로는 외부 객체에서 clone 메서드를 호출할 수 없다.
* Cloneable을 구현한 클래스의 인스턴스에서 clone을 호출하면 그 객체의 모든 필드들을 복사한 객체를 반환
<br>Cloneable을 구현하지 않은 클래스의 인스턴스에서 호출 시 CloneNotSupportedException을 던진다.
* 실무에서 CLoneable을 구현한 클래스는 clone 메서드를 public으로 제공하며, 사용자는 당연히 복제가 제대로 이뤄지리라 기대한다.
<br>**clone 메서드를 사용할 경우 생성자를 사용하지 않고도 객체를 생성할 수 있다.**
* 

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
