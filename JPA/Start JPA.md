# JPA 실습
> Maven 프로젝트를 생성하여 hibernate와 h2database를 활용하여 실습을 진행

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<project xmlns="http://maven.apache.org/POM/4.0.0" 
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> 
 <modelVersion>4.0.0</modelVersion> 
 <groupId>jpa-basic</groupId> 
 <artifactId>ex1-hello-jpa</artifactId> 
 <version>1.0.0</version> 
 <dependencies> 
 <!-- JPA 하이버네이트 --> 
 <dependency> 
 <groupId>org.hibernate</groupId> 
 <artifactId>hibernate-entitymanager</artifactId> 
 <version>5.3.10.Final</version> 
 </dependency> 
 <!-- H2 데이터베이스 --> 
 <dependency> 
 <groupId>com.h2database</groupId> 
 <artifactId>h2</artifactId> 
 <version>1.4.199</version> 
 </dependency> 
 </dependencies> 
</project> 
```
- hibernate의 버전은 자신이 사용하는 스프링부트 프로젝트가 권장해주는 버전과 맞춰주는 것이 좋다.
- h2database의 버전은 자신이 사용하는 h2database의 버전과 맞춰 주는 것이 좋다.


```XML
<?xml version="1.0" encoding="UTF-8"?> 
<persistence version="2.2" 
 xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd"> 
 <persistence-unit name="hello"> 
 <properties> 
 <!-- 필수 속성 --> 
 <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/> 
 <property name="javax.persistence.jdbc.user" value="sa"/> 
 <property name="javax.persistence.jdbc.password" value=""/> 
 <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/> 
 <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/> 
 
 <!-- 옵션 --> 
 <property name="hibernate.show_sql" value="true"/> 
 <property name="hibernate.format_sql" value="true"/> 
 <property name="hibernate.use_sql_comments" value="true"/> 
 <!--<property name="hibernate.hbm2ddl.auto" value="create" />--> 
 </properties> 
 </persistence-unit> 
</persistence> 
```
- /META-INF/persistence.xml로 경로를 무조건 맞춰 주어야 한다.
- JPA는 특정 데이터베이스에 종속되지 않아 필수 속성에서 자신이 사용하는 database를 설정해주기만 하면 된다.
- 옵션을 추가하여 콘솔에서 보고싶은 형태의 hibernate의 결과값을 확인할 수 있다.
- Persistence가 정해진 경로의 persistence.xml의 설정 정보를 먼저 조회한 후 생성이 이루어지는 것이 JPA의 구동 방식이기 때문에 꼭 필요하다.

## 객체와 테이블 생성, 매핑
> @Entity : JPA가 관리할 객체  
> @Id : 데이터베이스 PK와 매핑
```JAVA
package hellojpa; 

import javax.persistence.Entity; 
import javax.persistence.Id; 

@Entity 
public class Member { 
 @Id 
 private Long id; 
 private String name; 

 //Getter, Setter 생성
}
```
### EmtityMnagerFactory를 생성해 준다.
> 영속석 컨텍스트에 접근하여 엔티티에 대한 DB 적업을 제공
```JAVA
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;
import java.util.List;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        EntityManager entityManager = emf.createEntityManager();

        // 트랜잭션 안에서 작업이 이루어져야 한다.
        EntityTransaction tx = entityManager.getTransaction();
        tx.begin();

        // 등록
        // 수정
        // 삭제
        // 회원 단 건 조회 추가하기.

        // JPQL을 사용한 모든 회원 조회
        try{
            List<Member> result = entityManager.createQuery("select m from Member as m", Member.class)
                    .setFirstResult(5) // 페이징 처리를 간단하게 할 수 있다.
                    .setMaxResults(8)
                    .getResultList();

            for(Member member : result) {
                System.out.println("member = " + member.getName());
            }

            tx.commit();
        } catch (Exception e) {
            tx.rollback();
        }finally {
            entityManager.close();
        }
        emf.close();
    }
}
```
- 엔티티 매니저 팩토리는 하나만 생성해서 에플리케이션 전체에 공유
- 엔티티 매니저는 쓰레드간에 공유 X(사용하고 버린다)
- JPA의 모든 데이터 변경은 트랜잭션 안에서 실행(예외처리를 해주는 것이 옳바른 코드)  

내일 주석으로 적어둔 코드 추가하고 추가 작성하기...
JPQL 내용 정리 후 추가하기




