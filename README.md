# Spring boot 정리

> 참고 레퍼런스
> [점프 투 스프링 부트](https://wikidocs.net/book/7601)


---

Controller는 서버에 전달된 클라이언트의 요청을 처리하는 자바 클래스이다.

- URL 요청을 처리하고 폼은 사용자의 입력을 검증한다

Entity 클래스는 데이터베이스에 데이터를 저장하고 조회하기 위한 클래스이고 DTO 클래스는 데이터 베이스로 조회한 데이터들을 관리하기 위한 클래스이다. 

- Entity - DB 영역에서 데이터를 만들어서 던져줌
- DTO - 컨트롤러 ↔ 서비스 ↔  레포지토리 간의 데이터 전달
- 데이터를 관리하는 데 사용하는 ORM의 자바 클래스를 엔티티(entity)라고 한다. 엔티티는 데이터베이스의 테이블과 매핑되는 자바 클래스를 말한다.

### 엔티티 - 데이터 테이블 생성

- @Entity - 엔터티로 인식
- @Getter @Setter - Lombok 라이브러리 생성자 및 get,set 메서드를 작성하지 않아도 알아서 캄파일 시 생성
- @GeneratedValue - 자동으로 1씩 증가
- 엔티티의 속성은 @Column 애너테이션을 사용하지 않더라도 테이블의 열로 인식한다. 테이블의 열로 인식하고 싶지 않다면 @Transient 애너테이션을 사용한다.
- @ManyToOne N:1  @ManyToMany N:N

### annotation 정리

- @ResponseBody - 문자열을 리턴하라는 의미
- @Autowired - 스프링의 의존성 주입(DI: 스프링이 객체를 대신 생성하여 주입하는 기법)
- assertEquals(기댓값, 실제값) - 테스트에서 예상한 결과가 실제 결과와 동일한지를 확인
- Optional - 값을 처리하기 위한 클래스, null일 경우 유연하게 처리
- @Transactional - 테스트 코드에서 DB세션을 메서드가 종료될 때 까지 유지
- @RequiredArgsConstructor - final이 붙은 속성을 포함하는 생성자를 자동으로 만들어주는 역할
- @PathVariable - {question_id}같은 변하는 id값을 얻기 위해 사용, 파라미터를 매핑
- @Bean - 스프링에 의해 생성 또는 관리되는 객체를 의미 애너테이션을 통해 자바 코드 내에서 별도로 빈을 정의하고 등록
- @RestController - @RequestBody와 합쳐짐, JSON형태로 객체를 변환하기 위함
- @RequestParam - HTTP 요청을 파라미터 이름으로 바인딩
- @Data: @Getter, @Setter, @ToString, @EqualsAndHashCode, @RequiredArgsConstructor를 적용
- @ModelAttribute: 객체를 생성하고, 요청 파라미터 값을 자동으로 입력

Model - 자바 클래스와 템플릿 간의 연결고리 역할, 모델 객체의 값을 템플릿에서 그 값을 사용할 수 있다.

```jsx
model.addAttribute("questuonList", questionList);

        <tr th:each="question : ${questionList}">
            <td th:text="${question.subject}"></td>
            <td th:text="${question.createDate}"></td>
        </tr>
```

QuestionController의 list 메서드에서 조회한 질문 목록 데이터를 ‘questionList’라는 이름으로 Model 객체에 저장했다. 타임리프는 Model 객체에 저장한 questionList를 `${questionList}`로 읽을 수 있다. 위의 코드는 `questionList`에 저장된 데이터를 하나씩 꺼내 `question` 변수에 대입한 후 `questionList`의 개수만큼 반복하며 `<tr> ... </tr>` 문장을 출력하라는 의미이다. 자바의 for each 문을 떠올리면 쉽게 이해할 수 있을 것이다.

Service - 컨트롤러에서 리포지토리를 직접 호출하지 않고 중간에 서비스를 두어 데이터를 처리

- 복잡한 코드를 모듈화함
- 엔티티 객체를 DTO 객체로 변환할 수 있다. - 엔티티를 컨트롤러에서 직접 접근하면  좋지 않으므로 대신 사용할 DTO가 필요, 이 엔티티 객체와 DTO객체 끼리 변환하기 위해 서비스 사용

컨트롤러 → 서비스 → 레포지터리 순서로 접근

폼(form) - 사용자가 입력한 데이터를 검증

페이징 - JPA에 존재하는 패키지로 페이지를 처리할 때 사용

GET 방식에서 값을 전달하기 위해 ?와&기호를 사용한다. 첫 번째 파라미터는 ?기호를, 그 이후 추가되는 값은 &기호를 사용한다.
