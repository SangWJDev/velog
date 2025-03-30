<p>Spring boot로 제작한 프로젝트의 마무리 단계에서 swagger를 활용해 OpenAPI를 작성해 보려고 한다. OpenAPI<img alt="" src="https://velog.velcdn.com/images/tkdalsrb123/post/321ca85f-deba-4a94-aec8-8603fb884d5e/image.png" />
에 대해 찾아보려니 헷갈리는 부분에 대해 명확히 짚고 넘어가고자 한다. </p>
<h2 id="openapi-vs-open-api">OpenAPI VS Open API</h2>
<p>이 두 용어는 공백 한끗차이이다. 하지만 전혀 다른 뜻을 가지고 있으며 앞으로 설명할 때 OpenAPI는 OAS: OpenAPI Specification 라고 말하겠다.
OAS는 RESTful API 기반으로 정의된 규칙에 맞게 API spec을 json이나 yaml 방식으로 표현하는 용어이다. 직접 소스코드나 문서를 보지 않고 서비스를 이해할 수 있다는 장점이 있다.
 Open API는 개방된 API로 누구나 사용할 수 있도록 API의 엔드포인트를 개방해둔 것으로 누구나 해당 API를 요청하여 데이터를 제공 받을 수 있다.
<img alt="공공데이터포털 Open API 소개" src="https://velog.velcdn.com/images/tkdalsrb123/post/5ea579db-b295-4aa2-b086-6c49956faf89/image.png" title="https://www.data.go.kr/ugs/selectPublicDataUseGuideView.do" /></p>
<h2 id="swagger20-swagger30-openapi30">Swagger2.0? Swagger3.0? OpenAPI3.0?</h2>
<p>Swagger에 대해 찾아보면 version에 따라 부르는 이름들이 달라 헷갈린다. 왜그러냐하면 Swagger는 2010년대 초 Tam Wordink라는 사람이 Wordnik 기업 자체 API용 UI로 개발하였고 2015년 초에는 SmartBear라는 회사에서 Swagger를 인수하였다. 2015년 말에 OpenAPI Initiative에 Swagger를 기부하면서 OpenAPI Specification(OAS)로 이름이 변경되었다. 그래서 2.0을 표기할 때에는 <code>&quot;swagger&quot;: &quot;2.0&quot;</code>, 3.0을 표기할 때에는 <code>&quot;openapi&quot;: &quot;3.0&quot;</code>이라고 한다. 즉, Swagger는 OAS를 작성하기 위한 하나의 tool이며 중간에 이러한 과정때문에 사람들이 많이들 혼용해서 사용하는 것 같다.</p>
<h2 id="api는-무엇인가">API는 무엇인가?</h2>
<p>이제 계속해서 나오는 용어 API에 대해 자세히 알아보자. API는 Application Programming Interface로 다양한 애플리케이션과 서비스가 서로 소통하고 데이터를 교환하는 방식이다. 예를들면 프론트엔드 개발자와 백엔드 개발자가 서로 데이터를 주고 받을 때 사용하는 방식이 API이다. 특징으로는 다음과 같다.</p>
<ul>
<li>추상화: 복잡한 내부 로직을 숨기고 간단한 인터페이스 제공</li>
<li>표준화: 일관된 방식으로 데이터 교환 가능</li>
<li>모듈성: 애플리케이션을 독립적인 모듈로 분리 가능</li>
<li>확장성: 기존 시스템에 새로운 기능 추가 용이</li>
</ul>
<h2 id="api의-종류">API의 종류</h2>
<p>개발을 시작하기에 앞서 프론트엔드 개발자와 백엔드 개발자가 어떤 식으로 데이터를 주고 받을지 고민해볼 것이다. 우선 API의 종류에 대해 찾아보고 해당 프로젝트에 어울리는 API를 선택하면 될 것 같다.</p>
<h3 id="1-rest-api">1. REST API</h3>
<p><strong>REST (Representational State Transfer)</strong>는 HTTP 프로토콜을 기반으로 한 웹 API 디자인 패턴이다.</p>
<ul>
<li><strong>특징</strong><ul>
<li>클라이언트-서버 구조</li>
<li>무상태성 (Stateless)</li>
<li>캐시 가능 (Cacheable)</li>
<li>계층 구조 (Layered System)</li>
</ul>
</li>
<li><strong>예제 (JSON 기반 REST API 요청 및 응답)</strong><pre><code class="language-http">GET /users/1 HTTP/1.1
Host: example.com
Content-Type: application/json</code></pre>
<pre><code class="language-json">{
  &quot;id&quot;: 1,
  &quot;name&quot;: &quot;홍길동&quot;,
  &quot;email&quot;: &quot;hong@example.com&quot;
}</code></pre>
</li>
</ul>
<h3 id="2-soap-api">2. SOAP API</h3>
<p><strong>SOAP (Simple Object Access Protocol)</strong>는 XML 기반의 프로토콜로, 주로 엔터프라이즈 시스템에서 사용된다.</p>
<ul>
<li><strong>특징</strong><ul>
<li>XML 형식의 메시지 교환</li>
<li>높은 보안성과 트랜잭션 지원</li>
<li>HTTP, SMTP 등 다양한 프로토콜에서 동작 가능</li>
</ul>
</li>
<li><strong>예제 (SOAP 요청 및 응답)</strong><pre><code class="language-xml">&lt;soapenv:Envelope xmlns:soapenv=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot;&gt;
  &lt;soapenv:Body&gt;
    &lt;getUser&gt;
      &lt;id&gt;1&lt;/id&gt;
    &lt;/getUser&gt;
  &lt;/soapenv:Body&gt;
&lt;/soapenv:Envelope&gt;</code></pre>
<pre><code class="language-xml">&lt;soapenv:Envelope&gt;
  &lt;soapenv:Body&gt;
    &lt;getUserResponse&gt;
      &lt;name&gt;홍길동&lt;/name&gt;
      &lt;email&gt;hong@example.com&lt;/email&gt;
    &lt;/getUserResponse&gt;
  &lt;/soapenv:Body&gt;
&lt;/soapenv:Envelope&gt;</code></pre>
</li>
</ul>
<h3 id="3-graphql-api">3. GraphQL API</h3>
<p><strong>GraphQL</strong>은 Facebook에서 개발한 쿼리 언어로, 클라이언트가 원하는 데이터만 요청할 수 있는 유연한 API이다.</p>
<ul>
<li><strong>특징</strong><ul>
<li>클라이언트가 필요한 데이터만 요청 가능</li>
<li>단일 엔드포인트에서 다양한 데이터 조회 가능</li>
<li>중첩된 데이터 요청이 용이함</li>
</ul>
</li>
<li><strong>예제 (GraphQL 쿼리 및 응답)</strong><pre><code class="language-graphql">query {
  user(id: 1) {
    name
    email
  }
}</code></pre>
<pre><code class="language-json">{
  &quot;data&quot;: {
    &quot;user&quot;: {
      &quot;name&quot;: &quot;홍길동&quot;,
      &quot;email&quot;: &quot;hong@example.com&quot;
    }
  }
}</code></pre>
</li>
</ul>
<h3 id="4-grpc-api">4. gRPC API</h3>
<p><strong>gRPC</strong>는 Google에서 개발한 고성능 원격 프로시저 호출(Remote Procedure Call) 프레임워크이다.</p>
<ul>
<li><p><strong>특징</strong></p>
<ul>
<li>프로토콜 버퍼(Protocol Buffers, ProtoBuf)를 사용하여 직렬화</li>
<li>HTTP/2 기반의 빠른 통신</li>
<li>양방향 스트리밍 지원</li>
</ul>
</li>
<li><p><strong>예제 (gRPC 서비스 정의 및 요청)</strong></p>
<pre><code class="language-proto">syntax = &quot;proto3&quot;;

service UserService {
  rpc GetUser (UserRequest) returns (UserResponse);
}

message UserRequest {
  int32 id = 1;
}

message UserResponse {
  string name = 1;
  string email = 2;
}</code></pre>
</li>
</ul>
<h3 id="5-api-선택-기준">5. API 선택 기준</h3>
<ul>
<li><strong>데이터 형식과 구조</strong>: REST는 JSON, SOAP는 XML, gRPC는 ProtoBuf를 사용함.</li>
<li><strong>성능</strong>: REST는 HTTP 기반, gRPC는 HTTP/2 기반으로 속도가 더 빠름.</li>
<li><strong>보안</strong>: SOAP는 높은 보안성을 제공하지만 무겁고, REST는 OAuth 같은 인증 방식을 활용함.</li>
<li><strong>사용 편의성</strong>: GraphQL은 유연한 데이터 요청이 가능하나 학습 곡선이 있음.</li>
</ul>
<h2 id="api-설계">API 설계</h2>
<p>어떤 종류의 API를 사용할지 정했으면 프로젝트 구성원들과 함께 API 규칙을 정하여야 한다. 무작정 API 규칙을 정하는 것보다 각각의 API 설계 원칙에 고려하여 규칙을 작성하는 것이 현명한 판단이 될 것이다.</p>
<h3 id="restful-api-설계">RESTful API 설계</h3>
<p>REST에는 여섯가지의 기본 원칙이 있으며, 이 원칙을 준수한 인터페이스는 RESTful 하다고 표현한다.</p>
<h3 id="1-clientserver-구조">1. Client–Server 구조</h3>
<ul>
<li>클라이언트와 서버는 서로 독립적으로 개발될 수 있어야 한다.</li>
<li>클라이언트는 오직 URI를 통해 리소스를 식별해야 하며, 서버의 내부 구현에 의존하지 않아야 한다.</li>
<li>인터페이스가 변경되지 않는 한, 클라이언트와 서버는 독립적으로 변경 및 대체될 수 있다.</li>
<li><strong>(관심사의 명확한 분리, Separation of Concerns)</strong></li>
</ul>
<h3 id="2-stateless-무상태성">2. Stateless (무상태성)</h3>
<ul>
<li>각 클라이언트 요청에는 요청을 처리하는 데 필요한 모든 정보가 포함되어야 한다.</li>
<li>서버는 클라이언트의 상태를 저장하지 않으며, 요청 간 세션 정보를 유지하지 않는다.</li>
<li>인증 및 인가 정보 또한 클라이언트가 관리하며, 각 요청 시 포함하여 서버에 전달해야 한다.</li>
</ul>
<h3 id="3-cacheable-캐싱-가능">3. Cacheable (캐싱 가능)</h3>
<ul>
<li>서버는 <code>Response cache-control</code> 헤더를 활용하여 해당 응답이 캐싱 가능한지 여부를 제공해야 한다.</li>
<li>클라이언트는 캐싱 가능한 응답을 저장하여 서버와의 불필요한 요청을 줄이고 성능을 향상시킬 수 있다.</li>
<li>적절한 캐싱을 활용하면 서버의 부하를 줄이고 가용성을 높일 수 있다.</li>
</ul>
<h3 id="4-uniform-interface-일관된-인터페이스">4. Uniform Interface (일관된 인터페이스)</h3>
<p>REST는 시스템 전체를 단순화하고 가시성을 개선하기 위해 <strong>4가지 인터페이스 규칙</strong>을 정의한다.</p>
<ol>
<li><p><strong>Identification of Resources (자원의 식별)</strong>  </p>
<ul>
<li>각 요청에서 개별 자원을 명확히 식별할 수 있어야 한다.  </li>
<li>예: <code>GET /users/1</code> (ID가 1인 사용자를 조회)</li>
</ul>
</li>
<li><p><strong>Manipulation of Resources Through Representations (자원의 표현을 통한 조작)</strong>  </p>
<ul>
<li>클라이언트는 적절한 표현과 메타데이터를 통해 서버가 자원을 변경, 삭제할 수 있도록 요청해야 한다.  </li>
<li>예: <code>PUT /users/1</code> 요청 시 사용자 정보를 JSON 형식으로 제공하여 수정 가능하게 함.</li>
</ul>
</li>
<li><p><strong>Self-Descriptive Messages (자체 설명적 메시지)</strong>  </p>
<ul>
<li>메시지 자체에 요청을 어떻게 처리해야 하는지에 대한 정보가 포함되어야 한다.  </li>
<li>예: <code>Content-Type</code> 헤더를 활용하여 응답 데이터의 형식을 명확히 지정.</li>
</ul>
</li>
<li><p><strong>Hypermedia as the Engine of Application State (HATEOAS, 하이퍼미디어 기반 상태 전이)</strong>  </p>
<ul>
<li>응답에는 단순한 데이터뿐만 아니라, 관련된 링크를 포함하여 상태 전이를 가능하게 해야 한다.  </li>
<li>예: <code>GET /users/1</code> 응답에 해당 사용자의 프로필 수정 링크 포함.</li>
</ul>
</li>
</ol>
<h3 id="5-layered-system-다중-계층-구조">5. Layered System (다중 계층 구조)</h3>
<ul>
<li>REST 아키텍처는 다중 계층을 지원하며, API 서버, 인증 서버, 데이터베이스 서버 등을 분리할 수 있다.</li>
<li>각 계층은 자신과 직접 통신하는 계층만을 인식하며, 전체 시스템의 구조를 알 필요가 없다.</li>
<li>클라이언트는 API 서버와만 통신할 수 있으며, 내부의 다른 계층(예: DB 서버)과 직접 상호작용할 수 없다.</li>
</ul>
<h3 id="6-code-on-demand-선택-사항">6. Code on Demand (선택 사항)</h3>
<ul>
<li>서버는 클라이언트에서 실행할 수 있는 코드를 제공할 수 있다.</li>
<li>이를 통해 클라이언트가 사전에 구현해야 할 기능을 줄이고 확장성을 높일 수 있다.</li>
<li>예: JavaScript 코드 스니펫을 API 응답으로 제공하여 클라이언트에서 실행되도록 함.</li>
</ul>
<h2 id="블로그-api-설계">블로그 API 설계</h2>
<h3 id="1-리소스-정의">1. 리소스 정의</h3>
<p>블로그 API에서 다룰 주요 리소스는 다음과 같다.</p>
<ul>
<li><strong>게시글 (posts)</strong></li>
<li><strong>댓글 (comments)</strong></li>
<li><strong>사용자 (users)</strong></li>
<li><strong>카테고리 (categories)</strong></li>
</ul>
<hr />
<h3 id="2-엔드포인트-설계">2. 엔드포인트 설계</h3>
<p>RESTful 원칙을 따르는 엔드포인트는 아래와 같이 정의된다.</p>
<h4 id="📝-게시글-관련">📝 <strong>게시글 관련</strong></h4>
<table>
<thead>
<tr>
<th>메서드</th>
<th>엔드포인트</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><code>GET</code></td>
<td><code>/posts</code></td>
<td>모든 게시글 조회</td>
</tr>
<tr>
<td><code>GET</code></td>
<td><code>/posts/{id}</code></td>
<td>특정 게시글 조회</td>
</tr>
<tr>
<td><code>POST</code></td>
<td><code>/posts</code></td>
<td>새 게시글 작성</td>
</tr>
<tr>
<td><code>PUT</code></td>
<td><code>/posts/{id}</code></td>
<td>게시글 수정</td>
</tr>
<tr>
<td><code>DELETE</code></td>
<td><code>/posts/{id}</code></td>
<td>게시글 삭제</td>
</tr>
</tbody></table>
<h4 id="💬-댓글-관련">💬 <strong>댓글 관련</strong></h4>
<table>
<thead>
<tr>
<th>메서드</th>
<th>엔드포인트</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><code>GET</code></td>
<td><code>/posts/{id}/comments</code></td>
<td>특정 게시글의 모든 댓글 조회</td>
</tr>
<tr>
<td><code>POST</code></td>
<td><code>/posts/{id}/comments</code></td>
<td>특정 게시글에 새 댓글 작성</td>
</tr>
<tr>
<td><code>PUT</code></td>
<td><code>/comments/{id}</code></td>
<td>댓글 수정</td>
</tr>
<tr>
<td><code>DELETE</code></td>
<td><code>/comments/{id}</code></td>
<td>댓글 삭제</td>
</tr>
</tbody></table>
<h4 id="👤-사용자-관련">👤 <strong>사용자 관련</strong></h4>
<table>
<thead>
<tr>
<th>메서드</th>
<th>엔드포인트</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><code>GET</code></td>
<td><code>/users/{id}</code></td>
<td>특정 사용자 정보 조회</td>
</tr>
<tr>
<td><code>POST</code></td>
<td><code>/users</code></td>
<td>새 사용자 등록</td>
</tr>
<tr>
<td><code>PUT</code></td>
<td><code>/users/{id}</code></td>
<td>사용자 정보 수정</td>
</tr>
<tr>
<td><code>GET</code></td>
<td><code>/users/{id}/posts</code></td>
<td>특정 사용자의 모든 게시글 조회</td>
</tr>
</tbody></table>
<h4 id="🏷-카테고리-관련">🏷 <strong>카테고리 관련</strong></h4>
<table>
<thead>
<tr>
<th>메서드</th>
<th>엔드포인트</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><code>GET</code></td>
<td><code>/categories</code></td>
<td>모든 카테고리 조회</td>
</tr>
<tr>
<td><code>GET</code></td>
<td><code>/categories/{id}/posts</code></td>
<td>특정 카테고리의 모든 게시글 조회</td>
</tr>
</tbody></table>
<p>이러한 API 설계를 기반으로 블로그 서비스의 RESTful API를 구축할 수 있다.</p>
<h2 id="마무리">마무리</h2>
<p>Swagger로 OpenAPI를 작성하려다 보니 자연스럽게 REST API 설계까지 깊이 있게 공부하게 되었다. 결국 하나의 도구나 기술을 제대로 활용하려면 그에 필요한 배경 지식까지 폭넓게 이해해야 한다는 점을 다시금 깨달았다. 하지만 그만큼 API 설계를 더 확실하게 이해할 수 있었고, 왜 이렇게 설계해야 하는지에 대한 이유도 더욱 명확해졌다. </p>
<p>이번 학습을 통해 API에 대한 개념을 탄탄히 다질 수 있었고, 앞으로의 개발 과정에서도 더 체계적으로 API를 설계할 수 있을 것 같다.</p>
<h2 id="출처">출처</h2>
<p><a href="https://gruuuuu.github.io/programming/openapi/">Swagger와 OpenAPI</a>
<a href="https://jinn-blog.tistory.com/190">API 설계</a>
<a href="https://prohannah.tistory.com/156">RESTful API</a></p>