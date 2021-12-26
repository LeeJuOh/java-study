# key 정리

### 한문장 정리

- 키(Key)는 데이터베이스에서 조건에 만족하는 튜플을 찾거나 순서대로 정렬할 때 다른 튜플들과 구별할 수 있는 유일한 기준이 되는 Attribute(속성)

### 키란?

- 키(Key)는 데이터베이스에서 조건에 만족하는 튜플을 찾거나 순서대로 정렬할 때 다른 튜플들과 구별할 수 있는 유일한 기준이 되는 Attribute(속성)
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3bf43a92-4b19-4c71-8749-75390a4c178d/Untitled.png)

### **Super Key (슈퍼키)**

- 유일성은 만족하지만, 최소성은 만족하지 못하는 키

### **Candidate Key (후보키)**

- Tuple을 유일하게 식별하기 위해 사용하는 속성들의 부분 집합. (기본키로 사용할 수 있는 속성들)
- 2가지 조건 만족
    - 유일성 : Key로 하나의 Tuple을 유일하게 식별할 수 있음
    - 최소성 : 꼭 필요한 속성으로만 구성


### **Primary Key (기본키)**

- 후보키 중 선택한 Main Key
- 특징
    - Null 값을 가질 수 없음
    - 동일한 값이 중복될 수 없음

### **Alternate Key (대체키)**

- 후보키 중 기본키를 제외한 나머지 키 = 보조키

### **Foreign Key (외래키)**

- 다른 릴레이션의 기본키를 그대로 참조하는 속성의 집합
- 외래키는 중복과 널값을 가질수 있다. (후보키에 속하지 않기 때문)

## 참고 출처
[https://ooeunz.tistory.com/3](https://ooeunz.tistory.com/3)
