- 변수를 private로 정의하는 이유 → 변수에 대한 의존하지 않았으면 싶어서
- 그렇다면 왜 get, set으로 public화 할까?

### 6.1 자료 추상화

```java
public class Point {
	public double x;
	public double y;
}
```

```java
public interface Point {
	double getX();
	double getY();
	void setCartesian(double x, double y);
	double getR();
	double getTheta();
	void setPolar(double R, double theta);
}
```

2번째는 직교좌표계를 쓰는지 극좌표계를 쓰는지 알 길이 없다. 

get(조회)시에는 개별적으로 읽어야하며, 좌표를 설정할때는 두값을 한꺼번에 설정함으로서 클래스 메소드가 정책을 강제하도록 한다.

반면 1번째는 직교좌표계를 사용

이 방법은 구현을 노출한다. private으로 선언하더라도 각 값마다 get , set을 제공하면 구현을 외부로 노출한 셈이다.

자료를 세세하게 공개하기 보다는 추상적인 개념으로 표현하는 편이 낫다.

### 6.2 자료/객체 비대칭

- 객체는 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개
- 자료구조는 자료를 그대로 공개하며 별다른 함수는 제공하지 않는다.
- 객체와 자료구조는 근본적으로 양분된다. 상호 보완적
    - 절차적인 코드는 기존 자료구조를 변경하지 않으면서 새함수를 추가하기 쉽다.
    - 객체지향 코드는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽다.
    - ex) 도형 클래스
        - 새로운 함수(둘레 구하기)를 추가하고 싶다면? ⇒ 절차적인 코드에서는 고치기 쉬움
            - 반면 절차적인 코드에서 새 도형 클래스를 추가하려면 geometry에서 기존구현부분도 모두 고쳐야함
        - 새로운 도형클래스를 추가하고 싶다면? ⇒ 객체지향 코드에서 쉬움
            - 반면 객체지향 코드에서 새 함수를 추가하려고 한다면 모든 클래스 수정해야함

### 6.3 디미터 법칙

- 모듈은 자신이 조작하는 객체의 속사정을 몰라야한다는 법칙
- 즉, 객체는 조회 함수로 내부 구조를 공개하면 안된다는 의미
1. 기차 충돌 (train wreck)
    
    ```java
     final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
    ```
    
    여러대의 객체가 한줄로 이루어진 기차처럼 보이기 때문 [G36]
    
    getOptions를 통해서 getAbsolutePath까지 호출되는 상황 중간에 뭔가가 변경이 일어나게 되면 이 코드는 무조건 고쳐져야한다. 따라서 변경에 용이하기 위해서는 이부분의 의존도를 낮춰야한다.
    
2. 잡종 구조
    - 두가지를 섞는것은 단점만 모아놓은 구조
3. 구조체 감추기
    - 위 예에서 절대경로를 구하는 의도를 파악하고 함수 하나로 정의하면 됨
    
    ```java
    BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
    ```
    

### 6.4 자료 전달 객체

- 공개 변수만 있고 함수가 없는 클래스 DTO(Data Transfer Object)
1. 활성 레코드

### 6.5 결론

미완성
