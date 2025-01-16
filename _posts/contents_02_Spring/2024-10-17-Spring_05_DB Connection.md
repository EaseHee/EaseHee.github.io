---
layout: single
title: "Spring_05_DB Connection"
date: 2024-10-17
categories:
  - Spring
tags:
  - DB
  - JDBC
  - MyBatis
  - JPA
---

<br>

---


- ## 스프링 프레임워크의 DB 연결 방법 4가지
<pre><code>
1. 직접 PreparedStatement에 쿼리문을 전달.
2. JdbcDaoSupport 클래스 이용.
  : JdbcTemplate과 RowMapper를 이용.
3. MyBatis를 이용.
  : DataMapper.xml
    SqlMapperInterface (@Select)
4. JPA를 이용.
  : @Entity
</code></pre>


대략적인 흐름을 정리해보니 다음과 같았다. <br>
>
1> DB 연결 설정 <br>
2> DAO, DTO를 이용한 데이터 저장 및 전달 <br>
3> Business Logic <br>
4> Main <br>

<br>
결국 DB와 연결은 DAO가 담당하기 때문에 <br>
@Repository 스테레오 타입의 객체가 중추 역할을 수행한다. <br>
<br>

- ### 1. PreparedStatement 사용
> @Repository에서 직접 Connection 객체를 반환
<pre><code>
preparedStatement = connection.prepareStatement("SELECT no, name FROM emp");
resultSet = preparedStatement.executeQuery();
while(resultSet.next()) {
	DTO dto = new DTO();
	dto.setNo(resultSet.getInt("no"));
	dto.setName(resultSet.getInt("name"));
	list.add(dto);
}
</code></pre>
와 같은 방식으로 쿼리문이 DAO 객체 내에 삽입되어 있어서 기능의 분리가 되지 않은 상태. <br>
<br>


- ### 2. JdbcDaoSupport 상속
> @Repository에서 JdbcDaoSupport의 setDataSource()를 호출하여 DataSource를 전달. <br>
RowMapper 인터페이스의 mapRow() 메서드 오버라이딩. <br>
jdbcTemplate 객체의 query()를 호출하여 쿼리문과 매퍼를 인자로 전달. <br>


> cf. AOP를 이용하여 @Aspect에 부가 기능을 삽입하는 경우 
<pre><code>
1> 기본 인프라 설정 
    DTO 객체
    DataSource 객체 (@Component)

2> DB 연결 설정
    ** DAO 객체 (@Repository) **
      extends JdbcDaoSupport
      setDataSource(dataSource) -> 생성자에서 부모의 setter() 호출
      RowMapper.mapRow(ResultSet, rowNum)
      getJdbcTemplate().query(SQL, mapper) "return : List"

3> Business Logic
    @Service (핵심 로직)

4> Advice
    @Aspect, @Around("execution()")
    ProceedingJoinPoint -> proceed() 

5> 어노테이션 및 패키지 설정 
    @Configuration
    @ComponentScan
    @EnableAspectAutoProxy
</code></pre>

<br>
<hr>
<br>

> 전체 흐름 이해를 위한 예시 코드

- DataSource 
> DB 연결 설정 <br>
<strong>DriverManagerDataSource</strong> 상속<br>

<pre><code>
@Component
public class DataSource extends DriverManagerDataSource {
	
	public DataSource () {
		setDriverClassName("org.mariadb.jdbc.Driver");
		setUrl("jdbc:mariadb://svc.sel4.cloudtype.app:12345/test");
		setUsername("root");
		setPassword("1111");
	}
}
</code></pre>
<br>



- DTO
<pre><code>
@Data
public class Dto {
	private String number, data;
}
</code></pre>
<br>


- DAO implement
> DB 연결 <br>
@Repository <br>
<strong>JdbcDaoSupport</strong> 상속<br>
setDataSource() <br>
***** <br>
dataSource 변수를 필드 멤버로 선언하고 <br>
@Aurowired로 필드 인스턴스를 주입할 경우 <br>
자식 객체의 setter()로 호출되기 때문에 <br>
JdbcDaoSupport 클래스의 setDataSource()를 호출할 수 없다.<br><br>
또한 매개변수로 DataSource를 전달받지 않고 <br>
필드 변수에 @AutoWired를 이용한 Field Injection의 경우 <br>
생성자 호출 단계에서 setter()가 호출되지 않았기 때문에 객체의 인스턴스가 저장되지 않는다. <br>
***** <br>
RowMapper 인터페이스 내 mapRow() 오버라이딩 <br>
<pre><code>
@Repository
public class DaoImpl extends JdbcDaoSupport implements DaoInter {
	
	@Autowired
	public DaoImpl(DataSource dataSource) {
		setDataSource(dataSource);
	}
	@Override
	public List&lt;Dto&gt; selectList() {
		RowMapper&lt;Dto&gt; mapper = ((rs, rowNum) -> {
			Dto dto = new Dto();
			dto.setNumber(rs.getString("number"));
			dto.setData(rs.getString("data"));
			return dto;
		});
		return getJdbcTemplate().query("SELECT number, data FROM `table`", mapper);
	}
}
</code></pre>
<br>


- Business Logic implement 
> 핵심 로직 <br>
@Service <br>
<pre><code>
@Service
public class BusinessImpl implements BusinessInter {
	
	@Autowired
	private DaoInter DaoInter;
	
	@Override
	public void showData() {
		DaoInter.selectList().forEach(s -> {
			System.out.println(s.getNumber() + " " + s.getData());
		});
		System.out.println("건수 :  " + DaoInter.selectList().size());
	}
}
</code></pre>
<br>



- Advice <br>
> @Aspect <br>
@Around("execution(표현식)") : JoinPoint 검색 <br>
ProceedingJoinPoint.proceed() -> 핵심 로직 실행 메서드 <br>
<pre><code>
@Aspect
@Component
public class Advice {
	
	@Around("execution(public void showData())")
	public Object ok(ProceedingJoinPoint joinPoint) throws Throwable {
		if (!isChecked()) {
			System.out.println("잘못된 입력입니다.");
			return null;
		}
		return joinPoint.proceed();
	}
	
	public boolean isChecked() {
		// 별도의 클래스로 분리
		// 핵심 로직 실행 전 부가 기능 (ID, PW 확인)
		Scanner scanner = new Scanner(System.in);
		System.out.print("아이디를 입력 : ");
		String id = scanner.nextLine();
		
		System.out.print("비밀번호를 입력 : ");
		String pw = scanner.nextLine();

		if (!id.equals("ok") || !pw.equals("123")) {
			System.out.println("불일치");
			return false;
		}
		return true;
	}
}
</code></pre>
<br>


- Main
> @Configuration : 패키지 내 어노테이션 조회 설정 <br>
@ComponentScan : 패키지 검색 <br>
@EnableAspectJAutoProxy : 부가 기능(Aspect)을 검색하여 Advice를 삽입. <br>
<pre><code>
@Configuration
@ComponentScan(basePackages = "pack")
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class Main {
	public static void main(String[] args) {
		AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(Main.class);
		BusinessInter inter = context.getBean("businessImpl", BusinessInter.class);
        	inter.showData();
    }
}
</code></pre>
<br>



<br>

--- 

#### ❌ 오류 : 'dataSource' or 'jdbcTemplate' is required
<details>
<summary>DB 연결 오류</summary>
<pre><code>
1> DataSource 확인 
	DriverManagerDataSource 상속 여부 
	-> 생성자 호출 시 부모의 setDriverClassName(), setUrl(), setUsername(), setPassword() 호출
	@Component 어노테이션 작성 여부
2> @Repository 확인 
	JdbcDaoSupport 상속 여부 
	-> 생성자 호출 시 부모의 setDataSource() 호출 
	-> 쿼리문 실행 시 getJdbcTemplate() query() 호출 
</code></pre>
</details>

<br>
<hr>
<br>


- ### 3. MyBatis 
> 
1> <strong>&lt;mapper&gt;</strong>를 이용한 방식 <br> 
DataMapper.xml에 쿼리문을 분리하여 관리. <br>
Configuration.xml에서 &lt;mappers&gt; 를 이용하여 매핑. <br>
SqlMapConfig 객체에서 SqlSessionfactory를 이용하여 Configuration.xml 전달. <br>
<br>
2> <strong>@Select</strong> Annotation을 이용한 방식 <br>
SqlMapperInter에 쿼리문을 객체로 저장. <br>
SqlMapConfig.xml에서 팩토리 설정에 Mapper 추가. <br>
cf. Configuration은 DB 연결 설정만 저장. <br>


<br>

1> .xml : <strong>&lt;mapper&gt;</strong> <br>

> DataMapper.xml
<pre><code>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;

&lt;mapper namespace="dev"&gt;
  &lt;!-- sql 문장을 별도로 분리 --&gt;

  &lt;!-- select : PrepareStatement 사용 | resultType: resultSet "반환 티입 : List" --&gt;
  &lt;!-- connection - prepareStatement - resultSet 과정을 분리 --&gt;
  &lt;select id="selectDataAll" resultType="dto"&gt; &lt;!-- Alias로 입력해도 된다. "Configuration.xml에서 설정 --&gt;
    select code, sang, su, dan from sangdata &lt;!-- order by code asc 생략 가능 --&gt; &lt;!-- ; 은 작성하지 않는다. sql 은 문자열! --&gt;
  &lt;/select&gt;

  &lt;!-- 권장 : 가독성이 좋다 && 매핑은 #{} | id 는 객체의 변수 개념 --&gt;
  &lt;select id="selectDataById" parameterType="string" resultType="dto"&gt; &lt;!-- 입력할 매개변수 타입 : string / int 등 --&gt;
    select code, sang, su, dan from sangdata where code=#{code}
  &lt;/select&gt;

&lt;/mapper&gt;
</code></pre>


> Configuration.xml
<pre><code>
 &lt;!-- 매퍼 : sql을 관리하는 xml 파일 --&gt;
 &lt;mappers&gt; &lt;!-- 경로에 패키지 폴더 구성으로 입력 --&gt;
  &lt;mapper resource="pack/mybatis/DataMapper.xml" /&gt;
  &lt;!-- 인사담당, 재무담당 등 여러 기능이 추가되면 여기에 추가한다. --&gt;
 &lt;/mappers&gt;
</code></pre>




<br>

2> SqlMapperInter : <strong>@Select</strong> <br>

<pre><code>
package pack.model;

import java.util.List;

import org.apache.ibatis.annotations.Select;

// .xml을 대체할 매퍼 객체
public interface SqlMapperInter {
	// MyBatis의 Annotation @Select  
	@Select("select code, sang, su, dan from sangdata")
	List&lt;DataDto&gt; selectAll();
}
</code></pre>

<br>

- 데이터 엑세스 계층 흐름도 <br>

![ ](/assets/image/2024-10-17-데이터 엑세스 계층 흐름도.png)
> 출처 : [https://cafe.daum.net/flowlife/](https://cafe.daum.net/flowlife/HqLk/60)


<br>
<hr>
<br>


- ### 4. JPA
> Entity 객체 : DB 테이블에 매핑할 클래스 <br>
persistance.xml에 DB연결 정보 등 저장 <br>
<strong>@Entity</strong> Annotation 활용 <br>

<pre><code>
/**
 * *** JPA는 낙타 표기법으로 이름을 작성한 경우 언더스코어 네이밍 컨벤션으로 자동 변환한다. (스네이크 표기법) ***
 *  MemEntity -> mem_entity
 *  MeM -> me_m
 *  myName -> my_name
 * @Table 어노테이션으로 name 속성을 부여한다.
 */
@Entity // 실제 테이블과 매핑되는 클래스
@Table(name="mem") 
public class MemEntity { 
    // TODO 1017 Spring _ DB 04 _ JPA
    @Id
    @Column(name = "num") // 실제 테이블 내 필드명을 명시적으로 지정
	private int num; // 대문자를 섞으면 "_" 로 변환된다는 걸 항상 염두에 둘 것.

    /* 가독성을 위해서 명시하는 것이 좋다. */
    @Column(name = "name")
    private String name;
    private String addr;

    // JPA는 기본생성자를 호출하기 때문에 항상 만들어준다.
    public MemEntity(){};
    
    public MemEntity(int num, String name, String addr) {
        this.num = num;
        this.name = name;
        this.addr = addr;
    }
}
</code></pre>

<hr>

- ### 테이블을 조인하는 경우 <br>
@JoinColumn(name="필드명", referencedColumnName="참조 테이블 필드명") <br>
<br>
JPQL 작성 시 Entity(클래스)를 기준으로 작성하기 때문에 <br>
필드명과 클래스명, 참조할 변수명 또한 자바의 문법에 따라 대소문자를 구분하여 작성해야 한다. <br>
<br>
JPQL : "SELECT e.empno, e.ename, d.dname, e.job FROM Emp e JOIN e.dept d" <br>


> @Entity : Dept, Emp <br>

<pre><code>
@Entity
@Table(name = "DEPT")
public class Dept {
    /**
     * JOIN
     * @See model.Emp
     */
    @Id
    @Column(name = "DEPTNO")
    private int deptno;
    private String dname;
}
</code></pre>

<pre><code>
@Entity
@Table(name = "EMP")
public class Emp {
    @Id // PK
    @Column(name = "EMPNO")
    private int empno;
    
    private String ename, job;

    /**
     * JOIN 하는 경우 참조할 Entity를 멤버 변수로 선언.
     * @ManyToOne 테이블간 관계(1:1 vs 1:n)
     * @JoinColumn 참조할 테이블의 컬럼(필드)
     * @param name 현재 테이블 내 변수명
     * @param referencedColumnName 참조할 테이블 내 변수명
     * @See model.Dept
     */
    @ManyToOne
    @JoinColumn(name = "deptno", referencedColumnName="deptno")
    private Dept dept;
}
</code></pre>


> @Repository : DAO _ JPQL 전달 <br>

<pre><code>
/** 
 * JPQL은 엔티티(클래스)를 기준으로 쿼리문을 작성한다. 
 * 조인할 때에도 멤버변수로 선언한 변수(dept)를 기준으로 조인한다.
 * @JoinColumn(name = "deptno", referencedColumnName="deptno")
 */
String jpql = "SELECT e.empno, e.ename, e.job, d.dname FROM Emp e JOIN e.dept d";
/**
 * 조회한 레코드를 배열에 담는다. 
 * 각 레코드에는 필드들이 배열로 담겨있다.
 * 문자열과 정수형이 모두 담기기 때문에 Object로 저장한다.
 */ 
TypedQuery<Object[]> query = em.createQuery(jpql, Object[].class);
list = query.getResultList();
</code></pre>

> @Service : 비즈니스 로직 수행 <br>
Repository 인스턴스를 치환(저장) 후 다형성을 이용한 메서드 호출. <br>
<pre><code>
@Autowired
private DataInter dataInter;
@Override
public void showData() {
    /* 데이터 타입의 오류를 확인하지 않겠다는 주석 */
    @SuppressWarnings("unchecked")
    List<Object[]> list = dataInter.getList();
    
    /* Logger 한번 써보기.. */
    logger.info("empno ename job dname");
    
    list.forEach(result -> {
        logger.info(result[0] + " " 
                  + result[1] + " " 
                  + result[2] + " " 
                  + result[3]);
    });
}
</code></pre>

> JQPL 작성 시 엔티티 간 참조 관계
![](/assets/image/2024-10-19-JPA_JOIN_JPQL.png)
<br>



> 클래스와 Entity(테이블)의 관계에 대한 다이어그램...

![ ](/assets/image/2024-10-19-JPA_JOIN_UML.png) | ![](/assets/image/spacer.png)



<br>
<hr>
<br>




참고 사이트 <br>
[https://cafe.daum.net/flowlife/](https://cafe.daum.net/flowlife/HqLk/60) <br>
[https://skyblue300a.tistory.com/](https://skyblue300a.tistory.com/7) <br>