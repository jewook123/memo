### 의도를 분명히 밝혀라
```java
public List<int[]> getThem() {
  List<int[]> list1 = new ArrayList<int[]>();
  for (int[] x : theList)
    if (x[0] == 4)
      list1.add(x)
  return list1;
}
```
위 코드는 독자가 다음 정보를 안다고 명시적으로 가정해버렸다.
<br>
1. theList에 무엇이 들었을까?
2. theList에서 0번째 값이 어째서 중요한가?
3. 값 4는 무슨 의미인가?
4. 함수가 반환하는 리스트 list1을 어떻게 사용하는가?
<br>
각 개념에 이름만 붙여도 코드가 ```훨씬``` 나아진다.

```java
public List<int[]> getFlaggedCells() {
  List<int[]> flaggedCells = new ArrayList<int[]>();
  for (int[] cell : gameBoard)
    if (cell[STATUS_VALUE == FLAGGED)
      flaggedCells.add(cell);
  return flaggedCells;
}
```
### 그릇된 정보를 피하라
- 널리 쓰이는 의미가 있는 단어를 다른 의미로 사용해도 안 된다.
  -  hp, aix, sco 등.. 각 분야에서 쓰이는 단어
-  서로 흡사한 이름은 사용않도록 주의한다.
  - XYZControllerForEfficientHandlingOfStrings , XYZControllerForEfficientStorageOfStrings ( 😅햇갈림 )
  - 소문자 l, 숫자 1 , 대문자 O 등은 진짜 햇갈린다.

### 의미 있게 구분하라
- 동일한 범위 안에서 다른 두 개념에 같은 이름을 사용하면 안된다
- 연속적인 숫자를 덧붙인 이름 (a1, a2, a3 ... ) 사용하면 안된다.
- 불용어(noise word)를 추가한 이름은 아무런 정보 제공 X
  -  info, data는 a , an , the와 마찬가지로 의미가 불분명한 불용어다
  -  불용어는 중복이다.
    -  moneyAccount, moneyAccountInfo, moneyAccounts 
    -  moneyAmount, money
    -  customer, customerInfo
    -  accountData, account
    -  theMessage, message
 
### 발음하기 쉬운 이름 사용해라
```같이 일할때 불편할거 같은건 안된다.```

### 검색하기 쉬운 이름을 사용해라
```같이 일할때 불편할거 같은건 안된다.```

### 인코딩을 피하라
 - 헝가리식 표기법
 - 멤버 변수 접두어
 - 인터페이스 클래스와 구현 클래스
   
### 자신의 기억력을 자랑하지 마라
 - 변수 이름을 자신이 아는 이름으로 변환해야 한다면 그 변수 이름은 바람직하지 못하다.
 - 일반적으로 문제영역이나 해법 영역에서 사용하지 않는 이름을 사용했기 때문

### 클래스 이름
 - 명사, 명사구 적합
 - 동사 X
 - 적당한 예 : Customer, WikiPage, Account, AddressParser
 - 부적당한 예 : Manager, Processor, Data, Info 와 같은 단어(불용어)는 뺀다.

### 메소드 이름
 - 동사, 동사구 적합
 - 적당한 예 : postPayment, deletePage, save
 - 접근자, 변경자, 조건자,는 자바빈 표준에 따라 get, set, is를 붙인다.
 - 생성자를 중복해 정의할때 정적팩토리 메소드 사용
 ```
 Complex fulcrumPoint = Complex.FromRealNumber(23.0);
 
 Complex fulcrumPoint = new Complex(23.0);
 ```
 위 코드가 아래보다 낫다.
 
### 기발한 이름은 피하라
```같이 일할때 불편할거 같은건 안된다.```

### 개념 하나에 단어 하나를 사용하라
- fetch, retrieve, get이라고 각각 부르면 혼란스럽다.
- controller, manager, driver를 섞어 쓰면 혼란스럽다.

### 말장난 하지마라
```같이 일할때 불편할거 같은건 안된다.```
- 한 단어를 두 가지 목적으로 사용하지마라

### 해법 영역에서 사용하는 이름을 사용하라
- 비지터 패턴에 익숙한 프로그래머는 AccountVisitor라는 이름을 금방 이해한다.

### 문제 영역과 관련 있는 이름을 사용하라
- 적절한 '프로그래머 용어'가 없다면 문제 영역에서 이름을 가져온다.

### 의미 있는 맥락을 추가하라
- firstName, lastName, street, state
- 만약에 state가 다른데서 쓰인다면 ```address```의 state인지 ```상태```의 state인지 구분하기 어렵다.

### 불필요한 맥락을 없애라
- '고급 휘발유 충전소'라는 응용프로그램을 짠다고 가정할때 모든 클래스에 고.휘.충으로 시작하겠다고 생각하는건 바람직하지 못하다.

### 마치면서
- 좋은 이름을 선택하려면 설명하는 능력이 뛰어나야 하고 문화적인 배경이 같아야 한다.
