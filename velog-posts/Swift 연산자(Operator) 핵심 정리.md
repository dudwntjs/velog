<p>프로그래밍 개발의 기본 중 기본인 <strong>연산자(Operator)</strong>에 대해 알아보겠습니다!</p>
<p>Swift의 연산자는 생각보다 까다롭고 똑똑하답니다. 계산기 두드리듯 뚝딱 되는 것부터, Swift에만 있는 아주 독특한 녀석들까지 있는데요!
연산자들을 정복하고, 가독성 넘치고 깔끔한 코드를 짜도록 해봅시다💪</p>
<h2 id="1-산술-연산자-arithmetic-operators">1. 산술 연산자 (Arithmetic Operators)</h2>
<p>수학적 계산을 수행하는 가장 기본적인 연산자입니다.</p>
<table>
<thead>
<tr>
<th>구분</th>
<th>연산자</th>
<th>예시</th>
<th>의미</th>
</tr>
</thead>
<tbody><tr>
<td><strong>단항</strong></td>
<td><code>-</code></td>
<td><code>-a</code></td>
<td>값의 부호를 변경 (양수↔음수)</td>
</tr>
<tr>
<td><strong>이항</strong></td>
<td><code>+</code></td>
<td><code>a + b</code></td>
<td>더하기</td>
</tr>
<tr>
<td><strong>이항</strong></td>
<td><code>-</code></td>
<td><code>a - b</code></td>
<td>빼기</td>
</tr>
<tr>
<td><strong>이항</strong></td>
<td><code>*</code></td>
<td><code>a * b</code></td>
<td>곱하기</td>
</tr>
<tr>
<td><strong>이항</strong></td>
<td><code>/</code></td>
<td><code>a / b</code></td>
<td>나누기 (몫을 반환)</td>
</tr>
<tr>
<td><strong>이항</strong></td>
<td><code>%</code></td>
<td><code>a % b</code></td>
<td>나머지 연산 (나머지 값을 반환)</td>
</tr>
</tbody></table>
<blockquote>
<p>⚠️ <strong>주의사항: 공백 규칙</strong>
Swift는 연산자 양쪽의 공백에 민감합니다. 
<code>a+b</code>나 <code>a + b</code>는 괜찮지만, <code>a +b</code>처럼 한쪽만 공백을 주면 에러가 발생합니다! 양쪽 공백을 동일하게 유지해야 합니다.</p>
</blockquote>
<br />

<hr />
<h2 id="2-비교-연산자-comparison-operators">2. 비교 연산자 (Comparison Operators)</h2>
<p>두 값을 비교하며, 결과값은 항상 <strong>Bool 타입(<code>true</code> 또는 <code>false</code>)</strong>으로 반환됩니다.</p>
<ul>
<li><code>a &lt; b</code> : a가 b보다 작으면 <code>true</code></li>
<li><code>a &gt; b</code> : a가 b보다 크면 <code>true</code></li>
<li><code>a &lt;= b</code> : a가 b보다 작거나 같으면 <code>true</code></li>
<li><code>a &gt;= b</code> : a가 b보다 크거나 같으면 <code>true</code></li>
<li><code>a == b</code> : a와 b가 같으면 <code>true</code> (수학의 <code>=</code> 역할)</li>
<li><code>a != b</code> : a와 b가 다르면 <code>true</code><br />

</li>
</ul>
<hr />
<h2 id="3-논리-연산자-logical-operators">3. 논리 연산자 (Logical Operators)</h2>
<p>논리 값(<code>Bool</code>)을 비교하여 결과를 냅니다.
조건문(<code>if</code>)이나 반복문(<code>while</code>)에서 매우 자주 사용됩니다.</p>
<table>
<thead>
<tr>
<th>연산자</th>
<th>기호</th>
<th>의미</th>
<th>특징</th>
</tr>
</thead>
<tbody><tr>
<td>NOT</td>
<td><strong>!a</strong></td>
<td>논리 부정</td>
<td>true → false, false → true (반대로 뒤집기)</td>
</tr>
<tr>
<td>AND</td>
<td>*<em>a &amp;&amp; b *</em></td>
<td>논리 곱</td>
<td>둘 다 true일 때만 결과가 true (까다로움😖)</td>
</tr>
<tr>
<td>OR</td>
<td>*<em>a || b *</em></td>
<td>논리 합</td>
<td>둘 중 하나라도 true이면 결과가 true (관대함☺️)</td>
</tr>
<tr>
<td><br /></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody></table>
<hr />
<h2 id="4-삼항-조건-연산자-ternary-conditional-operator💡">4. 삼항 조건 연산자 (Ternary Conditional Operator)💡</h2>
<p>조건에 따라 두 가지 값 중 하나를 선택할 때 사용합니다.
<code>if-else</code> 문을 단 한 줄로 줄여주는 마법 같은 연산자입니다✨</p>
<ul>
<li><strong>표기:</strong> <code>조건문 ? 참일_때_값 : 거짓일_때_값</code></li>
<li><strong>의미:</strong> 조건이 <code>true</code>이면 앞의 값을, <code>false</code>이면 뒤의 값을 반환합니다.</li>
</ul>
<pre><code class="language-swift">let height = 185
let result = height &gt; 180 ? &quot;키가 커요&quot; : &quot;적당해요&quot;
// 결과: &quot;키가 커요&quot;</code></pre>
<p><code>if-else</code>로 4~5줄 써야 할 코드를 한 줄로 끝낼 수 있어 코드가 아주 간결해집니당👍
Swift 코드짤 때 가장 많이 의식해서 쓰는 연산자같아용🤓</p>
<br />

<hr />
<h2 id="5-범위-연산자-range-operators⭐">5. 범위 연산자 (Range Operators)⭐</h2>
<p>Swift에서만 볼 수 있는 독특한 연산자로, 특정 구간의 범위를 나타낼 때 사용합니다.</p>
<h3 id="①-닫힌-범위-연산자-closed-range">① 닫힌 범위 연산자 (Closed Range)</h3>
<ul>
<li><strong>표기:</strong> <code>a...b</code></li>
<li><strong>범위:</strong> a부터 b까지 (<strong>b 포함</strong>)</li>
<li><strong>예시:</strong> <code>1...5</code> → <code>1, 2, 3, 4, 5</code></li>
</ul>
<h3 id="②-반-닫힌-범위-연산자-half-open-range">② 반 닫힌 범위 연산자 (Half-Open Range)</h3>
<ul>
<li><strong>표기:</strong> <code>a..&lt;b</code></li>
<li><strong>범위:</strong> a부터 b 미만까지 (<strong>b 미포함</strong>)</li>
<li><strong>예시:</strong> <code>1..&lt;5</code> → <code>1, 2, 3, 4</code></li>
<li><strong>주로 어디에 쓰나요?</strong> 배열의 인덱스가 0부터 시작하므로, 배열을 순회할 때 매우 유용🌟</li>
</ul>
<blockquote>
<p>⚠️ <strong>런타임 에러 주의!</strong>
범위 연산자는 항상 <strong>[작은 숫자]...[큰 숫자]</strong> 순서여야 합니다. <code>5...1</code>처럼 작성하면 실행 중 에러(<code>fatal error</code>)가 발생합니다.</p>
</blockquote>
<br />

<hr />
<h2 id="6-대입-연산자--복합-대입-연산자">6. 대입 연산자 &amp; 복합 대입 연산자</h2>
<p>값을 변수에 할당하거나, 연산과 할당을 한 번에 처리합니다.</p>
<ul>
<li><code>a = b</code> : b의 값을 a에 대입</li>
<li><code>a += b</code> : <code>a = a + b</code>와 동일</li>
<li><code>a -= b</code> : <code>a = a - b</code>와 동일</li>
<li><code>a *= b</code> : <code>a = a * b</code>와 동일</li>
<li><code>a /= b</code> : <code>a = a / b</code>와 동일</li>
<li><code>a %= b</code> : <code>a = a % b</code>와 동일<br />

</li>
</ul>
<hr />
<p>지금까지 Swift의 다양한 연산자들을 살펴봤습니다! 정리해보니까 종류가 많네요😅 마지막으로 간단하게 정리해볼게요❕</p>
<blockquote>
<ul>
<li><strong>공백 주의:</strong> <code>a + b</code>는 되지만 <code>a+ b</code>는 안 됩니다. (연산자 양쪽 공백은 항상 동일하게!)</li>
</ul>
</blockquote>
<ul>
<li><strong>삼항 연산자:</strong> <code>if-else</code> 쓰기 귀찮을 때 한 줄로 해결하는 최고의 꿀템입니다.</li>
<li><strong>범위 연산자:</strong> 배열 인덱스를 다루거나 특정 횟수만큼 반복할 땐 <code>...</code>과 <code>..&lt;</code>를 꼭 기억하세요.</li>
<li><strong>대입 연산자:</strong> <code>a += 1</code>처럼 복합 대입 연산자를 쓰면 코드가 훨씬 간결해집니다.</li>
</ul>
<br />

<p><span style="color: gray;">사실 코딩할 때 문법을 하나하나 떠올리며 짜진 않지만, 이렇게 한 번씩 개념을 정리해두면 나중에 복잡한 로직을 짤 때 훨씬 덜 헷갈리고 코드도 깔끔해지더라고요ㅎㅎ
앞으로도 계속해서 Swift 기초 문법에 대해 정리해나가겠습니다🤩</span></p>