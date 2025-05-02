<h2 id="🎯-테스트의-본질은-100-커버리지가-아니다">🎯 테스트의 본질은 '100% 커버리지'가 아니다</h2>
<p>테스트의 가치는 단순히 모든 코드를 한 번 실행해봤다는 <strong>&quot;커버리지 수치&quot;</strong>에서 오는 것이 아니다.</p>
<p>오히려 중요한 건 다음과 같다.</p>
<ul>
<li>✅ <strong>테스트가 확인하지 못한 코드가 어떤 것인가?</strong></li>
<li>✅ <strong>테스트가 실제 프로그래밍 실수를 얼마나 잘 잡아내는가?</strong></li>
</ul>
<blockquote>
<p>100% 커버리지를 달성했다고 해도</p>
<p>→ 애플리케이션이 올바르게 동작하는 것을 <strong>보장하진 않는다.</strong></p>
</blockquote>
<h2 id="테스트커버리지"><img alt="테스트커버리지" src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKQlc7%2FbtqALQ3sJnv%2FCQzAMuWkgiPqUMQmDpuFr0%2Fimg.png" /></h2>
<h2 id="✅-커버리지-강박보다-의미-있는-테스트에-집중하자">✅ 커버리지 강박보다 '의미 있는 테스트'에 집중하자</h2>
<table>
<thead>
<tr>
<th>❌ 좋지 않은 테스트</th>
<th>✅ 좋은 테스트</th>
</tr>
</thead>
<tbody><tr>
<td>단순 실행만 해서 커버리지만 높이는 테스트</td>
<td>명확한 조건 → 기대 결과를 검증하는 테스트</td>
</tr>
<tr>
<td>assert 없이 실행만 하고 끝남</td>
<td>실패/예외를 정확히 잡아내는 테스트</td>
</tr>
<tr>
<td>복잡한 로직을 무조건 cover만 하려는 테스트</td>
<td>예외 상황, 경계값을 고려한 케이스 중심 테스트</td>
</tr>
</tbody></table>
<blockquote>
<p>&quot;테스트는 품질 보증 수단인 동시에, 코드 설계 도구다.&quot;</p>
</blockquote>
<hr />
<h2 id="🧩-tdd의-진짜-정체">🧩 TDD의 진짜 정체</h2>
<blockquote>
<p>TDD(Test-Driven Development)는</p>
<p><strong>테스트 기술이 아니라, 분석과 설계 기술이다.</strong></p>
</blockquote>
<p>📖 *&quot;TDD의 아이러니 중 하나는 테스트 기술이 아니라는 점이다.
TDD는 분석 기술이며, 설계 기술이기도 하다.&quot;*
— 켄트 벡, <em>Test-Driven Development by Example</em></p>
<hr />
<h2 id="🛠️-tdd와-일반-개발-방식-비교">🛠️ TDD와 일반 개발 방식 비교</h2>
<table>
<thead>
<tr>
<th>단계</th>
<th>일반 개발</th>
<th>TDD 개발</th>
</tr>
</thead>
<tbody><tr>
<td>1단계</td>
<td>로직 먼저 작성</td>
<td>실패하는 테스트 먼저 작성</td>
</tr>
<tr>
<td>2단계</td>
<td>예외 처리 추가</td>
<td>예외 케이스 테스트부터 작성</td>
</tr>
<tr>
<td>3단계</td>
<td>테스트 작성</td>
<td>테스트 통과를 위한 최소 로직 작성</td>
</tr>
<tr>
<td>4단계</td>
<td>리팩터링 (종종 테스트 깨짐)</td>
<td><strong>테스트 통과를 유지한 채</strong> 구조를 정리</td>
</tr>
</tbody></table>
<hr />
<h2 id="❗️-tdd에서-리팩터링이란">❗️ TDD에서 리팩터링이란?</h2>
<p>리팩터링이란 단순히 &quot;코드를 다시 짠다&quot;는 뜻이 아니다.</p>
<p>리팩터링은 <strong>동작은 그대로 두면서 코드의 구조만 개선하는 작업</strong>이어야 한다.</p>
<p>테스트가 깨진다면 → <strong>그건 리팩터링이 아니라, 기능 변경</strong>이다.</p>
<blockquote>
<p>리팩터링은 역할과 협력 구조를 재조정하는 작업이다.</p>
</blockquote>
<hr />
<h2 id="🧠-tdd를-잘-하려면">🧠 TDD를 잘 하려면?</h2>
<blockquote>
<p>결국 객체 간 메시지 흐름과 책임 분리를 명확히 이해해야 한다.</p>
</blockquote>
<ul>
<li>어떤 객체가 누구에게 메시지를 보내는가?</li>
<li>이 책임은 이 객체가 가져야 할 일인가?</li>
<li>테스트는 이 상호작용을 드러내고 있는가?</li>
</ul>
<hr />
<h2 id="✍️-마무리">✍️ 마무리</h2>
<p>TDD를 잘 한다는 것은</p>
<p>단순히 테스트 코드를 먼저 쓰는 습관이 아니라,</p>
<p><strong>기능 요구를 명확히 분석하고, 설계 방향을 명확히 표현할 수 있는 사고방식</strong>을 갖는 것이다.</p>
<p>테스트는 품질을 높이는 수단이기도 하지만,</p>
<p>그보다 더 근본적으로는 <strong>더 좋은 설계를 만들기 위한 도구</strong>다.</p>
<blockquote>
<p>&quot;좋은 테스트는 코드보다 먼저 명확한 생각을 요구한다.&quot;</p>
</blockquote>
<h3 id="참고자료">참고자료</h3>
<p><a href="https://dublin-java.tistory.com/57">https://dublin-java.tistory.com/57</a>
<a href="https://engineerinsight.tistory.com/34">https://engineerinsight.tistory.com/34</a>
<a href="https://inpa.tistory.com/entry/QA-%F0%9F%93%9A-TDD-%EB%B0%A9%EB%B2%95%EB%A1%A0-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%A3%BC%EB%8F%84-%EA%B0%9C%EB%B0%9C">https://inpa.tistory.com/entry/QA-📚-TDD-방법론-테스트-주도-개발</a></p>