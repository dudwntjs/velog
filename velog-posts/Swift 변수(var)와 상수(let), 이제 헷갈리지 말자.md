<p>Swift는 <strong>메모리에 저장된 데이터를 개발자가 쉽게 다룰 수 있도록 <span style="background-color: #d1ebf9;">변수</span>(variable)와 <span style="background-color: #ffdce0;">상수</span>(constant)</strong>를 제공합니다.
컴퓨터는 모든 데이터를 <strong>메모리의 특정 주소</strong>에 저장하지만, 이 주소는 너무 복잡해 사람이 직접 다루기 어렵습니다😵‍💫</p>
<p>그래서 프로그래밍 언어에서는 메모리 주소에 <strong>사람이 이해하기 쉬운 이름</strong>을 붙일 수 있도록 합니다.
이렇게 메모리의 특정 위치에 붙인 이름이 바로 <strong><span style="background-color: #d1ebf9;">변수</span>와 <span style="background-color: #ffdce0;">상수</span></strong>입니다.</p>
<p>개발자가 코드에서 변수나 상수 이름을 사용하면, 컴파일러는 그 이름에 연결된 메모리 주소를 찾아가 그 안에 저장된 값을 읽어 사용합니다.
따라서 우리는 복잡한 주소를 신경 쓰지 않고 <strong>이름만으로 값을 저장하고 꺼낼 수 있습니다.</strong></p>
<h2 id="변수와-상수란">변수와 상수란?</h2>
<p>변수와 상수는 <strong>값을 저장한다는 공통점</strong>이 있지만, <strong>값을 변경할 수 있는지 여부</strong>에서 차이가 있습니다.</p>
<table>
<thead>
<tr>
<th>구분</th>
<th><span style="background-color: #d1ebf9;">변수</span></th>
<th><span style="background-color: #ffdce0;">상수</span></th>
</tr>
</thead>
<tbody><tr>
<td>값 저장</td>
<td>가능</td>
<td>가능</td>
</tr>
<tr>
<td>값 변경</td>
<td><strong>여러 번 변경 가능</strong></td>
<td><strong>한 번 저장하면 변경 불가</strong></td>
</tr>
<tr>
<td>사용 목적</td>
<td>실행 중 값이 변하는 경우</td>
<td>항상 같은 값이어야 하는 경우</td>
</tr>
<tr>
<td>예시 상황</td>
<td>점수, 카운트, 상태 값 등</td>
<td>기준값, 고정 메시지, 설정 값 등</td>
</tr>
<tr>
<td>키워드</td>
<td><code>var</code></td>
<td><code>let</code></td>
</tr>
</tbody></table>
<aside>

<ul>
<li>실행 중 변하는 값 → <strong><span style="background-color: #d1ebf9;">변수</span></strong></li>
<li>항상 같은 값 → <strong><span style="background-color: #ffdce0;">상수</span></strong></aside>

</li>
</ul>
<h2 id="타입type-제한">타입(Type) 제한</h2>
<p>변수나 상수는 <strong>아무 값이나 자유롭게</strong> 저장할 수 있는 것은 아닙니다.</p>
<p>Swift에서는</p>
<ul>
<li><strong>처음 저장한 값의 타입이 변수/상수의 타입이 됩니다</strong></li>
<li>이후에는 <strong>같은 타입의 값만</strong> 저장할 수 있습니다</li>
</ul>
<blockquote>
<p>처음에 정수(Int)를 저장 → 이후에도 정수만 가능
처음에 문자열(String)을 저장 → 이후에도 문자열만 가능</p>
</blockquote>
<p>변수와 상수는 초기값과 동일한 타입의 값만 변경/저장 가능합니다.
타입이 다른 값을 넣으려고 하면 ❌ <strong>컴파일 오류 ❌</strong> 가 발생합니다.</p>
<h2 id="상수는-왜-필요할까">상수는 왜 필요할까?</h2>
<p>변수에 값을 넣고 안 바꾸면 되지, 왜 상수가 필요하지🤔 라고 생각했었는데요!</p>
<p>하지만 <strong><span style="background-color: #ffdce0;">상수</span></strong>를 사용하면 다음과 같은 장점이 있습니다.</p>
<ul>
<li>실수로 값이 변경되는 것을 <strong>미리 방지</strong></li>
<li>값의 성격이 명확해져 <strong>가독성과 유지보수성 향상</strong></li>
<li>코드의 의도를 더 분명하게 표현 가능</li>
</ul>
<p>따라서 <strong>변하지 않는 값은 <span style="background-color: #ffdce0;">상수</span>로 선언하는 것이 더 안전하고 효율적</strong>입니다.</p>
<h2 id="변수와-상수-선택이-헷갈릴-때">변수와 상수 선택이 헷갈릴 때</h2>
<p>어떤 값을 변수로 할지 상수로 할지 애매하다면 <strong>일단 <span style="background-color: #d1ebf9;">변수</span>로 선언해도 괜찮습니다.</strong></p>
<p>Swift에서는</p>
<ul>
<li>한 번 값이 할당된 이후</li>
<li>코드 어디에서도 변경되지 않는 변수</li>
</ul>
<p>가 있으면,
컴파일러가 이를 <strong><span style="background-color: #ffdce0;">상수(let)</span>로 바꾸는 것이 좋다</strong>고 알려줍니다💡
이 과정을 통해 자연스럽게 변수와 상수의 사용 기준을 알게 되는 것 같아요😋</p>
<h2 id="선언과-초기화initialize">선언과 초기화(Initialize)</h2>
<p>Swift에서는 변수와 상수를 <strong>먼저 선언한 후 사용</strong>해야 합니다.</p>
<h3 id="선언과-초기화를-함께-하는-경우">선언과 초기화를 함께 하는 경우</h3>
<pre><code class="language-swift">var a = 3
let b = &quot;hi&quot;</code></pre>
<h3 id="선언과-초기화를-분리하는-경우">선언과 초기화를 분리하는 경우</h3>
<ul>
<li><strong><span style="background-color: #d1ebf9;">변수(var)</span></strong>: 가능 ⭕</li>
</ul>
<pre><code class="language-swift">var year: Int
year = 1999</code></pre>
<ul>
<li><strong><span style="background-color: #ffdce0;">상수(let)</span></strong>: 기본적으로 불가능 ❌
  → 선언과 동시에 초기화해야 함</li>
</ul>
<pre><code class="language-swift">let birthYear = 1980</code></pre>
<h2 id="이름식별자-규칙-핵심">이름(식별자) 규칙 핵심</h2>
<ol>
<li><strong>연산자와 공백 사용 불가</strong><ul>
<li><code>+ - * /</code>, 공백 ❌</li>
<li><code>_</code>(언더바) ⭕<br /></li>
</ul>
</li>
<li><strong>예약어(키워드) 사용 불가</strong><ul>
<li><code>class</code>, <code>struct</code>, <code>enum</code> 등 ❌</li>
<li>대소문자 변경 시 사용 가능 (<code>Class</code> ⭕)<br /></li>
</ul>
</li>
<li><strong>이름의 첫 글자는 숫자 불가</strong><ul>
<li><code>2abc</code> ❌</li>
<li><code>abc2</code> ⭕</li>
</ul>
</li>
</ol>
<p>⚠️ Swift는 한글·특수문자도 허용하지만, <strong>실무에서는 영어 + 숫자 + 언더바만 사용하는 것이 원칙</strong>입니다.</p>
<br />

<blockquote>
<p>이제 Swift에서 <strong>변수와 상수</strong>의 역할과 사용 기준이 조금 더 명확해지셨나요??
이번 정리를 통해 <strong>var와 let</strong>을 언제 사용해야 하는지 이해하셨으면 좋겠습니다ㅎㅅㅎ</p>
</blockquote>