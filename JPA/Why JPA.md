# JPA

> 무한반복, 지루한 코드 CRUD...   
ex) 필드를 변경하면 모든 관련 SQL을 수정 해줘야 한다...  


## 객체를 자바 컬렉션에 저장 하듯이 DB에 저장할 순 없을까?

### JPA(JAVA Persistence API)
>JPA는 인터페이스의 모음   
JPA 2.1 표준 명세를 구현한 3가지 구현체   
하이버네이트, EclipseLink, DataNucleus
- 자바 진영의 ORM 기술 표준 (Object-Relational Mapping)
- 객체는 객체대로 설계
- 관계형 데이터 베이스는 관계형 데이터베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
- 대중적인 언어는 대부분 ORM 기술이 존재

### JPA를 왜 사용해야 하는가?
> 1. SQL문이 아닌 Method를 통해 DB를 조작할 수 있어 개발자는 객체 모델을 이용하여 비즈니스 로직을 구성하는데만 집중할 수 있다.   
> 2. 객체지향적인 코드 작성이 가능하다. 객체지향적 접근만 고려하면 되기 때문에 생산성이 증가한다.   
> 3. 매핑하는 정보가 Class로 명시 돼있어 ERD를 보는 의존도를 낮출 수 있고 유지보수 및 리팩토링에 유리하다.   
> 4. 예를들어 A데이터베이스를 사용하다가 B데이터 베이스로 변환한다고 가정하면 ORM을 사용하여 쿼리를 수정할 필요가 없다.(벤더의 독립성)
- SQL중심적인 개발에서 객체 중심으로 개발
- 생산성 증가
- 유지보수
- 패러다임의 불일치 해결(1차 캐시와 동일성 보장(identity)과 같은)
- 성능
- 데이터 접근 추상화와 벤더 독립성
- 표준

#### 유지보수
- 기존에는 필드 변경시 모든 SQL을 수정 해야 했지만 JPA는 필드만 추가 한다면 SQL은 JPA가 처리한다.

#### 생산성
<pre>
<code>
jpa.persist(member) // 저장
Member member = jpa.find(memberid) // 조회
member.setName("변경할이름") // 수정
jpa.remove(member) // 삭제
</code>
</pre>
- JPA는 수정 메소드를 제공하지 않는다. JPA는 데이터 수정 시, 매핑된 객체(테이블 데이터)를 조회해서 값을 변경 후 커밋하면 DB서버에 UPDATE문을 전송하여 UPDATE를 실행 해준다.

#### 성능
- 1차 캐시와 동일성(identity)보장
- 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
- 지연 로딩(Lazy Loading)

**스프링에서 흔히 사용하는 것으로 갈고있는 JPA는, JPA를 이용하는 Spring-Data-JPA 프레임워크이지 JPA는 아니다.**

참조 블로그 - https://dbjh.tistory.com/77   
참조 서적 - 자바 ORM 표준 JPA 프로그래밍(김영한)




