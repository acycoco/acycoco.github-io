# 2023-06-02

# Customers 목록보기

```java
package com.example.demo.controller;

import com.example.demo.models.Customer;
import com.example.demo.repositories.CustomerRepository;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.List;
@Controller
public class CustomerController {
    CustomerRepository customerRepository;

    public CustomerController(CustomerRepository customerRepository) {
        this.customerRepository = customerRepository;
    }

    @GetMapping(value = "/customers")
    public String list(Model model){
        List<Customer> customers = (List<Customer>) customerRepository.findAll();
        model.addAttribute("customers", customers);
        return "customers/customerList";
    }
```

templetes밑의 customers.customerList.html

```java
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<title>Customer List</title>
<body>
<div class="container">
    <div>
        <table>
            <thead>
            <tr>
                <th>#</th>
                <th>firstName</th>
                <th>lastName</th>
            </tr>
            </thead>
            <tbody>
            <tr th:each="customer : ${customers}">
                <td th:text="${customer.id}"></td>
                <td th:text="${customer.firstName}"></td>
                <td th:text="${customer.lastName}"></td>
            </tr>
            </tbody>
        </table>
    </div>
</div> <!-- /container -->
</body>
</html>
```

/customers

![Untitled](2023-06-02%20c8db660a50214db688aad2a81af68f63/Untitled.png)

---

# RESTful

Json, HTML

HTTP의 메소드      Get → 주소에  : / kewg? ge.egggw 이런 방식으로 , 남도 다볼 수 있는 default방식

Post → 포장을 해서     insert

Put  → update

Delete

같은 주소에서 메소드만 바꿔서

TCP   

![Untitled](2023-06-02%20c8db660a50214db688aad2a81af68f63/Untitled%201.png)

![Untitled](2023-06-02%20c8db660a50214db688aad2a81af68f63/Untitled%202.png)

![Untitled](2023-06-02%20c8db660a50214db688aad2a81af68f63/Untitled%203.png)

---

# Person

```java
@Entity
public class Person {
    @Id //이거는 id다.
    @GeneratedValue(strategy = GenerationType.AUTO) //알아서 관리
    private long id;
    private String firstName;
    private String lastName;

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
}
```

```java
import com.example.demo.models.Person;
import org.springframework.data.repository.PagingAndSortingRepository;
import org.springframework.data.repository.CrudRepository;
import org.springframework.data.repository.query.Param;
import org.springframework.data.rest.core.annotation.RepositoryRestResource;

import java.util.List;

@RepositoryRestResource(collectionResourceRel = "people", path= "people")
public interface PersonRepository extends
        PagingAndSortingRepository<Person, Long>,
        CrudRepository<Person, Long> {
    List<Person> findByLastName(@Param("name") String name);

}
```

pagingAndSoringRepository를 이용해서

![Untitled](2023-06-02%20c8db660a50214db688aad2a81af68f63/Untitled%204.png)

---

# 다중상속

클래스 구현다중상속 x,   인터페이스 구현다중상속O

---

# spring.io

[https://spring.academy/courses/building-a-rest-api-with-spring-boot](https://spring.academy/courses/building-a-rest-api-with-spring-boot)

여기서 학습하면 됩니다.

---

# 설명

hostname:port

/index.html  상대경로    브라우저는 이걸 띄워주는 기능밖에 없음

→ @GetMapping(””)

MVC

브라우저 — 뷰(화면) — 서버 — 모델(entity, repository) — 컨트롤러(비즈니스 로직 실행해주는 애)

DTO —> entity

데이터를 가지고 와서 쓰는거

@Bean     컴포넌트의 일종

관리를 스프링에 넘김

@ 를 하고 클래스를 만드면 스프링에 등록시킨다.

---

# Customer

```java
@Entity           //저장소에 쓸 거로 만들어줌 table로 등록
public class Customer {
    @Id //이거는 id다.
    @GeneratedValue(strategy = GenerationType.AUTO) //알아서 관리
    private long id;
    private String firstName;
    private String lastName;

    protected Customer() { //상속받아서 사용할 일이 없음
        // 기본 생성자
    }
    public Customer(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    @Override
    public String toString() {
        return String.format("Customer[id=%d, firstName=%s. lastName=%s",
                id, firstName, lastName);
    }

    public long getId() {
        return id;
    }

    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }
}
```

---

# REST

---

```java
@Bean //시작할 때 자동으로 시작     shell에 찍히겠금, log로
	public CommandLineRunner demo(CustomerRepository repository) {   //의존성 주입 자동으로 입력해줌
		return args -> {
			repository.save(new Customer("leo", "aa"));

			for (Customer customer: repository.findAll()){
				log.info(customer.toString());
			}
			log.info("");
		};
	}
```

![Untitled](2023-06-02%20c8db660a50214db688aad2a81af68f63/Untitled%205.png)

application.properties에서 test.db로 바꾸면 test.db가 생성되는 것을 볼 수 있습니다.

auto = update가 아닌 none으로 만드면 연결?만 해줍니다.

![Untitled](2023-06-02%20c8db660a50214db688aad2a81af68f63/Untitled%206.png)

# MVC

---

model  - document / data

view   -  화면 UI 

controller  - 처리하는 역할

# Hello World

---

1.코드로 하는 방법 

![Untitled](2023-06-02%20c8db660a50214db688aad2a81af68f63/Untitled%207.png)

2.정적인 방법- 속도는 더 빠름 

![Untitled](2023-06-02%20c8db660a50214db688aad2a81af68f63/Untitled%208.png)

# tips

---

themypu 이거 기초 배우면 html을 더 쉽게 할 수 있음

# 최초설정

---

```
driverClassName=org.sqlite.JDBC
# url=jdbc:sqlite:memory:test?cache=shared
url=jdbc:sqlite:test.db
username=sa
password=sa
spring.jpa.database-platform=org.hibernate.community.dialect.SQLiteDialect
hibernate.hbm2ddl.auto=update
hibernate.show_sql=true
```

```
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

- hibernate.ddl-auto은 서버와 DB 간의 데이터(테이블 형태 등) 동기화 방법에 대해 정의
    - 최초 테이블 생성을 위해 update를 사용하였지만, 운영 서버에서는 none 또는 validate를 이용하는것을 권장
- spring.jpa.show-sql은 자동으로 생성된 쿼리문 출력여부를 결정
- apllication.properties 여기만 바꿔주면 됨

# Springboot

---

static - 정적인 html파일

template - 연결정보

[application.properties](http://application.properties) - 정보

![Untitled](2023-06-02%20c8db660a50214db688aad2a81af68f63/Untitled%209.png)

# 다:다

---

중간 테이블에 id를 넣어서 추가적인 정보

추가적인 정보가 없는 경우 

• [https://rubykr.github.io/rails_guides/association_basics.html](https://rubykr.github.io/rails_guides/association_basics.html) - 참고할 자료

# client - server

---

HTTP → TCP를 이용해 통신(연결되어야 통신) ↔ UDP

패킷을 보냄 

---

# 데이터베이스 개발 생명주기

1. 요구사항 분석
2. 개념적 설계 → 글로 설계하기
3. 논리적 설계 → 그림
4. 물리적 설계 → 인덱스, 속도 빠르게
5. 데이터베이스 생성
6. 데이터 로딩
7. 데이터 테스트
8. 데이터베이스 유지관리 → 정규화(성능 향상)

---

# 정규화

1. 제 1정규형(1NF) : 모든 속성(컬럼)이 원자값을 갖도록 하는 단계   [주민등록번호, 주소 등등 원자값으로]
2. 제 2정규형(2NF) : 테이블 모든 속성이 기본키에 완전 함수적 종속이 되도록하는 단계 [성적이 과목코드와 기본키인 학번에 종속된다. 과목에서 학생 테이블로 분리, 두개의 테이블로 분리]
3. 제 3 정규형(3NF) : 기본키를 제워한 속성들 간의 이행 종속성이 없어야 함 [ 성적은 학생과 관련없어서 분리]

---