# 이펙티브 자바 정리

## 9장 일반적인 프로그래밍 원칙

### 아이템 57. 지역변수의 번위를 최소화하라

> **지역변수의 범위를 줄이는 기법**<br>
> 가장 처음 쓰일 때 선언하기 (가장 좋음)<br>
> 메서드를 작게 유지하고 한 가지 기능에 집중하는 것

* C언어의 경우 지역변수를 코드 블록의 첫 머리에 선언함.
* 자바에서는 문장을 선언할 수 있는 곳이면 어디든 선언 가능.
* **거의 모든 지역변수는 선언과 동시에 초기화해야 한다**
  * try-catch문은 이 규칙에서 제외
  * 변수를 초기화하는 표현식에서 검사 예외를 던질 가능성이 있다면 try 블록 안에서 초기화<br>그렇지 않으면 예외가 블록을 넘어 메서드에까지 전파됨
  
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
  
  
### 아이템 59. 라이브러리를 익히고 사용하라

```java
// 코드 59-1 흔하지만 문제가 심각한 코드
static Random rnd = new Random();

static int random(int n) {
  return Math.abs(rnd.nextInt()) % n;
}
```
* 3가지 문제점
  * n이 그리 크지 않은 2의 제곱수라면 얼마 지나지 않아 같은 수열이 반복됨.
  * n이 2의 제곱수가 아니라면 몇몇 숫자가 평균적으로 더 자주 반환됨.
  * n값이 크면 이 현상은 더 두드러짐.
  
