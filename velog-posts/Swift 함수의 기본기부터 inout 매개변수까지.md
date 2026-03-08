<p>스위프트(Swift)를 공부하다 보면 'Swift<strong>는 객체지향 언어이면서 동시에 함수형 프로그래밍 언어이다</strong>'라는 말을 들어보셨을 거에요.
그만큼 Swift에서 함수(Function)가 차지하는 비중과 중요성은 어마어마합니다😯</p>
<p>오늘은 Swift에서 함수를 어떻게 정의하고 호출하는지, 그리고 다른 언어와 차별화되는 <strong>스위프트만의 독특한 매개변수(Parameter) 다루기</strong> 기법들을 정리해 보겠습니다‼️</p>
<br />

<hr />
<h2 id="1-함수란-무엇인가">1. 함수란 무엇인가?</h2>
<p>프로그래밍에서 함수란 <strong>프로그램의 실행 과정 중에서 독립적으로 처리될 수 있는 부분을 분리하여 구조화한 객체</strong>를 의미합니다.
쉽게 말해, 외부 의존성 없이 툭 떼어 실행할 수 있는 코드 덩어리를 하나의 '캡슐'처럼 포장해 놓은 것이죠.</p>
<blockquote>
<p><strong>함수를 사용하면 좋은 점👍</strong></p>
</blockquote>
<ul>
<li><strong>재사용성:</strong> 동일한 코드를 여러 번 작성할 필요 없이 함수 호출만으로 처리할 수 있습니다.</li>
<li><strong>가독성:</strong> 전체 프로세스를 기능 단위로 쪼개어 이름(함수명)을 붙이므로 코드 흐름을 이해하기 쉽습니다.</li>
<li><strong>유지보수:</strong> 로직을 변경해야 할 때 함수 내부만 수정하면 되므로 관리가 용이합니다.</li>
</ul>
<br />

<hr />
<h2 id="2-함수-정의와-호출-방법">2. 함수 정의와 호출 방법</h2>
<p>스위프트에서 함수를 정의할 때는 명시적으로 <code>func</code> 키워드를 사용합니다.</p>
<pre><code class="language-swift">func 함수이름(매개변수1: 타입, 매개변수2: 타입) -&gt; 반환타입 {
    // 실행할 내용
    return 반환값
}</code></pre>
<p>매개변수와 반환값의 유무에 따라 함수는 다양하게 형태를 띨 수 있습니다. 반환값이 없는 경우 <code>-&gt; Void</code>를 쓰거나 아예 생략할 수 있습니다.</p>
<h4 id="1-매개변수와-반환값이-모두-없는-함수">1. 매개변수와 반환값이 모두 없는 함수</h4>
<pre><code class="language-swift">func printHello() {
    print(&quot;안녕하세요!&quot;)
}</code></pre>
<h4 id="2-매개변수만-있는-함수">2. 매개변수만 있는 함수</h4>
<pre><code class="language-swift">func printHelloWithName(name: String) {
    print(&quot;\(name)님, 안녕하세요&quot;)
}</code></pre>
<h4 id="3-매개변수와-반환값이-모두-있는-함수">3. 매개변수와 반환값이 모두 있는 함수</h4>
<pre><code class="language-swift">func sayHelloWithName(name: String) -&gt; String {
    return &quot;\(name)님, 안녕하세요&quot;
}</code></pre>
<br />

<hr />
<h3 id="2-튜플tuple을-활용한-다중-값-반환">2. 튜플(Tuple)을 활용한 다중 값 반환</h3>
<p>스위프트에서 함수는 기본적으로 <strong>단 하나의 값</strong>만 반환할 수 있습니다.
그래서 연관된 데이터를 한 번에 반환하고 싶을 때마다 구조체(Struct)나 클래스(Class)를 새로 만들어야 하는 건 부담스러울 수 있어요😂</p>
<p>이럴 때가볍게 여러 값을 묶어주는 <strong>튜플(Tuple)</strong>이 완벽한 해결책이 됩니다🌟</p>
<pre><code class="language-swift">// 튜플 타입을 typealias로 정의하여 가독성 향상
typealias UserInfo = (h: Int, age: Int, n: String)

func getUserInfo() -&gt; UserInfo {
    let height = 190
    let age = 20
    let name = &quot;sun&quot;

    return (height, age, name)
}

let info = getUserInfo()</code></pre>
<p>위 코드에서 눈여겨볼 점은 바로 <code>typealias</code>의 활용입니다❗️</p>
<p>함수의 반환형 자리에 <code>(h: Int, age: Int, n: String)</code>처럼 길고 복잡한 튜플 구조를 매번 적는다면 코드가 금방 지저분해지겠죠?
대신 이를 <code>UserInfo</code>라는 깔끔한 별명으로 묶어주면, 전체적인 코드의 가독성이 확연하게 올라가고 유지보수하기도 훨씬 편해집니다👍</p>
<br />

<hr />
<h2 id="3-스위프트만의-독특한-매개변수parameter-다루기">3. 스위프트만의 독특한 매개변수(Parameter) 다루기</h2>
<p>타 언어 개발자들이 스위프트를 처음 접할 때 가장 낯설어하는 부분이 바로 <strong>매개변수 호출 방식</strong>입니다.
이는 과거 Objective-C의 유산(호환성)을 이어받으면서 발전한 문법이라고 합니당</p>
<h3 id="1-내부외부-매개변수-분리와-생략-_">1) 내부/외부 매개변수 분리와 생략 (<code>_</code>)</h3>
<p>스위프트는 함수 외부에서 호출할 때 쓰는 이름(인자 레이블)과 내부에서 사용하는 변수명을 다르게 설정할 수 있습니다.</p>
<pre><code class="language-swift">func printHello(to name: String, welcomeMessage msg: String) {
    print(&quot;\(name)님, \(msg)&quot;)
}

printHello(to: &quot;sun&quot;, welcomeMessage: &quot;환영합니다!&quot;)</code></pre>
<p>스위프트에서는 함수의 매개변수를 용도에 따라 <strong>'외부 매개변수(Argument Label)'</strong>와 <strong>'내부 매개변수(Parameter Name)'</strong> 두 가지로 나누어 사용할 수 있습니다. 위 코드를 보면 매개변수 자리에 단어가 두 개씩 나란히 적혀 있는 것을 볼 수 있죠?</p>
<blockquote>
<ul>
<li><strong>외부 매개변수 (<code>to</code>, <code>welcomeMessage</code>):</strong> 함수를 밖에서 호출할 때 사용하는 이름표(레이블)입니다. 함수를 사용하는 사람에게 &quot;여기에 어떤 값을 넣어야 하는지&quot; 친절하게 알려주는 역할을 합니다.</li>
</ul>
</blockquote>
<ul>
<li><strong>내부 매개변수 (<code>name</code>, <code>msg</code>):</strong> 함수 내부 블록 <code>{ }</code> 안에서 실제로 값을 꺼내어 로직을 처리할 때 사용하는 변수명입니다.</li>
</ul>
<p>이렇게 두 가지로 이름을 분리하면 아주 큰 장점이 있는데요!
함수 내부에서는 <code>name</code>, <code>msg</code>처럼 짧고 실용적인 변수명을 사용해 코드를 간결하게 짤 수 있고, 반대로 함수를 호출하는 외부에서는 <code>printHello(to: &quot;sun&quot;, welcomeMessage: &quot;환영합니다!&quot;)</code>처럼 마치 영어 문장을 읽는 듯이 자연스럽고 명확하게 코드를 작성할 수 있어요☺️</p>
<p>만약 호출할 때 레이블을 쓰는 게 귀찮다면 <strong>언더바(<code>_</code>)</strong>를 사용하여 생략할 수도 있습니다.</p>
<pre><code class="language-swift">func printHello(_ name: String, _ msg: String) {
    print(&quot;\(name)님, \(msg)&quot;)
}

printHello(&quot;홍길동&quot;, &quot;반갑습니다&quot;)</code></pre>
<p>이러면 레이블 없이 깔끔하게 호출 가능합니다🥳</p>
<h3 id="2-가변-인자-">2) 가변 인자 (<code>...</code>)</h3>
<p>입력받을 인자의 개수가 정해지지 않았을 때는 타입 뒤에 <code>...</code>을 붙여 가변 인자로 만듭니다. 내부에서는 이 값들을 <strong>배열</strong>로 처리합니다.</p>
<pre><code class="language-swift">func avg(score: Int...) -&gt; Double {
    var total = 0
    for r in score {
        total += r
    }
    return Double(total) / Double(score.count)
}

print(avg(score: 95, 100, 88, 92)) // sun의 4과목 평균: 93.75</code></pre>
<h3 id="3-기본값을-갖는-매개변수">3) 기본값을 갖는 매개변수</h3>
<p>자주 사용하는 값이 있다면 매개변수에 기본값(<code>=</code>)을 지정해 둘 수 있습니다. 기본값이 있는 매개변수는 함수 호출 시 생략이 가능합니다.</p>
<pre><code class="language-swift">func echo(message: String, newline: Bool = true) {
    if newline {
        print(message)
    } else {
        print(message, terminator: &quot;&quot;)
    }
}

echo(message: &quot;sun님 안녕하세요&quot;)</code></pre>
<p><code>newline 파라미터</code>를 생략해도 자동으로 <strong>true</strong>가 적용됩니다.</p>
<br />

<hr />
<h2 id="4-매개변수는-변수가-아니라-상수let다">4. 매개변수는 변수가 아니라 '상수(let)'다</h2>
<p>함수 내부에서 인자로 받은 값을 수정하려고 하면 에러(<code>Left side of mutating operator isn't mutable</code>)가 발생합니다.</p>
<img src="https://velog.velcdn.com/images/dudwntjs/post/b39526d6-86d7-4cea-b0f2-6fa36412ff7a/image.png" width="1000" />

<p>Swift에서 함수의 매개변수는 기본적으로 상수(<code>let</code>)로 정의되기 때문입니다.</p>
<p>만약 <strong>함수 내부에서 매개변수 값을 가공</strong>해야 한다면, 내부에서 동일한 이름의 변수(<code>var</code>)를 새로 선언하여 값을 복사한 뒤 사용하면 됩니다💡</p>
<pre><code class="language-swift">func incrementAge(currentAge: Int) -&gt; Int {
    var currentAge = currentAge
    currentAge += 1
    return currentAge
}

print(incrementAge(currentAge: 20))</code></pre>
<p>기존에 있던 이름(매개변수)을 새로운 변수가 덮어써서 가려버리는 기법을 프로그래밍 용어로 <strong>섀도잉(Shadowing)</strong>이라고 부릅니다.</p>
<br />

<hr />
<h2 id="5-값-전달-vs-참조-전달-inout-매개변수">5. 값 전달 vs 참조 전달: <code>inout</code> 매개변수</h2>
<p>Swift 함수에서도 격리된 함수 내부에서 <strong>함수 외부의 원본 변수 값 자체를 수정</strong>하는 마법 같은 키워드가 있습니다. 바로 <code>inout</code> 입니다.</p>
<blockquote>
<ul>
<li><strong>값에 의한 전달(Pass by Value):</strong> 원본 값을 복사하여 전달. 내부에서 지지고 볶아도 외부 원본은 그대로 (Swift의 기본 동작)</li>
</ul>
</blockquote>
<ul>
<li><strong>참조에 의한 전달(Pass by Reference):</strong> 값이 저장된 <strong>메모리 주소</strong>를 전달. 내부에서 수정하면 외부 원본도 같이 수정</li>
</ul>
<pre><code class="language-swift">var count = 30

func foo(paramCount: inout Int) -&gt; Int {
    paramCount += 1
    return paramCount
}

print(foo(paramCount: &amp;count))
print(count)</code></pre>
<p>이렇게 매개변수 타입 앞에 <code>inout</code> 키워드를 명시해 주고, 함수를 호출할 때는 인자값 앞에 <strong>주소 추출 연산자(<code>&amp;</code>)</strong>를 붙여, 값이 아닌 '메모리 주소'를 직접 전달합니다.
이렇게 하면 함수 내부에서 값을 수정했을 때, 껍데기(복사본)가 아닌 <strong>함수 외부의 원본 변수(<code>count</code>) 값도 똑같이 변경</strong>됩니다🤩</p>
<blockquote>
<p>⚠️ <strong>주의:</strong> <code>inout</code> 매개변수에는 메모리 주소를 넘겨야 하므로 상수(<code>let</code>)나 리터럴 값(ex: <code>30</code>)은 전달할 수 없고 오직 <strong>변수(<code>var</code>)만 전달</strong>할 수 있습니다.</p>
</blockquote>
<br />

<hr />
<h2 id="6-변수의-생존-범위scope와-생명-주기">6. 변수의 생존 범위(Scope)와 생명 주기</h2>
<p>프로그램 내에서 변수가 숨 쉴 수 있는 구역을 스코프(Scope)라고 합니다. 중괄호 <code>{ }</code> 블록이 하나의 생존 구역이 되는 거에요!</p>
<blockquote>
<ul>
<li><strong>전역 변수(Global Variable):</strong> 프로그램 최상위에 선언되어 어디서든 접근 가능</li>
</ul>
</blockquote>
<ul>
<li><strong>지역 변수(Local Variable):</strong> 함수나 조건문 등 특정 블록 <code>{ }</code> 내부에서 선언되어, 해당 블록이 끝나면(return) 메모리에서 함께 소멸</li>
</ul>
<p>만약 함수 외부(전역)와 함수 내부(지역)에 <strong>동일한 이름의 변수</strong>가 있다면 어떻게 될까요?</p>
<pre><code class="language-swift">var name = &quot;sun&quot; // 전역 변수

func printName() {
    var name = &quot;moon&quot; // 지역 변수 (전역 변수 name을 섀도잉)
    print(&quot;함수 안에서 부르면? -&gt; \(name)&quot;)
}

printName()
print(&quot;함수 밖에서 부르면? -&gt; \(name)&quot;)</code></pre>
<blockquote>
<ul>
<li><strong><code>printName()</code> 실행 결과:</strong> <code>함수 안에서 부르면? -&gt; moon</code></li>
</ul>
</blockquote>
<ul>
<li><strong>함수 밖 <code>print</code> 실행 결과:</strong> <code>함수 밖에서 부르면? -&gt; sun</code></li>
</ul>
<p>이렇게 Swift는 <strong>가장 가까운 블록에 있는 내부 변수(지역 변수)를 우선적으로 사용</strong>합니다.
이를 변수의 섀도잉(Shadowing)이라고 부르며, 외부 변수와는 완전히 독립적으로 취급됩니다.</p>
<br />

<hr />
<p>이번편은 Swift 함수의 뼈대와 매개변수의 다양한 활용법, 변수의 스코프까지 알아보았습니다.</p>
<p>다음편에서는 Swift 가 왜 함수형 언어인지를 보여주는 <strong>'일급 객체로서의 함수'와 '클로저(Closure)'의 세계</strong>로 넘어가 보겠습니다! 기대해주세요ㅎㅎ</p>