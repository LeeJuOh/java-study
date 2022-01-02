## 나쁜 코드

- 자신이 짠 쓰레기 코드를 나중에 정리하겠다고 하지만 그 나중은 결코 오지 않는다.
    - 르블랑의 법칙..
- 나쁜 코드는 개발 속도를 크게 떨어뜨린다.
    - Technical Debt 발생

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/996ae6ba-fbcf-4921-91fa-cbd56fa96763/.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/996ae6ba-fbcf-4921-91fa-cbd56fa96763/.jpg)

## 그렇다면 클린코드란?

- 우아하고 효율적인 코드
- 철저한 오류 처리
- 한가지에 집중하는 코드
- 가독성이 높은 코드
- 다른 사람이 고치기 쉬운 코드
- 단순하고 직접적이고 필요한 내용만 담아야한다.
- 간단해서 버그가 숨어들지 못한다
- 중복이 없다
- 단위 테스트와 인수 테스트가 존재한다
- 모든 테스트를 통과한다

## 주요 원칙(일반적)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b4c5caac-51bd-4cc4-9b57-7818f8f323a8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b4c5caac-51bd-4cc4-9b57-7818f8f323a8/Untitled.png)

## 네이밍

- 의도를 밝혀라

```java
int d → int day
```

- 비슷한 이름을 사용하지 않는다

```java
XYZControllerForEfficientHandingOfStrings 와
XYZControllerForEfficientStorageOfStrings 의 차이가 한 눈에 들어오는가
```

- 변수 이름에 타입을 넣지 않는다 (헝가리안 표기법?)

```java
PhoneNumber phoneString (x)
List<Integer> accountList (x) → accounts
```

- 클래스 이름은 명사나 명사구가 좋다 , 동사구는 사용x

```java
Customer, Account, WikiPage 같은 이름은 좋다
Manager, Proccesor, Data, Info 같은 이름은 피한다
```

- 메서드 이름은 동사나 동사구가 좋다

```java
접근자, 변경자, 조건자는 javabean 표준에 따라 get, set, is를 붙인다.
```

- 일관성 있는 어휘를 사용해라

```java
역할이 비슷한 메서드에 클래스마다 get, fetch, retrieve라고 제각각 부르지 마라
```

- 의미 있는 맥락을 추가하라, 클래스, 함수, 이름 공간, 접두어
- 검색하기 쉬운 이름을 사용하라
- 한 개념에 한 단어를 사용하라

## 함수

- 작게 만들어라
- 한 가지 일만 해라
- 함수 당 추상화 수준은 하나로
    - 위에서 아래로 코드읽기, 내려가기 규칙(한 함수 다음에는 추상화 수준이 한단계 낮은 함수)
- switch문은 다형적 객체를 생성하는 코드안에서만 사용하도록 해야한다.
    - 즉 저차원 클래스에 숨기고 절대로 반복하지 않는 방향으로 사용한다는 뜻
- 서술적인 이름 사용
- 함수 인수는 적을수록 좋다
- 플래그 인수는 추하다, 함수가 여러 가지를 처리한다고 공표하는 셈
- 인수 객체를 사용해라
- 명령과 조회를 분리하라
    - 함수는 뭔가를 수행하거나 뭔가에 답하거나 둘 중 하나만 해야한다.
- try-catch 분리하라, 오류 처리도 한가지 작업이다

    ```java
    try {
      deletePage(page);
      registry.deleteReference(page.name);
      configKeys.deleteKey(page.name.makeKey());
    } catch(Exception e) {
      logger.log(e.getMessage());
    }
    
    위 코드 보다는 아래가 좋다.
    
    public void delete(Page page) {
        try {
            deletePageAndAllReferences(page);
        } catch(Exception e) {
            logError(e);
        }
    }
    
    private void deletePageAndAllReferences(Page paage) throws Exception {
        deletePage(page);
      registry.deleteReference(page.name);
      configKeys.deleteKey(page.name.makeKey());
    }
    
    private void logError(Exception e) {
        logger.log(e.getMesssage());
    }
    ```

    - 중복 제거해라

  ## 주석

    - 가급적이면 코드로 표현
    - TODO,  중요성을 강조하는 주석, 결과를 경고하는 주석은 바람직할 수 있다.

  ## 클래스

    - 변수와 유틸리티 함수는 가능한 공개하지 않는 편이 낫지만 반드시 숨겨야하는 법칙도 없다.
        - 즉 캡슐화가 기본, 그리고 캡슐화를 풀어주는 결정은 최후의 수단이다.
    - 작게 만들어라
        - 즉, 단일 책임 원칙
    - 인스턴스 변수가 작게 유지함으로써 응집도를 높여라
        - 응집도가 높다 = 메서드가 변수를 더 많이 사용한다.
        - 모든 인스턴스 변수를 메서드마다 사용하는 클래스는 응집도가 가장 높다.
        - 만약 특정 메서드만이 사용하는 인스턴스 변수가 많아진다는 것은 클래스를 쪼개야 한다는 신호
    - 응집도를 유지하면 작은 클래스 여럿이 나온다
    - 추상화된 것에 의존하여 변경으로부터 격리하라
        - 이렇게 결합도를 줄이면 자연스럽게 OCP, DIP을 따르게 된다.

  ## switch

    ```java
    public Money calculatePay(Employee e) throws InvalidEmployeeType {
    	switch (e.type) { 
    		case COMMISSIONED:
    			return calculateCommissionedPay(e); 
    		case HOURLY:
    			return calculateHourlyPay(e); 
    		case SALARIED:
    			return calculateSalariedPay(e); 
    		default:
    			throw new InvalidEmployeeType(e.type); 
    	}
    }
    ```

    - 함수가 길다. (새 직원 유형을 추가하면 더 길어진다.)
    - `한 가지`작업만 수행하지 않는다.
    - SRP(Single Responsibility Principle)를 위반한다. (코드를 변경할 이유가 여럿이기 때문이다.)
    - OCP(Open Closed Principle)를 위반한다.(새 직원 유형을 추가할 때마다 코드를 변경하기 때문이다.)

```java
public abstract class Employee {
	public abstract boolean isPayday();
	public abstract Money calculatePay();
	public abstract void deliverPay(Money pay);
}
-----------------
public interface EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType; 
}
-----------------
public class EmployeeFactoryImpl implements EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
		switch (r.type) {
			case COMMISSIONED:
				return new CommissionedEmployee(r) ;
			case HOURLY:
				return new HourlyEmployee(r);
			case SALARIED:
				return new SalariedEmploye(r);
			default:
				throw new InvalidEmployeeType(r.type);
		} 
	}
}
```

- switch문은 작게 만들기 어렵지만(if/else의 연속 도 마찬가지!), 다형성을 이용하여 switch문을 abstract factory에 숨겨 다형적 객체를 생성하는 코드 안에서만 switch를 사용하도록 한다.
