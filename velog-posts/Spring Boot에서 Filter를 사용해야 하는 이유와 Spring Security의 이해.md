<p>Spring Boot로 웹 백엔드 개발을 공부하면서 가장 먼저 마주한 기능 중 하나는 <strong>Filter</strong>다.<br />단순한 전/후처리 정도로만 여겨졌던 Filter가 사실은 <strong>Spring Security 같은 보안 프레임워크의 핵심 기반</strong>이라는 것을 알게 되면서 생각이 많이 확장되었다.<br />이 글에서는 내가 이해한 내용을 정리해본다.</p>
<hr />
<h2 id="☕-filter는-어디서-왔을까">☕ Filter는 어디서 왔을까?</h2>
<p><code>javax.servlet.Filter</code>는 <strong>Spring이 만든 기능이 아니라</strong>,  
<strong>Java Servlet 스펙에 정의된 표준 인터페이스</strong>이다.<br />즉, 톰캣, 제티 같은 서블릿 컨테이너에서 요청(Request)과 응답(Response)을 가로채서 처리할 수 있도록 만들어진 구조다.</p>
<hr />
<h2 id="🚀-spring-boot에서-filter를-사용할-수-있는-이유">🚀 Spring Boot에서 Filter를 사용할 수 있는 이유</h2>
<p>Spring Boot에서는 <code>spring-boot-starter-web</code> 의존성만 추가하면 다음과 같은 기능이 자동 포함된다.</p>
<ul>
<li>서블릿 API (<code>javax.servlet.*</code>)</li>
<li>내장 톰캣</li>
<li>Spring MVC</li>
</ul>
<p>그래서 별도로 <code>Filter</code>를 위한 의존성을 추가하지 않아도, 바로 다음과 같이 사용할 수 있다.</p>
<pre><code class="language-java">@Component
public class LoggingFilter implements Filter {
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
        throws IOException, ServletException {
        System.out.println(&quot;요청이 들어왔습니다&quot;);
        chain.doFilter(request, response);
        System.out.println(&quot;응답을 반환합니다&quot;);
    }
}</code></pre>
<p>Filter는 <code>chain.doFilter()</code>를 기준으로 앞뒤에 코드를 작성하여 <strong>요청 전/후를 가로채는 구조</strong>로 작동한다.<br />이는 마치 Controller를 감싸는 <strong>중간 인터셉터</strong>처럼 동작하며, <strong>공통 처리, 보안 전처리, 로깅 등 다양한 목적</strong>으로 활용된다.</p>
<hr />
<h2 id="🎯-filter를-사용하는-이유">🎯 Filter를 사용하는 이유</h2>
<p>Filter는 다음과 같은 상황에서 유용하게 사용된다:</p>
<table>
<thead>
<tr>
<th align="left">사용 목적</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody><tr>
<td align="left">✅ 요청 로깅</td>
<td align="left">어떤 요청이 들어왔고, 어떤 응답이 나갔는지 기록</td>
</tr>
<tr>
<td align="left">✅ 응답 시간 측정</td>
<td align="left">요청이 들어온 시점부터 응답까지 걸린 시간 계산</td>
</tr>
<tr>
<td align="left">✅ JWT 인증</td>
<td align="left">Authorization 헤더에서 토큰 추출 및 검증</td>
</tr>
<tr>
<td align="left">✅ 공통 인코딩 처리</td>
<td align="left">모든 요청에 UTF-8 적용 등</td>
</tr>
<tr>
<td align="left">✅ CORS 처리</td>
<td align="left">클라이언트 도메인 접근 허용 헤더 설정</td>
</tr>
<tr>
<td align="left">✅ 헤더 및 파라미터 조작</td>
<td align="left">필터 레벨에서 파라미터 변경, 헤더 추가 등</td>
</tr>
</tbody></table>
<p>Filter는 Controller에 도달하기 전에 작동하므로,<br /><strong>모든 요청에 공통적으로 적용하고 싶은 로직</strong>을 담는 데 적합하다.</p>
<hr />
<h2 id="🔐-spring-security는-필터-체인-기반-보안-프레임워크">🔐 Spring Security는 필터 체인 기반 보안 프레임워크</h2>
<p>Spring Security는 결국 <strong>Filter 기반 구조를 확장한 보안 프레임워크</strong>다.<br />서블릿의 <code>javax.servlet.Filter</code>를 기반으로 하여,<br /><strong>인증(Authentication), 인가(Authorization)</strong> 같은 보안 로직을 수십 개의 필터로 나누어 처리한다.</p>
<p>이 필터들은 내부적으로 <strong>FilterChainProxy</strong>라는 메인 필터에 의해 연결되어 작동한다.</p>
<p><img alt="spring security와 filter" src="https://docs.spring.io/spring-security/reference/_images/servlet/architecture/filterchainproxy.png" /></p>
<h3 id="🔗-예시-주요-security-필터">🔗 예시: 주요 Security 필터</h3>
<table>
<thead>
<tr>
<th align="left">필터 이름</th>
<th align="left">역할</th>
</tr>
</thead>
<tbody><tr>
<td align="left"><code>SecurityContextPersistenceFilter</code></td>
<td align="left">세션에서 인증 정보 로드 및 저장</td>
</tr>
<tr>
<td align="left"><code>UsernamePasswordAuthenticationFilter</code></td>
<td align="left">로그인 요청 처리 (form 기반)</td>
</tr>
<tr>
<td align="left"><code>BearerTokenAuthenticationFilter</code></td>
<td align="left">JWT 토큰 인증</td>
</tr>
<tr>
<td align="left"><code>ExceptionTranslationFilter</code></td>
<td align="left">인증/인가 실패 예외 처리</td>
</tr>
<tr>
<td align="left"><code>FilterSecurityInterceptor</code></td>
<td align="left">권한(ROLE) 체크 수행</td>
</tr>
</tbody></table>
<p>이 구조는 일반 Filter처럼 <strong><code>chain.doFilter()</code>를 중심으로 감싸는 구조</strong>이며,<br />필요하면 내가 만든 JWT 필터를 <strong><code>addFilterBefore()</code></strong> 등을 통해 끼워 넣을 수도 있다.</p>
<hr />
<h2 id="🧠-핵심-정리">🧠 핵심 정리</h2>
<ul>
<li><code>javax.servlet.Filter</code>는 Spring이 아닌 Java EE(Jakarta EE)의 표준이다.</li>
<li>Spring Boot에서는 <code>spring-boot-starter-web</code>만으로 Filter를 바로 사용할 수 있다.</li>
<li>Filter는 <code>chain.doFilter()</code>를 기준으로 요청/응답을 전후 처리한다.</li>
<li><strong>Spring Security는 수십 개의 필터로 구성된 보안 필터 체인을 기반으로 작동</strong>한다.</li>
<li>Filter를 먼저 이해하면 Spring Security의 구조도 자연스럽게 이해된다.</li>
</ul>
<hr />
<blockquote>
<p>결국, Spring Security도 Filter의 확장이다.<br />기초 개념을 이해하고 직접 Filter를 구현해보면,<br />Spring Security의 흐름과 인증 처리 로직도 훨씬 명확하게 다가온다.</p>
</blockquote>