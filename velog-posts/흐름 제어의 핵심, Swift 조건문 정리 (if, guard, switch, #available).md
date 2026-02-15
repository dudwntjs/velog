<p>프로그래밍에서 <strong>조건문(Conditional Statements)</strong>은 하나 이상의 조건값에 따라 특정 구문을 실행하도록 프로그램의 흐름을 분기하는 역할을 합니다.</p>
<p>Swift에서는 이 조건문을 어떻게 더 안전하고 깔끔하게 사용하는지, 저와 함께 차근차근 파헤쳐 봅시다🤓</p>
<h2 id="1-if-구문-가장-보편적인-분기문"><strong>1. if 구문: 가장 보편적인 분기문</strong></h2>
<p><code>if</code> 구문은 조건식의 결과가 참(<code>true</code>)일 때만 코드 블록을 실행합니다.
가장 보편적이고 직관적인 조건문입니다.</p>
<h3 id="기본-형식"><strong>기본 형식</strong></h3>
<pre><code class="language-swift">if &lt;조건식&gt; {
    &lt;실행할 구문&gt;
}</code></pre>
<ul>
<li><strong>Bool 타입 필수</strong>
  조건식은 반드시 <code>true</code> 또는 <code>false</code>를 반환해야 합니다.
  (C 언어처럼 <code>0</code>이나 <code>1</code> 사용 불가 ❌)  <br /></li>
<li><strong>중괄호 생략 불가</strong>
  실행 코드가 한 줄이라도 <code>{}</code>를 반드시 작성해야 합니다.</li>
</ul>
<h3 id="논리적-구멍을-막는-if--else"><strong>논리적 구멍을 막는 if ~ else</strong></h3>
<p>조건이 참이 아닐 때 실행할 코드가 있다면 <code>else</code>를 사용합니다.</p>
<p>이는 수학적으로 <strong>이율배반(Antinomy) 사건</strong>을 형성하여, 두 블록 중 하나는 반드시 실행되도록 보장합니다.</p>
<pre><code class="language-swift">var age = 21
var adult = 19

if age &lt; adult {
    print(&quot;당신은 미성년자!&quot;)
} else {
    print(&quot;당신은 성년자!&quot;)
}</code></pre>
<h3 id="다중-분기와-중첩"><strong>다중 분기와 중첩</strong></h3>
<pre><code class="language-swift">if condition1 {

}else if condition2 {

}else {

}</code></pre>
<ul>
<li>여러 조건을 순차적으로 비교할 수 있음</li>
<li>첫 번째로 참이 되는 조건만 실행</li>
<li><strong>3단계 이상의 중첩은 가독성을 심각하게 해침 🤢</strong>
  👉 중첩이 깊어진다면 <code>switch</code>나 <code>guard</code>로 리팩토링을 고려하는 게 좋아요!</li>
</ul>
<br />

<hr />
<h2 id="2-guard-구문-조기-종료early-exit"><strong>2. guard 구문: 조기 종료(Early Exit)</strong></h2>
<p><code>guard</code>는 조건을 만족하지 않을 경우 <strong>즉시 함수(또는 반복문)를 종료</strong>하는 구조입니다.
정상 흐름을 아래쪽에 유지하고, 비정상 흐름을 위에서 걸러내는 필터 역할을 합니다.</p>
<h3 id="기본-형식-1"><strong>기본 형식</strong></h3>
<pre><code class="language-swift">guard &lt;조건식&gt; else {
    &lt;조건이 false일 때 실행될 코드&gt;
    return // break, continue, throw 등 종료 구문 필수
}</code></pre>
<ul>
<li><code>else</code> 블록 안에는 반드시 <strong>종료 구문</strong>이 있어야 함</li>
<li>코드 Depth(들여쓰기 깊이)를 증가시키지 않음</li>
<li>오류 방지에 매우 효과적</li>
</ul>
<h3 id="실전-예제-0으로-나누기-방지"><strong>실전 예제 (0으로 나누기 방지)</strong></h3>
<pre><code class="language-swift">func divide(base: Int) {
    // base가 0이 아니어야 한다는 조건을 '가드'합니다.
    guard base != 0 else {
        print(&quot;연산할 수 없습니다. (Divide By Zero 오류)&quot;)
        return
    }
    print(&quot;결과: \(100 / base)&quot;)
}</code></pre>
<ul>
<li>조건 불만족 상황을 입구에서 차단</li>
<li>코드 가독성 향상</li>
<li>정상 로직이 더 잘 드러남
  ⇒ 이런 이유 때문에 <code>guard</code>는 실무에서 매우 많이 사용됩니다✨</li>
</ul>
<br />

<hr />
<h2 id="3-switch-구문-강력한-패턴-매칭과-분기"><strong>3. switch 구문: 강력한 패턴 매칭과 분기</strong></h2>
<p><code>switch</code> 구문은 단순히 값의 일치 여부를 확인하는 것을 넘어, <strong>패턴(Pattern)</strong>을 통해 실행 블록을 결정하는 매우 강력한 조건문입니다.</p>
<p>Swift의 <code>switch</code>는 다른 언어보다 훨씬 안전하고 표현력이 뛰어납니다🤩</p>
<h3 id="기본-형식-2">기본 형식</h3>
<pre><code class="language-swift">switch &lt;비교대상&gt; {
case &lt;패턴1&gt;:
    &lt;실행 코드&gt;
case &lt;패턴2&gt;:
    &lt;실행 코드&gt;
default:
    &lt;기타 경우&gt;
}</code></pre>
<h4 id="1-암시적-fallthrough-없음">(1) 암시적 fallthrough 없음</h4>
<p>Swift의 <code>switch</code>는 일치하는 <code>case</code> 하나만 실행하고 자동 종료됩니다.</p>
<pre><code class="language-swift">let value = 2

switch value {
case 1:
    print(&quot;1입니다.&quot;)
case 2:
    print(&quot;2입니다.&quot;)
default:
    print(&quot;기타 값&quot;)
}</code></pre>
<p><code>break</code>를 쓰지 않아도 다음 케이스로 넘어가지 않습니다👍</p>
<h4 id="2-완전성exhaustivity">(2) 완전성(Exhaustivity)</h4>
<p><code>switch</code>는 가능한 모든 경우를 반드시 처리해야 합니다.
빠진 경우가 있다면 <code>default</code>를 작성해야 하며, 그렇지 않으면 컴파일 에러가 발생합니다🚨</p>
<h4 id="3-다중-패턴-매칭">(3) 다중 패턴 매칭</h4>
<p>쉼표(,)를 이용해 여러 값을 한 번에 처리할 수 있습니다.</p>
<pre><code class="language-swift">let value = 3

switch value {
case 0, 1:
    print(&quot;0 또는 1&quot;)
case 2, 3:
    print(&quot;2 또는 3&quot;)
default:
    print(&quot;기타&quot;)
}</code></pre>
<h4 id="4-범위-매칭">(4) 범위 매칭</h4>
<p>값 하나하나 비교하지 않고 범위를 사용할 수 있습니다.</p>
<pre><code class="language-swift">let score = 85

switch score {
case 90...100:
    print(&quot;A&quot;)
case 80..&lt;90:
    print(&quot;B&quot;)
default:
    print(&quot;C 이하&quot;)
}</code></pre>
<h4 id="5-튜플과-where-조건">(5) 튜플과 where 조건</h4>
<p>여러 값을 동시에 비교하거나 추가 조건을 붙일 수 있습니다.</p>
<pre><code class="language-swift">let point = (3, -3)

switch point {
case let (x, y) where x == -y:
    print(&quot;x == -y 선상&quot;)
case let (x, y):
    print(&quot;일반 좌표: \(x), \(y)&quot;)
}</code></pre>
<h3 id="if-vs-switch-한눈에-비교하기">if vs switch, 한눈에 비교하기</h3>
<blockquote>
<h4 id="if">if</h4>
<p><strong>단순한 조건</strong>이 필요할 때
참/거짓(<code>True</code>/<code>False</code>)으로 명확히 나뉘는 경우
<code>age &gt; 20</code>처럼 단순한 크기 비교가 중심일 때</p>
</blockquote>
<blockquote>
<h4 id="switch">switch</h4>
</blockquote>
<ul>
<li><strong>복잡한 패턴</strong>을 나눌 때</li>
<li>비교해야 할 대상이 많거나 다양한 경우</li>
<li><strong>튜플, 범위 연산자</strong> 등 구조적인 매칭이 필요할 때</li>
</ul>
<br />

<hr />
<h2 id="4-available-os-버전별-분기"><strong>4. #available: OS 버전별 분기</strong></h2>
<p>최신 API를 사용할 때 하위 버전 기기에서 앱이 죽는 것을 방지하기 위해 사용합니다.</p>
<pre><code class="language-swift">if #available(iOS 9, OSX 10.10, watchOS 1, *) {
    // 해당 버전 이상에서만 실행될 최신 API 코드
} else {
    // 하위 버전 사용자를 위한 예외 처리 코드
}</code></pre>
<ul>
<li><code>*</code>는 필수이며, 나열되지 않은 나머지 플랫폼에서도 정상 작동함을 의미</li>
<li>런타임 안정성 확보</li>
</ul>
<br />

<hr />
<h3 id="📝-조건문-최종-정리">📝 조건문 최종 정리</h3>
<blockquote>
<ul>
<li><strong>단순한 참/거짓 분기</strong>는 <code>if</code></li>
</ul>
</blockquote>
<ul>
<li><strong>조건 불만족 시 빠른 종료</strong>는 <code>guard</code></li>
<li><strong>다양한 패턴(범위, 튜플) 매칭</strong>은 <code>switch</code></li>
<li><strong>OS 버전 체크</strong>는 <code>#available</code></li>
</ul>
<p>상황에 맞는 적절한 조건문을 선택하면 코드의 가독성이 높아지고 리소스를 절약할 수 있습니다😚</p>
<p><span style="color: gray;">그동안 무심코 <code>if</code>만 남발하고 있지는 않았는지, 중첩이 깊어졌는데도 그냥 두고 있지는 않았는지 이번 정리를 통해 한 번쯤 돌아보게 되네요ㅎㅎ</span></p>