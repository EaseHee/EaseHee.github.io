---
layout: single
title: "Spring_06_Spring Boot"
date: 2024-10-18
categories:
  - Spring
tags:
  - SpringBoot
---

<br>

---
# SpringBoot


스프링 프레임워크의 기능을 쉽게 사용하기 위해 확장한 개념. <br>
Web Application Server (WAS)를 설치할 필요가 없다. <br>
내장 서버 : Tomcat, Jetty, Undertow <br>

기본적으로 jsp를 읽지 않기 때문에 별도의 dependency 설정을 추가해야 한다. <br>
<br>
<hr>


## SpringFramework 의 MVC 동작 방식
<pre><code>
- DispatcherServlet 
    = “Front Controller”
    : 클라이언트의 모든 요청을 전달받는다. 
- HandlerMapping 
    : 요청에 맞는 컨트롤러 검색 (cf. 핸들러 = 컨트롤러 | 매핑 = 검색) 
    → “return Controller”
- Controller 
    : Model에 Business Logic 요청 
    → "return ModelAndView" : 모델에서 처리한 결과와 뷰리졸버에서 처리할 정보를 담아서 반환
- ViewResolver 
    = “application.properties”
    : 컨트롤러로부터 "View 객체"를 받아서 폴더 경로 및 확장자를 작성한 후 반환
    → "return View”
</code></pre>

<image class="image-medium" src="/assets/image/2024-10-15-Spring_MVC_Container.004.png" />
<!-- ![](/assets/image/2024-10-15-Spring_MVC_Container.003.png) | ![](/assets/image/2024-10-15-Spring_MVC_Container.004.png) -->

<details>
<summary class="summary-title">💡 [API] DispatcherServlet</summary>
<details>
<summary>View Comments</summary>
<div markdown="1">
```java
package org.springframework.web.servlet;

@SuppressWarnings("serial")
public class DispatcherServlet extends FrameworkServlet {

    /** Well-known name for the MultipartResolver object in the bean factory for this namespace. */
    public static final String MULTIPART_RESOLVER_BEAN_NAME = "multipartResolver";

    /** Well-known name for the LocaleResolver object in the bean factory for this namespace. */
    public static final String LOCALE_RESOLVER_BEAN_NAME = "localeResolver";

    /**
    * Well-known name for the ThemeResolver object in the bean factory for this namespace.
    * @deprecated as of 6.0, with no direct replacement
    */
    @Deprecated
    public static final String THEME_RESOLVER_BEAN_NAME = "themeResolver";

    /**
    * Well-known name for the HandlerMapping object in the bean factory for this namespace.
    * Only used when "detectAllHandlerMappings" is turned off.
    * @see #setDetectAllHandlerMappings
    */
    public static final String HANDLER_MAPPING_BEAN_NAME = "handlerMapping";

    /**
    * Well-known name for the HandlerAdapter object in the bean factory for this namespace.
    * Only used when "detectAllHandlerAdapters" is turned off.
    * @see #setDetectAllHandlerAdapters
    */
    public static final String HANDLER_ADAPTER_BEAN_NAME = "handlerAdapter";

    /**
    * Well-known name for the HandlerExceptionResolver object in the bean factory for this namespace.
    * Only used when "detectAllHandlerExceptionResolvers" is turned off.
    * @see #setDetectAllHandlerExceptionResolvers
    */
    public static final String HANDLER_EXCEPTION_RESOLVER_BEAN_NAME = "handlerExceptionResolver";

    /**
    * Well-known name for the RequestToViewNameTranslator object in the bean factory for this namespace.
    */
    public static final String REQUEST_TO_VIEW_NAME_TRANSLATOR_BEAN_NAME = "viewNameTranslator";

    /**
    * Well-known name for the ViewResolver object in the bean factory for this namespace.
    * Only used when "detectAllViewResolvers" is turned off.
    * @see #setDetectAllViewResolvers
    */
    public static final String VIEW_RESOLVER_BEAN_NAME = "viewResolver";

    /**
    * Well-known name for the FlashMapManager object in the bean factory for this namespace.
    */
    public static final String FLASH_MAP_MANAGER_BEAN_NAME = "flashMapManager";

    /**
    * Request attribute to hold the current web application context.
    * Otherwise only the global web app context is obtainable by tags etc.
    * @see org.springframework.web.servlet.support.RequestContextUtils#findWebApplicationContext
    */
    public static final String WEB_APPLICATION_CONTEXT_ATTRIBUTE = DispatcherServlet.class.getName() + ".CONTEXT";

    /**
    * Request attribute to hold the current LocaleResolver, retrievable by views.
    * @see org.springframework.web.servlet.support.RequestContextUtils#getLocaleResolver
    */
    public static final String LOCALE_RESOLVER_ATTRIBUTE = DispatcherServlet.class.getName() + ".LOCALE_RESOLVER";

    /**
    * Request attribute to hold the current ThemeResolver, retrievable by views.
    * @see org.springframework.web.servlet.support.RequestContextUtils#getThemeResolver
    * @deprecated as of 6.0, with no direct replacement
    */
    @Deprecated
    public static final String THEME_RESOLVER_ATTRIBUTE = DispatcherServlet.class.getName() + ".THEME_RESOLVER";

    /**
    * Request attribute to hold the current ThemeSource, retrievable by views.
    * @see org.springframework.web.servlet.support.RequestContextUtils#getThemeSource
    * @deprecated as of 6.0, with no direct replacement
    */
    @Deprecated
    public static final String THEME_SOURCE_ATTRIBUTE = DispatcherServlet.class.getName() + ".THEME_SOURCE";

    /**
    * Name of request attribute that holds a read-only {@code Map<String,?>}
    * with "input" flash attributes saved by a previous request, if any.
    * @see org.springframework.web.servlet.support.RequestContextUtils#getInputFlashMap(HttpServletRequest)
    */
    public static final String INPUT_FLASH_MAP_ATTRIBUTE = DispatcherServlet.class.getName() + ".INPUT_FLASH_MAP";

    /**
    * Name of request attribute that holds the "output" {@link FlashMap} with
    * attributes to save for a subsequent request.
    * @see org.springframework.web.servlet.support.RequestContextUtils#getOutputFlashMap(HttpServletRequest)
    */
    public static final String OUTPUT_FLASH_MAP_ATTRIBUTE = DispatcherServlet.class.getName() + ".OUTPUT_FLASH_MAP";

    /**
    * Name of request attribute that holds the {@link FlashMapManager}.
    * @see org.springframework.web.servlet.support.RequestContextUtils#getFlashMapManager(HttpServletRequest)
    */
    public static final String FLASH_MAP_MANAGER_ATTRIBUTE = DispatcherServlet.class.getName() + ".FLASH_MAP_MANAGER";

    /**
    * Name of request attribute that exposes an Exception resolved with a
    * {@link HandlerExceptionResolver} but where no view was rendered
    * (e.g. setting the status code).
    */
    public static final String EXCEPTION_ATTRIBUTE = DispatcherServlet.class.getName() + ".EXCEPTION";

    /** Log category to use when no mapped handler is found for a request. */
    public static final String PAGE_NOT_FOUND_LOG_CATEGORY = "org.springframework.web.servlet.PageNotFound";

    /**
    * Name of the class path resource (relative to the DispatcherServlet class)
    * that defines DispatcherServlet's default strategy names.
    */
    private static final String DEFAULT_STRATEGIES_PATH = "DispatcherServlet.properties";

    /**
    * Common prefix that DispatcherServlet's default strategy attributes start with.
    */
    private static final String DEFAULT_STRATEGIES_PREFIX = "org.springframework.web.servlet";


    /** Additional logger to use when no mapped handler is found for a request. */
    protected static final Log pageNotFoundLogger = LogFactory.getLog(PAGE_NOT_FOUND_LOG_CATEGORY);

    /** Store default strategy implementations. */
    @Nullable
    private static Properties defaultStrategies;

    /** Detect all HandlerMappings or just expect "handlerMapping" bean?. */
    private boolean detectAllHandlerMappings = true;

    /** Detect all HandlerAdapters or just expect "handlerAdapter" bean?. */
    private boolean detectAllHandlerAdapters = true;

    /** Detect all HandlerExceptionResolvers or just expect "handlerExceptionResolver" bean?. */
    private boolean detectAllHandlerExceptionResolvers = true;

    /** Detect all ViewResolvers or just expect "viewResolver" bean?. */
    private boolean detectAllViewResolvers = true;

    /** Throw a NoHandlerFoundException if no Handler was found to process this request? *.*/
    private boolean throwExceptionIfNoHandlerFound = true;

    /** Perform cleanup of request attributes after include request?. */
    private boolean cleanupAfterInclude = true;

    /** MultipartResolver used by this servlet. */
    @Nullable
    private MultipartResolver multipartResolver;

    /** LocaleResolver used by this servlet. */
    @Nullable
    private LocaleResolver localeResolver;

    /** ThemeResolver used by this servlet. */
    @Deprecated
    @Nullable
    private ThemeResolver themeResolver;

    /** List of HandlerMappings used by this servlet. */
    @Nullable
    private List<HandlerMapping> handlerMappings;

    /** List of HandlerAdapters used by this servlet. */
    @Nullable
    private List<HandlerAdapter> handlerAdapters;

    /** List of HandlerExceptionResolvers used by this servlet. */
    @Nullable
    private List<HandlerExceptionResolver> handlerExceptionResolvers;

    /** RequestToViewNameTranslator used by this servlet. */
    @Nullable
    private RequestToViewNameTranslator viewNameTranslator;

    /** FlashMapManager used by this servlet. */
    @Nullable
    private FlashMapManager flashMapManager;

    /** List of ViewResolvers used by this servlet. */
    @Nullable
    private List<ViewResolver> viewResolvers;

    private boolean parseRequestPath;


    /**
    * Create a new {@code DispatcherServlet} that will create its own internal web
    * application context based on defaults and values provided through servlet
    * init-params. Typically used in Servlet 2.5 or earlier environments, where the only
    * option for servlet registration is through {@code web.xml} which requires the use
    * of a no-arg constructor.
    * <p>Calling {@link #setContextConfigLocation} (init-param 'contextConfigLocation')
    * will dictate which XML files will be loaded by the
    * {@linkplain #DEFAULT_CONTEXT_CLASS default XmlWebApplicationContext}
    * <p>Calling {@link #setContextClass} (init-param 'contextClass') overrides the
    * default {@code XmlWebApplicationContext} and allows for specifying an alternative class,
    * such as {@code AnnotationConfigWebApplicationContext}.
    * <p>Calling {@link #setContextInitializerClasses} (init-param 'contextInitializerClasses')
    * indicates which {@code ApplicationContextInitializer} classes should be used to
    * further configure the internal application context prior to refresh().
    * @see #DispatcherServlet(WebApplicationContext)
    */
    public DispatcherServlet() {
        super();
        setDispatchOptionsRequest(true);
    }

    /**
    * Create a new {@code DispatcherServlet} with the given web application context. This
    * constructor is useful in Servlet environments where instance-based registration
    * of servlets is possible through the {@link ServletContext#addServlet} API.
    * <p>Using this constructor indicates that the following properties / init-params
    * will be ignored:
    * <ul>
    * <li>{@link #setContextClass(Class)} / 'contextClass'</li>
    * <li>{@link #setContextConfigLocation(String)} / 'contextConfigLocation'</li>
    * <li>{@link #setContextAttribute(String)} / 'contextAttribute'</li>
    * <li>{@link #setNamespace(String)} / 'namespace'</li>
    * </ul>
    * <p>The given web application context may or may not yet be {@linkplain
    * ConfigurableApplicationContext#refresh() refreshed}. If it has <strong>not</strong>
    * already been refreshed (the recommended approach), then the following will occur:
    * <ul>
    * <li>If the given context does not already have a {@linkplain
    * ConfigurableApplicationContext#setParent parent}, the root application context
    * will be set as the parent.</li>
    * <li>If the given context has not already been assigned an {@linkplain
    * ConfigurableApplicationContext#setId id}, one will be assigned to it</li>
    * <li>{@code ServletContext} and {@code ServletConfig} objects will be delegated to
    * the application context</li>
    * <li>{@link #postProcessWebApplicationContext} will be called</li>
    * <li>Any {@code ApplicationContextInitializer}s specified through the
    * "contextInitializerClasses" init-param or through the {@link
    * #setContextInitializers} property will be applied.</li>
    * <li>{@link ConfigurableApplicationContext#refresh refresh()} will be called if the
    * context implements {@link ConfigurableApplicationContext}</li>
    * </ul>
    * If the context has already been refreshed, none of the above will occur, under the
    * assumption that the user has performed these actions (or not) per their specific
    * needs.
    * <p>See {@link org.springframework.web.WebApplicationInitializer} for usage examples.
    * @param webApplicationContext the context to use
    * @see #initWebApplicationContext
    * @see #configureAndRefreshWebApplicationContext
    * @see org.springframework.web.WebApplicationInitializer
    */
    public DispatcherServlet(WebApplicationContext webApplicationContext) {
        super(webApplicationContext);
        setDispatchOptionsRequest(true);
    }
```
</div>
</details>

<li>summary</li>
<div markdown="1">
```java
package org.springframework.web.servlet;
@SuppressWarnings("serial")
public class DispatcherServlet extends FrameworkServlet {

    public static final String HANDLER_MAPPING_BEAN_NAME = "handlerMapping";
    public static final String HANDLER_ADAPTER_BEAN_NAME = "handlerAdapter";
    public static final String VIEW_RESOLVER_BEAN_NAME = "viewResolver";
    public static final String REQUEST_TO_VIEW_NAME_TRANSLATOR_BEAN_NAME = "viewNameTranslator";

    private boolean throwExceptionIfNoHandlerFound = true;

    @Nullable
    private List<HandlerMapping> handlerMappings;
    @Nullable
    private List<HandlerAdapter> handlerAdapters;
    @Nullable
    private List<ViewResolver> viewResolvers;
    @Nullable
    private RequestToViewNameTranslator viewNameTranslator;
    @Nullable
    private List<HandlerExceptionResolver> handlerExceptionResolvers;

    private boolean parseRequestPath;

    public DispatcherServlet() {
        super();
        setDispatchOptionsRequest(true);
    }

    public DispatcherServlet(WebApplicationContext webApplicationContext) {
        super(webApplicationContext);
        setDispatchOptionsRequest(true);
    }
```
</div>
</details>

<br>
<hr>

    @Controller
     DispatcherServlet -> HandlerMapping을 통해 결정, 요청 전달.
     인스턴스 생성, 클라이언트와 데이터 입출력을 제어하는 클래스에 적용
     클라이언트의 요청을 처리한 후 지정된 view에 모델 객체를 전달하는 역할
    
    @ResponseBody 
     일반 문자열, Map, JSON 등의 Java 객체를 http 응답 본문 객체로 변환하여 클라이언트로 전송
    
    @RestController 
     = @Controller + @ResponseBody


## 1. [legacy] View를 jsp로 설정한 경우 (권장 X)

<details markdown="1">
<summary>[legacy] ver</summary>
    

### springweb01_jsp 
- Client(index.html) → Controller → View(list.jsp) <br>

    <details>
    <summary>[GET] return ModelAndView</summary>
    <div markdown="1">
    ```html
    <!-- uri : 논리적인 요청 : O | 파일명 : X -->
    &lt;!-- uri : 논리적인 요청 : O | 파일명 : X --&gt;
    <a href="test1">컨트롤러에게 요청 1</a>
    <a href="test2">컨트롤러에게 요청 2</a>
    <a href="test3">컨트롤러에게 요청 3</a>
    <a href="test4">컨트롤러에게 요청 4</a>
    ```

    ```java
    @Controller
    public class TestController {
        // @RequestMapping _ 클라이언트의 요청(urlPatterns 값)을 처리 : GET, POST 모두 처리
        @RequestMapping("test1")  // forward 방식의 전송을 하기 때문에 URL에 test1이 노출된다.
        public ModelAndView abc() {
            System.out.println("abc() 처리");
            
            // Model에서 데이터를 가져왔다는 가정
            String result = "모델 반환 정보";
            
            // 모델 반환 정보를 뷰(jsp)로 전달
            ModelAndView modelAndView = new ModelAndView();
            modelAndView.setViewName("list"); // "/WEB-INF/views/list.jsp"
            // WEB-INF 폴더는 forwarding으로만 접근할 수 있다.
            // ModelAndView .addObject() : 서블릿의 request.setAttribue("msg", result); 와 동일하다. 
            modelAndView.addObject("msg", result); 
            return modelAndView; // forward
        }
    ```

    ```java
        @RequestMapping("test1")  // forward 방식의 전송을 하기 때문에 URL에 test1이 노출된다.
        public ModelAndView abc() {
            System.out.println("abc() 처리");
                /* ViewName, AttributeKey, AttributeValue 를 생성자로 전달. (생성자 오버로딩) */
            return new ModelAndView("list", "msg", result);
        }
    ```

    ```java
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
    ```
    </div>
    </details>

    <details>
    <summary>💡 [API] ModelAndView</summary>
    <div markdown="1">
    <details>
    <summary>View Comments</summary>
    <div markdown="1">
    ```java
    package org.springframework.web.servlet;

    /**
     * Holder for both Model and View in the web MVC framework.
     * Note that these are entirely distinct. This class merely holds
     * both to make it possible for a controller to return both model
     * and view in a single return value.
     *
     * <p>Represents a model and view returned by a handler, to be resolved
     * by a DispatcherServlet. The view can take the form of a String
     * view name which will need to be resolved by a ViewResolver object;
     * alternatively a View object can be specified directly. The model
     * is a Map, allowing the use of multiple objects keyed by name.
     *
     * @author Rod Johnson
     * @author Juergen Hoeller
     * @author Rob Harrop
     * @author Rossen Stoyanchev
     * @see DispatcherServlet
     * @see ViewResolver
     * @see HandlerAdapter#handle
     * @see org.springframework.web.servlet.mvc.Controller#handleRequest
     */
    public class ModelAndView {

        /** View instance or view name String. */
        @Nullable
        private Object view;

        /** Model Map. */
        @Nullable
        private ModelMap model;

        /** Optional HTTP status for the response. */
        @Nullable
        private HttpStatusCode status;

        /** Indicates whether this instance has been cleared with a call to {@link #clear()}. */
        private boolean cleared = false;


        /**
         * Default constructor for bean-style usage: populating bean
         * properties instead of passing in constructor arguments.
         * @see #setView(View)
         * @see #setViewName(String)
         */
        public ModelAndView() {
        }

        /**
         * Convenient constructor when there is no model data to expose.
         * Can also be used in conjunction with {@code addObject}.
         * @param viewName name of the View to render, to be resolved
         * by the DispatcherServlet's ViewResolver
         * @see #addObject
         */
        public ModelAndView(String viewName) {
            this.view = viewName;
        }

        /**
         * Convenient constructor when there is no model data to expose.
         * Can also be used in conjunction with {@code addObject}.
         * @param view the View object to render
         * @see #addObject
         */
        public ModelAndView(View view) {
            this.view = view;
        }

        /**
         * Create a new ModelAndView given a view name and a model.
         * @param viewName name of the View to render, to be resolved
         * by the DispatcherServlet's ViewResolver
         * @param model a Map of model names (Strings) to model objects
         * (Objects). Model entries may not be {@code null}, but the
         * model Map may be {@code null} if there is no model data.
         */
        public ModelAndView(String viewName, @Nullable Map<String, ?> model) {
            this.view = viewName;
            if (model != null) {
                getModelMap().addAllAttributes(model);
            }
        }

        /**
         * Create a new ModelAndView given a View object and a model.
         * <em>Note: the supplied model data is copied into the internal
         * storage of this class. You should not consider to modify the supplied
         * Map after supplying it to this class</em>
         * @param view the View object to render
         * @param model a Map of model names (Strings) to model objects
         * (Objects). Model entries may not be {@code null}, but the
         * model Map may be {@code null} if there is no model data.
         */
        public ModelAndView(View view, @Nullable Map<String, ?> model) {
            this.view = view;
            if (model != null) {
                getModelMap().addAllAttributes(model);
            }
        }

        /**
         * Create a new ModelAndView given a view name and HTTP status.
         * @param viewName name of the View to render, to be resolved
         * by the DispatcherServlet's ViewResolver
         * @param status an HTTP status code to use for the response
         * (to be set just prior to View rendering)
         * @since 4.3.8
         */
        public ModelAndView(String viewName, HttpStatusCode status) {
            this.view = viewName;
            this.status = status;
        }

        /**
         * Create a new ModelAndView given a view name, model, and HTTP status.
         * @param viewName name of the View to render, to be resolved
         * by the DispatcherServlet's ViewResolver
         * @param model a Map of model names (Strings) to model objects
         * (Objects). Model entries may not be {@code null}, but the
         * model Map may be {@code null} if there is no model data.
         * @param status an HTTP status code to use for the response
         * (to be set just prior to View rendering)
         * @since 4.3
         */
        public ModelAndView(@Nullable String viewName, @Nullable Map<String, ?> model, @Nullable HttpStatusCode status) {
            this.view = viewName;
            if (model != null) {
                getModelMap().addAllAttributes(model);
            }
            this.status = status;
        }

        /**
         * Convenient constructor to take a single model object.
         * @param viewName name of the View to render, to be resolved
         * by the DispatcherServlet's ViewResolver
         * @param modelName name of the single entry in the model
         * @param modelObject the single model object
         */
        public ModelAndView(String viewName, String modelName, Object modelObject) {
            this.view = viewName;
            addObject(modelName, modelObject);
        }

        /**
         * Convenient constructor to take a single model object.
         * @param view the View object to render
         * @param modelName name of the single entry in the model
         * @param modelObject the single model object
         */
        public ModelAndView(View view, String modelName, Object modelObject) {
            this.view = view;
            addObject(modelName, modelObject);
        }


        /**
         * Set a view name for this ModelAndView, to be resolved by the
         * DispatcherServlet via a ViewResolver. Will override any
         * pre-existing view name or View.
         */
        public void setViewName(@Nullable String viewName) {
            this.view = viewName;
        }

        /**
         * Return the view name to be resolved by the DispatcherServlet
         * via a ViewResolver, or {@code null} if we are using a View object.
         */
        @Nullable
        public String getViewName() {
            return (this.view instanceof String name ? name : null);
        }

        /**
         * Set a View object for this ModelAndView. Will override any
         * pre-existing view name or View.
         */
        public void setView(@Nullable View view) {
            this.view = view;
        }

        /**
        * Return the View object, or {@code null} if we are using a view name
        * to be resolved by the DispatcherServlet via a ViewResolver.
        */
        @Nullable
        public View getView() {
            return (this.view instanceof View v ? v : null);
        }

        /**
        * Indicate whether this {@code ModelAndView} has a view, either
        * as a view name or as a direct {@link View} instance.
        */
        public boolean hasView() {
            return (this.view != null);
        }

        /**
         * Return whether we use a view reference, i.e. {@code true}
         * if the view has been specified via a name to be resolved by the
         * DispatcherServlet via a ViewResolver.
         */
        public boolean isReference() {
            return (this.view instanceof String);
        }

        /**
         * Return the model map. May return {@code null}.
         * Called by DispatcherServlet for evaluation of the model.
         */
        @Nullable
        protected Map<String, Object> getModelInternal() {
            return this.model;
        }

        /**
         * Return the underlying {@code ModelMap} instance (never {@code null}).
         */
        public ModelMap getModelMap() {
            if (this.model == null) {
                this.model = new ModelMap();
            }
            return this.model;
        }

        /**
         * Return the model map. Never returns {@code null}.
         * To be called by application code for modifying the model.
         */
        public Map<String, Object> getModel() {
            return getModelMap();
        }

        /**
         * Set the HTTP status to use for the response.
         * <p>The response status is set just prior to View rendering.
         * @since 4.3
         */
        public void setStatus(@Nullable HttpStatusCode status) {
            this.status = status;
        }

        /**
        * Return the configured HTTP status for the response, if any.
        * @since 4.3
        */
        @Nullable
        public HttpStatusCode getStatus() {
            return this.status;
        }


        /**
         * Add an attribute to the model.
         * @param attributeName name of the object to add to the model (never {@code null})
         * @param attributeValue object to add to the model (can be {@code null})
         * @see ModelMap#addAttribute(String, Object)
         * @see #getModelMap()
         */
        public ModelAndView addObject(String attributeName, @Nullable Object attributeValue) {
            getModelMap().addAttribute(attributeName, attributeValue);
            return this;
        }

        /**
         * Add an attribute to the model using parameter name generation.
         * @param attributeValue the object to add to the model (never {@code null})
         * @see ModelMap#addAttribute(Object)
         * @see #getModelMap()
         */
        public ModelAndView addObject(Object attributeValue) {
            getModelMap().addAttribute(attributeValue);
            return this;
        }

        /**
         * Add all attributes contained in the provided Map to the model.
         * @param modelMap a Map of attributeName &rarr; attributeValue pairs
         * @see ModelMap#addAllAttributes(Map)
         * @see #getModelMap()
         */
        public ModelAndView addAllObjects(@Nullable Map<String, ?> modelMap) {
            getModelMap().addAllAttributes(modelMap);
            return this;
        }


        /**
         * Clear the state of this ModelAndView object.
         * The object will be empty afterwards.
         * <p>Can be used to suppress rendering of a given ModelAndView object
         * in the {@code postHandle} method of a HandlerInterceptor.
         * @see #isEmpty()
         * @see HandlerInterceptor#postHandle
         */
        public void clear() {
            this.view = null;
            this.model = null;
            this.cleared = true;
        }

        /**
         * Return whether this ModelAndView object is empty,
         * i.e. whether it does not hold any view and does not contain a model.
         */
        public boolean isEmpty() {
            return (this.view == null && CollectionUtils.isEmpty(this.model));
        }

        /**
         * Return whether this ModelAndView object is empty as a result of a call to {@link #clear}
         * i.e. whether it does not hold any view and does not contain a model.
         * <p>Returns {@code false} if any additional state was added to the instance
         * <strong>after</strong> the call to {@link #clear}.
         * @see #clear()
         */
        public boolean wasCleared() {
            return (this.cleared && isEmpty());
        }


        /**
        * Return diagnostic information about this model and view.
        */
        @Override
        public String toString() {
            return "ModelAndView [view=" + formatView() + "; model=" + this.model + "]";
        }

        private String formatView() {
            return isReference() ? "\"" + this.view + "\"" : "[" + this.view + "]";
        }
    ```
    </div>
    </details>

    ```java
    package org.springframework.web.servlet;
    
    public class ModelAndView {
        @Nullable
        private Object view;
        @Nullable
        private ModelMap model;
        @Nullable
        private HttpStatusCode status;
        private boolean cleared = false;

        public ModelAndView() {
        }
        public ModelAndView(String viewName) {
            this.view = viewName;
        }
        public ModelAndView(View view) {
            this.view = view;
        }

        public ModelAndView(String viewName, @Nullable Map<String, ?> model) {
            this.view = viewName;
            if (model != null) {
                getModelMap().addAllAttributes(model);
            }
        }
        public ModelAndView(View view, @Nullable Map<String, ?> model) {
            this.view = view;
            if (model != null) {
                getModelMap().addAllAttributes(model);
            }
        }
        public ModelAndView(String viewName, HttpStatusCode status) {
            this.view = viewName;
            this.status = status;
        }

        public ModelAndView(@Nullable String viewName, @Nullable Map<String, ?> model, @Nullable HttpStatusCode status) {
            this.view = viewName;
            if (model != null) {
                getModelMap().addAllAttributes(model);
            }
            this.status = status;
        }
        public ModelAndView(String viewName, String modelName, Object modelObject) {
            this.view = viewName;
            addObject(modelName, modelObject);
        }
        public ModelAndView(View view, String modelName, Object modelObject) {
            this.view = view;
            addObject(modelName, modelObject);
        }
    ```
    </div>
    </details>

    <details>
    <summary>[POST]</summary>
    <div markdown="1">

    1> @RequestMapping(value="", method=RequestMethod.POST) <br>
       Model.addAtrribute(Key, Value) <br>

    ```html
    <form action="test5" method="post">
        <input type="submit" value="컨트롤러에게 요청 5">
    </form>
    ```

    2> GET -> POST

    ```html
    <a href="javascript:func();">컨트롤러에게 요청 5_2</a> : &lt;a&gt;로 POST 요청
    <form name="f"></form>
    <script>
        function func() {
            f.action = "test5";
            f.method = "post"; //POST방식
            f.submit();
        }
    </script>
    ```

    ```java
    @RequestMapping(value = "test5", method=RequestMethod.POST)
    public String abc5(Model model) {
        model.addAttribute("msg", String.valueOf(model.getClass()) + "요청 처리 성공5");
        return "list";
    }
    ```

    3> return ModeAndView
    ```html
    <form action="test6" method="post">
        <input type="submit" value="컨트롤러에게 요청 6">
    </form>
    ```

    ```java
    @PostMapping(value = "test6")
    public ModelAndView abc6() {
        return new ModelAndView("list", "msg", "요청 처리 성공 6");
    }
    ```

    4> Model.addAttribute(Key, Value)
    ```html
    <form action="test7" method="post">
        <input type="submit" value="컨트롤러에게 요청 7">
    </form>
    ```

    ```java
    @PostMapping("test7")
    public String abc7(Model model) {
        model.addAttribute("msg", "POST 요청 처리 성공 7");
        return "list";
    }
    ```
    </div>
    </details>
    
    <details>
    <summary>[GET] return JSON</summary>
    <div markdown="1">
    ```html
    <a href="test8">컨트롤러에게 요청 8</a>
    <a href="test8_1">컨트롤러에게 요청 8_1</a> 
        : DataVo로 데이터 전달 (Jackson Library)
    <a href="test8_2">컨트롤러에게 요청 8_2</a>
    ```

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

    public class DataVo {
        // (cf) POJO : Plain Old Java Object : 순수한 데이터로만 이루어진 자바 객체 
        private String name;
        private int code;
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        public int getCode() {
            return code;
        }
        public void setCode(int code) {
            this.code = code;
        }
    }
    ```
    </div>
    <img src="/assets/image/2024-10-18-ResponseBody_JSON_01.png" />
    <img src="/assets/image/2024-10-18-ResponseBody_JSON_02.png" />
    </details>

    <details>
    <summary>[GET & POST] URL이 같을 경우</summary>
    <div markdown="1">

    ```html
    <form action="test9" method="get">
        <input type="submit" value="컨트롤러에게 요청 9 - GET">
    </form>	
    <!-- URL은 동일하고 요청방식만 다른 경우 -->
    <form action="test9" method="post">
        <input type="submit" value="컨트롤러에게 요청 9 - POST">
    </form>
    ```

    ```java
    @Controller
    @RequestMapping("test9")	// 컨트롤러 단위로 urlPattern을 설정하여 요청을 처리하는 경우
    public class TestController2 {
        // 요청 처리방식만 메서드로 분리
        
        @RequestMapping(method=RequestMethod.GET)
        public String def1(Model model) {
            model.addAttribute("msg", "요청 처리 성공 9 - GET");
            return "list";
        }
        
        @RequestMapping(method=RequestMethod.POST)
        public String def2(Model model) {
            model.addAttribute("msg", "요청 처리 성공 9 - POST");
            return "list";
        }
    }
    ```
    </div>
    </details>

    <details>
    <summary>[GET] 요청 동시 처리</summary>
    <div markdown="1">
    ```html
    <a href="java/korea">컨트롤러에게 요청 10 - 요청 데이터 : korea</a> <br>
    <a href="java/good">컨트롤러에게 요청 10 - 요청 데이터 : good</a> <br>
    <a href="java/nice">컨트롤러에게 요청 10 - 요청 데이터 : nice</a> <br>
    <a href="java/ok">컨트롤러에게 요청 10 - 요청 데이터 : ok</a> <br>
    ```

    ```java
    @Controller
    public class TestController3 {

        @RequestMapping("/java/korea")
        public String ghi1(Model model) {
            model.addAttribute("msg", "요청 처리 성공 10 - java/korea");
            return "list";
        }

        // 여러 개의 요청을 처리할 때에는 배열로 받는다
        @GetMapping(value = {"java/good","java/nice", "java/ok"}) 
        public String ghi2(Model model) {
            model.addAttribute("msg", "요청 처리 성공 10 - java/good nice ok");
            return "list";
        }
    }
    ```
    </div>
    </details>
    

    <details>
    <summary>[main] @SpringBootApplication</summary>
    <div markdown="1">
    ```java
    @SpringBootApplication
    public class Springweb01JspApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(Springweb01JspApplication.class, args);
    
            // 위와 동일
            // 방법 1
            /* SpringApplication application = new SpringApplication(Springweb01JspApplication.class);
            application.setWebApplicationType(WebApplicationType.NONE);
            application.run(args).getBean(Springweb01JspApplication.class).execute(); */
    
            // 방법 2
            /* SpringApplication.run(Springweb01StartApplication.class, args)
                            .getBean(Springweb01StartApplication.class).execute(); */
            
            // 스프링부트에서도 일반 어플리케이션 환경의 작업들을 모두 수행할 수 있다.
        }
        
        @Autowired
        MyClass myClass;
        
        private void execute() {
            System.out.println("응용 프로그램 실행");
            myClass.abc();
        }
    }
    
    ```
    </div>
    </details>

    <details>
    <summary>View (list.jsp)</summary>
    <div markdown="1">
    
    ```html
    <%@ page contentType="text/html; charset=UTF-8" %>
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title></title>
    </head>
    <!-- TODO 1018 SpringBoot _ jsp 파일 설정 -->
    <body>
        <ul>
            <li>${ requestScope.msg }</li>
            <li>${ msg }</li>
        </ul>
        <%
            /* ModelAndView가 응답한 데이터를 반환하여 출력 */
            String str = String.valueOf(request.getAttribute("msg"));
            out.print(str);
        %>
    </body>
    </html>
    ```
    </div>
    </details>

#### ❌ 오류 : NoResourceFoundException
- 
    <details>
    <summary class="summary-title">No static resource</summary>
    <div markdown="1">
    ```java
    org.springframework.web.servlet.resource.NoResourceFoundException: No static resource login.
    ```
    </div>

    <details>
    <summary>@Configuration 클래스 _ main() 실행 클래스 <br>
    💡 @ComponentScan(basePackages={””, “”}) 작성</summary>
    <div markdown="1">

    ```java
    /**
     * @ComponentScan 
        * 하위 패키지가 아닌 경우는 명시적으로 스캔을 걸어주면 된다.
        * 실제로는 하위 패키지 내에 작성되기 때문에 직접 설정할 필요는 없지만
        * 극단적인 경우를 예제로 다루는 것 
        */

    @ComponentScan(basePackages= {"pack", "controller", "business", "model"})
    @SpringBootApplication
    public class Springweb02JspApplication {
        
        public static void main(String[] args) {
            SpringApplication.run(Springweb02JspApplication.class, args);
        }
    }
    ```
    </div>
    </details>

    <details>
    <summary>💡 [API] @ComponentScan</summary>
    <div markdown="1">
    ```java
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


### springweb02_jsp
- Login & Input

    <details>
    <summary class="summary-title"><b>Login Logic</b></summary>
    <pre>
    - Client (index.html)
    → Controller (LoginController) : redirect
    → Client (input.html)
    → Controller (LoginController) : forward
    → View (result.jsp)
    </pre>

    <details>
    <summary>Client (index.html)</summary>
    <div markdown="1">    
    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    </head>
    <!-- TODO 1018 Spring Boot 02 _ jsp _ 요청 데이터를 전달받아서 처리 -->
    <body>
        <h1>메인</h1>
        <a href="/login">로그인</a> <br>
        <a href="/insdata">자료 읽기</a> <br>
    </body>
    </html>
    ```
    </div>
    </details>

    <details>
    <summary>Controller</summary>
    <div markdown="1">    
    ```java
    @Controller
    public class LoginController {
        // log 정보 출력용 : Logger - 진행 중 발생하는 문제점 추적, 운영상태 모니터링
        private final Logger logger = LoggerFactory.getLogger(this.getClass());

        @GetMapping("login")
        public String submitCall() {
            /**
                * .html 파일은 클라이언트 측 전송 방식으로 redirect 방식으로 전송한다.
                * redirect 방식으로 전송할 경우 반드시 명시해준다. 
                * 기본값 : forward -> ViewResolver에서 전달되어 prefix, suffix가 추가된다.
                */
            return "redirect:http://localhost/login.html";
        }
        
    //	@PostMapping("login")
        public String submit(HttpServletRequest request, Model model) {
            String id = request.getParameter("id");
            String pwd = request.getParameter("pwd");
            System.out.println(id + " " + pwd);
            // 로거 이용
            logger.info(id + " " + pwd);
            String data = "";
            
            if ("aa".equals(id) && "11".equals(pwd)) {
                data = "로그인 성공";
            } else {
                data = "로그인 실패";
            }
            
            model.addAttribute("data", data);
            return "result"; // RequestDispatcher.forward (!중요) 
        }
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
    }
    ```
    </div>
    </details>

    <details>
    <summary>Client (login.html)</summary>
    <div markdown="1">    
    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    </head>
    <body>
    **자료 입력**<p/>
    <form action="login" method="post">
        i d : <input type="text" name="id"> <br>
        pwd : <input type="text" name="pwd"> <br>
        <input type="submit" value="전송">
    </form>
    </body>
    </html>
    ```
    </div>
    </details>

    <details>
    <summary>View (result.jsp)</summary>
    <div markdown="1">    
    ```html
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <!DOCTYPE html> <!-- : HTML5 (database, canvas, ...) -->
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    </head>
    <!-- TODO 1018 Spring Boot 02 _ jsp _ 요청 데이터를 전달받아서 처리 -->
    <body>
        결과 : ${ data } <!-- EL : jsp이기 때문에 쓴다 -->
    </body>
    </html>
    ```
    </div>
    </details>
    </details>


    <details>
    <summary class="summary-title"><b>Input Logic</b></summary>
        <pre>
        - Client (index.html) : Input
        → Controller (InputController) : redirect
        → Client (input.html)
        → Controller (InputController)
        → Model ⇒ “Business Logic” _ (SangpumBean, SangpumModel : @Service)
        → Controller (InputController) : forward
        → View (result.jsp)
        </pre>

    <li>Client (index.html)</li>
    <details>
    <summary>Controller</summary>
    <div markdown="1">
    ```java
    @Controller
    public class InputController {
        
        @Autowired
        private SangpumModel sangpumModel;
        
        @GetMapping("insdata")
        public String submitCall() {
            return "redirect:http://localhost/input.html";
        }
        
        /**
         * @param model : 응답 데이터의 Key, Value를 Attribute에 추가
         * @param bean : 많은 양의 데이터를 전송하기 위해 FormBean을 활용
         * @return : ViewName
         * @See org.springframework.ui.Model
         */
        @PostMapping("insdata")
        public String submit(Model model, SangpumBean bean) {
            model.addAttribute("data", sangpumModel.compute(bean));
            return "result";
        }
    }
    ```
    </div>
    </details>

    <details>
    <summary>Client (input.html)</summary>
    <div markdown="1">    
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <!-- TODO 1018 Spring Boot _ jsp 03 _ FormBean 이용 -->
    <body>
    <p>** 자료 입력 ** </p>
    <form action="insdata" method="POST">
    품명 : <input type="text" name="sang"> <br>
    수량 : <input type="text" name="su"> <br>
    단가 : <input type="text" name="dan"> <br>
    <input type="submit" name="전송">
    </form>
    </body>
    </html>
    ```
    </div>
    </details>

    <details>
    <summary>Model (Bean, Service)</summary>
    <span>Bean</span>
    <div markdown="1">    
    ```java
    @Getter
    @Setter
    public class SangpumBean {
        private String sang;
        private int su, dan;
    }
    ```
    </div>
    <span>Service</span>
    <div markdown="1">
    ```java
    /**
     * DB 연결은 아니기 때문에 @Repository 스테레오 타입이 아니다.
     */
    @Service 
    public class SangpumModel {
        public String compute(SangpumBean bean) {
            String data = "";
            data = data.concat("품명 : " + bean.getSang());
            data = data.concat("\n");
            data = data.concat("금액 : " + (bean.getSu() * bean.getDan())); 
            return data;
        }
    }
    ```
    </div>
    </details>
    <li>View (result.jsp)</li>
    </div>
    </details>


### springweb_03_jsp
- 구구단 요청 및 출력 예제


    <details>
    <summary class="summary-title">Request</summary>
    <details>
    <summary>Client (index.html)</summary>
    <div markdown="1">
    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    </head>
    <!-- TODO 1018 Spring Boot _ jsp 04 _ 예제 구구단 출력 -->
    <body>
        <h1>메인</h1>
        <details>
            <summary>토글 접기 / 펼치기</summary>
            <div markdown="1">
                <a href="/multiple">구구단</a> <br>
            </div>
            내용 없음.
        </details>
    </body>
    </html>
    ```
    </div>
    </details>
    
    <details>
    <summary>Controller : redirect</summary>
    <div markdown="1">
    ```java
    @Controller
    public class InputController {       
        @GetMapping("multiple")
        public String redirect() {
            return "redirect:http://localhost/input.html";
        }
    }
    ```
    </div>
    </details>

    <details>
    <summary>Client (input.html)</summary>
    <div markdown="1">
    ```html
    <form action="multiple" method="post">
        단수 입력 : <input type="number" name="num" min="1" max="9">
        <input type="submit" name="확인" />
    </form>
    ```
    </div>
    </details>
    </details> <!-- Request Toggle End -->

    <details>
    <summary class="summary-title">Response</summary>
    <details>
    <summary>Model</summary>
    <div markdown="1">
    ```java
    @Service
    public class ServiceModel {
        /* 하나의 문자열로 저장 */
        public String getTable(int num) {
            String gugu = num + "단 구구단 출력<br>";
            /**
            * 전달 받은 숫자를 반복문을 이용하여 문자열로 구구단을 출력
            * ex) num = 3
            * 3 x 1 = 3
            * 3 x 2 = 6 
            */
            for (int i = 0; i < 9; i++) {
                gugu = gugu.concat(num + " x " + (i+1) + " = " + num*(i+1) + "<br>");
            }
            return gugu;
        }
        /* 행별 데이터를 문자열 배열에 저장 */
        public String[] getTable2(int num) {
            String[] gugu = new String[10];
            gugu[0] = num + "단 구구단 출력<br>";
            for (int i = 1; i < 10; i++) {
                gugu[i] = num + " x " + i + " = " + (num * i) + "<br>";
            }
            return gugu;
        }
    }
    ```
    </div>
    </details>
    
    <details>
    <summary>Controller : forward</summary>
    <div markdown="1">
    ```java
    @Controller
    public class InputController {
        @Autowired
        private ServiceModel serviceModel;
        /** 
        * @param @RequestParam : 요청 변수의 value값을 통해 매개변수로 전달
        * @param org.springframework.ui.Model : Attribute를 설정하여 View에 응답 데이터 전달
        */
        @PostMapping("multiple")
        public String forward(@RequestParam(value="num") int num, Model model) {
            model.addAttribute("data", serviceModel.getTable(num));
            // JSTL 출력을 위한 문자열 배열 반환
            model.addAttribute("data2", serviceModel.getTable2(num));
            // forward -> application.properties -> prefix, suffix
            return "result";
        }
    }
    ```
    </div>
    </details>

    <details>
    <summary>View (result.jsp)</summary>
    <div markdown="1">
    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <%@ taglib prefix="c" uri="jakarta.tags.core"%>
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    </head>
    <!-- TODO 1018 Spring Boot _ jsp 04 _ 예제 -->
    <body>
        <h2>구구단 출력 페이지</h2>
        ${ requestScope.data }
        <hr>
        <c:forEach var="i" items="${data2}">
            ${i}
        </c:forEach>
    </body>
    </html>
    ```
    </div>
    </details>

    </details>
</details>


#### ❌ 오류 : ClassNotFoundException
- 
    <details>
    <summary>: javax.servlet.jsp.tagext.TagLibraryValidator</summary>
    <div markdown="1">
    <details>
    <summary>오류 원인</summary>
    <span>javax.servlet.jstl</span>
    <div markdown="1">
    스프링 부트 3.0 버전 이상부터는 JSTL 1.2를 사용할 수 없다고 한다.
    pom.xml에 dependency 추가  
    javax → jakarta
    </div>
    <img src="/assets/image/2024-10-18-JSTL_ERROR_01.png">
    <img src="/assets/image/2024-10-18-JSTL_ERROR_02_javax.png">
    </details>
    <details>
    <summary>해결 방법</summary>
    <span>javax.servlet을 jakarta.servlet으로 변경해준다.</span>
    <img src="/assets/image/2024-10-18-JSTL_Jakarta_Dependency.png">        
        불러올 때에도
    <div markdown="1">
        <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    </div>   
        javax의 uri가 아니라 jakarta uri를 작성한다.
    <div markdown="1">
        <%@ taglib prefix="c" uri="jakarta.tags.core"%>
    </div>
    </details>
    </div>
    </details>


--- 

## 2. [Thymeleaf] View를 html로 설정한 경우 (권장)

<details markdown="1">
<summary>[Thymeleaf] ver</summary>

- 스프링 부트의 폴더는 static 폴더와 templates 폴더로 구성된다.

![](/assets/image/2024-10-24-static_templates.png)

static 폴더는 사용자의 접근이 가능한 영역으로 CSR 방식의 전송이 가능하다. 
> ex. redirect

templates 폴더는 사용자의 접근이 불가능한 영역으로 SSR 방식의 전송만 가능하다. 
> ex. forward

<h3>1. 페이지 이동은 Controller가 모두 통제한다.</h3>
welcome page = @GetMapping("/") <br>
index.html 파일도 templates 폴더 내에 작성한다. <br>

```java
@GetMapping("/")
public String index(){
    return "index";
}
```

<h3>2. Controller가 응답 데이터를 Model 객체에 저장하여 반환한다.</h3>

html 파일 내 타임리프 문법을 사용하여 데이터 반환 및 출력. <br>

> <h4>타임리프 문법을 사용하기 위해서는 아래의 속성을 html 엘리먼트에 적용해야 한다. </h4>
템플릿 엔진에 의해 자바 코드가 있는 경우와 없는 경우 구분하여 출력된다. <br>

![](/assets/image/2024-10-21-template_engine_01.png)
> forward 방식의 페이지 이동과 타임 리프 문법이 html 태그 내에 입력된 소스 코드
<br>

![](/assets/image/2024-10-21-template_engine_02.png)
> 템플릿 엔진에 의해 프로그램으로부터 반환한 데이터가 html 엘리먼트 내에 적용된다.
<br>

```html
<html xmlns:th="http://www.thymeleaf.org">
```

```html
<a th:href="@{insert}">회원 추가</a>
<table border="1">
    <tr>
        <th>회원 번호</th><th>회원명</th><th>주소</th><th></th>
    </tr>
    <th:block th:if="${list.size > 0}">
        <tr th:each="data : ${list}" th:object="${data}">
            <td th:text="*{num}"></td>
            <td th:text="*{name}"></td>
            <td th:text="*{addr}"></td>
            <td>
                <a th:href="@{/update(num=*{num})}">수정</a> / 
                <a th:href="@{/delete(num=*{num})}">삭제</a>
            </td>
        </tr>
        <tr>
            <td colspan="4" th:text="|회원 수 : ${list.size}|"></td>
        </tr>
    </th:block>
</table>
<br>
<a th:href="@{/}">메인으로</a> <!-- 요청명이 없는 경우 Controller가 index.html로 forwaring -->
```
</details>

타임리프의 예약어에 대해서는 <br>
[Thymeleaf와 템플릿 엔진](/2024/10/19/Thymeleaf)에서... <br>


---

참고 사이트 <br>
[https://cafe.daum.net/flowlife](https://cafe.daum.net/flowlife/HrhB/82)