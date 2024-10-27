---
layout: post
title: "Spring_03_@Annotation"
date: 2024-10-15
categories: blog
category: Spring
---

<br>

---
# Annotation
Annotation은 주석이라는 뜻이지만 프로그래밍에서는 컴파일에 포함되지 않는 코드를 <br>
주석, 혹은 /* Comment */라는 이름으로 부르고 있기 때문에 <br>
@Annotation은 그냥 어노테이션이라고 부른다. <br>
또한 자바에서의 어노테이션은 인터페이스와 같이 특수한 기능을 수행한다. <br>


어노테이션은 수행 기능에 따라 다음과 같이 구분한다.<br>
> 
1> 의존성 주입 (DI) <br>
2> 관점 지향 프로그래밍 (AOP) <br>
3> 웹 계층 (Layer) <br>
4> 데이터 계층 (Layer) <br>
5> 트랜젝션 <br>
<br>
0> 커스터마이징 <br>

<hr>

# 1> 의존성 주입
객체 간 의존성 관리 및 구성 요소 선언 <br>

<h1>☑️ org.springframework.stereotype</h1>

<details>
<summary class="summary-title">@Component</summary>
<li class="font-lg">org.springframework.stereotype.Component</li>
<li class="font-lg">의존성 주입을 위한 기본 Annotation</li>
<li class="font-lg">개발자가 직접 작성한 Class를 Bean으로 등록하기 위한 어노테이션</li>
<li class="font-lg">@Configuration(ApplicationContext)의 @ComponentScan을 통해 <br>
&emsp;&emsp;자동으로 검색되어 Bean으로 등록된다.</li>

<details>
<summary>예시</summary>
<div markdown="1">

```java
@Component(value="testComponent")
public class ComponentClass {
	// private ComponentClass testComponent = new ComponenClass(); 와 같다.
}
```
</div>
</details>
</details>

<details>
<summary class="summary-title">@Service</summary>
<li class="font-lg">org.springframework.stereotype.Service</li>
<li class="font-lg">비즈니스 로직을 수행하는 Component</li>
<li class="font-lg">@Component와 동일하게 동작하지만 비즈니스 로직을 수행하는 클래스임을 명시할 때 작성한다.</li>
<details>
<summary>예시</summary>
<div markdown="1">

```java
@Service // @Component와 동일. 비즈니스 로직을 수행함을 명시
public class ServiceClass {
	// private ServiceClass serviceClass = new ServiceClass();	
}
```
</div>
</details>
</details>

<details>
<summary class="summary-title">@Repository</summary>
<li class="font-lg">org.springframework.stereotype.Repository</li>
<li class="font-lg">DAO _ DB와의 상호작용을 담당</li>
<li class="font-lg">예외 변환 (Exception Translation)</li>
<details>
<summary>예시</summary>
<div markdown="1">

```java
@Repository
public class TestDao {
	@Autowired
	private TestRepository repository; // sql문을 구현한 인터페이스 필드 멤버로 선언

	public List<TestDto> select() {
		return repository.findAll(); // 인터페이스의 반환값을 리턴
	}
}
```
</div>
</details>
</details>

<details>
<summary class="summary-title">@Controller</summary>
<li class="font-lg">org.springframework.stereotype.Controller</li>
<li class="font-lg">Spring MVC Controller 클래스</li>
<li class="font-lg">요청 처리 & 응답 반환</li>
<li class="font-lg">@RequestMapping과 함께 쓰인다.</li>
<details>
<summary>예시</summary>
<div markdown="1">

```java
@Controller
public class TestController {
	@Autowired // DB와 연결할 객체 @Repository를 필드 멤버로 활용
	private Dao dao;

	@GetMapping("/") // 요청 페이지가 없는 경우 welcome 페이지로 redirect
	public String index() {
		return "index"; // welcome page
	}
	@PostMapping("search") // 검색어를 전달하며 페이지를 요청할 경우 FormBean에 저장하여 Dao 메서드 호출
	public String search(Bean bean, org.springframework.ui.Model model) {
		model.addAttribute("list", dao.search(bean));
		return "result"; // templates/result.html 로 forward
	}
}
```
</div>
</details>
</details>


<h1>☑️ org.springframework.beans.factory.annotation</h1>

<details>
<summary class="summary-title">@Autowired</summary>
<li class="font-lg">org.springframework.beans.factory.annotation.Autowired</li>
<li class="font-lg">Constructor, setter, field에 사용</li>
<li class="font-lg">타입에 의한 매핑 (객체에 대한 의존성 : Bean 주입)</li>

<details>
<summary class="summary-title">Spring의 Bean 주입 3가지 방법</summary>
<ol>
<li class="font-lg">@Autowired</li>
<li class="font-lg">setter</li>
<li class="font-lg">@AllArgsConstructor (권장)</li>
</ol>
</details>

<details>
<summary>예시</summary>
<div markdown="1">

```java
@어노테이션
public class 클래스 {
	@Aurowired // 구현체를 주입받는다 
	private TestInterface interface; // 필드 인젝션

	@Autowired
	private Constructor(TestClass test) { // 생성자 인젝션
		this test = test;
	}
}
```
</div>
</details>
</details>


<details>
<summary class="summary-title">@Qualifier</summary>
<li class="font-lg">org.springframework.beans.factory.annotation.Qualifier</li>
<li class="font-lg">Bean으로 등록된 객체의 ID를 통해 주입받는다</li>
<li class="font-lg">@Autowired와 함께 쓰이며 타입이 아닌 Bean의 Id에 매핑할 때 쓰인다.</li>
<li class="font-lg">
	Bean객체의 ID를 찾지 못할 경우 <br>
	⛔️ 'NoSuchBeanDefinitionException' 발생 <br>
&emsp;&emsp;자연스럽게 Bean Id에 오타는 없는지 확인한다.
</li>
<details>
<summary>예시</summary>
<div markdown="1">

```java
@어노테이션
public class 클래스 {
	@Aurowired 
	@Qualifier(value="testImpl") // Bean의 ID와 매핑
	private TestInterface interface;
}
```
</div>
</details>
</details>


<details>
<summary class="summary-title">@Value</summary>
<li class="font-lg">org.springframework.beans.factory.annotation.Value</li>
<li class="font-lg">설정 파일(application.properties 또는 application.yml)에서 값을 읽어와 주입할 때 사용</li>
<li class="font-lg">멤버에 작성한다. _ 외부 클래스의 name값을 주입</li>
<details>
<summary>예시</summary>
<div markdown="1">

```java
	@Value("dataInfo.name") // 1> 외부 클래스의 name 값을 주입
	private String name; // name = dataInfo.getName(); 과 동일
	private String part;

	@Autowired // 어노테이션을 작성하지 않으면 주입되지 않는다. (당연한 얘기) 
	// xml 에서 <constructor-arg value="전산부"/> 를 입력 시 xml 설정값이 주입된다.
	public MyProcess(@Value("총무부") String part) { // @Value("#{dataInfo.part}") String part 가능
		this.part = part;	// 2> 생성자를 통해 part가 주입된다.
	}
```
</div>
</details>

<details>
<summary>예시 전문</summary>
<div markdown="1">

```java
package anno3_etc;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

@Service // 비즈니스 로직을 수행할 클래스 : Service
public class MyProcess {
	// TODO 1015 Spring 05 _ 기타 어노테이션 _ SpEL _ @Value
	// SpEL : 동적으로 표현식을 해석.
	// #{표현식} : @Component로 작성된 클래스를 앞글자 소문자의 변수에 접근하여 getter()를 호출할 수 있다.

	// @Value는 멤버에 작성한다. : 1> 외부 클래스의 name값을 주입
	@Value("#{dataInfo.name}") // getName() 메서드를 호출한다.
	private String name;
	private String part;

	public MyProcess() {
		
	}
	
	@Autowired // 어노테이션을 작성하지 않으면 주입되지 않는다. (당연한 얘기) 
	// xml 에서 <constructor-arg value="전산부"/> 를 입력 시 xml 설정값이 주입된다.
	public MyProcess(@Value("총무부") String part) { // @Value("#{dataInfo.part}") String part 가능
		this.part = part;	// 2> 생성자를 통해 part가 주입된다.
	}
	
	@Value("#{dataInfo.job}") // public 으로 선언했기 때문에 바로 접근할 수 있다.
	private String job; 
	
	@Value("100")		// 새로운 변수에 값을 주입할 수도 있다.
	private int height;
	
	@Value("1, 2, 3, 4")	// 배열을 저장할 수도 있다.
	private int arr[];
	
	public void showResult () {
		System.out.println("name : " + name);
		System.out.println("part : " + part);
		System.out.println("job : " + job);
		System.out.println("height + 3 : " + (height + 3));
		System.out.println("arr : " + arr[0] + " " + arr[3]);
	}
}
```
</div>
</details>
</details>


<h1>☑️ org.springframework.context.annotation</h1>

<details>
<summary class="summary-title">@Bean</summary>
<li class="font-lg">org.springframework.context.annotation.Bean</li>
<li class="font-lg">개발자가 직접 제어가 불가능한 외부 라이브러리 등을 Bean으로 만들 때 사용되는 어노테이션</li>
<li class="font-lg">메서드를 통해서 Bean 정의</li>
<li class="font-lg">메서드의 반환 객체를 Spring 컨테이너 Bean으로 등록</li>
<details>
<summary>예시</summary>
<div markdown="1">

```java
@어노테이션
public class 클래스 {
	// ArrayList를 반환하며 Bean으로 등록한다. 
	// ArrayList<String> testArrayList = new ArrayList<>(); 와 동일
	@Bean(name="testArrayList") 
	public ArrayList<String> array() {
		return new ArrayList<String>();
	}
}
```
</div>
</details>
</details>


<details>
<summary class="summary-title">@Configuration</summary>
<li class="font-lg">org.springframework.context.annotation.Configuration</li>
<li class="font-lg">Spring 설정 클래스를 정의</li>

<details>
<summary>예시</summary>

<pre><code>
 &#064;Configuration
 public class AppConfig {
     &#064;Bean
     public MyBean myBean() {
         // instantiate, configure and return bean ...
     }
 }
</code></pre>

</details>

<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java
/*
 * Copyright 2002-2023 the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package org.springframework.context.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.annotation.AliasFor;
import org.springframework.stereotype.Component;

/**
 * Indicates that a class declares one or more {@link Bean @Bean} methods and
 * may be processed by the Spring container to generate bean definitions and
 * service requests for those beans at runtime, for example:
 *
 * <pre class="code">
 * &#064;Configuration
 * public class AppConfig {
 *
 *     &#064;Bean
 *     public MyBean myBean() {
 *         // instantiate, configure and return bean ...
 *     }
 * }</pre>
 *
 * <h2>Bootstrapping {@code @Configuration} classes</h2>
 *
 * <h3>Via {@code AnnotationConfigApplicationContext}</h3>
 *
 * <p>{@code @Configuration} classes are typically bootstrapped using either
 * {@link AnnotationConfigApplicationContext} or its web-capable variant,
 * {@link org.springframework.web.context.support.AnnotationConfigWebApplicationContext
 * AnnotationConfigWebApplicationContext}. A simple example with the former follows:
 *
 * <pre class="code">
 * AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
 * ctx.register(AppConfig.class);
 * ctx.refresh();
 * MyBean myBean = ctx.getBean(MyBean.class);
 * // use myBean ...
 * </pre>
 *
 * <p>See the {@link AnnotationConfigApplicationContext} javadocs for further details, and see
 * {@link org.springframework.web.context.support.AnnotationConfigWebApplicationContext
 * AnnotationConfigWebApplicationContext} for web configuration instructions in a
 * {@code Servlet} container.
 *
 * <h3>Via Spring {@code <beans>} XML</h3>
 *
 * <p>As an alternative to registering {@code @Configuration} classes directly against an
 * {@code AnnotationConfigApplicationContext}, {@code @Configuration} classes may be
 * declared as normal {@code <bean>} definitions within Spring XML files:
 *
 * <pre class="code">
 * &lt;beans&gt;
 *    &lt;context:annotation-config/&gt;
 *    &lt;bean class="com.acme.AppConfig"/&gt;
 * &lt;/beans&gt;
 * </pre>
 *
 * <p>In the example above, {@code <context:annotation-config/>} is required in order to
 * enable {@link ConfigurationClassPostProcessor} and other annotation-related
 * post processors that facilitate handling {@code @Configuration} classes.
 *
 * <h3>Via component scanning</h3>
 *
 * <p>Since {@code @Configuration} is meta-annotated with {@link Component @Component},
 * {@code @Configuration} classes are candidates for component scanning &mdash;
 * for example, using {@link ComponentScan @ComponentScan} or Spring XML's
 * {@code <context:component-scan/>} element &mdash; and therefore may also take
 * advantage of {@link Autowired @Autowired}/{@link jakarta.inject.Inject @Inject}
 * like any regular {@code @Component}. In particular, if a single constructor is
 * present, autowiring semantics will be applied transparently for that constructor:
 *
 * <pre class="code">
 * &#064;Configuration
 * public class AppConfig {
 *
 *     private final SomeBean someBean;
 *
 *     public AppConfig(SomeBean someBean) {
 *         this.someBean = someBean;
 *     }
 *
 *     // &#064;Bean definition using "SomeBean"
 *
 * }</pre>
 *
 * <p>{@code @Configuration} classes may not only be bootstrapped using component
 * scanning, but may also themselves <em>configure</em> component scanning using
 * the {@link ComponentScan @ComponentScan} annotation:
 *
 * <pre class="code">
 * &#064;Configuration
 * &#064;ComponentScan("com.acme.app.services")
 * public class AppConfig {
 *     // various &#064;Bean definitions ...
 * }</pre>
 *
 * <p>See the {@link ComponentScan @ComponentScan} javadocs for details.
 *
 * <h2>Working with externalized values</h2>
 *
 * <h3>Using the {@code Environment} API</h3>
 *
 * <p>Externalized values may be looked up by injecting the Spring
 * {@link org.springframework.core.env.Environment} into a {@code @Configuration}
 * class &mdash; for example, using the {@code @Autowired} annotation:
 *
 * <pre class="code">
 * &#064;Configuration
 * public class AppConfig {
 *
 *     &#064;Autowired Environment env;
 *
 *     &#064;Bean
 *     public MyBean myBean() {
 *         MyBean myBean = new MyBean();
 *         myBean.setName(env.getProperty("bean.name"));
 *         return myBean;
 *     }
 * }</pre>
 *
 * <p>Properties resolved through the {@code Environment} reside in one or more "property
 * source" objects, and {@code @Configuration} classes may contribute property sources to
 * the {@code Environment} object using the {@link PropertySource @PropertySource}
 * annotation:
 *
 * <pre class="code">
 * &#064;Configuration
 * &#064;PropertySource("classpath:/com/acme/app.properties")
 * public class AppConfig {
 *
 *     &#064;Inject Environment env;
 *
 *     &#064;Bean
 *     public MyBean myBean() {
 *         return new MyBean(env.getProperty("bean.name"));
 *     }
 * }</pre>
 *
 * <p>See the {@link org.springframework.core.env.Environment Environment}
 * and {@link PropertySource @PropertySource} javadocs for further details.
 *
 * <h3>Using the {@code @Value} annotation</h3>
 *
 * <p>Externalized values may be injected into {@code @Configuration} classes using
 * the {@link Value @Value} annotation:
 *
 * <pre class="code">
 * &#064;Configuration
 * &#064;PropertySource("classpath:/com/acme/app.properties")
 * public class AppConfig {
 *
 *     &#064;Value("${bean.name}") String beanName;
 *
 *     &#064;Bean
 *     public MyBean myBean() {
 *         return new MyBean(beanName);
 *     }
 * }</pre>
 *
 * <p>This approach is often used in conjunction with Spring's
 * {@link org.springframework.context.support.PropertySourcesPlaceholderConfigurer
 * PropertySourcesPlaceholderConfigurer} that can be enabled <em>automatically</em>
 * in XML configuration via {@code <context:property-placeholder/>} or <em>explicitly</em>
 * in a {@code @Configuration} class via a dedicated {@code static} {@code @Bean} method
 * (see "a note on BeanFactoryPostProcessor-returning {@code @Bean} methods" of
 * {@link Bean @Bean}'s javadocs for details). Note, however, that explicit registration
 * of a {@code PropertySourcesPlaceholderConfigurer} via a {@code static} {@code @Bean}
 * method is typically only required if you need to customize configuration such as the
 * placeholder syntax, etc. Specifically, if no bean post-processor (such as a
 * {@code PropertySourcesPlaceholderConfigurer}) has registered an <em>embedded value
 * resolver</em> for the {@code ApplicationContext}, Spring will register a default
 * <em>embedded value resolver</em> which resolves placeholders against property sources
 * registered in the {@code Environment}. See the section below on composing
 * {@code @Configuration} classes with Spring XML using {@code @ImportResource}; see
 * the {@link Value @Value} javadocs; and see the {@link Bean @Bean} javadocs for details
 * on working with {@code BeanFactoryPostProcessor} types such as
 * {@code PropertySourcesPlaceholderConfigurer}.
 *
 * <h2>Composing {@code @Configuration} classes</h2>
 *
 * <h3>With the {@code @Import} annotation</h3>
 *
 * <p>{@code @Configuration} classes may be composed using the {@link Import @Import} annotation,
 * similar to the way that {@code <import>} works in Spring XML. Because
 * {@code @Configuration} objects are managed as Spring beans within the container,
 * imported configurations may be injected &mdash; for example, via constructor injection:
 *
 * <pre class="code">
 * &#064;Configuration
 * public class DatabaseConfig {
 *
 *     &#064;Bean
 *     public DataSource dataSource() {
 *         // instantiate, configure and return DataSource
 *     }
 * }
 *
 * &#064;Configuration
 * &#064;Import(DatabaseConfig.class)
 * public class AppConfig {
 *
 *     private final DatabaseConfig dataConfig;
 *
 *     public AppConfig(DatabaseConfig dataConfig) {
 *         this.dataConfig = dataConfig;
 *     }
 *
 *     &#064;Bean
 *     public MyBean myBean() {
 *         // reference the dataSource() bean method
 *         return new MyBean(dataConfig.dataSource());
 *     }
 * }</pre>
 *
 * <p>Now both {@code AppConfig} and the imported {@code DatabaseConfig} can be bootstrapped
 * by registering only {@code AppConfig} against the Spring context:
 *
 * <pre class="code">
 * new AnnotationConfigApplicationContext(AppConfig.class);</pre>
 *
 * <h3>With the {@code @Profile} annotation</h3>
 *
 * <p>{@code @Configuration} classes may be marked with the {@link Profile @Profile} annotation to
 * indicate they should be processed only if a given profile or profiles are <em>active</em>:
 *
 * <pre class="code">
 * &#064;Profile("development")
 * &#064;Configuration
 * public class EmbeddedDatabaseConfig {
 *
 *     &#064;Bean
 *     public DataSource dataSource() {
 *         // instantiate, configure and return embedded DataSource
 *     }
 * }
 *
 * &#064;Profile("production")
 * &#064;Configuration
 * public class ProductionDatabaseConfig {
 *
 *     &#064;Bean
 *     public DataSource dataSource() {
 *         // instantiate, configure and return production DataSource
 *     }
 * }</pre>
 *
 * <p>Alternatively, you may also declare profile conditions at the {@code @Bean} method level
 * &mdash; for example, for alternative bean variants within the same configuration class:
 *
 * <pre class="code">
 * &#064;Configuration
 * public class ProfileDatabaseConfig {
 *
 *     &#064;Bean("dataSource")
 *     &#064;Profile("development")
 *     public DataSource embeddedDatabase() { ... }
 *
 *     &#064;Bean("dataSource")
 *     &#064;Profile("production")
 *     public DataSource productionDatabase() { ... }
 * }</pre>
 *
 * <p>See the {@link Profile @Profile} and {@link org.springframework.core.env.Environment}
 * javadocs for further details.
 *
 * <h3>With Spring XML using the {@code @ImportResource} annotation</h3>
 *
 * <p>As mentioned above, {@code @Configuration} classes may be declared as regular Spring
 * {@code <bean>} definitions within Spring XML files. It is also possible to
 * import Spring XML configuration files into {@code @Configuration} classes using
 * the {@link ImportResource @ImportResource} annotation. Bean definitions imported from
 * XML can be injected &mdash; for example, using the {@code @Inject} annotation:
 *
 * <pre class="code">
 * &#064;Configuration
 * &#064;ImportResource("classpath:/com/acme/database-config.xml")
 * public class AppConfig {
 *
 *     &#064;Inject DataSource dataSource; // from XML
 *
 *     &#064;Bean
 *     public MyBean myBean() {
 *         // inject the XML-defined dataSource bean
 *         return new MyBean(this.dataSource);
 *     }
 * }</pre>
 *
 * <h3>With nested {@code @Configuration} classes</h3>
 *
 * <p>{@code @Configuration} classes may be nested within one another as follows:
 *
 * <pre class="code">
 * &#064;Configuration
 * public class AppConfig {
 *
 *     &#064;Inject DataSource dataSource;
 *
 *     &#064;Bean
 *     public MyBean myBean() {
 *         return new MyBean(dataSource);
 *     }
 *
 *     &#064;Configuration
 *     static class DatabaseConfig {
 *         &#064;Bean
 *         DataSource dataSource() {
 *             return new EmbeddedDatabaseBuilder().build();
 *         }
 *     }
 * }</pre>
 *
 * <p>When bootstrapping such an arrangement, only {@code AppConfig} need be registered
 * against the application context. By virtue of being a nested {@code @Configuration}
 * class, {@code DatabaseConfig} <em>will be registered automatically</em>. This avoids
 * the need to use an {@code @Import} annotation when the relationship between
 * {@code AppConfig} and {@code DatabaseConfig} is already implicitly clear.
 *
 * <p>Note also that nested {@code @Configuration} classes can be used to good effect
 * with the {@code @Profile} annotation to provide two options of the same bean to the
 * enclosing {@code @Configuration} class.
 *
 * <h2>Configuring lazy initialization</h2>
 *
 * <p>By default, {@code @Bean} methods will be <em>eagerly instantiated</em> at container
 * bootstrap time.  To avoid this, {@code @Configuration} may be used in conjunction with
 * the {@link Lazy @Lazy} annotation to indicate that all {@code @Bean} methods declared
 * within the class are by default lazily initialized. Note that {@code @Lazy} may be used
 * on individual {@code @Bean} methods as well.
 *
 * <h2>Testing support for {@code @Configuration} classes</h2>
 *
 * <p>The Spring <em>TestContext framework</em> available in the {@code spring-test} module
 * provides the {@code @ContextConfiguration} annotation which can accept an array of
 * <em>component class</em> references &mdash; typically {@code @Configuration} or
 * {@code @Component} classes.
 *
 * <pre class="code">
 * &#064;ExtendWith(SpringExtension.class)
 * &#064;ContextConfiguration(classes = {AppConfig.class, DatabaseConfig.class})
 * class MyTests {
 *
 *     &#064;Autowired MyBean myBean;
 *
 *     &#064;Autowired DataSource dataSource;
 *
 *     &#064;Test
 *     void test() {
 *         // assertions against myBean ...
 *     }
 * }</pre>
 *
 * <p>See the
 * <a href="https://docs.spring.io/spring/docs/current/spring-framework-reference/testing.html#testcontext-framework">TestContext framework</a>
 * reference documentation for details.
 *
 * <h2>Enabling built-in Spring features using {@code @Enable} annotations</h2>
 *
 * <p>Spring features such as asynchronous method execution, scheduled task execution,
 * annotation driven transaction management, and even Spring MVC can be enabled and
 * configured from {@code @Configuration} classes using their respective "{@code @Enable}"
 * annotations. See
 * {@link org.springframework.scheduling.annotation.EnableAsync @EnableAsync},
 * {@link org.springframework.scheduling.annotation.EnableScheduling @EnableScheduling},
 * {@link org.springframework.transaction.annotation.EnableTransactionManagement @EnableTransactionManagement},
 * {@link org.springframework.context.annotation.EnableAspectJAutoProxy @EnableAspectJAutoProxy},
 * and {@link org.springframework.web.servlet.config.annotation.EnableWebMvc @EnableWebMvc}
 * for details.
 *
 * <h2>Constraints when authoring {@code @Configuration} classes</h2>
 *
 * <ul>
 * <li>Configuration classes must be provided as classes (i.e. not as instances returned
 * from factory methods), allowing for runtime enhancements through a generated subclass.
 * <li>Configuration classes must be non-final (allowing for subclasses at runtime),
 * unless the {@link #proxyBeanMethods() proxyBeanMethods} flag is set to {@code false}
 * in which case no runtime-generated subclass is necessary.
 * <li>Configuration classes must be non-local (i.e. may not be declared within a method).
 * <li>Any nested configuration classes must be declared as {@code static}.
 * <li>{@code @Bean} methods may not in turn create further configuration classes
 * (any such instances will be treated as regular beans, with their configuration
 * annotations remaining undetected).
 * </ul>
 *
 * @author Rod Johnson
 * @author Chris Beams
 * @author Juergen Hoeller
 * @since 3.0
 * @see Bean
 * @see Profile
 * @see Import
 * @see ImportResource
 * @see ComponentScan
 * @see Lazy
 * @see PropertySource
 * @see AnnotationConfigApplicationContext
 * @see ConfigurationClassPostProcessor
 * @see org.springframework.core.env.Environment
 * @see org.springframework.test.context.ContextConfiguration
 */
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {

	/**
	 * Explicitly specify the name of the Spring bean definition associated with the
	 * {@code @Configuration} class. If left unspecified (the common case), a bean
	 * name will be automatically generated.
	 * <p>The custom name applies only if the {@code @Configuration} class is picked
	 * up via component scanning or supplied directly to an
	 * {@link AnnotationConfigApplicationContext}. If the {@code @Configuration} class
	 * is registered as a traditional XML bean definition, the name/id of the bean
	 * element will take precedence.
	 * <p>Alias for {@link Component#value}.
	 * @return the explicit component name, if any (or empty String otherwise)
	 * @see AnnotationBeanNameGenerator
	 */
	@AliasFor(annotation = Component.class)
	String value() default "";

	/**
	 * Specify whether {@code @Bean} methods should get proxied in order to enforce
	 * bean lifecycle behavior, e.g. to return shared singleton bean instances even
	 * in case of direct {@code @Bean} method calls in user code. This feature
	 * requires method interception, implemented through a runtime-generated CGLIB
	 * subclass which comes with limitations such as the configuration class and
	 * its methods not being allowed to declare {@code final}.
	 * <p>The default is {@code true}, allowing for 'inter-bean references' via direct
	 * method calls within the configuration class as well as for external calls to
	 * this configuration's {@code @Bean} methods, e.g. from another configuration class.
	 * If this is not needed since each of this particular configuration's {@code @Bean}
	 * methods is self-contained and designed as a plain factory method for container use,
	 * switch this flag to {@code false} in order to avoid CGLIB subclass processing.
	 * <p>Turning off bean method interception effectively processes {@code @Bean}
	 * methods individually like when declared on non-{@code @Configuration} classes,
	 * a.k.a. "@Bean Lite Mode" (see {@link Bean @Bean's javadoc}). It is therefore
	 * behaviorally equivalent to removing the {@code @Configuration} stereotype.
	 * @since 5.2
	 */
	boolean proxyBeanMethods() default true;

	/**
	 * Specify whether {@code @Bean} methods need to have unique method names,
	 * raising an exception otherwise in order to prevent accidental overloading.
	 * <p>The default is {@code true}, preventing accidental method overloads which
	 * get interpreted as overloaded factory methods for the same bean definition
	 * (as opposed to separate bean definitions with individual conditions etc).
	 * Switch this flag to {@code false} in order to allow for method overloading
	 * according to those semantics, accepting the risk for accidental overlaps.
	 * @since 6.0
	 */
	boolean enforceUniqueMethods() default true;

}
```
</div>
</details>
</details>


<details>
<summary class="summary-title">@EnableAspectJAutoProxy</summary>
<li class="font-lg">org.springframework.context.annotation.EnableAspectJAutoProxy</li>
<li class="font-lg">@Aspect 어노테이션이 있는 부가 기능을 처리</li>
<li class="font-lg">@Configuration과 함께 쓰인다.</li>
<li class="font-lg">&lt;aop:aspectj-autoproxy&gt; XML element와 유사</li>
<br>
<li class="font-lg">Spring AOP : Dynamic Proxy _ 인터페이스를 구현해서 객체에 간접적으로 접근</li>
<li class="font-lg">AspectJ AOP : BCI (Byte Code Instrumentation) _ 바이트 코드를 직접 수정</li>
<br>
<li class="font-lg">(proxyTargetClass = true) : </li>

<details>
<summary>API 내용 확인</summary>

<details>
<summary>View Comments</summary>

<div markdown="1">

```java
/*
 * Copyright 2002-2021 the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package org.springframework.context.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * Enables support for handling components marked with AspectJ's {@code @Aspect} annotation,
 * similar to functionality found in Spring's {@code <aop:aspectj-autoproxy>} XML element.
 * To be used on @{@link Configuration} classes as follows:
 *
 * <pre class="code">
 * &#064;Configuration
 * &#064;EnableAspectJAutoProxy
 * public class AppConfig {
 *
 *     &#064;Bean
 *     public FooService fooService() {
 *         return new FooService();
 *     }
 *
 *     &#064;Bean
 *     public MyAspect myAspect() {
 *         return new MyAspect();
 *     }
 * }</pre>
 *
 * Where {@code FooService} is a typical POJO component and {@code MyAspect} is an
 * {@code @Aspect}-style aspect:
 *
 * <pre class="code">
 * public class FooService {
 *
 *     // various methods
 * }</pre>
 *
 * <pre class="code">
 * &#064;Aspect
 * public class MyAspect {
 *
 *     &#064;Before("execution(* FooService+.*(..))")
 *     public void advice() {
 *         // advise FooService methods as appropriate
 *     }
 * }</pre>
 *
 * In the scenario above, {@code @EnableAspectJAutoProxy} ensures that {@code MyAspect}
 * will be properly processed and that {@code FooService} will be proxied mixing in the
 * advice that it contributes.
 *
 * <p>Users can control the type of proxy that gets created for {@code FooService} using
 * the {@link #proxyTargetClass()} attribute. The following enables CGLIB-style 'subclass'
 * proxies as opposed to the default interface-based JDK proxy approach.
 *
 * <pre class="code">
 * &#064;Configuration
 * &#064;EnableAspectJAutoProxy(proxyTargetClass=true)
 * public class AppConfig {
 *     // ...
 * }</pre>
 *
 * <p>Note that {@code @Aspect} beans may be component-scanned like any other.
 * Simply mark the aspect with both {@code @Aspect} and {@code @Component}:
 *
 * <pre class="code">
 * package com.foo;
 *
 * &#064;Component
 * public class FooService { ... }
 *
 * &#064;Aspect
 * &#064;Component
 * public class MyAspect { ... }</pre>
 *
 * Then use the @{@link ComponentScan} annotation to pick both up:
 *
 * <pre class="code">
 * &#064;Configuration
 * &#064;ComponentScan("com.foo")
 * &#064;EnableAspectJAutoProxy
 * public class AppConfig {
 *
 *     // no explicit &#064;Bean definitions required
 * }</pre>
 *
 * <b>Note: {@code @EnableAspectJAutoProxy} applies to its local application context only,
 * allowing for selective proxying of beans at different levels.</b> Please redeclare
 * {@code @EnableAspectJAutoProxy} in each individual context, e.g. the common root web
 * application context and any separate {@code DispatcherServlet} application contexts,
 * if you need to apply its behavior at multiple levels.
 *
 * <p>This feature requires the presence of {@code aspectjweaver} on the classpath.
 * While that dependency is optional for {@code spring-aop} in general, it is required
 * for {@code @EnableAspectJAutoProxy} and its underlying facilities.
 *
 * @author Chris Beams
 * @author Juergen Hoeller
 * @since 3.1
 * @see org.aspectj.lang.annotation.Aspect
 */
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import(AspectJAutoProxyRegistrar.class)
public @interface EnableAspectJAutoProxy {

	/**
	 * Indicate whether subclass-based (CGLIB) proxies are to be created as opposed
	 * to standard Java interface-based proxies. The default is {@code false}.
	 */
	boolean proxyTargetClass() default false;

	/**
	 * Indicate that the proxy should be exposed by the AOP framework as a {@code ThreadLocal}
	 * for retrieval via the {@link org.springframework.aop.framework.AopContext} class.
	 * Off by default, i.e. no guarantees that {@code AopContext} access will work.
	 * @since 4.3.1
	 */
	boolean exposeProxy() default false;

}
```
</div>


</details>

<pre><code>
&#064;Configuration
&#064;EnableAspectJAutoProxy(proxyTargetClass = true)
public class AppConfig {
    &#064;Bean
    public FooService fooService() {
        return new FooService();
    }
    &#064;Bean
    public MyAspect myAspect() {
        return new MyAspect();
    }
}
</code></pre>
<br>

<pre>
FooService는 일반 클래스이고
MyAspect 클래스는 @Aspect의 부가 기능에 해당한다.
FooService의 모든 메서드가 실행되기 이전에 MyAspect의 advice()가 실행되도록 한다.
</pre>

<br>
<pre><code>
public class FooService {
    // various methods
}
</code></pre>

<pre><code>
&#064;Aspect
public class MyAspect {
    &#064;Before("execution(* FooService+.*(..))")
    public void advice() {
        // advise FooService methods as appropriate
    }
}
</code></pre>

</details>
</details>



<details>
<summary class="summary-title">@ComponentScan</summary>
<li class="font-lg">org.springframework.context.annotation.ComponentScan</li>
<li class="font-lg">패키지를 설정하여 패키지 내 컴포넌트 스캔</li>
<li class="font-lg">
	대상 파일이 ApplicationContext 파일이 속한 패키지 외부에 있는 경우 <br>
&emsp;&ensp;명시적으로 패키지를 작성해야한다.<br>
</li>

<details>
<summary>API 내용 확인</summary>

<details>
<summary>View Comments</summary>
<div markdown="1">

```java
/*
 * Copyright 2002-2023 the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package org.springframework.context.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Repeatable;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.beans.factory.support.BeanNameGenerator;
import org.springframework.core.annotation.AliasFor;
import org.springframework.core.type.filter.TypeFilter;

/**
 * Configures component scanning directives for use with {@link Configuration @Configuration}
 * classes.
 *
 * <p>Provides support comparable to Spring's {@code <context:component-scan>}
 * XML namespace element.
 *
 * <p>Either {@link #basePackageClasses} or {@link #basePackages} (or its alias
 * {@link #value}) may be specified to define specific packages to scan. If specific
 * packages are not defined, scanning will occur recursively beginning with the
 * package of the class that declares this annotation.
 *
 * <p>Note that the {@code <context:component-scan>} element has an
 * {@code annotation-config} attribute; however, this annotation does not. This is because
 * in almost all cases when using {@code @ComponentScan}, default annotation config
 * processing (e.g. processing {@code @Autowired} and friends) is assumed. Furthermore,
 * when using {@link AnnotationConfigApplicationContext}, annotation config processors are
 * always registered, meaning that any attempt to disable them at the
 * {@code @ComponentScan} level would be ignored.
 *
 * <p>See {@link Configuration @Configuration}'s Javadoc for usage examples.
 *
 * <p>{@code @ComponentScan} can be used as a <em>{@linkplain Repeatable repeatable}</em>
 * annotation. {@code @ComponentScan} may also be used as a <em>meta-annotation</em>
 * to create custom <em>composed annotations</em> with attribute overrides.
 *
 * <p>Locally declared {@code @ComponentScan} annotations always take precedence
 * over and effectively <em>hide</em> {@code @ComponentScan} meta-annotations,
 * which allows explicit local configuration to override configuration that is
 * <em>meta-present</em> (including composed annotations meta-annotated with
 * {@code @ComponentScan}).
 *
 * @author Chris Beams
 * @author Juergen Hoeller
 * @author Sam Brannen
 * @since 3.1
 * @see Configuration
 */
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Repeatable(ComponentScans.class)
public @interface ComponentScan {

	/**
	 * Alias for {@link #basePackages}.
	 * <p>Allows for more concise annotation declarations if no other attributes
	 * are needed &mdash; for example, {@code @ComponentScan("org.my.pkg")}
	 * instead of {@code @ComponentScan(basePackages = "org.my.pkg")}.
	 */
	@AliasFor("basePackages")
	String[] value() default {};

	/**
	 * Base packages to scan for annotated components.
	 * <p>{@link #value} is an alias for (and mutually exclusive with) this
	 * attribute.
	 * <p>Use {@link #basePackageClasses} for a type-safe alternative to
	 * String-based package names.
	 */
	@AliasFor("value")
	String[] basePackages() default {};

	/**
	 * Type-safe alternative to {@link #basePackages} for specifying the packages
	 * to scan for annotated components. The package of each class specified will be scanned.
	 * <p>Consider creating a special no-op marker class or interface in each package
	 * that serves no purpose other than being referenced by this attribute.
	 */
	Class<?>[] basePackageClasses() default {};

	/**
	 * The {@link BeanNameGenerator} class to be used for naming detected components
	 * within the Spring container.
	 * <p>The default value of the {@link BeanNameGenerator} interface itself indicates
	 * that the scanner used to process this {@code @ComponentScan} annotation should
	 * use its inherited bean name generator, e.g. the default
	 * {@link AnnotationBeanNameGenerator} or any custom instance supplied to the
	 * application context at bootstrap time.
	 * @see AnnotationConfigApplicationContext#setBeanNameGenerator(BeanNameGenerator)
	 * @see AnnotationBeanNameGenerator
	 * @see FullyQualifiedAnnotationBeanNameGenerator
	 */
	Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;

	/**
	 * The {@link ScopeMetadataResolver} to be used for resolving the scope of detected components.
	 */
	Class<? extends ScopeMetadataResolver> scopeResolver() default AnnotationScopeMetadataResolver.class;

	/**
	 * Indicates whether proxies should be generated for detected components, which may be
	 * necessary when using scopes in a proxy-style fashion.
	 * <p>The default is defer to the default behavior of the component scanner used to
	 * execute the actual scan.
	 * <p>Note that setting this attribute overrides any value set for {@link #scopeResolver}.
	 * @see ClassPathBeanDefinitionScanner#setScopedProxyMode(ScopedProxyMode)
	 */
	ScopedProxyMode scopedProxy() default ScopedProxyMode.DEFAULT;

	/**
	 * Controls the class files eligible for component detection.
	 * <p>Consider use of {@link #includeFilters} and {@link #excludeFilters}
	 * for a more flexible approach.
	 */
	String resourcePattern() default ClassPathScanningCandidateComponentProvider.DEFAULT_RESOURCE_PATTERN;

	/**
	 * Indicates whether automatic detection of classes annotated with {@code @Component}
	 * {@code @Repository}, {@code @Service}, or {@code @Controller} should be enabled.
	 */
	boolean useDefaultFilters() default true;

	/**
	 * Specifies which types are eligible for component scanning.
	 * <p>Further narrows the set of candidate components from everything in {@link #basePackages}
	 * to everything in the base packages that matches the given filter or filters.
	 * <p>Note that these filters will be applied in addition to the default filters, if specified.
	 * Any type under the specified base packages which matches a given filter will be included,
	 * even if it does not match the default filters (i.e. is not annotated with {@code @Component}).
	 * @see #resourcePattern()
	 * @see #useDefaultFilters()
	 */
	Filter[] includeFilters() default {};

	/**
	 * Specifies which types are not eligible for component scanning.
	 * @see #resourcePattern
	 */
	Filter[] excludeFilters() default {};

	/**
	 * Specify whether scanned beans should be registered for lazy initialization.
	 * <p>Default is {@code false}; switch this to {@code true} when desired.
	 * @since 4.1
	 */
	boolean lazyInit() default false;


	/**
	 * Declares the type filter to be used as an {@linkplain ComponentScan#includeFilters
	 * include filter} or {@linkplain ComponentScan#excludeFilters exclude filter}.
	 */
	@Retention(RetentionPolicy.RUNTIME)
	@Target({})
	@interface Filter {

		/**
		 * The type of filter to use.
		 * <p>Default is {@link FilterType#ANNOTATION}.
		 * @see #classes
		 * @see #pattern
		 */
		FilterType type() default FilterType.ANNOTATION;

		/**
		 * Alias for {@link #classes}.
		 * @see #classes
		 */
		@AliasFor("classes")
		Class<?>[] value() default {};

		/**
		 * The class or classes to use as the filter.
		 * <p>The following table explains how the classes will be interpreted
		 * based on the configured value of the {@link #type} attribute.
		 * <table border="1">
		 * <tr><th>{@code FilterType}</th><th>Class Interpreted As</th></tr>
		 * <tr><td>{@link FilterType#ANNOTATION ANNOTATION}</td>
		 * <td>the annotation itself</td></tr>
		 * <tr><td>{@link FilterType#ASSIGNABLE_TYPE ASSIGNABLE_TYPE}</td>
		 * <td>the type that detected components should be assignable to</td></tr>
		 * <tr><td>{@link FilterType#CUSTOM CUSTOM}</td>
		 * <td>an implementation of {@link TypeFilter}</td></tr>
		 * </table>
		 * <p>When multiple classes are specified, <em>OR</em> logic is applied
		 * &mdash; for example, "include types annotated with {@code @Foo} OR {@code @Bar}".
		 * <p>Custom {@link TypeFilter TypeFilters} may optionally implement any of the
		 * following {@link org.springframework.beans.factory.Aware Aware} interfaces, and
		 * their respective methods will be called prior to {@link TypeFilter#match match}:
		 * <ul>
		 * <li class="font-lg">{@link org.springframework.context.EnvironmentAware EnvironmentAware}</li>
		 * <li class="font-lg">{@link org.springframework.beans.factory.BeanFactoryAware BeanFactoryAware}
		 * <li class="font-lg">{@link org.springframework.beans.factory.BeanClassLoaderAware BeanClassLoaderAware}
		 * <li class="font-lg">{@link org.springframework.context.ResourceLoaderAware ResourceLoaderAware}
		 * </ul>
		 * <p>Specifying zero classes is permitted but will have no effect on component
		 * scanning.
		 * @since 4.2
		 * @see #value
		 * @see #type
		 */
		@AliasFor("value")
		Class<?>[] classes() default {};

		/**
		 * The pattern (or patterns) to use for the filter, as an alternative
		 * to specifying a Class {@link #value}.
		 * <p>If {@link #type} is set to {@link FilterType#ASPECTJ ASPECTJ},
		 * this is an AspectJ type pattern expression. If {@link #type} is
		 * set to {@link FilterType#REGEX REGEX}, this is a regex pattern
		 * for the fully-qualified class names to match.
		 * @see #type
		 * @see #classes
		 */
		String[] pattern() default {};

	}

}
```
</div>
</details>

<div markdown="1">

```java
package org.springframework.context.annotation;
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Repeatable(ComponentScans.class)
public @interface ComponentScan {

    /**
     * Alias for {@link #basePackages}.
     * <p>Allows for more concise annotation declarations if no other attributes
     * are needed &mdash; for example, {@code @ComponentScan("org.my.pkg")}
     * instead of {@code @ComponentScan(basePackages = "org.my.pkg")}.
     */
    @AliasFor("basePackages")
    String[] value() default {};

    /**
     * Base packages to scan for annotated components.
     * <p>{@link #value} is an alias for (and mutually exclusive with) this
     * attribute.
     * <p>Use {@link #basePackageClasses} for a type-safe alternative to
     * String-based package names.
     */
    @AliasFor("value")
    String[] basePackages() default {};
```
</div>
</details>

</details>

<hr>

# 2> AOP
핵심 로직에 추가할 부가 기능(횡단 관심사항)을 모듈화하는 방법 제공

![](/assets/image/2024-10-16-Spring_AOP_용어.png)
> AOP 단계별 용어

<br>

<details>
<summary class="summary-title">@Aspect</summary>
<li class="font-lg">org.springframework.stereotype.Aspect</li>
<li class="font-lg">AOP에서 사용하는 관점(Aspect)을 정의하는 클래스</li>
<li class="font-lg">포인트컷(Pointcut)과 어드바이스(Advice)를 포함</li>

<details>
<summary>예시 : 로그 출력 Advice</summary>

<div markdown="1">

```java
package aopex2;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;
import org.springframework.util.StopWatch;

@Aspect // @Aspect (부가 기능)
@Component // 클래스
public class LoggingAdvice {

    // @Around : 핵심 로직 앞뒤에 Aspect(부가 기능) 추가
    @Around("execution(* *..*LogicInter*.*(..)) or execution(public void selectAll())") 
    public Object logging(ProceedingJoinPoint joinPoint) throws Throwable {
        // 핵심 메서드 수행 전 처리 전 내용 "Aspect" : 인증(로그인), 트랜젝션, 로그 출력 등
        // JoinPoint.getSignature() : 설정된 메서드명을 반환 "return : Signature (인터페이스)"
        String methodName = String.valueOf(joinPoint.getSignature()); 
        
        // org.springframework.util.StopWatch : 소요 시간 확인용
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        System.out.println(methodName + " 시작 전 작업");
        
        /* 핵심 로직 중 설정된 메서드 : pointcut _ "interceptor 대상" */
        Object object = joinPoint.proceed(); // Annotation 적용 시 뒤에 가려져서 보이지 않는다. (숨어서 실행)

        System.out.println(methodName + " 처리 후 마무리");
        stopWatch.stop();
        System.out.println("처리 소요 시간 : " + stopWatch.getTotalTimeMillis());

        return object;
    }
}
```
</div>
</details>
</details>


<h1>☑️ org.aspectj.lang.annotation</h1>

<h3>JdkRegexpMethodPointcut : 정규 표현식을 이용하여 Pointcut을 정의</h3>
<h3>AspectJExpressionPointcut : AspectJ 정규 표현식을 이용하여 Pointcut을 정의</h3>
<h3>&emsp; : execution(반환값패키지.클래스.메서드(인수))</h3>

<details>
<summary class="summary-title">@Before</summary>
<li class="font-lg">org.aspectj.lang.annotation.Before</li>
<li class="font-lg">핵심 로직 메서드가 실행되기 전에 부가 기능 로직 실행</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@After</summary>
<li class="font-lg">org.aspectj.lang.annotation.After</li>
<li class="font-lg">핵심 로직 메서드가 실행된 후에 부가 기능 실행</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@AfterReturning</summary>
<li class="font-lg">org.aspectj.lang.annotation.AfterReturning</li>
<li class="font-lg">핵심 로직 메서드가 반환값을 반환한 후 부가 기능 실행</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@AfterThrowing</summary>
<li class="font-lg">org.aspectj.lang.annotation.AfterThrowing</li>
<li class="font-lg">핵심 로직 메서드가 예외를 발생시킨 후 부가 기능 실행</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@Around</summary>
<li class="font-lg">org.aspectj.lang.annotation.Around</li>
<li class="font-lg">핵심 로직 메서드 실행 전후 부가 기능 로직 실행</li>
<li class="font-lg">메서드 실행을 가로채고 제어</li>
<details>
<summary>예시</summary>
<div markdown="1">

```java
// @Around : 핵심 로직 앞뒤에 Aspect(부가 기능) 추가
@Around("execution(* *..*LogicInter*.*(..)) or execution(public void selectAll())") 
public Object logging(ProceedingJoinPoint joinPoint) throws Throwable {
    // 핵심 메서드 수행 전 처리 전 내용 "Aspect" : 인증(로그인), 트랜젝션, 로그 출력 등
    // JoinPoint.getSignature() : 설정된 메서드명을 반환 "return : Signature (인터페이스)"
    String methodName = String.valueOf(joinPoint.getSignature()); 
    
    // org.springframework.util.StopWatch : 소요 시간 확인용
    StopWatch stopWatch = new StopWatch();
    stopWatch.start();
    System.out.println(methodName + " 시작 전 작업");
    
    /* 핵심 로직 중 설정된 메서드 : pointcut _ "interceptor 대상" */
    Object object = joinPoint.proceed(); // Annotation 적용 시 뒤에 가려져서 보이지 않는다. (숨어서 실행)

    System.out.println(methodName + " 처리 후 마무리");
    stopWatch.stop();
    System.out.println("처리 소요 시간 : " + stopWatch.getTotalTimeMillis());

    return object;
}
```
</div>
</details>
</details>


<hr>

# 3> Web Layer
Spring MVC와 관련된 웹 어플리케이션 개발에 사용되는 어노테이션

<h1>☑️ org.springframework.web.bind.annotation</h1>

<details>
<summary class="summary-title">@RequestMapping</summary>
<li class="font-lg">org.springframework.web.bind.annotation.RequestMapping</li>
<li class="font-lg">Handler Adapter</li>
<li class="font-lg">HTTP 요청을 특정 메서드 또는 클래스에 매핑</li>
<li class="font-lg">GET, POST, PUT, DELETE 요청 처리</li>

<details>
<summary>예시</summary>
<div markdown="1">

```java
@Controller
@RequestMapping("test") // "test"로 시작하는 요청은 모두 처리
public class TestController {
	@Autowired
	private TestDao dao;

	// "test/list"의 GET 방식 요청
	@RequestMapping(value = "list", method=RequestMethod.GET)
	public org.springframework.ui.ModelAndView list() {
		return new org.springframework.ui.ModelAndView("list").addObject("list", dao.selectAll());
		// 응답 : "templates/list.html"로 forward // 변수명 "list"에 List를 저장하여 View에 전달
	}
	// "test/insert"의 POST 방식 요청 _ 입력 데이터 Bean에 저장 후 전달
	@RequestMapping(value = "insert", method=RequestMethod.POST)
	public String insert(Bean bean) {
		return "redirect:list"; // "list"로 반환 시 forward로 요청되기 때문에 list 출력을 위해 redirect로 명시적으로 요청
	}
	/**
	 * .html 파일은 클라이언트 측 전송 방식으로 redirect 방식으로 전송한다.
	 * redirect 방식으로 전송할 경우 반드시 명시해준다. 
	 * 기본값 : forward -> ViewResolver에서 전달되어 prefix, suffix가 추가된다.
	 */
}
```
</div>
</details>

<details>
<summary>예시 정리본</summary>
<div markdown="1">

```java
package pack;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.servlet.ModelAndView;


/**
 * @Controller
 * DispatcherServlet -> Handler Mapper를 거쳐서 요청을 받는다
 * 인스턴스 생성, 클라이언트와 데이터 입출력을 제어하는 클래스에 적용
 * 클라이언트의 요청을 처리한 후 지정된 view에 모델 객체를 전달하는 역할
 */
//@Controller

/**
 * @ResponseBody 
 * 일반 문자열, Map, JSON 등의 Java 객체를 http 응답 본문 객체로 변환하여 클라이언트로 전송
 */
//@ResponseBody 

/**
 * @RestController 
 * @Controller + @ResponseBody
 */
/*
@RestController 
public class TestController {
	@RequestMapping("test1")
	public String abc() {
		System.out.println("abc()처리");
		return "금요일이다~";
	} //  JSON으로 반환할 수도 있다.
}
*/

@Controller
public class TestController {
    // @RequestMapping _ 클라이언트의 요청(urlPatterns 값)을 처리 : GET, POST 모두 처리
	@RequestMapping("test1")  // forward 방식의 전송을 하기 때문에 URL에 test1이 노출된다.
    public ModelAndView abc() {
		System.out.println("abc() 처리");
		
		// Model에서 데이터를 가져왔다는 가정
		String result = "모델 반환 정보";
        // return null;
		
		// 모델 반환 정보를 뷰(jsp)로 전달
        /* 
		ModelAndView modelAndView = new ModelAndView();
		modelAndView.setViewName("list"); // "/WEB-INF/views/list.jsp"
		// WEB-INF 폴더는 forwarding으로만 접근할 수 있다.
		// ModelAndView .addObject() : 서블릿의 request.setAttribue("msg", result); 와 동일하다. 
		modelAndView.addObject("msg", result); 
		return modelAndView; // forward
        */

        /* ViewName, AttributeKey, AttributeValue 를 생성자로 전달. (생성자 오버로딩) */
        return new ModelAndView("list", "msg", result);
    }

    @RequestMapping(value="test2", method=RequestMethod.GET)
    public ModelAndView abc2() {
        return new ModelAndView("list", "msg", "요청 처리 성공 2");
    }
    // 위와 동일 _ 방식을 설정해서 Mapping하는 경우
    @GetMapping("test3")
    public ModelAndView abc3() {
        return new ModelAndView("list", "msg", "요청 처리 성공 3");
    }

    // org.springframework.ui.Model 객체를 활용해서 Attribute로 Key, Value를 추가
    // *** 반환값 : ViewName ***
    @GetMapping("test4")
    public String abc4(Model model) {
        model.addAttribute("msg", "요청 처리 성공4");
        return "list";
    }

    @RequestMapping(value = "test5", method=RequestMethod.POST)
    public String abc5(Model model) {
        model.addAttribute("msg", String.valueOf(model.getClass()) + "요청 처리 성공5");
        return "list";
    }

    @PostMapping(value = "test6")
    public ModelAndView abc6() {
    	return new ModelAndView("list", "msg", "요청 처리 성공 6");
    }
    
    @PostMapping("test7")
    public String abc7(Model model) {
    	model.addAttribute("msg", "POST 요청 처리 성공 7");
    	return "list";
    }
    
    @GetMapping({"test8", "test8_2"}) // 배열로 받을 수 있다.
    @ResponseBody // 일반 문자열, Map, JSON 등의 Java 객체를 전달할 수 있다.  
    public String abc8() {
        return "일반 문자열, Map, JSON 등의 Java 객체를 전달할 수 있다.";
    }
    
    @GetMapping("test8_1")
    @ResponseBody // 일반 문자열, Map, JSON 등의 Java 객체를 전달할 수 있다. 
    public DataVo abc8_1() {
        DataVo dataVo = new DataVo();
        dataVo.setCode(10);
        dataVo.setName("가을비");
        return dataVo; // JSON 타입으로 출력된다. "pretty print 선택 가능"
    }
}
```
</div>
</details>


<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java
/*
 * Copyright 2002-2024 the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package org.springframework.web.bind.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.aot.hint.annotation.Reflective;
import org.springframework.core.annotation.AliasFor;

/**
 * Annotation for mapping web requests onto methods in request-handling classes
 * with flexible method signatures.
 *
 * <p>Both Spring MVC and Spring WebFlux support this annotation through a
 * {@code RequestMappingHandlerMapping} and {@code RequestMappingHandlerAdapter}
 * in their respective modules and package structures. For the exact list of
 * supported handler method arguments and return types in each, please use the
 * reference documentation links below:
 * <ul>
 * <li>Spring MVC
 * <a href="https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-ann-arguments">Method Arguments</a>
 * and
 * <a href="https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-ann-return-types">Return Values</a>
 * </li>
 * <li>Spring WebFlux
 * <a href="https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html#webflux-ann-arguments">Method Arguments</a>
 * and
 * <a href="https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html#webflux-ann-return-types">Return Values</a>
 * </li>
 * </ul>
 *
 * <p><strong>NOTE:</strong> This annotation can be used both at the class and
 * at the method level. In most cases, at the method level applications will
 * prefer to use one of the HTTP method specific variants
 * {@link GetMapping @GetMapping}, {@link PostMapping @PostMapping},
 * {@link PutMapping @PutMapping}, {@link DeleteMapping @DeleteMapping}, or
 * {@link PatchMapping @PatchMapping}.
 *
 * <p><strong>NOTE:</strong> This annotation cannot be used in conjunction with
 * other {@code @RequestMapping} annotations that are declared on the same element
 * (class, interface, or method). If multiple {@code @RequestMapping} annotations
 * are detected on the same element, a warning will be logged, and only the first
 * mapping will be used. This also applies to composed {@code @RequestMapping}
 * annotations such as {@code @GetMapping}, {@code @PostMapping}, etc.
 *
 * <p><b>NOTE:</b> When using controller interfaces (e.g. for AOP proxying),
 * make sure to consistently put <i>all</i> your mapping annotations &mdash; such
 * as {@code @RequestMapping} and {@code @SessionAttributes} &mdash; on
 * the controller <i>interface</i> rather than on the implementation class.
 *
 * @author Juergen Hoeller
 * @author Arjen Poutsma
 * @author Sam Brannen
 * @since 2.5
 * @see GetMapping
 * @see PostMapping
 * @see PutMapping
 * @see DeleteMapping
 * @see PatchMapping
 */
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Mapping
@Reflective(ControllerMappingReflectiveProcessor.class)
public @interface RequestMapping {

	/**
	 * Assign a name to this mapping.
	 * <p><b>Supported at the type level as well as at the method level!</b>
	 * When used on both levels, a combined name is derived by concatenation
	 * with "#" as separator.
	 * @see org.springframework.web.servlet.mvc.method.annotation.MvcUriComponentsBuilder
	 * @see org.springframework.web.servlet.handler.HandlerMethodMappingNamingStrategy
	 */
	String name() default "";

	/**
	 * The path mapping URIs &mdash; for example, {@code "/profile"}.
	 * <p>This is an alias for {@link #path}. For example,
	 * {@code @RequestMapping("/profile")} is equivalent to
	 * {@code @RequestMapping(path="/profile")}.
	 * <p>See {@link #path} for further details.
	 */
	@AliasFor("path")
	String[] value() default {};

	/**
	 * The path mapping URIs &mdash; for example, {@code "/profile"}.
	 * <p>Ant-style path patterns are also supported (e.g. {@code "/profile/**"}).
	 * At the method level, relative paths (e.g. {@code "edit"}) are supported
	 * within the primary mapping expressed at the type level.
	 * Path mapping URIs may contain placeholders (e.g. <code>"/${profile_path}"</code>).
	 * <p><b>Supported at the type level as well as at the method level!</b>
	 * When used at the type level, all method-level mappings inherit
	 * this primary mapping, narrowing it for a specific handler method.
	 * <p><strong>NOTE</strong>: A handler method that is not mapped to any path
	 * explicitly is effectively mapped to an empty path.
	 * @since 4.2
	 */
	@AliasFor("value")
	String[] path() default {};

	/**
	 * The HTTP request methods to map to, narrowing the primary mapping:
	 * GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE.
	 * <p><b>Supported at the type level as well as at the method level!</b>
	 * When used at the type level, all method-level mappings inherit this
	 * HTTP method restriction.
	 */
	RequestMethod[] method() default {};

	/**
	 * The parameters of the mapped request, narrowing the primary mapping.
	 * <p>Same format for any environment: a sequence of "myParam=myValue" style
	 * expressions, with a request only mapped if each such parameter is found
	 * to have the given value. Expressions can be negated by using the "!=" operator,
	 * as in "myParam!=myValue". "myParam" style expressions are also supported,
	 * with such parameters having to be present in the request (allowed to have
	 * any value). Finally, "!myParam" style expressions indicate that the
	 * specified parameter is <i>not</i> supposed to be present in the request.
	 * <p><b>Supported at the type level as well as at the method level!</b>
	 * When used at the type level, all method-level mappings inherit this
	 * parameter restriction.
	 */
	String[] params() default {};

	/**
	 * The headers of the mapped request, narrowing the primary mapping.
	 * <p>Same format for any environment: a sequence of "My-Header=myValue" style
	 * expressions, with a request only mapped if each such header is found
	 * to have the given value. Expressions can be negated by using the "!=" operator,
	 * as in "My-Header!=myValue". "My-Header" style expressions are also supported,
	 * with such headers having to be present in the request (allowed to have
	 * any value). Finally, "!My-Header" style expressions indicate that the
	 * specified header is <i>not</i> supposed to be present in the request.
	 * <p>Also supports media type wildcards (*), for headers such as Accept
	 * and Content-Type. For instance,
	 * <pre class="code">
	 * &#064;RequestMapping(value = "/something", headers = "content-type=text/*")
	 * </pre>
	 * will match requests with a Content-Type of "text/html", "text/plain", etc.
	 * <p><b>Supported at the type level as well as at the method level!</b>
	 * When used at the type level, all method-level mappings inherit this
	 * header restriction.
	 * @see org.springframework.http.MediaType
	 */
	String[] headers() default {};

	/**
	 * Narrows the primary mapping by media types that can be consumed by the
	 * mapped handler. Consists of one or more media types one of which must
	 * match to the request {@code Content-Type} header. Examples:
	 * <pre class="code">
	 * consumes = "text/plain"
	 * consumes = {"text/plain", "application/*"}
	 * consumes = MediaType.TEXT_PLAIN_VALUE
	 * </pre>
	 * <p>If a declared media type contains a parameter, and if the
	 * {@code "content-type"} from the request also has that parameter, then
	 * the parameter values must match. Otherwise, if the media type from the
	 * request {@code "content-type"} does not contain the parameter, then the
	 * parameter is ignored for matching purposes.
	 * <p>Expressions can be negated by using the "!" operator, as in
	 * "!text/plain", which matches all requests with a {@code Content-Type}
	 * other than "text/plain".
	 * <p><b>Supported at the type level as well as at the method level!</b>
	 * If specified at both levels, the method level consumes condition overrides
	 * the type level condition.
	 * @see org.springframework.http.MediaType
	 * @see jakarta.servlet.http.HttpServletRequest#getContentType()
	 */
	String[] consumes() default {};

	/**
	 * Narrows the primary mapping by media types that can be produced by the
	 * mapped handler. Consists of one or more media types one of which must
	 * be chosen via content negotiation against the "acceptable" media types
	 * of the request. Typically those are extracted from the {@code "Accept"}
	 * header but may be derived from query parameters, or other. Examples:
	 * <pre class="code">
	 * produces = "text/plain"
	 * produces = {"text/plain", "application/*"}
	 * produces = MediaType.TEXT_PLAIN_VALUE
	 * produces = "text/plain;charset=UTF-8"
	 * </pre>
	 * <p>If a declared media type contains a parameter (e.g. "charset=UTF-8",
	 * "type=feed", "type=entry") and if a compatible media type from the request
	 * has that parameter too, then the parameter values must match. Otherwise,
	 * if the media type from the request does not contain the parameter, it is
	 * assumed the client accepts any value.
	 * <p>Expressions can be negated by using the "!" operator, as in "!text/plain",
	 * which matches all requests with a {@code Accept} other than "text/plain".
	 * <p><b>Supported at the type level as well as at the method level!</b>
	 * If specified at both levels, the method level produces condition overrides
	 * the type level condition.
	 * @see org.springframework.http.MediaType
	 */
	String[] produces() default {};

}
```
</div>
</details>
</details>


<details>
<summary>
<b>@GetMapping<br>
&emsp;@PostMapping<br>
&emsp;@PutMapping<br>
&emsp;@DeleteMapping</b></summary>
<li class="font-lg">org.springframework.web.bind.annotation.GetMapping</li>
<span class="font-lg">&emsp; org.springframework.web.bind.annotation.PostMapping</span> <br>
<span class="font-lg">&emsp; org.springframework.web.bind.annotation.PutMapping</span> <br>
<span class="font-lg">&emsp; org.springframework.web.bind.annotation.DeleteMapping</span> <br>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java
@RequestMapping("test") // "test"로 시작하는 요청은 모두 처리
public class TestController {
	@Autowired
	private TestDao dao;

	// "test/list"의 GET 방식 요청
	@GetMapping("list")
	public org.springframework.ui.ModelAndView list() {
		return new org.springframework.ui.ModelAndView("list").addObject("list", dao.selectAll());
		// 응답 : "templates/list.html"로 forward // 변수명 "list"에 List를 저장하여 View에 전달
	}
	// "test/insert"의 POST 방식 요청 _ 입력 데이터 Bean에 저장 후 전달
	@PostMapping("insert")
	public String insert(Bean bean) {
		return "redirect:list"; // "list"로 반환 시 forward로 요청되기 때문에 list 출력을 위해 redirect로 명시적으로 요청
	}
}
```
</div>
</details>
</details>


<details>
<summary class="summary-title">@RequestParam</summary>
<li class="font-lg">org.springframework.web.bind.annotation.RequestParam</li>
<li class="font-lg">
<pre>
Spring MVC 에서는 Request Parameters가 
쿼리 변수, 폼 데이터, 복수 요청 변수에 매핑된다.
그 이유는 Servlet API가 쿼리 변수와 폼 데이터와 결합하여 
"Parameters"라고 불리는 하나의 Map으로 결합되기 때문인데 
이 단일 맵은 Request Body로부터 자동 parsing된 것을 포함한다.
</pre>
</li>
<details>
<summary>예시</summary>
<div markdown="1">

```java
/**
 * @RequestParam(value) 요청_변수를_어노테이션으로_입력받을_수_있다.
 * *** 입력 데이터가 많은 경우에는 FormBean을 활용해야 한다. ***
 * @param id
 * @param pwd
 * @return
 */
@PostMapping("login")
public String submit(@RequestParam(value="id") String id, @RequestParam(value="pwd") String/* int 로 작성하면 자동 캐스팅된다. */ pwd, Model model) {
	logger.info(id + " " + pwd);
	String data = "";
	
	if ("aa".equals(id) && "11".equals(pwd)) {
		data = "로그인 성공";
	} else {
		data = "로그인 실패";
	}
	model.addAttribute("data", data);
	return "result";
}
```
</div>
</details>

<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java
/*
 * Copyright 2002-2018 the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package org.springframework.web.bind.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.util.Map;

import org.springframework.core.annotation.AliasFor;

/**
 * Annotation which indicates that a method parameter should be bound to a web
 * request parameter.
 *
 * <p>Supported for annotated handler methods in Spring MVC and Spring WebFlux
 * as follows:
 * <ul>
 * <li>In Spring MVC, "request parameters" map to query parameters, form data,
 * and parts in multipart requests. This is because the Servlet API combines
 * query parameters and form data into a single map called "parameters", and
 * that includes automatic parsing of the request body.
 * <li>In Spring WebFlux, "request parameters" map to query parameters only.
 * To work with all 3, query, form data, and multipart data, you can use data
 * binding to a command object annotated with {@link ModelAttribute}.
 * </ul>
 *
 * <p>If the method parameter type is {@link Map} and a request parameter name
 * is specified, then the request parameter value is converted to a {@link Map}
 * assuming an appropriate conversion strategy is available.
 *
 * <p>If the method parameter is {@link java.util.Map Map&lt;String, String&gt;} or
 * {@link org.springframework.util.MultiValueMap MultiValueMap&lt;String, String&gt;}
 * and a parameter name is not specified, then the map parameter is populated
 * with all request parameter names and values.
 *
 * @author Arjen Poutsma
 * @author Juergen Hoeller
 * @author Sam Brannen
 * @since 2.5
 * @see RequestMapping
 * @see RequestHeader
 * @see CookieValue
 */
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestParam {

	/**
	 * Alias for {@link #name}.
	 */
	@AliasFor("name")
	String value() default "";

	/**
	 * The name of the request parameter to bind to.
	 * @since 4.2
	 */
	@AliasFor("value")
	String name() default "";

	/**
	 * Whether the parameter is required.
	 * <p>Defaults to {@code true}, leading to an exception being thrown
	 * if the parameter is missing in the request. Switch this to
	 * {@code false} if you prefer a {@code null} value if the parameter is
	 * not present in the request.
	 * <p>Alternatively, provide a {@link #defaultValue}, which implicitly
	 * sets this flag to {@code false}.
	 */
	boolean required() default true;

	/**
	 * The default value to use as a fallback when the request parameter is
	 * not provided or has an empty value.
	 * <p>Supplying a default value implicitly sets {@link #required} to
	 * {@code false}.
	 */
	String defaultValue() default ValueConstants.DEFAULT_NONE;

}
```
</div>
</details>
</details>


<details>
<summary class="summary-title">@RequestBody</summary>
<li class="font-lg">org.springframework.web.bind.annotation.RequestBod</li>
<li class="font-lg">HTTP 요청 본문을 객체로 변환하여 받을 때 사용</li>
<li class="font-lg"></li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@ResponseBody</summary>
<li class="font-lg">org.springframework.web.bind.annotation.ResponseBody</li>
<li class="font-lg">메서드 반환값을 응답 본문에 직접 넣어 반환</li>
<li class="font-lg">일반 문자열, Map, JSON 등의 Java 객체를 http 응답 본문 객체로 변환하여 클라이언트로 전송</li>
<li class="font-lg">메서드의 반환값이 Web의 Response Body에 전달된다. annotated handler 메서드에 의해 처리된다.</li>

<details>
<summary>예시</summary>
<div markdown="1">

```java
@GetMapping({"test8", "test8_2"}) // 배열로 받을 수 있다.
@ResponseBody // 일반 문자열, Map, JSON 등의 Java 객체를 전달할 수 있다.  
public String abc8() {
    String value = "일반 문자열, Map, JSON 등의 Java 객체를 전달할 수 있다.";
    return value;
}
    
@GetMapping("test8_1")
@ResponseBody // 일반 문자열, Map, JSON 등의 Java 객체를 전달할 수 있다. 
public DataVo abc8_1() {
    DataVo dataVo = new DataVo();
    dataVo.setCode(10);
    dataVo.setName("가을비");
    return dataVo; // JSON 타입으로 출력된다. "pretty print 선택 가능"
}
```
</div>
</details>


<details>
<summary>API 내용 확인</summary>

<div markdown="1">

```java
package org.springframework.web.bind.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * Annotation that indicates a method return value should be bound to the web
 * response body. Supported for annotated handler methods.
 *
 * <p>As of version 4.0 this annotation can also be added on the type level in
 * which case it is inherited and does not need to be added on the method level.
 *
 * @author Arjen Poutsma
 * @since 3.0
 * @see RequestBody
 * @see RestController
 */
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ResponseBody {

}
```
</div>
</details>

</details>


<details>
<summary class="summary-title">@RestController</summary>
<li class="font-lg">org.springframework.web.bind.annotation.RestController</li>
<li class="font-lg">@Controller + @ResponseBody</li>
<li class="font-lg">JSON 또는 XML 형식의 데이터를 반환하는 컨트롤러</li>

<details>
<summary>예시</summary>
<div markdown="1">

```java
@RestController 
public class TestController {
	@RequestMapping("test") // method=RequestMethod.XXX 를 명시하지 않으면 GET, POST 모두 처리
	public String returnJson() {
		return "JSON으로 반환";
	}
}
```
</div>
</details>


<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java
package org.springframework.web.bind.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.core.annotation.AliasFor;
import org.springframework.stereotype.Controller;

/**
 * A convenience annotation that is itself annotated with
 * {@link Controller @Controller} and {@link ResponseBody @ResponseBody}.
 * <p>
 * Types that carry this annotation are treated as controllers where
 * {@link RequestMapping @RequestMapping} methods assume
 * {@link ResponseBody @ResponseBody} semantics by default.
 *
 * <p><b>NOTE:</b> {@code @RestController} is processed if an appropriate
 * {@code HandlerMapping}-{@code HandlerAdapter} pair is configured such as the
 * {@code RequestMappingHandlerMapping}-{@code RequestMappingHandlerAdapter}
 * pair which are the default in the MVC Java config and the MVC namespace.
 *
 * @author Rossen Stoyanchev
 * @author Sam Brannen
 * @since 4.0
 */
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Controller
@ResponseBody
public @interface RestController {

	/**
	 * The value may indicate a suggestion for a logical component name,
	 * to be turned into a Spring bean in case of an autodetected component.
	 * @return the suggested component name, if any (or empty String otherwise)
	 * @since 4.0.1
	 */
	@AliasFor(annotation = Controller.class)
	String value() default "";

}
```
</div>
</details>
</details>


<details>
<summary class="summary-title">@PathVariable</summary>
<li class="font-lg">org.springframework.web.bind.annotation.PathVariable</li>
<li class="font-lg">URI 경로의 변수 값을 메서드 매개변수로 전달</li>
<li class="font-lg">값에 접근할 때에는 {중괄호}로 묶어서 매개변수로 전달</li>

<details>
<summary>예시</summary>
<div markdown="1">

```java
/**
 * 경로 내에 변수를 담아서 요청할 수 있다.
 * @XxxMapping 요청 매핑 어노테이션에서 값에 접근할 때에는 {중괄호}로 묶어서 매개변수로 전달한다.
 * @param @PathVariable 경로 변수를 매개변수로 받을 때에는 {중괄호}내에 작성한 변수명으로 받는다.
 */
@RequestMapping(value = "ent/{company}/singer/{singer}")
public String pathVariable (Model model, @RequestParam("title") String title, 
                @PathVariable("company") String company,
                @PathVariable("singer") String singer) {

    String str = "소속사 : " + company + ", 가수 : " + singer + ", 대표곡 : " + title;
    model.addAttribute("msg", str);
    return "list";
}
```
</div>
</details>


<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java
package org.springframework.web.bind.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.core.annotation.AliasFor;

/**
 * Annotation which indicates that a method parameter should be bound to a URI template
 * variable. Supported for {@link RequestMapping} annotated handler methods.
 *
 * <p>If the method parameter is {@link java.util.Map Map&lt;String, String&gt;}
 * then the map is populated with all path variable names and values.
 *
 * @author Arjen Poutsma
 * @author Juergen Hoeller
 * @since 3.0
 * @see RequestMapping
 * @see org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter
 */
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface PathVariable {

	/**
	 * Alias for {@link #name}.
	 */
	@AliasFor("name")
	String value() default "";

	/**
	 * The name of the path variable to bind to.
	 * @since 4.3.3
	 */
	@AliasFor("value")
	String name() default "";

	/**
	 * Whether the path variable is required.
	 * <p>Defaults to {@code true}, leading to an exception being thrown if the path
	 * variable is missing in the incoming request. Switch this to {@code false} if
	 * you prefer a {@code null} or Java 8 {@code java.util.Optional} in this case.
	 * e.g. on a {@code ModelAttribute} method which serves for different requests.
	 * @since 4.3.3
	 */
	boolean required() default true;

}
```
</div>
</details>
</details>

<hr>

# 4> Data Layer

데이터베이스 및 ORM 관련 어노테이션 <br>

<h1>☑️ javax.persistence</h1>

<details>
<summary class="summary-title">@Entity</summary>
<li class="font-lg">javax.persistence.Entity</li>
<li class="font-lg">JPA</li>
<li class="font-lg">DB 테이블과 매핑</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@Table</summary>
<li class="font-lg">javax.persistence.Table</li>
<li class="font-lg">엔티티 클래스가 매핑될 데이터베이스의 테이블</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@Id</summary>
<li class="font-lg">javax.persistence.Id</li>
<li class="font-lg">엔티티의 기본 키(PK)</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@GeneratedValue</summary>
<li class="font-lg">javax.persistence.GeneratedValue</li>
<li class="font-lg">기본 키 값을 자동으로 생성</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@Column</summary>
<li class="font-lg">javax.persistence.Column</li>
<li class="font-lg">엔티티 클래스의 필드를 테이블의 컬럼에 매핑</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@JoinColumn</summary>
<li class="font-lg">javax.persistence.JoinColumn</li>
<li class="font-lg">테이블 조인 (외래 키 참조)</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary>
<b>@OneToMany<br>
&emsp;@ManyToOne<br>
&emsp;@OneToOne<br>
&emsp;@ManyToMany</b></summary>
<li class="font-lg">javax.persistence.OneToMany</li>
<span class="font-lg">&emsp; javax.persistence.ManyToOne</span> <br>
<span class="font-lg">&emsp; javax.persistence.OneToOne</span> <br>
<span class="font-lg">&emsp; javax.persistence.ManyToMany</span> <br>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>

<hr>

# 5> Transaction
트랜젝션 처리 어노테이션

<h1>☑️ org.springframework.transaction.annotation</h1>

<details>
<summary>@Transactional</summary>
<li class="font-lg">org.springframework.transaction.annotation.Transactional</li>
<li class="font-lg">트랜젝션 자동 처리</li>
<li class="font-lg">예외 발생 시 자동 롤백, 정상 실행 시 커밋</li>
<details>
<summary>예시</summary>
<div markdown="1">

```java
// 등록, 수정, 삭제 시 트랜젝션 기능 수행
@Transactional 
public int insert(Bean bean) {
	return repository.save(getEntity(bean));
}

@Transactional
public int update(Bean bean) {
	return repository.save(getEntity(bean));
}

@Transactional
public void delete(Bean bean) {
	repository.delete(getEntity(bean));
}
```
</div>
</details>
</details>


<hr>

## 0> Custom
개발자가 직접 만드는 어노테이션


<details>
<summary>@interface</summary>
<li class="font-lg">java.lang.annotation.Annotation를 상속</li>
<li class="font-lg">내부 메서드는 abstract (추상 메서드)</li>
<details>
<summary>제약 사항</summary>

<pre>
어노테이션 타입 선언 시 제네릭 불가능
메서드 매개변수 전달 불가능
메서드 선언부 예외 throws 불가능
</pre>

</details>
</details>



---
참고 사이트 <br>
