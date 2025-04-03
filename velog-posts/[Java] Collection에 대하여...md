<h3 id="📌-자바-컬렉션-완전-정리-코딩테스트-필수-개념-🚀">📌 자바 컬렉션 완전 정리! 코딩테스트 필수 개념 🚀</h3>
<hr />
<h2 id="💡-자바-collection-이거만-알면-끝">💡 자바 Collection, 이거만 알면 끝!</h2>
<p>자바에서 데이터를 효율적으로 다루려면 <strong>Collection</strong>을 제대로 이해해야 합니다. 코딩테스트에서는 특히 다양한 컬렉션을 활용하는 문제들이 많이 나오는데요! <strong>어떤 상황에서 어떤 컬렉션을 써야 하는지, 그리고 주요 메서드까지</strong> 정리해드릴게요. 😎</p>
<hr />
<h2 id="🔥-자바-collection-개념부터-잡자">🔥 자바 Collection 개념부터 잡자!</h2>
<h3 id="✅-collection이란">✅ Collection이란?</h3>
<p><code>Collection</code>은 여러 개의 데이터를 <strong>효율적으로 저장하고 관리</strong>하기 위한 <strong>자바의 자료구조 라이브러리</strong>입니다. <strong>크게 List, Set, Queue, Map</strong>으로 나뉘며, 각 컬렉션은 특정한 특성을 가지고 있어요.</p>
<p>📌 <strong>Collection 주요 특징</strong></p>
<ul>
<li><strong>List</strong> (순서 보장, 중복 허용) → <code>ArrayList</code>, <code>LinkedList</code>, <code>Vector</code></li>
<li><strong>Set</strong> (순서X, 중복X) → <code>HashSet</code>, <code>TreeSet</code>, <code>LinkedHashSet</code></li>
<li><strong>Queue</strong> (FIFO 구조) → <code>PriorityQueue</code>, <code>Deque</code></li>
<li><strong>Map</strong> (Key-Value 저장) → <code>HashMap</code>, <code>TreeMap</code>, <code>LinkedHashMap</code></li>
</ul>
<p>💡 코딩테스트에서는 <strong>빠른 접근이 중요한 경우 <code>HashMap</code></strong>, <strong>정렬이 필요한 경우 <code>TreeSet</code></strong>, <strong>순서를 유지해야 하면 <code>LinkedList</code></strong> 등을 사용합니다.</p>
<hr />
<h2 id="🎯-코딩테스트에서-자주-쓰는-컬렉션과-주요-메서드">🎯 코딩테스트에서 자주 쓰는 컬렉션과 주요 메서드</h2>
<h3 id="1️⃣-arraylist-배열-기반-리스트-빠른-조회">1️⃣ <code>ArrayList</code> (배열 기반 리스트, <strong>빠른 조회</strong>)</h3>
<ul>
<li><strong>특징</strong>: 가변 크기 배열, <code>get()</code> 속도 빠름 (<code>O(1)</code>), <code>add()</code> 속도 빠름 (<code>O(1)</code>)</li>
<li><strong>자주 쓰는 메서드</strong><pre><code class="language-java">List&lt;Integer&gt; list = new ArrayList&lt;&gt;();
list.add(10);  // 값 추가
list.get(0);   // 값 가져오기
list.size();   // 크기 확인
list.remove(0); // 값 삭제</code></pre>
</li>
</ul>
<h3 id="2️⃣-linkedlist-연결-리스트-빠른-삽입삭제">2️⃣ <code>LinkedList</code> (연결 리스트, <strong>빠른 삽입/삭제</strong>)</h3>
<ul>
<li><strong>특징</strong>: 노드 기반 리스트, <code>add()/remove()</code> 속도 빠름 (<code>O(1)</code>)</li>
<li><strong>자주 쓰는 메서드</strong><pre><code class="language-java">LinkedList&lt;Integer&gt; list = new LinkedList&lt;&gt;();
list.addFirst(1);  // 앞에 추가
list.addLast(2);   // 뒤에 추가
list.removeFirst(); // 앞에서 삭제
list.removeLast();  // 뒤에서 삭제</code></pre>
</li>
</ul>
<h3 id="3️⃣-hashset-중복-제거-빠른-검색">3️⃣ <code>HashSet</code> (중복 제거, <strong>빠른 검색</strong>)</h3>
<ul>
<li><strong>특징</strong>: <code>contains()</code> 속도 빠름 (<code>O(1)</code>), 순서 없음</li>
<li><strong>자주 쓰는 메서드</strong><pre><code class="language-java">Set&lt;Integer&gt; set = new HashSet&lt;&gt;();
set.add(3);
set.contains(3);  // 포함 여부 확인
set.remove(3);    // 삭제</code></pre>
</li>
</ul>
<h3 id="4️⃣-priorityqueue-우선순위-큐-힙-기반-정렬-구조">4️⃣ <code>PriorityQueue</code> (우선순위 큐, <strong>힙 기반 정렬 구조</strong>)</h3>
<ul>
<li><strong>특징</strong>: 자동 정렬됨 (최소 힙이 기본)</li>
<li><strong>자주 쓰는 메서드</strong><pre><code class="language-java">PriorityQueue&lt;Integer&gt; pq = new PriorityQueue&lt;&gt;();
pq.add(5);
pq.add(2);
pq.poll(); // 최솟값 반환 후 삭제</code></pre>
</li>
</ul>
<h3 id="5️⃣-hashmap-key-value-저장-빠른-검색">5️⃣ <code>HashMap</code> (Key-Value 저장, <strong>빠른 검색</strong>)</h3>
<ul>
<li><strong>특징</strong>: <code>get()/put()</code> 속도 빠름 (<code>O(1)</code>)</li>
<li><strong>자주 쓰는 메서드</strong><pre><code class="language-java">Map&lt;String, Integer&gt; map = new HashMap&lt;&gt;();
map.put(&quot;apple&quot;, 10);
map.get(&quot;apple&quot;);  // 값 가져오기
map.containsKey(&quot;apple&quot;); // 키 존재 여부 확인</code></pre>
</li>
</ul>
<hr />
<h2 id="🎖️-코딩테스트에서-언제-어떤-컬렉션을-쓸까">🎖️ 코딩테스트에서 언제 어떤 컬렉션을 쓸까?</h2>
<table>
<thead>
<tr>
<th><strong>상황</strong></th>
<th><strong>추천 컬렉션</strong></th>
</tr>
</thead>
<tbody><tr>
<td>데이터 순서 유지 &amp; 빠른 조회</td>
<td><code>ArrayList</code></td>
</tr>
<tr>
<td>빠른 삽입/삭제 필요할 때</td>
<td><code>LinkedList</code></td>
</tr>
<tr>
<td>중복 제거 &amp; 빠른 탐색</td>
<td><code>HashSet</code></td>
</tr>
<tr>
<td>정렬이 필요한 경우</td>
<td><code>TreeSet</code></td>
</tr>
<tr>
<td>우선순위가 중요한 경우</td>
<td><code>PriorityQueue</code></td>
</tr>
<tr>
<td>(Key, Value) 저장 &amp; 빠른 검색</td>
<td><code>HashMap</code></td>
</tr>
</tbody></table>
<hr />
<h2 id="💬-마무리">💬 마무리!</h2>
<p>자바에서 <strong>Collection</strong>을 잘 활용하면 <strong>코딩테스트에서 코드 속도와 가독성을 높일 수 있습니다</strong>. 코딩테스트 문제를 풀 때는 <strong>필요한 기능이 무엇인지 먼저 판단한 뒤, 가장 적절한 컬렉션을 선택</strong>하는 것이 중요해요! 🚀</p>
<p>여러분은 코딩테스트에서 <strong>어떤 컬렉션을 가장 자주 사용하시나요?</strong> 🤔 댓글로 공유해 주세요! 😊</p>