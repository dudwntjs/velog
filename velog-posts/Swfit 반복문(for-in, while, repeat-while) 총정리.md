<p>프로그래밍에서 특정 코드 블록을 반복해서 실행하는 것을 루프(Loop)라고 합니다.</p>
<p>Swift에서는 반복 횟수가 미리 정해져 있는지에 따라 크게 <strong>두 가지 방식</strong>으로 나뉘며, 세부적으로는 <strong>세 가지 구문</strong>을 제공합니다.</p>
<h3 id="📌-swift-반복문"><strong>📌 Swift 반복문?</strong></h3>
<p>반복문은 기준에 따라 크게 다음과 같이 분류됩니다.</p>
<ol>
<li><p><strong>횟수에 의한 반복</strong>: <code>for-in</code> 구문 (정해진 횟수만큼!)</p>
</li>
<li><p><strong>조건에 의한 반복</strong>: <code>while</code> 및 <code>repeat-while</code> 구문 (조건이 맞을 때까지!)</p>
<br />
</li>
</ol>
<hr />
<h2 id="1-정해진-횟수만큼-반복하는-for-in-구문"><strong>1. 정해진 횟수만큼 반복하는 <code>for-in</code> 구문</strong></h2>
<p><code>for-in</code> 구문은 스위프트에서 가장 많이 사용되는 반복문입니다. 주로 배열, 범위 데이터, 문자열 같은 <strong>순회 대상</strong>의 크기만큼 반복됩니다.</p>
<h4 id="기본-형식"><strong>기본 형식</strong></h4>
<pre><code class="language-swift">for &lt;루프 상수&gt; in &lt;순회 대상&gt; {
    &lt;실행할 구문&gt;
}</code></pre>
<ul>
<li><strong>순회 대상:</strong> 배열(Array), 딕셔너리(Dictionary), 집합(Set), 범위 데이터, 문자열 등이 올 수 있습니다.</li>
<li><strong>루프 상수:</strong> 순회 대상의 아이템을 차례로 넘겨받는 상수로, <code>{ }</code> 안에서만 사용 가능합니다. 별도로 선언(<code>let</code>)할 필요 없이 자동으로 생성됩니다.</li>
</ul>
<h4 id="주요-특징-및-예시"><strong>주요 특징 및 예시</strong></h4>
<ol>
<li><p><strong>범위 연산자 활용:</strong> <code>1...5</code>와 같이 사용하면 5번 반복합니다.</p>
<pre><code class="language-swift"> for row in 1...5 {
     print(row) // 1, 2, 3, 4, 5 출력
 }</code></pre>
</li>
<li><p><strong>문자열 순회:</strong> 문자열의 각 문자(Character)를 하나씩 추출할 때 사용합니다.</p>
<pre><code class="language-swift"> for char in &quot;swift&quot;.characters {
     print(char)
 }</code></pre>
</li>
<li><p><strong>루프 상수 생략 (<code>_</code>):</strong> 반복 횟수만 중요하고 데이터 값은 필요 없을 때 언더바(<code>_</code>)를 사용합니다.</p>
<pre><code class="language-swift"> for _ in 1...5 {
     print(&quot;Hello!&quot;) // 5번 반복 출력
 }</code></pre>
</li>
<li><p><strong>중첩 루프:</strong> 구구단처럼 반복문 안에 반복문을 넣어 사용할 수 있습니다.</p>
</li>
</ol>
<hr />
<h2 id="2-조건에-따라-반복하는-while-구문"><strong>2. 조건에 따라 반복하는 <code>while</code> 구문</strong></h2>
<p><code>while</code> 구문은 반복 횟수가 명확하지 않거나, 특정 조건이 만족되는 동안 계속 실행해야 할 때 사용합니다.</p>
<h4 id="기본-형식-1"><strong>기본 형식</strong></h4>
<pre><code class="language-swift">while &lt;조건식&gt; {
    &lt;실행할 구문&gt;
}</code></pre>
<ul>
<li><strong>작동 원리:</strong> 루프를 시작하기 전에 <strong>조건식을 먼저 평가</strong>합니다. 조건이 <code>true</code>이면 실행하고, <code>false</code>가 되면 즉시 종료합니다.</li>
<li><strong>주의:</strong> 조건이 처음부터 <code>false</code>라면 내부 코드는 <strong>단 한 번도 실행되지 않습니다.</strong></li>
</ul>
<hr />
<h2 id="3-최소-한-번은-실행하는-repeat-while-구문"><strong>3. 최소 한 번은 실행하는 <code>repeat-while</code> 구문</strong></h2>
<p>다른 언어의 <code>do-while</code> 문과 동일한 역할을 합니다. 스위프트 2.0부터 <code>do</code> 키워드가 예외 처리용으로 사용되면서 <code>repeat</code>으로 명칭이 변경되었습니다.</p>
<h4 id="기본-형식-2"><strong>기본 형식</strong></h4>
<pre><code class="language-swift">repeat {
    &lt;실행할 구문&gt;
} while &lt;조건식&gt;</code></pre>
<ul>
<li><strong>작동 원리:</strong> 실행 블록을 <strong>먼저 실행한 후</strong>에 조건식을 평가합니다.</li>
<li><strong>핵심 차이:</strong> 조건이 처음부터 <code>false</code>일지라도, <strong>최소한 한 번은 코드가 실행</strong>된다는 점이 일반 <code>while</code>문과의 결정적인 차이입니다.</li>
</ul>
<hr />
<h3 id="📝-반복문-한눈에-비교하기"><strong>📝 반복문 한눈에 비교하기</strong></h3>
<table>
<thead>
<tr>
<th><strong>구분</strong></th>
<th><strong>for-in</strong></th>
<th><strong>while</strong></th>
<th><strong>repeat-While</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>기준</strong></td>
<td>횟수 기반 (순회 대상)</td>
<td>조건 기반 (선 평가)</td>
<td>조건 기반 (사후 평가)</td>
</tr>
<tr>
<td><strong>특징</strong></td>
<td>정해진 범위만큼 반복</td>
<td>조건이 참인 동안 반복</td>
<td>최소 1회 실행 보장</td>
</tr>
<tr>
<td><strong>주요 용도</strong></td>
<td>컬렉션 데이터 처리</td>
<td>입력 대기, 무한 루프</td>
<td>실행 후 조건 체크 필요시</td>
</tr>
</tbody></table>
<h4 id="반복문-제어할-땐"><strong>반복문 제어할 땐</strong></h4>
<ul>
<li><strong><code>break</code></strong>: 루프를 즉시 종료하고 빠져나갑니다.</li>
<li><strong><code>continue</code></strong>: 현재 루프를 중단하고 다음 반복 회차로 넘어갑니다.</li>
</ul>
<hr />
<br />

<blockquote>
<p>처음에는 <code>while</code>과 <code>repeat-while</code>의 차이가 머릿속에서 잘 정리가 안 되더라고요😅 
어차피 조건에 따라 반복하는 건 똑같지 않나 싶기도 했는데, <strong>언제 조건을 따지느냐</strong>에 따라 결과가 완전히 달라질 수 있는 아주 중요한 차이가 있습니다❕</p>
</blockquote>
<p>앞으로 상황에 딱 맞는 적절한 루프를 선택해서 더 깔끔한 코드를 짜기 위해 노력해보아요🤗 </p>