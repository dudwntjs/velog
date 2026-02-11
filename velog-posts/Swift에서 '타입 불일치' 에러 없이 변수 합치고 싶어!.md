<p>안녕하세요! 오늘은 Swift의 <strong>엄격한 타입 사랑</strong> 때문에 발생하는 에러를 해결해 보려고 합니다.
다른 언어에서는 대충 합쳐도 찰떡같이 알아듣던 것들이, Swift에서는 &quot;어딜 감히!&quot;라며 빨간 줄을 띄우곤 하죠😅</p>
<h2 id="swift는-끼리끼리-문화">Swift는 '끼리끼리' 문화?</h2>
<p>Swift는 타입에 있어서만큼은 아주 보수적입니다😤
문자열(<code>String</code>)과 숫자(<code>Int</code>)를 합치고 싶을 때, 우리는 흔하게 이렇게 +으로 붙이면 되는 거 아닌가? 생각합니다.</p>
<blockquote>
<p>&quot;우리 강아지 나이는 &quot; + 5</p>
</blockquote>
<p>하지만 Swift 컴파일러는 차갑게 대답합니다. <strong>&quot;안 돼, 돌아가. 타입 맞춰와🙅&quot;</strong></p>
<p>한 번 정해진 타입은 절대 바뀌지 않기 때문에, 우리가 직접 <strong>새로운 타입의 객체</strong>를 만들어줘야 합니다🌟</p>
<h3 id="해결책-1-직접-옷-갈아입히기-타입-변환👗">해결책 1: 직접 옷 갈아입히기 (타입 변환)👗</h3>
<p>가장 정석적인 방법은 <strong>숫자를 문자열이라는 옷으로 갈아입혀서 새로운 객체</strong>를 만드는 거예요.</p>
<pre><code class="language-swift">var dogName = &quot;우리집 초코의 나이는 &quot;
var age = 5

// age를 String()으로 감싸서 문자열로 재탄생!
var dogInfo = dogName + String(age) 

print(dogInfo) // 결과: 우리집 초코의 나이는 5</code></pre>
<p>⚠️ 기존의 <code>age</code> 변수가 변하는 게 아니라, <code>String(height)</code>를 통해 <strong>새로운 문자열 인스턴스</strong>가 만들어지는 거랍니다!
기존 변수는 그대로 <code>Int</code>예요😉
<br /></p>
<h3 id="해결책-2-만능-치트키-문자열-템플릿-템플릿-🪄">해결책 2: 만능 치트키, '문자열 템플릿' 템플릿 🪄</h3>
<p>사실 매번 <code>String()</code>으로 감싸는 건 너무 귀찮죠? 그래서 Swift가 제공하는 아주 편리한 기능이 있습니다.</p>
<p>바로 <strong>문자열 템플릿(String Interpolation)</strong>입니다!
사용법은 간단합니다! 문자열 안에서 <code>\(변수명)</code>만 써주면 끝납니다👍</p>
<pre><code class="language-swift">let cafe = &quot;별다방&quot;
let coffeePrice = 4500

// \( ) 안에 넣기만 하면 타입 변환 따위 신경 안 써도 됨!
let menuInfo = &quot;\(cafe) 아메리카노는 \(coffeePrice)원입니다.&quot;

print(menuInfo) // 결과: 별다방 아메리카노는 4500원입니다.</code></pre>
<p>이 기능이 정말 좋은 이유는, 괄호 안에서 <strong>간단한 계산</strong>도 가능하기 때문이에요🦄</p>
<pre><code class="language-swift">let americano = 4500
let cake = 6000
let totalDesc = &quot;총 결제 금액은 \(americano + cake)원입니다.&quot;

print(totalDesc) // 결과: 총 결제 금액은 10500원입니다.</code></pre>
<hr />
<h2 id="어떤-걸-쓸까요">어떤 걸 쓸까요?</h2>
<table>
<thead>
<tr>
<th><strong>방법</strong></th>
<th><strong>특징</strong></th>
<th><strong>추천 상황</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>타입 변환 (<code>String()</code>)</strong></td>
<td>명시적으로 새 객체를 생성함</td>
<td>변수 하나를 확실히 바꿔야 할 때</td>
</tr>
<tr>
<td><strong>문자열 템플릿 (<code>\( )</code>)</strong></td>
<td>사용법이 매우 쉽고 직관적임</td>
<td><strong>문장 중간에 값을 넣을 때 (🌟강력추천!)</strong></td>
</tr>
</tbody></table>
<br />

<p>Swift가 타입에 엄격한 건 여러분을 괴롭히려는 게 아니라, <strong>사고를 미연에 방지하기 위한 안전장치</strong>입니다‼️
이제 빨간 줄이 떠도 당황하지 말고 <code>\( )</code> 을 사용해보세요🪄</p>