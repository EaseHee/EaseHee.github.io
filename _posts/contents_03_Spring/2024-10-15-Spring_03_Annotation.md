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
1> 의존성 주입 (IoC) <br>
2> 관점 지향 프로그래밍 (AOP) <br>
3> 웹 계층 (Layer) <br>
4> 데이터 계층 (Layer) <br>
5> 트랜젝션 <br>
<br>
0> 커스터마이징 <br>

<hr>

## 1> 의존성 주입
객체 간 의존성 관리 및 구성 요소 선언 <br>

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





<details>
<summary class="summary-title">@Bean</summary>
<li class="font-lg">org.springframework.context.annotation.Bean</li>
<li class="font-lg">개발자가 직접 제어가 불가능한 외부 라이브러리 등을 Bean으로 만들 때 사용되는 어노테이션</li>
<li class="font-lg">메서드에 작성</li>
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

<br>


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
	Bean객체의 ID를 찾지 못할 경우 'NoSuchBeanDefinitionException' 발생 <br>
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
<li class="font-lg"></li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>





<!-- 
<table>
    <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
        <tr>
            <th>Stereotype</th>
            <th>특징</th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>@Component</td>
            <td>스프링 빈으로 등록하겠다는 표시</td>
            <td>ApplicationContext -> 컨테이너가 빈으로 등록</td>
        </tr>
        <tr>
            <td>@Service</td>
            <td>비즈니스 로직을 수행하는 Component</td>
            <td></td>
        </tr>
        <tr>
            <td>@Repository</td>
            <td>DB와 상호작용을 담당하는 Component<br>(Persistance, DAO)</td>
            <td>DB 예외 : Spring based Unchecked Exception</td>
        </tr>
        <tr>
            <td>@Controller</td>
            <td>프레젠테이션 로직을 수행하는 Component</td>
            <td>MVC 컨트롤러 : 요청 처리 & 응답 반환</td>
        </tr>
    </tbody>
    <thead>
        <tr>
            <th></th><th></th><th></th>
        </tr>
    </thead>
</table> 
-->




<details>
<summary class="summary-title">@Configuration</summary>
<li class="font-lg"></li>
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
<summary class="summary-title">@EnableAspectJAutoProxy</summary>
<li class="font-lg"></li>
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
<summary class="summary-title">@ComponentScan</summary>
<li class="font-lg">패키지를 설정하여 패키지 내 컴포넌트 스캔</li>
<li class="font-lg"></li>
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

<hr>

## 2> AOP
핵심 로직에 추가할 부가 기능(횡단 관심사항)을 모듈화하는 방법 제공


<details>
<summary class="summary-title">@Aspect</summary>
<li class="font-lg">org.springframework.stereotype.Aspect</li>
<li class="font-lg">AOP에서 사용하는 관점(Aspect)을 정의하는 클래스</li>
<li class="font-lg">포인트컷(Pointcut)과 어드바이스(Advice)를 포함</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@Before</summary>
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
<summary class="summary-title">@After</summary>
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
<summary class="summary-title">@AfterReturning</summary>
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
<summary class="summary-title">@AfterThrowing</summary>
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
<summary class="summary-title">@Around</summary>
<li class="font-lg">메서드 실행 전후 로직 실행</li>
<li class="font-lg">메서드 실행을 가로채고 제어</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>



## 3> Web Layer
Spring MVC와 관련된 웹 어플리케이션 개발에 사용되는 어노테이션


<details>
<summary class="summary-title">@RequestMapping</summary>
<li class="font-lg">org.springframework.web.bind.annotation.RequestMapping</li>
<li class="font-lg">HTTP 요청을 특정 메서드 또는 클래스에 매핑</li>
<li class="font-lg">GET, POST 등 요청 처리</li>
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

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@RequestParam</summary>
<li class="font-lg">org.springframework.web.bind.annotation.RequestParam</li>
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
<summary class="summary-title">@RequestBody</summary>
<li class="font-lg">org.springframework.web.bind.annotation.RequestBod</li>
<li class="font-lg">HTTP 요청 본문을 객체로 변환하여 받을 때 사용</li>
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
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

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
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>


<details>
<summary class="summary-title">@PathVariable</summary>
<li class="font-lg">org.springframework.web.bind.annotation.PathVariable</li>
<li class="font-lg">URI 경로의 변수 값을 메서드 매개변수로 전달</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>

<hr>

## 4> Data Layer

데이터베이스 및 ORM 관련 어노테이션 <br>


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

## 5> Transaction
트랜젝션 처리 어노테이션


<details>
<summary>@Transactional</summary>
<li class="font-lg">org.springframework.transaction.annotation.Transactional</li>
<li class="font-lg">트랜젝션 자동 처리</li>
<li class="font-lg">예외 발생 시 자동 롤백, 정상 실행 시 커밋</li>
<details>
<summary>API 내용 확인</summary>
<div markdown="1">

```java

```
</div>
</details>
</details>




## 0> Custom
개발자가 직접 만드는 어노테이션


<details>
<summary>@interface</summary>
<li class="font-lg">java.lang.annotation.Annotation를 상속</li>
<li class="font-lg">내부 메서드는 abstract (추상 메서드)</li>
<details>
<summary>제약 사항</summary>
<div markdown="1">

```java
어노테이션 타입 선언 시 제네릭 불가능
메서드는 매개변수를 가질 수 없다.
메서드 선언부에 throws로 예외를 던질 수 없다.
```
</div>
</details>
</details>



