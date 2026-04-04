<p>안녕하세요! 오늘은 Swift가 객체지향 언어이면서 동시에 <strong>함수형 언어</strong>로 불리는 결정적인 이유, 바로 <strong>일급 객체(First-Class Object)로서의 함수</strong>에 대해 알아보겠습니다.
<br /> 
영국의 컴퓨터 과학자 크리스토퍼 스트래치(Christopher Strachey)가 처음 사용한 이 개념은, 프로그래밍 언어 내에서 <strong>함수가 ⭐️특별 대우⭐️</strong>를 받는다는 것을 의미합니다. 
과연 어떤 능력을 갖추고 있는지 하나씩 살펴볼까요?
  <br /></p>
<hr />
<h2 id="1-일급-객체의-3가지-조건">1. 일급 객체의 3가지 조건</h2>
<p>어떤 객체가 '일급'의 지위를 가지려면 다음 조건들을 만족해야 합니다.
Swift의 함수는 이 까다로운 조건을 다 통과한 능력자에요😎</p>
<blockquote>
<h3 id="일급-객체의-조건">일급 객체의 조건</h3>
</blockquote>
<ol>
<li><p>변수나 상수에 <strong>대입</strong>할 수 있다.</p>
</li>
<li><p>함수의 <strong>인자값(파라미터)</strong>으로 전달할 수 있다.</p>
</li>
<li><p>함수의 <strong>반환값(리턴값)</strong>으로 사용할 수 있다.</p>
<br />

</li>
</ol>
<h2 id="2-변수나-상수에-함수-대입하기">2. 변수나 상수에 함수 대입하기</h2>
<p>우리는 보통 함수의 '결과값'을 변수에 담죠?
하지만 일급 함수는 <strong>함수 그 자체</strong>를 변수에 쏙! 집어넣을 수 있습니다.</p>
<pre><code class="language-swift">func foo(base: Int) -&gt; String {
    return &quot;결과값은 \(base + 1)입니다.&quot;
}

let fn1 = foo(base: 5)
let fn2 = foo

fn2(5)</code></pre>
<ul>
<li><code>fn1</code> → 함수 실행 결과가 저장된 값 (<code>String</code>)</li>
<li><code>fn2</code> → 함수 자체가 저장됨</li>
<li><code>fn2(5)</code>처럼 변수로 함수를 호출할 수 있음
→ 함수가 값처럼 다뤄지는 걸 볼 수 있어요‼️</li>
</ul>
<p>함수를 변수에 대입할 때는 함수를 실행하는 것이 아니라, <strong>함수라는 객체</strong>만 전달하는 거에요! 그래서 <code>fn2(5)</code>처럼 변수 이름을 마치 함수 이름인 것처럼 똑같이 호출할 수 있게 됩니다🤓</p>
  <br />

<h2 id="3-함수를-반환하는-함수">3. 함수를 반환하는 함수</h2>
<p>Swift에서는 함수가 실행된 결과로 <strong>또 다른 함수를 리턴</strong>할 수 있습니다.</p>
<p>이를 통해 상황에 맞는 함수를 동적으로 선택할 수 있어요!</p>
<pre><code class="language-swift">func plus(a: Int, b: Int) -&gt; Int { return a + b }
func minus(a: Int, b: Int) -&gt; Int { return a - b }

func calc(_ operand: String) -&gt; (Int, Int) -&gt; Int {
    switch operand {
    case &quot;+&quot;: return plus
    case &quot;-&quot;: return minus
    default: return plus
    }
}

let resultFn = calc(&quot;+&quot;) // plus 함수가 반환됨!
print(resultFn(3, 4))    // 결과: 7</code></pre>
<p>위의 <code>calc</code> 함수처럼 함수를 반환할 때는, 반환 타입 자리의 함수 타입(예: <code>(Int, Int) -&gt; Int</code>)을 정확히 해석하는 게 포인트예요! 
”정수 두 개를 받아 정수 하나를 뱉는 함수를 돌려주겠다!&quot;라는 뜻입니다😊</p>
  <br />


<h2 id="4-함수를-인자로-받는-함수-콜백-함수">4. 함수를 인자로 받는 함수 (콜백 함수)</h2>
<p>함수를 다른 함수의 파라미터로 넘길 수도 있습니다. 이걸 활용하면 코드 내부를 고치지 않고도 외부에서 실행 흐름을 내 맘대로 조절하는 <strong>콜백(Callback)</strong> 구조를 만들 수 있어요!🤝</p>
<pre><code class="language-swift">func broker(base: Int, function fn: (Int) -&gt; Int) -&gt; Int {
    return fn(base) // 인자로 받은 함수를 내부에서 대신 실행!
}

print(broker(base: 3, function: { $0 + 1 })) // 결과: 4</code></pre>
<ul>
<li>함수 <code>fn</code>을 파라미터로 받음</li>
<li>내부에서 <code>fn(base)</code> 실행</li>
<li><code>{ $0 + 1 }</code> → 클로저(익명 함수)</li>
<li>외부에서 동작을 주입하는 구조 = <strong>콜백 패턴</strong></li>
</ul>
<p>이 방식은 특정 작업이 끝난 후 성공/실패 처리를 할 때 아주 유용해요. Swift에서는 이걸 더 간결하게 쓰기 위해 <strong>클로저(Closure)</strong>라는 친구를 자주 소환합니다.</p>
  <br />

<h2 id="5-함수의-중첩과-클로저-nested-functions">5. 함수의 중첩과 클로저 (Nested Functions)</h2>
<p>함수 안에 또 다른 함수를 작성하는 <strong>중첩 함수</strong>!
여기가 정말 중요한 포인트인데요🤩</p>
<blockquote>
<ul>
<li><strong>은닉성:</strong> 내부 함수는 외부 함수 밖에서는 절대 보이지 않아요! (보안 철저🔒)</li>
</ul>
</blockquote>
<ul>
<li><strong>값의 캡처(Capture):</strong> 외부 함수가 종료되어도, 내부 함수가 주변 변수를 참조하고 있다면 그 값을 끝까지 기억하고 유지합니다.</li>
</ul>
<pre><code class="language-swift">func basic(param: Int) -&gt; (Int) -&gt; Int {
    let value = param + 20

    func append(add: Int) -&gt; Int {
        return value + add // 외부 함수의 'value'를 캡처해서 기억해요!
    }

    return append
}

let myClosure = basic(param: 10) 
print(myClosure(10)) // 40 (이미 끝난 함수의 value인 30을 기억 중!)</code></pre>
<ul>
<li><code>append</code>는 <code>basic</code> 내부에 있는 <strong>중첩 함수</strong></li>
<li><code>value</code>를 캡처해서 기억함</li>
<li><code>basic</code>이 끝나도 <code>value</code>는 살아있음</li>
</ul>
<p><strong>클로저</strong>는 '내부 함수'와 그 함수가 태어난 '주변 환경(Context)'을 통째로 저장한 보따리 같은 객체라고 이해하면 쉬워요🎒</p>
<br />

<hr />
<p><strong>Swift에서 함수가 일급 객체</strong>라는 것은, 함수를 단순히 실행 코드가 아니라 하나의 데이터(값)처럼 자유롭게 다룰 수 있다는 뜻입니다.</p>
<p>이 특성을 잘 활용하면 코드의 재사용성을 극대화하고, 훨씬 유연한 프로그래밍이 가능해져요!
다음에는 이 일급 함수의 연장선이자 <strong>Swift의 꽃인 '클로저'</strong>의 더 깊은 문법에 대해 알아보겠습니다 🥳</p>