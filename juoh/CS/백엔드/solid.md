# 객체 지향 프로그래밍

### 한 문장 정리

- 우리가 주변에서 사물을 인지하는 방식대로 프로그래밍하는 인간 중심적인 프로그래밍 패러다임
- 현실 세계의 사물의 속성과 기능을 분석한 다음, 데이터와 함수로 정의함으로써 프로그래밍 하는 것이다.

### 클래스와 객체

- 클래스(Class) : 같은 속성과 기능을 가진 객체를 총칭하는 개념 / 사물을 분류 / 개념
- 객체(Object) : 세상에 존재하는 모든 고유한 사물 / 실체
- 클래스와 객체를 작명할 때도 클래스는 분류로, 객체 참조 변수명은 유일무이한 사물의 이름으로 작명해야 한다.

### 객체지향의 4대 특징

- 캡슐화(Encapsulation) : 정보 은닉(Information Hiding)
- 상속(Inheritance) : 재사용과 확장(extends)
- 추상화(Abstraction) : 모델링(modeling)
- 다형성(Polymorphism) : 사용 편의성

### 추상화(모델링)

- 추상화란 구체적인 것을 분해해서 관심 영역에 대한 특성만을 가지고 재조합하는 것
- 추상화 = 모델링 = Java의 Class 키워드
- 세상에 존재하는 유일무이한 객체를 특성(속성 + 기능)에 따라 분류해 보니 객체를 통칭할 수 있는 집합적 개념, 즉 클래스(분류)가 나오게 된다.
    - 객체는 유일무이(unique)한 사물이다.
    - 클래스는 같은 특성을 지닌 여러 객체를 총칭하는 집합의 개념이다.
- 이때 내가 구체적인 사물들(객체)을 관심있는 분야(Context)에 대한 공통된 특성(속성(Property)+ 기능(Method)) 만을 찾아내어 재조합한다.
- 객체 지향에서 추상화의 결과는 Class이다.

### 캡슐화

- 객체의 필드, 메소드를 하나로 묶고, 실제 구현 내용을 감추는 것(정보은닉)을 의미한다.
- 정보은닉을 통해 클래스 내부 구현의 응집도는 높이고 결합도는 낮추기 위해 사용
- 정보 은닉(Inforamtion Hiding)은 접근 제어자를 통한 접근 제한을 의미
    - private : 본인만 접근 가능
    - [default] : 같은 패키지 내의 클래스에서 접근 가능
    - protected : 상속 / 같은 패키지 내의 클래스에서 접근 가능
    - public : 모두가 접근 가능
- 정적 멤버인 경우 클래스명.정적멤버 형식으로 접근하자
    - ex) 사람.인구수 / 펭귄.다리개수 -> O
    - ex2) 홍길동.인구수 / 뽀로로.다리개수 -> X
    - 메모리 접근 방식에 있어서도 클래스명.정적멤버 형식이 더 유리하다.

### 상속

- 객체 지향에서 상속은 상위 클래스의 특성을 하위 클래스에서 상속(특성 상속)하고 거기에 더해 필요한 특성을 추가, 즉 확장해서 사용할 수 있다는 의미다.
- 상위 클래스 - 하위 클래스 / 슈퍼 클래스 - 서브 클래스 / 부모 클래스 - 자식클래스
- 하위 클래스는 상위 클래스다.
    - 객체 지행 설계 5원칙 중 LSP(리스코프 치환 원칙)
- 상속 관계가 성립하려면 두 클래스의 관계는 is-a(kind of) 관계
    - 하위 클래스 is a kind of 상위 클래스
        - ex) 펭귄 is a kind of 조류 -> 펭귄은 조류의 한 분류다.
        - ex) 펭귄 is a kind of 동물 -> 펭귄은 동물의 한 분류다.
- has-a 관계를 상속관계로 하면 안된다.
    - 사람  - 팔
    - 자동차 - 바퀴
    - 새 - 날개
- 객체 지향에서의 상속은 상위 클래스의 특성을 확장하는 것이다.
- 객체 지향의 상속은 *is a kind of 관계*를 만족한다.
- JAVA에서 다중 상속은 2개 이상의 상위 클래스가 있을 때, 하위 클래스는 어떠한 속성을 재사용 및 확장하는 것이 모호하기에 다중 상속을 포기했다.

다형성

- 서로 다른 클래스의 객체가 같은 메시지를 받았을 때 각자의 방식으로 동작하는 능력
- **오버라이딩(Overriding)** : 같은 메서드 이름, 같은 인자 목록으로 상위 클래스의 메서드를 ***재정의***
    - 마치 인공위성에서 오토바이에 올라탄 사람을 볼 때, 오토바이는 보이지 않고 오토바이에 오버라이딩(올라타기)된 맨 위의 존재만 보이는 경우이다.
- **오버로딩(Overloading)** : 같은 메서드 이름, 다른 인자 목록으로 다수의 메서드를 ***중복 정의***
    - 인공위성에서 트럭에 적재된 운송물을 볼 때, 오버로딩(적재하기)된 경우는 옆으로 적대된 모든 적재물이 다 보인다.
- 상위 클래스 타입의 객체 참조 변수를 사용하더라도 하위 클래스에서 오버라이딩(재정의)한 메서드가 호출된다.
- 다형성의 본질
    - **역할과 구현을 분리**
    - 자바 언어의 다형성을 활용
    - 역할 = 인터페이스
    - 구현 = 인터페이스를 구현한 클래스, 구현 객체
    - 객체를 설계할 때 ***역할과 구현을 명확히 분리!!!***
    - 객체 설계시 역할(인터페이스)을 먼저 부여하고, 그 역할을 수행하는 구현 객체 만들기
    - 인터페이스를 구현한 객체 인스턴스를 **실행 시점**에 **유연**하게 **변경**할 수 있다.
    - 다형성의 본질을 이해하려면 **협력**이라는 객체사이의 관계에서 시작해야함
    - ***클라이언트를 변경하지 않고, 서버의 구현 기능을 유연하게 변경할 수 있다.***

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b996daa-bf68-4b9c-84b5-9cd65d442583/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b996daa-bf68-4b9c-84b5-9cd65d442583/Untitled.png)

### **객체 지향 설계 과정**

- 제공해야 할 기능을 찾고 세분화한다. 그리고 그 기능을 알맞은 객체에 할당한다.
- 기능을 구현하는데 필요한 데이터를 객체에 추가한다.
- 그 데이터를 이용하는 기능을 넣는다.
- 기능은 최대한 캡슐화하여 구현한다.
- 객체 간에 어떻게 메소드 요청을 주고받을 지 결정한다.

### **객체 지향 설계 원칙**

SOLID라고 부르는 5가지 설계 원칙이 존재한다.

1. **SRP(Single Responsibility) - 단일 책임 원칙**

   클래스는 단 한 개의 책임을 가져야 한다.

   클래스를 변경하는 이유는 단 한개여야 한다.

   이를 지키지 않으면, 한 책임의 변경에 의해 다른 책임과 관련된 코드에 영향이 갈 수 있다.

2. **OCP(Open-Closed) - 개방-폐쇄 원칙**

   자신의 확장에는 열려 있어야 하고, 주변의 변경에는 닫혀 있어야 한다.

   B라는 기능을 추가하고 싶을 때, A라는 원본 Code가 바뀌어서는 안된다는 의미이다.

   기능을 변경하거나 확장할 수 있으면서, 그 기능을 사용하는 코드는 수정하지 않는다.

   상위 클래스 또는 인터페이스를 중간에 둔다.

3. **LSP(Liskov Substitution) - 리스코프 치환 원칙**

   서브 타입(Sub-Type)은 언제나 자신의 기반 타입(Base-Type)으로 교체할 수 있어야 한다.

   상속 관계가 아닌 클래스들을 상속 관계로 설정하면, 이 원칙이 위배된다.

4. **ISP(Interface Segregation) - 인터페이스 분리 원칙**

   인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야 한다.

    - ***특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나 보다 낫다.***
    - **"클라이언트는 자신이 사용하지 않는 메서드에 의존 관계를 맺으면 안된다." - 로버트.C.마틴**
        - 인터페이스를 통해 메서드를 외부에 제공할 때는 최소한의 메서드만 제공하라
        - 인터페이스가 명확해지고, 대체 가능성이 높아진다.

   각 클라이언트가 필요로 하는 인터페이스들을 분리함으로써, 각 클라이언트가 사용하지 않는 인터페이스에 변경이 발생하더라도 영향을 받지 않도록 만들어야 한다.

   ex) isp 적용전

    ```java
    public interface SmartPhone {
        String telephone();
    
        String internet();
    }
    
    public interface IphoneFunction {
        String mp3();
    }
    
    public interface GalaxyFunction {
        String video();
    }
    
    public class Iphone implements SmartPhone, IphoneFunction {
        @Override
        public String telephone() {
            return "ring ring";
        }
    
        @Override
        public String internet() {
            return "connect complete!";
        }
    
        @Override
        public String mp3() {
            return "play mp3";
        }
    }
    
    public class Galaxy implements SmartPhone, GalaxyFunction {
        @Override
        public String telephone() {
            return "ring ring";
        }
    
        @Override
        public String internet() {
            return "connect complete!";
        }
    
        @Override
        public String video() {
            return "play video";
        }
    }
    ```

   ex) isp 적용후

    ```java
    public interface SmartPhone {
        String telephone();
    
        String internet();
    }
    
    public interface IphoneFunction {
        String mp3();
    }
    
    public interface GalaxyFunction {
        String video();
    }
    
    public class Iphone implements SmartPhone, IphoneFunction {
        @Override
        public String telephone() {
            return "ring ring";
        }
    
        @Override
        public String internet() {
            return "connect complete!";
        }
    
        @Override
        public String mp3() {
            return "play mp3";
        }
    }
    
    public class Galaxy implements SmartPhone, GalaxyFunction {
        @Override
        public String telephone() {
            return "ring ring";
        }
    
        @Override
        public String internet() {
            return "connect complete!";
        }
    
        @Override
        public String video() {
            return "play video";
        }
    }
    ```

5. **DIP(Dependency Inversion) - 의존 역전 원칙**

   고차원 모듈은 저차원 모듈에 의존하면 안된다. 이 두 모듈 모두 다른 추상화된 것에 의존해야 한다.

   추상화된 것은 구체적인 것에 의존하면 안된다. 구체적인 것이 추상화된 것에 의존해야 한다.

    - 구현 클래스에 의존하지 말고, 인터페이스에 의존하라는 뜻

   자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향 받지 않게 하는 것

   ex) 현재 고수준 모듈인 자동차는 저수준 모듈인 스노우타이어에 의존

   ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aa24bb83-2411-40ae-9e5b-1847d3c8cd1a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aa24bb83-2411-40ae-9e5b-1847d3c8cd1a/Untitled.png)

   ex) 저수준 모듈인 타이어는 이제 고수준 모듈인 자동차에 의존
