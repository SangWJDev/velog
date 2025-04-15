<h1 id="spring-vs-spring-boot">Spring VS Spring Boot</h1>
<table>
<thead>
<tr>
<th><strong>항목</strong></th>
<th><strong>Spring Framework</strong></th>
<th><strong>Spring Boot</strong></th>
</tr>
</thead>
<tbody><tr>
<td>목적</td>
<td>유연하고 확장성 높은 프레임워크 제공</td>
<td>설정 없이 빠르게 개발 가능하게 지원</td>
</tr>
<tr>
<td>설정</td>
<td>XML 또는 Java Config로 수많은 설정 필요</td>
<td>자동 설정 제공 (@SpringBootApplication)</td>
</tr>
<tr>
<td>웹 서버</td>
<td>외부 톰캣 필요 (war 파일 배포)</td>
<td>내장 웹 서버 포함 (jar 실행만으로 동작)</td>
</tr>
<tr>
<td>의존성</td>
<td>개발자가 직접 필요한 의존성을 조합</td>
<td>스타터(Starter)로 의존성 묶음 자동 제공</td>
</tr>
<tr>
<td>예시</td>
<td>Spring + 외부 Tomcat + web.xml 등</td>
<td>java -jar로 실행만 하면 서버 실행됨</td>
</tr>
</tbody></table>
<p>즉, Spring은 유연하지만 설정이 복잡하고 Spring Boot는 내장톰켓 + 자동설정으로 빠른 개발이 가능하다.</p>
<ul>
<li><strong>Spring Boot는 Spring을 “더 쉽고 빠르게” 쓰기 위한 도구</strong></li>
<li>복잡한 설정은 없애고, 개발자가 <strong>비즈니스 로직에 집중</strong>할 수 있게 도와주는 프레임워크</li>
</ul>
<h1 id="spring-boot-application-동작-순서">Spring Boot Application 동작 순서</h1>
<pre><code class="language-java">@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}</code></pre>
<hr />
<p><strong>1️⃣ 애플리케이션 시작</strong></p>
<ul>
<li>main() 메서드 실행</li>
<li>SpringApplication.run() 호출 → 내부적으로 SpringApplication 객체 생성 및 실행</li>
</ul>
<hr />
<p><strong>2️⃣ SpringApplication 객체 초기화</strong></p>
<ul>
<li>어떤 타입의 애플리케이션인지 판단 (웹인지, CLI인지 등)</li>
<li>ApplicationContext 전략 결정 (ex: AnnotationConfigServletWebServerApplicationContext)</li>
<li>로깅 시스템 초기화 (Logback, Log4j 등)</li>
</ul>
<hr />
<p><strong>3️⃣ @SpringBootApplication 스캔</strong></p>
<ul>
<li>@SpringBootApplication은 내부적으로 다음 3개를 포함<ul>
<li>@SpringBootConfiguration: @Configuration과 동일, 설정 클래스 역할</li>
<li>@EnableAutoConfiguration: 자동 설정 활성화</li>
<li>@ComponentScan: 현재 패키지부터 하위 컴포넌트들(@Component, @Service 등) 스캔</li>
</ul>
</li>
</ul>
<hr />
<p><strong>4️⃣ Environment 생성 및 초기화</strong></p>
<ul>
<li>application.properties 또는 application.yml 로드</li>
<li>OS 환경 변수, JVM 옵션 등과 통합한 Environment 객체 생성</li>
</ul>
<hr />
<p><strong>5️⃣ ApplicationContext 생성 및 구성</strong></p>
<ul>
<li>스프링의 핵심인 ApplicationContext를 생성</li>
<li>앞에서 로드한 설정을 바탕으로 빈(bean) 등록 시작</li>
<li>@Component, @Service, @Repository, @Controller, @Configuration 등 모두 등록</li>
</ul>
<hr />
<p><strong>6️⃣ Auto Configuration 수행</strong></p>
<ul>
<li>@EnableAutoConfiguration 덕분에 spring.factories 안의 설정 클래스들 자동 등록<ul>
<li>예: spring-boot-starter-web을 의존성으로 넣었다면</li>
</ul>
</li>
</ul>
<p>→ DispatcherServlet, Tomcat, Jackson 등이 자동으로 설정됨</p>
<hr />
<p><strong>7️⃣ ApplicationRunner / CommandLineRunner 실행</strong></p>
<ul>
<li>빈으로 등록된 ApplicationRunner 또는 CommandLineRunner 구현 클래스가 있다면 여기서 실행됨</li>
<li>주로 초기 데이터 셋업, 테스트용 코드 등에 사용</li>
</ul>
<pre><code class="language-java">@Component
public class TestRunner implements CommandLineRunner {
    public void run(String... args) {
        System.out.println(&quot;애플리케이션 실행 후 초기 작업 수행!&quot;);
    }
}</code></pre>
<hr />
<p><strong>8️⃣ 내장 웹 서버 실행</strong></p>
<ul>
<li>내장 톰캣 (또는 Jetty, Undertow)이 실행됨</li>
<li>웹 서버가 포트를 열고 HTTP 요청을 받을 준비 완료</li>
</ul>
<hr />
<p><strong>9️⃣ 애플리케이션 실행 완료</strong></p>
<ul>
<li>사용자 요청을 받을 수 있는 상태로 진입</li>
<li>이 시점 이후부터 컨트롤러나 REST API가 동작함</li>
</ul>
<hr />
<p><strong>🔁 간단히 도식으로 요약</strong></p>
<pre><code>main()
  ↓
SpringApplication.run()
  ↓
SpringApplication 초기화
  ↓
@Configuration, @ComponentScan, @EnableAutoConfiguration
  ↓
Environment 설정
  ↓
ApplicationContext 생성
  ↓
빈 등록 및 Auto Configuration
  ↓
웹 서버 실행 (Tomcat 등)
  ↓
요청 대기 및 서비스 시작</code></pre><h1 id="spring-boot-동작-순서tcpip-부터">Spring Boot 동작 순서(TCP/IP 부터)</h1>
<p><img alt="" src="https://velog.velcdn.com/images/tkdalsrb123/post/e3a8eee5-ddf7-4735-b00f-25eaae53e14e/image.png" /></p>
<p><strong>🧭 전체 흐름 개요</strong></p>
<pre><code>[Client] → [TCP/IP Layer] → [HTTP Layer] → [Tomcat] → [Servlet] → [Spring DispatcherServlet] → [Controller]</code></pre><hr />
<p><strong>🧩 1. 클라이언트가 요청을 보냄</strong></p>
<p>브라우저나 Postman에서 예를 들어 다음과 같이 요청했다고 하자:</p>
<pre><code>GET /hello HTTP/1.1
Host: myserver.com
User-Agent: Chrome</code></pre><hr />
<p><strong>🧩 2. TCP/IP 계층 (OS 레벨)</strong></p>
<p><strong>🧱 클라이언트 → 서버로 TCP 연결 시도</strong></p>
<ul>
<li>클라이언트가 myserver.com:8080 으로 <strong>TCP 3-way handshake</strong></li>
</ul>
<pre><code>SYN → SYN-ACK → ACK</code></pre><p><strong>🧱 서버 (Spring Boot 애플리케이션 실행 중)</strong></p>
<ul>
<li><strong>내장 톰캣이 8080 포트 열고</strong> ServerSocket 으로 대기 중</li>
<li>TCP 커널 계층에서 해당 포트로 연결을 accept</li>
</ul>
<hr />
<p><strong>🧩 3. HTTP Layer (Tomcat 처리)</strong></p>
<p><strong>🧾 HTTP 요청은 사실 TCP 바이트 스트림 속에 들어있음</strong></p>
<p>예: 아래처럼 raw byte 데이터가 도착</p>
<pre><code>GET /hello HTTP/1.1\r\n
Host: myserver.com\r\n
User-Agent: Chrome\r\n
\r\n</code></pre><p><strong>🧰 톰캣의 Http11InputBuffer가 이 바이트들을 읽고 → HTTP 요청 객체 생성</strong></p>
<ul>
<li>이 객체는 HttpServletRequest 타입</li>
<li>또한 응답 객체 HttpServletResponse 도 생성됨</li>
</ul>
<hr />
<p><strong>🧩 4. 톰캣이 서블릿으로 전달</strong></p>
<ul>
<li>Tomcat이 최종적으로 호출하는 핵심 객체는 <strong>DispatcherServlet</strong></li>
<li>DispatcherServlet은 Spring에서 등록한 <strong>Front Controller 패턴</strong>이야</li>
</ul>
<p><strong>🌟 즉, 흐름은 다음과 같아:</strong></p>
<pre><code>Tomcat → DispatcherServlet.service(request, response)</code></pre><hr />
<p><strong>🧩 5. DispatcherServlet 처리</strong></p>
<p><strong>DispatcherServlet은 내부에서 다음과 같은 작업 수행:</strong></p>
<ol>
<li>HandlerMapping을 통해 어떤 Controller가 이 요청을 처리할지 찾음</li>
</ol>
<pre><code class="language-java">@GetMapping(&quot;/hello&quot;)
public String hello() { ... }</code></pre>
<ol>
<li>HandlerAdapter를 통해 실제 메서드 호출</li>
<li>메서드 결과(String, @ResponseBody, 등)를 View 또는 JSON으로 변환</li>
</ol>
<hr />
<p><strong>🧩 6. Controller까지 도달</strong></p>
<p>Spring이 매핑된 메서드를 호출:</p>
<pre><code class="language-java">@GetMapping(&quot;/hello&quot;)
public String hello() {
    return &quot;Hello from Spring!&quot;;
}</code></pre>
<p>→ 결과는 HttpServletResponse 객체에 담겨</p>
<p>→ 톰캣이 다시 이걸 <strong>HTTP 응답 → TCP 응답</strong>으로 만들어 클라이언트로 전송해줘</p>
<hr />
<p><strong>🔄 요약: 요청에서 컨트롤러까지 흐름</strong></p>
<pre><code>[브라우저/클라이언트]
   │
   └── HTTP 요청 (TCP 위에 올림)
         ↓
[서버의 TCP 계층]
   └── 커널이 포트 8080에서 리스닝 중인 Tomcat에게 전달
         ↓
[Tomcat]
   └── 바이트 스트림 → HttpServletRequest/Response 객체 생성
         ↓
[DispatcherServlet (Spring)]
   └── HandlerMapping → Controller 메서드 실행
         ↓
[Controller]
   └── 응답 반환
         ↓
[Tomcat]
   └── 응답을 HTTP 메시지 → TCP 전송
         ↓
[클라이언트가 응답 수신]</code></pre><hr />
<p><img alt="" src="https://velog.velcdn.com/images/tkdalsrb123/post/41dae786-1f76-4b94-846d-b17ef361a40a/image.png" /></p>
<h1 id="mvc-패턴과-dispatcherservlet"><strong>MVC 패턴과 DispatcherServlet</strong></h1>
<p><img alt="" src="https://velog.velcdn.com/images/tkdalsrb123/post/17d6a7f6-f792-41b7-ae8a-8f1ffbeeeddf/image.png" /></p>
<h3 id="mvc-패턴이란">MVC 패턴이란?</h3>
<p><strong>MVC (Model-View-Controller)</strong> 패턴은 웹 애플리케이션에서 로직을 세 부분으로 나누는 설계 패턴</p>
<table>
<thead>
<tr>
<th><strong>구성 요소</strong></th>
<th><strong>역할</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>Model</strong></td>
<td>데이터 및 비즈니스 로직 (ex. DB에서 데이터 가져오기, 계산 처리 등)</td>
</tr>
<tr>
<td><strong>View</strong></td>
<td>사용자에게 보여지는 화면 (HTML, JSP, Thymeleaf 등)</td>
</tr>
<tr>
<td><strong>Controller</strong></td>
<td>사용자 입력/요청 처리, Model과 View 연결</td>
</tr>
</tbody></table>
<h3 id="dispatcherservlet이란"><strong>DispatcherServlet이란?</strong></h3>
<p><strong>Spring MVC의 핵심 중앙 컨트롤러(Front Controller)</strong> 역할을 하는 서블릿이야.</p>
<p><strong>핵심 역할</strong></p>
<ul>
<li>클라이언트로부터 들어온 요청을 받아서</li>
<li>어떤 컨트롤러에 전달할지 판단하고</li>
<li>뷰를 어떻게 응답할지 결정하는 <strong>중앙 처리자</strong></li>
</ul>
<hr />
<h3 id="dispatcherservlet의-주요-책임"><strong>DispatcherServlet의 주요 책임</strong></h3>
<table>
<thead>
<tr>
<th><strong>순서</strong></th>
<th><strong>기능</strong></th>
</tr>
</thead>
<tbody><tr>
<td>1</td>
<td>클라이언트의 요청을 받는다</td>
</tr>
<tr>
<td>2</td>
<td>HandlerMapping에게 어떤 컨트롤러가 처리할지 물어본다</td>
</tr>
<tr>
<td>3</td>
<td>HandlerAdapter를 통해 실제 컨트롤러 호출</td>
</tr>
<tr>
<td>4</td>
<td>컨트롤러 로직 실행 → 결과 객체 (ModelAndView) 반환</td>
</tr>
<tr>
<td>5</td>
<td>ViewResolver를 통해 어떤 뷰를 보여줄지 결정</td>
</tr>
<tr>
<td>6</td>
<td>최종적으로 뷰 렌더링 후 클라이언트에게 응답</td>
</tr>
</tbody></table>
<hr />
<p><strong>🔄 요청 처리 전체 흐름 (Spring Context 포함)</strong></p>
<pre><code>[클라이언트 요청]
       ↓
[내장 톰캣 (WAS)]
       ↓
[DispatcherServlet (Front Controller)]
       ↓
[HandlerMapping] ← 어떤 컨트롤러로?
       ↓
[HandlerAdapter] ← 실제 컨트롤러 실행 도와줌
       ↓
[Controller (비즈니스 로직)]
       ↓
[ModelAndView 반환]
       ↓
[ViewResolver] ← 어떤 화면(View) 보여줄까?
       ↓
[View (ex. Thymeleaf, JSP)]
       ↓
[DispatcherServlet이 최종 렌더링]
       ↓
[클라이언트에게 응답]</code></pre><hr />
<p><strong>🧠 DispatcherServlet은 언제 등록되냐?</strong></p>
<ul>
<li>Spring Boot에서는 @SpringBootApplication 실행 시 DispatcherServlet이 자동으로 등록돼.</li>
<li>ServletRegistrationBean을 통해 내부적으로 등록되며, URL 패턴은 기본적으로 &quot;/&quot; (전체 요청 처리)</li>
</ul>
<hr />
<p><strong>📌 예시 코드 흐름</strong></p>
<pre><code class="language-java">@GetMapping(&quot;/hello&quot;)
public String hello(Model model) {
    model.addAttribute(&quot;msg&quot;, &quot;Hello!&quot;);
    return &quot;helloPage&quot;; // View 이름
}</code></pre>
<ul>
<li>DispatcherServlet이 /hello 요청을 감지</li>
<li>HandlerMapping으로 해당 메서드 찾음</li>
<li>HandlerAdapter가 메서드 실행</li>
<li>ViewResolver가 &quot;helloPage&quot; → helloPage.html로 변환</li>
<li>클라이언트에게 응답!</li>
</ul>
<h3 id="참고자료">참고자료</h3>
<p><a href="https://studynote.oopy.io/ff1192e9-ed72-4747-bde5-df0ca17974b5#0af10789-0af2-4c09-bcd5-6708bb177241">스프링 웹 동작 흐름</a></p>
<p><a href="https://idkim97.github.io/2022-08-22-%EC%95%84%ED%8C%8C%EC%B9%98(Apache)%EC%99%80%20%ED%86%B0%EC%BA%A3(Tomcat)/#google_vignette">아파치와 톰캣</a></p>
<p><a href="https://nyyang.tistory.com/118">MVC와 DispatcherServlet</a></p>